# Jarvis skill — reference

Source: jerryhsieh991-lang/jarvis-os (https://github.com/jerryhsieh991-lang/jarvis-os), MIT license.
Also informed by giovanibarili/jarvis-plugin-skills.

## Architecture (from jarvis-os)

Five layers:
1. Brain — skills as SKILL.md folders; only the matched skill's body enters context.
2. Memory — plain-markdown vault, Obsidian-compatible, auto [[backlinks]].
3. Voice — local STT/TTS (not used in this chat skill).
4. Face — HUD (not used here).
5. Handoff — config-driven reskin.

## Memory design (from server/memory.py)

- Plain .md files, no database.
- Every note gets [[backlinks]] so a knowledge graph forms for free.
- Reports the assistant produces are written back, feeding future retrieval.
- Naive full-text search over the vault for retrieval.
- Folders: 00_Inbox (quick notes), 10_Reports (long outputs), 20_Sessions (episodic log).

## Skill format (from server/skills_loader.py)

Each skill is a folder with a SKILL.md containing YAML-ish frontmatter:
- name
- description
- triggers (comma-separated list; only the matched skill body is loaded)

## Router lanes (from server/router.py)

1. Regex fast-paths — instant, zero LLM cost (time, open app, notes).
2. Skill dispatch — only the matched SKILL.md body is injected as system prompt.
3. General chat — everything else, with vault search context when it helps.

## Key behaviors ported

- Startup summary from memory files.
- Append-only session log.
- Search-before-answer.
- Confidence transparency.
- Proactive next-step offers.
- Side-effect commands should be confirmation-gated (from the Hermes adapter design).
