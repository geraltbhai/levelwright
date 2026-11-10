# CLAUDE.md: Levelwright dev repo

This repo is built from **PROMPT.md**. That file is the spec and the source of truth.

## Every session
1. Read `PROGRESS.md` first. Resume from the next unchecked item.
2. Re-read **PROMPT.md §2 (Operating rules)** before starting a phase.
3. Checkpoint often: run the tests, update PROGRESS.md, commit, `git push origin main`, tag `checkpoint/pN-<slug>`.

## Gate before Phase 1
Run `gh api repos/geraltbhai/levelwright --jq .visibility` (or check the GitHub settings page). If the result isn't `private`, **stop and ask Sam to make the repo private**. The paid kit must never be built in a public repo.

## Hard rules (short version of PROMPT.md §2)
- Verify external formats and APIs against official docs and log them in `docs/SOURCES.md`. Never invent SDK methods or portal requirements; mark anything you couldn't check as `unverified`.
- No fake testimonials, counts, ratings or scarcity on the site. Every number comes from `kit/manifest.json` or a real report.
- Original copy and assets only. Never mention a competitor.
- Never commit secrets or personal emails. Never push full-kit files to a public repo.
- Ask Sam before public or irreversible actions (public repos, npm publish, production deploy, price changes).
