# Developer kit findings (uliya-skill-kit 0.7.7, catalog 2.2.0)

Downloaded from https://cdn.shopify.com/s/files/1/0630/9416/1602/files/uliya-skill-kit-0.7.7-en.zip. 27 files, about 60 KB zipped.

## What a Skill is

- A Markdown orchestration document. The parent may **only** run `delegate --target <agent> --instruction '<task>'`.
- Six registered experts handle 9 CLIs and 84 actions:

| Expert | CLIs |
|---|---|
| `photo-agent` | `ug-photo` |
| `media-agent` | `ug-music`, `ug-video` |
| `file-agent` | `ug-filesystem`, `ug-filemgr`, `file-workflow` |
| `document-agent` | `ug-document` |
| `knowledge-agent` | `ug-knowledge` |
| `security-monitor-agent` | `ug-surveillance` |

- Delegations end in one of five states: `completed`, `failed` or `cancelled` (final), or `waiting_user` or `waiting_approval` (the platform pauses and resumes). Writes need platform approval, either `gateway` or `workflow`.

## Parameter details that shaped the design

- `ug-photo search-picture`: `start-time` is filled only when the user's message contains that same four-digit year. So the Skill asks the carer to confirm explicit years.
- `ug-photo vision-vlm`: `path` is "the absolute image path the user explicitly gave". It may refuse paths that came from a search, so captions fall back to metadata. This is **the main thing to test on the NAS.**
- `ug-music search type=year` takes `start-year`/`end-year` (release year) plus `play`. This is the basis of the music window.
- `ug-document extract` offers a `medical-report` schema, and `ug-surveillance event-group-counts` counts known/stranger/human events. Neither is used yet.
- There is no scheduler, push alert or text-to-speech action, and video frames cannot be analysed (vision takes images only).

## Checker quirks (`scripts/check-skill.mjs`)

- The kit's own template heading `# Expert orchestration` **fails**. The checker requires `# Orchestration` (or `# Workflow`).
- Capability evidence must sit under `# Generation-time capability evidence`, with lines in the form ``- `cli` → `agent` `` (backticks and the → arrow). The kit's prose example uses `->` instead, which fails.
- The text must contain wording like "real … result", "upstream" or "downstream", or a warning is raised.
- Inside an `--instruction`, an apostrophe must be escaped shell-style (`'"'"'`); the doubled form `''` is rejected. The simplest fix is to avoid apostrophes.
- Placeholders in angle brackets (`<...>`) inside instructions raise warnings. The Skill uses `(insert …)` instead.
- The words "TODO" and "TBD" fail the check. A file-command word followed by a space (for example the read action's name) can trip the direct-file-API rule, so keep action names in backticks.
