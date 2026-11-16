---
name: reminiscence-session
description: Helps a family carer of a person living with dementia prepare, run and learn from a reminiscence session using the family's own photos and music stored on the NAS. Use when a carer asks to make a memory session, memory book or music-and-photos session about a named person, a decade or a place, or asks to log how a session went. Finds photos of the chosen people and years, writes short dignity-safe captions, picks songs from the person's own youth, assembles a printable memory book and album, and records the carer's spoken feedback in a care journal so later sessions favour what brought joy and avoid what caused distress. Private family data stays on the NAS. It does not diagnose, give medical advice or monitor the person.
---

# Goal

Give a family carer a ready-to-use reminiscence session for one person living with dementia, built only from that family's own photos and music on the NAS, and get better each time by remembering how the person responded.

A session is: a small set of familiar photos with short, warm captions, a song list from the person's teenage and early-adult years, gentle conversation openers, a printable memory book saved on the NAS, and an album to show on screen. After the session, the carer's spoken notes are logged so the next session leans towards what worked.

Supported scope: photos already in the NAS photo library, people already named in its person albums, music already in the NAS music library, and carer voice notes stored on the NAS. Out of scope: diagnosis, medical advice, medication, live camera monitoring, alerts, and anything outside the NAS.

This Skill has three modes. Pick the mode from the carer's request; if it is unclear, ask.

- **Prepare** a new session.
- **Play** a prepared session's music.
- **Log** how a session went.

Read [Session design rules](references/session-design.md) before writing any caption, conversation opener or song choice. Read [Care journal format](references/care-journal-format.md) before reading or writing a journal entry.

# Required input

Collect only what the parent workflow needs. Ask in one short message for anything missing. Never guess a year, a name or a folder.

For **Prepare**:

1. **Person's name** as the carer calls them (for example "Mum"). Used only in the memory book text.
2. **Birth year** of the person, as a four-digit year given by the carer. Used to compute the music window.
3. **Theme**, at least one of:
   - **People**: names exactly as they appear in the NAS person albums (for example "Jean").
   - **Years**: an explicit four-digit start year and end year written by the carer (for example "1970 to 1979"). Do not turn "the seventies" into dates yourself; ask the carer to confirm the years.
   - **Place**: a place name (for example "Blackpool").
4. **Number of photos**: the carer's number. If they do not give one, propose 12 and wait for confirmation.
5. **Session folder**: a real NAS folder the carer names for the memory book and journal. Never invent one.
6. **Avoid list** (optional but always asked once): people, places or topics that cause distress, for example a late spouse or a former home.

For **Play**: the saved memory book file the carer names, or the session name.

For **Log**: the voice note file path the carer names, its spoken language, and the session folder.

# Generation-time capability evidence

- `ug-photo` → `photo-agent`
- `ug-music` → `media-agent`
- `ug-knowledge` → `knowledge-agent`
- `ug-filesystem` → `file-agent`

Actions relied on (from the public catalog, version 2.2.0): `figure-albums`, `search-picture`, `meta`, `vision-vlm` and `create-album` (ug-photo); `search` with `type=year` and `control` (ug-music); `audio-transcription` and `save-text` (ug-knowledge); `find` and `cat` (ug-filesystem). State changes are `create-album` and `save-text`, both approval `workflow`. Music playback through `search` with `play` and through `control` is a player state change with approval `none`, so it runs only when the carer asks to play.

# Orchestration

Every delegation below is self-contained. Downstream steps use only real results returned upstream: values the carer gave, or values a previous step returned as verified facts. Each step names what is handed to the next.

## Prepare

**Step 1. Read the care journal (file-agent).** Delegate:

`delegate --target file-agent --instruction 'In the NAS folder the carer named as the session folder, find Markdown files whose names start with care-journal, newest first, at most 5. Return each real path. Then return the full text of each found file using the read action. Read only; change nothing. If there are no such files, say explicitly that none were found.'`

- `completed` with files: parse the "Responded well", "Neutral" and "Distress" lists described in the [Care journal format](references/care-journal-format.md). Merge every "Distress" item into the avoid list. Keep "Responded well" items as preferences.
- `completed` with an explicit "none found": this is a first session; continue with no preferences.
- `failed` or `cancelled`: tell the carer the journal could not be read, and ask whether to continue without past feedback. Do not continue until they answer.

**Step 2. Confirm the people (photo-agent).** Only if the theme names people. Delegate:

`delegate --target photo-agent --instruction 'List named person albums only (figure-albums with show-name named). For each name the carer gave, report whether a named person album matches exactly, matches approximately, or does not exist. Do not rename, merge or create any person album.'`

- If a name has no named album, tell the carer and offer: continue without that person, or stop so they can name the person in the photo app. Do not substitute a similar name.

**Step 3. Find the photos (photo-agent).** Delegate one search that combines the confirmed theme:

`delegate --target photo-agent --instruction 'Search pictures (search-picture) for still images only. Use person-name for each confirmed person, start-time and end-time only from the four-digit years the carer wrote, and location only if the carer named a place. Limit to the number of photos the carer confirmed. Return the resultRef, the matched total, the capture-time span, and for each previewed photo its picture ID, real path, capture date and named people. Exclude any photo whose named people or location appear on this avoid list: (insert the merged avoid list). Read only.'`

- `completed` with matches: keep the returned resultRef and the per-photo facts for steps 4 and 5.
- `completed` with no matches: report "no photos found for this theme" exactly. Do not widen the years, drop a person or add a place on your own. Offer the carer one change at a time.
- Fewer matches than requested: continue with what was found and say how many.

**Step 4. Write captions (photo-agent).** For each photo from step 3 that has a real path, delegate one photo at a time:

`delegate --target photo-agent --instruction 'Run vision-vlm on this one image path: (insert the real path from step 3). Prompt: Describe what is happening in this photo in one simple, warm sentence of at most 15 words, for an older adult. Mention only visible scenery, activity and mood. Do not guess any name, age, relationship or the year. Return only that sentence.'`

Then build the caption in the parent from verified facts only: named people from step 3, the capture year from step 3, and the vision sentence. Example shape: "Jean at the seaside, summer 1974. Everyone is laughing on the sand." Apply every rule in [Session design rules](references/session-design.md).

- If vision is refused, fails or the photo has no returned path, write a metadata-only caption from named people and year. Mark it in the book as "short caption". Never invent scene details.

**Step 5. Choose songs (media-agent).** Compute the music window in the parent: start year = birth year + 10, end year = birth year + 25. Delegate:

`delegate --target media-agent --instruction 'Search the NAS music library with search type year, start-year (insert computed start year) and end-year (insert computed end year). Do not start playback. Return up to 15 songs with title, artist and release year. If the library returns nothing for these years, say explicitly that no songs were found.'`

- Put any song from the journal's "Responded well" list first, remove any from "Distress", and keep up to 6 songs.
- No songs found: continue without music and say so in the book and in the final report.

**Step 6. Create the album (photo-agent).** Delegate:

`delegate --target photo-agent --instruction 'Create a regular album (create-album) named "Memory session - (insert theme) - (insert the current date)" from source-ref (insert resultRef from step 3). Do not use add-item. Do not move, delete or edit any photo. Return the real album status and album ID.'`

- This waits for platform approval (`waiting_approval`). Relay it and wait. Conversation agreement is not approval.
- `blocked` or `failed`: the memory book is still useful; continue to step 7 and report that no album was created.

**Step 7. Save the memory book (knowledge-agent).** Assemble the book in the parent using the layout in [Session design rules](references/session-design.md): title, person's name, photo list with captions and picture IDs, song list, conversation openers, and a blank "How did it go?" section. Delegate:

`delegate --target knowledge-agent --instruction 'Save this Markdown text (save-text with content) into the NAS folder (insert the session folder the carer named), filename-hint memory-book. Content: (insert the assembled Markdown). Return the verified output file path and byte count.'`

- `completed` with a verified path: give the carer that path.
- `completed` without a verified path: say the save could not be confirmed. Do not claim the book exists.

## Play

Confirm the saved book path with the carer. Read the song list from that file through file-agent (read action, read only). Then delegate:

`delegate --target media-agent --instruction 'Play this exact song now: title (insert title), artist (insert artist), using music search with play enabled. If it is not found, say so; do not play a different song.'`

Use `control` with `next`, `pause`, `resume` or `set-stop-timer` only when the carer asks. Never raise the volume above what the carer asks for.

## Log

**Step 1. Transcribe (knowledge-agent).** Delegate:

`delegate --target knowledge-agent --instruction 'Transcribe this NAS audio file: (insert the path the carer named). Spoken language: (insert the language the carer stated, or unspecified). Do not save the transcript. Return the transcript text, coverage and quality warnings.'`

- Low quality or partial coverage: show the carer the transcript and ask them to correct it before logging.

**Step 2. Sort the feedback (parent).** Sort each mention into "Responded well", "Neutral" or "Distress", linked to a photo, person, place or song from the memory book. Only use what the carer said. If something cannot be linked, list it under "Other notes". Show the sorted entry to the carer and ask for confirmation.

**Step 3. Save the journal entry (knowledge-agent).** After the carer confirms, delegate:

`delegate --target knowledge-agent --instruction 'Save this Markdown text (save-text with content) into the NAS folder (insert the session folder), filename-hint care-journal. Content: (insert the confirmed journal entry). Return the verified output file path.'`

# Completion and failure

Report each step's status separately, then an overall line.

- `completed`: continue only when the returned facts contain what the next step needs: a resultRef for the album, a real path for vision, a verified file path for the book. If a needed value is missing, stop that branch, say which value is missing, and never guess it.
- `failed`: stop the dependent steps. Report which step and which expert failed, keep everything already confirmed, and do not claim success.
- `cancelled`: report the step as not done. Do not retry automatically. Album creation and saving could repeat if retried, so ask the carer first.
- `waiting_user`: relay the expert's question to the carer and resume the same delegation with their answer. Do not start a parallel task.
- `waiting_approval`: relay the platform approval request and wait. Never treat "yes" in chat as approval. After resuming, check the final status.

Empty results: a search that explicitly reports no photos or no songs is a valid answer. Report it plainly and offer one narrower or different choice. Never fill a gap with photos, names or songs that were not returned.

Partial results: a session can be useful without music or without an album. The final report lists what was done, what was not, and why. For example: "Book saved (path). Album not created: approval declined. Music: 5 songs found."

Safety: no diagnosis, no medical claims, no monitoring. Everything stays on the NAS; this Skill sends nothing outside it. Use the NAS local model for vision where the platform offers one. Approval gates on album creation and saving are never skipped.

# References

- [Session design rules](references/session-design.md): caption, dignity, music and layout rules.
- [Care journal format](references/care-journal-format.md): how feedback is stored and read back.
- [Test plan](references/test-plan.md): core and edge tests using synthetic data.
- [Disclosures](references/disclosures.md): third-party components, AI tools and work done for this Hackathon.
