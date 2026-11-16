# Test plan

All tests use **synthetic data only**: a fictional family, with photos and voice notes created for testing (for example public-domain or self-taken stock photos tagged with fictional names). No real patient data is used.

## Synthetic test set

- About 60 photos from 1965 to 1985, with two person albums named "Jean" and "Arthur" and capture dates set.
- About 30 music files with release years 1955 to 1980.
- Two short carer voice notes in English, one positive and one with a "distress" mention.

## Core test 1: prepare a first session

Input: Mum, born 1950, people Jean, years 1970 to 1979, 12 photos, session folder chosen.
Expected: the journal step reports none found; up to 12 photos of Jean from the 1970s; captions follow the session design rules; songs released 1960 to 1975; an album is created after approval; the memory book is saved and its verified path reported.

## Core test 2: learn from feedback

Log a voice note saying the person loved song A and became upset at photos of Arthur. Then prepare a second session with people Jean and Arthur.
Expected: the journal entry lists song A under "Responded well" and Arthur under "Distress"; the second session excludes Arthur's photos, puts song A first and tells the carer why Arthur was left out.

## Edge case: nothing matches

Input: years 1940 to 1945 (no photos) and a person who has no named album.
Expected: reports the missing person album and "no photos found" exactly; does not widen the years or swap in another person; offers one change; creates no album and saves nothing.

## Also to check on the NAS

- Approval is declined for album creation: the book is still saved, and the report says no album was made.
- vision-vlm refuses a path: metadata-only captions are used and marked "short caption".
- Music library is empty for the window: the book says no songs were found.
- Repeat run with the same input: no duplicate album without the carer's consent.
