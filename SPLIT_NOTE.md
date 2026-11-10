# Per-listener train/test split

**Key prompt:** "Load into tables, report row counts and distinct listeners and tracks, and split each listener's plays by played_at."

## Holdout length
- Rule: per listener, plays sorted by `played_at` (`play_id` breaks ties); the last 20% of plays (at least 1) are held out.
- Result: 22 held-out plays across 18 listeners (14 hold out 1 play, 4 hold out 2); 58 training plays across 19 listeners.
- Held-out dates span 2024-07-31 → 2024-09-08 (40 days overall).
- Listener 101999 has a single play and no holdout; they are kept in training only.

## Checked
- Row counts: 80 plays, 19 listeners, 46 distinct tracks played; catalogue has 60 tracks in 10 genres.
- Every played `track_id` exists in the catalogue; no duplicate `play_id`s.
- Leakage: for all 18 listeners with a holdout, latest training date < earliest held-out date (strict; minimum gap 5 days). The script asserts this.

## Changed / decided
- Test fraction is set to 20% because no ratio was given (`TEST_FRAC` in `load_and_split.py`).
- Skips (<30s, 29 of 80 plays) are kept in both splits for now; whether they count as positive signal is still open.
- The source file name says 2,000 listeners, but only 19 are present. This still needs confirming.

## Files
- `load_and_split.py`: loads the CSVs, prints counts, writes the split
- `split_output.txt`: the script's printed output
