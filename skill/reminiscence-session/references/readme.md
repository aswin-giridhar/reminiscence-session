# README: Reminiscence Session

Skill version 0.1.0 (Round 1 pre-test package). Entry point: [SKILL.md](../SKILL.md).

## What this package is

A UGOS orchestration Skill that follows the Uliya Skill Development Standard. It contains **Markdown files only**:

| File | Role |
|---|---|
| `SKILL.md` | Entry point and manifest (frontmatter `name`, `description`); the full workflow |
| `references/readme.md` | This file: install, setup, removal, testing, dependencies, versions |
| `references/configuration-template.md` | Inputs a carer provides, with placeholders |
| `references/session-design.md` | Caption, dignity, music and memory-book rules |
| `references/care-journal-format.md` | Journal entry format and reading rules |
| `references/test-plan.md` | Core and edge tests with synthetic data |
| `references/disclosures.md` | Third-party components, AI tools, work done for the Hackathon |

There are no scripts, binaries, separate manifests, credentials or bundled media.

## Dependencies

No software dependencies. At runtime the Skill depends only on UGOS platform capabilities:

| Need | Expert and CLI | Catalog |
|---|---|---|
| Read care journal | `file-agent` · `ug-filesystem` | 2.2.0 |
| Photos, captions, albums | `photo-agent` · `ug-photo` | 2.2.0 |
| Music search and playback | `media-agent` · `ug-music` | 2.2.0 |
| Transcribe and save text | `knowledge-agent` · `ug-knowledge` | 2.2.0 |

It was built against uliya-skill-kit 0.7.7, capability catalog 2.2.0. No outside service or API is called.

## Setup (before first use)

1. In the NAS photo app, **name the people** who should appear in sessions (person albums). The Skill only uses named people.
2. Make sure the family **music library** is on the NAS, and that tracks carry release-year information.
3. Create one **session folder** on the NAS for memory books and care-journal entries (for example, a private folder for the family member).
4. Fill in the [configuration template](configuration-template.md) once, so every request contains the needed inputs.

## Install

The package is a standard Uliya Skill folder. Install it through UGREEN Skills Hub or the UGOS Skill installation flow described in UGREEN's developer documentation. *Installation has not yet been run on a real NAS (Round 1 does not require it); it will be validated on the finalist testing day.*

## Use

Ask in natural language. For example:
- "Prepare a memory session for Mum, born 1950, about Jean, 1970 to 1979, 12 photos, save to the session folder."
- "Play the songs from the last memory book."
- "Log how today's session went: here is my voice note."

## Removal

- Uninstall the Skill through Skills Hub or UGOS Skill management. The Skill keeps no hidden state.
- Files the Skill created stay yours. Delete them yourself if you no longer want them:
  - memory books (`memory-book…`) and journal entries (`care-journal…`) in the session folder,
  - albums named "Memory session - …" in the photo app.
- The Skill never moves, edits or deletes original photos, music or voice notes.

## Testing

1. **Static check:** run the kit's checker on this folder. The expected result is `static-pass` with 0 errors and 0 warnings.
2. **Functional tests:** follow the [test plan](test-plan.md) with synthetic data only.

## Versions

| Version | Date | Notes |
|---|---|---|
| 0.1.0 | 2026-11-16 | Round 1 pre-test package |
