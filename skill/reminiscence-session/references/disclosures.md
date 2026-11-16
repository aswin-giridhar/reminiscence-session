# Disclosures

## Principal functionality developed for this Hackathon

All of this Skill was newly written during the Hackathon:

- The three-mode workflow (Prepare, Play and Log) and its expert orchestration.
- The feedback loop that carries a carer's spoken notes into the next session's choices.
- The music window from birth year (reminiscence bump).
- The caption and dignity rules.
- The care journal format.
- The test plan.

## Platform components used

- UGREEN Uliya Skill Development Standard and UGOS Skill Creation Tool (uliya-skill-kit 0.7.7), provided by UGREEN for this Hackathon under its developer terms.
- NAS experts and CLI capabilities from the public catalog version 2.2.0: photo-agent (ug-photo), media-agent (ug-music), knowledge-agent (ug-knowledge) and file-agent (ug-filesystem).

## Third-party components and open-source libraries

None. The package contains only Markdown files. No code, libraries, models, datasets, images or music are bundled.

## External services and data flows

None added by this Skill. All reads, AI processing and writes are delegated to UGOS experts on the NAS. Vision captioning uses whatever model the platform's vision-vlm action is set to use; the NAS local model is preferred. The Skill does not call any outside API.

## AI tools used and their principal uses

- **Claude Code (Anthropic, Claude Opus model):** read the UGREEN Skill kit and capability catalog, checked action parameters, drafted the SKILL.md, the reference files and the proposal PDF text, and ran the kit's local static checker.
- The core idea, its direction and the final review are the entrant's own.

## Test data

Only synthetic, fictional data is used for testing. No real personal or medical data is included in this package.
