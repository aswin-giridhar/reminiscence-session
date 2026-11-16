# Decisions log

| When (GMT, Nov 16) | Decision | Why | Whose call |
|---|---|---|---|
| 01:42 | Dementia-care angle | It is the user's idea. It uses private home data (good NAS fit), the user is clear (family carers), and it stands apart from the kit's 7 example Skills | User |
| 01:46 | Build idea A, the reminiscence session builder; skip B, the doctor-visit brief | It is the most differentiated idea, and B moves into medical territory | User |
| 01:50 | Three modes: Prepare, Play, Log | The Log feedback loop is the main differentiator: sessions improve over time | Claude |
| 01:50 | Music window = birth year + 10 to birth year + 25 | The "reminiscence bump" from care practice. `ug-music` has a release-year filter | Claude |
| 01:55 | Caption fallback built from metadata | `vision-vlm` may accept only paths the user gave | Claude |
| 01:55 | Carer must confirm explicit years | `search-picture` fills years only when they appear literally in the user's message | Claude |
| 01:55 | One new journal file per session; old entries are never edited | `save-text` writes new files, so history is never lost | Claude |
| 02:00 | Working name "Reminiscence Session" / `reminiscence-session` | A placeholder; the FAQ suggests the entrant names the project | Claude, pending the user |
| 02:05 | Proposal answers trimmed from 622 to 472 words | The limit is 500 | Claude |
| 02:14 | Track: Bring Memories to Life | It matches the track wording; prizes are open to every track | Claude, confirmed with the user |
| 02:25 | Thumbnail variant 2 | It shows the daughter and mother clearly, and the NAS has no garbled text | Claude |

## Open items

- The user should pick the final name and may rewrite the PDF answers in their own words (FAQ recommendation).
- On the NAS (if a finalist): test `vision-vlm` with search paths, the music result fields, the approval flow, and caption quality. See `skill/reminiscence-session/references/test-plan.md`.
