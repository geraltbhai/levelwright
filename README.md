# Levelwright (dev monorepo, private)

Levelwright is an AI game-studio kit for Claude Code (with Codex, Cursor and Gemini CLI adapters). It comes with real CLI tools (`doctor`, `playtest`), starter templates, a portal adapter, a licensing pack, two proof demo games and a Three.js marketing site.

**Status:** spec only. The build hasn't started yet.

## Start the build
```bash
git clone https://github.com/geraltbhai/levelwright.git
cd levelwright
claude            # select Fable 5.2
```
Then say:
> Read PROMPT.md and execute it. If PROGRESS.md shows work already done, resume from the next unchecked item.

## Key files
| File | Purpose |
|---|---|
| `PROMPT.md` | The full end-to-end build spec |
| `CLAUDE.md` | Rules loaded automatically in every Claude Code session |
| `PROGRESS.md` | Checkpoint log that the builder keeps up to date |
| `DECISIONS.md` | Why non-obvious choices were made |

## Plans (one-time, via Polar)
Lite: free · Indie: $29 · Studio: $69 (5 seats). Change them in `site/src/config/pricing.ts` once the site exists.

> This repo must stay **private**, because it contains the paid kit. The free subset goes public separately as `levelwright-lite`.
