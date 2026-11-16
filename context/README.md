# Context: UGREEN Hackathon 2026 on AI NAS

The project's state in one place. Start here.

## Status (2026-11-16 02:25 local/GMT)

- Round 1 deadline: **Nov 16, 2026, 07:45 GMT** (Nov 15, 23:45 PT).
- Project: **Reminiscence Session**, a Uliya Skill helping family carers of people with dementia. Track: Bring Memories to Life.
- Ready to submit: ZIP, PDF, thumbnail and form text. Nothing has been tested on a real NAS (not required for Round 1).

## Files

| What | Where |
|---|---|
| Form text (name, pitch, thumbnail notes) | [submission-form.md](submission-form.md) |
| Thumbnail (3:2 JPG) | `context/thumbnail.jpg` |
| Hackathon rules digest | [hackathon-brief.md](hackathon-brief.md) |
| What the developer kit allows | [kit-findings.md](kit-findings.md) |
| Decisions and why | [decisions.md](decisions.md) |
| Skill source | `skill/reminiscence-session/` |
| Upload files | `submission/reminiscence-session.zip`, `submission/proposal.pdf` |
| Proposal source | `proposal/proposal.html` (rebuild the PDF with Edge headless) |
| UGREEN kit (untouched download) | `vendor/kit/uliya-skill-kit/` |

## Rebuild commands

Run from the repository root in Git Bash.

- Static check: `node vendor/kit/uliya-skill-kit/scripts/check-skill.mjs skill/reminiscence-session`
- PDF: `"/c/Program Files (x86)/Microsoft/Edge/Application/msedge.exe" --headless=new --no-pdf-header-footer --print-to-pdf="E:\Hackathon\ugreen_hackathon\submission\proposal.pdf" "file:///E:/Hackathon/ugreen_hackathon/proposal/proposal.html"`
- ZIP: zip the `skill/reminiscence-session` folder so that `reminiscence-session/` is the top-level folder in the archive.
