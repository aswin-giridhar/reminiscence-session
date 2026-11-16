## Inspiration

This project started with a visit to an elder care home, where I spent time with residents living with Alzheimer's. One moment stayed with me. A resident who had been quiet and withdrawn heard an old song from her youth, and she lit up: her face changed, she smiled and she started to sing along. The present could slip away, but the past was still there, waiting for the right song, photo or name to bring it back. I also saw how little time the people caring for residents had to find those things.

Reminiscence with familiar photos and music is one of the gentlest, most widely used activities in dementia care. A song from someone's youth or a photo of a sister at the seaside can bring back a smile and a conversation. But the family carers who could run these sessions are exhausted. Preparing one means digging through decades of photos, working out which songs belong to which era, and remembering which faces brought joy last time and which brought tears.

Meanwhile, everything a session needs is already sitting on the family NAS: the photo library, the faces already named in it, and the music collection. We wanted the NAS to do the remembering, so the carer can be present instead of searching.

## What it does

Reminiscence Session is a UGOS Skill with three modes:

- **Prepare.** The carer says, for example, *"Make a session for Mum, born 1950, about Jean, 1970 to 1979, 12 photos."* The Skill reads past care-journal notes and the avoid list, finds Jean's photos from those years, and writes short, dignity-safe captions that never quiz or correct. It picks songs from Mum's teens and twenties, creates an album after platform approval, and saves a printable memory book.
- **Play.** Starts the session music, only when the carer asks.
- **Log.** After the session, the carer records a voice note ("She loved that song, got upset at Arthur"). The Skill transcribes it and saves a care-journal entry. Next time, Arthur is left out, the song comes first, and the Skill says why.

The music window uses the "reminiscence bump", the age range people tend to remember most strongly. For a birth year \\( b \\), songs come from release years \\( [\,b+10,\; b+25\,] \\).

## How we built it

We followed the Uliya Skill Development Standard from the UGREEN Skill Developer Package. The Skill is a pure orchestration `SKILL.md`: the parent only delegates self-contained instructions to four registered experts.

- `photo-agent`: `figure-albums`, `search-picture`, `vision-vlm` and `create-album`
- `media-agent`: `ug-music` search by year and playback control
- `knowledge-agent`: `audio-transcription` and `save-text`
- `file-agent`: `find` and `cat` to read the care journal

Later steps use only verified results from earlier steps. All five delegation states are handled, and every write waits for platform approval. We checked each action's parameters against the capability catalog, and the package passes the kit's static checker with no errors or warnings.

## Challenges we ran into

- **Reading the fine print in the catalog changed the design.** Photo search fills a year only if that exact year appears in the user's message, so the Skill asks the carer to confirm explicit years rather than turning "the seventies" into dates. The vision action expects paths the user supplied, so captions have a fallback built from names and dates.
- **Writing for dignity.** Captions and conversation openers must never test or correct the person, and an avoid list must always win over "relevant" photos.
- **Making the Skill honest.** An empty search is reported as empty, never widened or filled with similar photos.

## Accomplishments that we're proud of

- A feedback loop: each session learns from the last, which no photo "memories" feature does.
- Four NAS experts working together in one caring workflow, with private family and health-related notes staying at home.
- A clean static check and a clear test plan built on synthetic data.

## What we learned

The NAS's strength is not storage alone. It is that the family's own named faces, years and music are already organised and private. The design came from reading the parameters closely, not from the action names.

## What's next for Reminiscence Session: Dementia Memory Care on UGREEN NAS

- Validate on a real iDX6011 Pro: vision captions on searched photos, music results and the approval flow.
- An optional doctor-visit brief that summarises the care journal, with citations.
- Shared family sessions, so siblings can add voice notes too.
- A large-print memory book layout.
