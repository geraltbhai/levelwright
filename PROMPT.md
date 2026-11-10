# Levelwright: end-to-end build prompt

> **How to start (for Sam):** clone this repo, open Claude Code in it with **Fable 5.2** selected, and say:
> `Read PROMPT.md and execute it. If PROGRESS.md shows work already done, resume from the next unchecked item.`
>
> Everything below is written to you, the builder.

---

## 0. Mission

Build and ship **v1.0 of Levelwright**. It has two parts:

1. **The kit**: a paid pack of AI specialists (subagents, skills, slash commands) **plus real CLI tools** that turn a coding agent into a small game studio. It covers browser games (Three.js, Phaser, PixiJS, Canvas), Godot 4 web export and Unity WebGL.
2. **The website**: a fast landing site with a **playable Three.js hero**, demo games, honest before/after proof, pricing and guides. Checkout goes through Polar.

**Done means all of the following are true:**

- [ ] One command installs the kit into any game project. It is safe (never overwrites the user's edits), works fully for Claude Code, and works for other agents at the level you verified.
- [ ] `levelwright doctor` and `levelwright playtest` run on real games and produce evidence reports with numbers.
- [ ] Two original demo games show **genuine** before/after results that the kit itself produced.
- [ ] The website builds, meets the budgets in §9.1, and its checkout buttons work as soon as Polar links are pasted into config.
- [ ] Everything is committed and pushed to `github.com/geraltbhai/levelwright` with checkpoint tags. `PROGRESS.md` says exactly what's done and what's left.

---

## 1. Product brief

**Who it's for:** solo and indie developers and small studios making browser, Godot 4 or Unity WebGL games. They use Claude Code (or Codex, Cursor or Gemini CLI) and want to ship to web portals and earn from ads, in-app purchases or licensing.

**The one-line promise:** *Find the bug, fix the feel, wire the portal, ship with proof.*

**Positioning:** most AI game-dev kits are prompt packs. Levelwright adds **tools that run and produce evidence**, a **lower price**, a **free tier**, **Godot support** and a **licensing workflow**.
**Never name or reference any competitor anywhere**: not on the site, in docs, in the kit or in commit messages.

### Pricing

All prices are one-time, in USD, with Polar as merchant of record. Keep them in one config file, `site/src/config/pricing.ts`, so they can be changed in one place.

| Plan | Price | What's included |
|---|---|---|
| **Lite** | Free | Public repo `levelwright-lite`: 2 agents, 6 skills, 2 commands, and `doctor` with the basic rule set |
| **Indie** | **$29** | The full kit for 1 named user, all 1.x updates, install via Claude Code plugin or npx |
| **Studio** | **$69** | Everything in Indie for up to 5 named users, plus a team conventions pack and priority email support |

### Differentiators

Each one must exist and work. If it doesn't, remove it from the site.

1. **Tools that run.** `doctor`, the playtest bot and budgets produce reports with numbers.
2. **A four-stage loop:** Diagnose → Feel → Business → Ship. It ends in a written ship/no-ship decision record.
3. **Engine coverage**, including Godot 4 web export, plus Unity WebGL guidance.
4. **One portal adapter interface** for CrazyGames, Poki, GameDistribution, GamePix, YouTube Playables and itch.io, plus a `none` adapter for local development.
5. **A licensing pack:** sell sheets, license tiers, a white-label build config and outreach drafts, for selling non-exclusive or exclusive licenses to portals.
6. **Pre-wired starter templates** for Three.js, Phaser, PixiJS and Godot.
7. **A safe installer:** dry-run, update and uninstall, and it never overwrites files the user changed. It works as a Claude Code plugin and through npx.
8. **Multi-agent adapters:** full support for Claude Code, and verified support levels for Codex, Cursor and Gemini CLI.
9. **A free Lite tier** that brings people in.

---

## 2. Operating rules (re-read at the start of every phase)

1. **Verify before you build.** Before writing anything that depends on an external format or API, read the current official docs and record the URL and the date you read it in `docs/SOURCES.md`. That covers:
   - Claude Code subagents, skills, slash commands, plugins and plugin marketplaces, CLAUDE.md
   - Codex `AGENTS.md` and its skills support, if any
   - Cursor rules
   - Gemini CLI `GEMINI.md`
   - every portal's SDK and submission requirements
   - Polar checkout links and its GitHub repository access benefit
   - the current stable releases of Three.js, Phaser, PixiJS, Astro and Playwright

   If you can't reach a doc, mark the feature that depends on it as `unverified` in the code, in the rule packs and in the site's coverage table. **Never invent API names, SDK methods, file formats or portal requirements.**
2. **Be honest on the site.** No testimonials, user counts, "trusted by" logos, star ratings, download counts, countdown timers or fake scarcity. Every number on the site must be computed from the repo (for example, counts from the manifest) or from real runs (doctor and playtest output). Before/after media must come from the real process in Phase 5.
3. **Keep everything original.** All copy, headings, names, art and code are original. Don't copy text, layout copy or assets from any other kit's website. Demo games use original procedural or low-poly art, or CC0 assets credited in `CREDITS.md`. No copyrighted characters, brands or logos.
4. **Checkpoint often.** At the end of every phase, and about every 45 minutes inside long phases:
   - run the tests
   - update `PROGRESS.md` (done / in progress / next / blockers / decisions)
   - commit with a Conventional Commit message
   - `git push origin main`
   - tag `checkpoint/pN-<slug>` and push the tag

   Usage limits can end a session at any moment, so never leave work uncommitted for long. When you resume, read `PROGRESS.md` first.
5. **Ask before anything public or irreversible:** creating public repos, publishing to npm, deploying to production, changing prices, or anything that costs money. Prepare everything, then stop and ask.
6. **Protect secrets.** Never commit tokens, `.env` files or personal email addresses. Use `.env.example` and `site/src/config/*.ts` with `TODO` placeholders.
7. **Quality beats quantity.** A few excellent specialists are better than a big count. Every skill ends with a verification step that measures before and after.
8. **Record decisions.** Write non-obvious choices to `DECISIONS.md` in the form: context → options → choice → why.
9. **Respect the priorities.** P0 must ship. P1 should ship. P2 is a bonus. Never trade P0 quality for P2 breadth.
10. **Use subagents sparingly.** They consume usage. Use them for batch-drafting skills if that helps, and always for the independent review in Phase 8. You own all integration work.

---

## 3. Repository layout

```
levelwright/                     # private dev monorepo (this repo)
├── PROMPT.md  CLAUDE.md  README.md  PROGRESS.md  DECISIONS.md  CREDITS.md
├── kit/                         # → released to private repo geraltbhai/levelwright-kit (what buyers get)
│   ├── .claude/agents/          # subagents
│   ├── .claude/skills/<name>/SKILL.md (+ scripts/)
│   ├── .claude/commands/        # slash commands
│   ├── .claude-plugin/          # plugin.json + marketplace.json (per current docs)
│   ├── adapters/{codex,cursor,gemini}/   # generated from the same sources
│   ├── bin/levelwright.mjs      # CLI entry
│   ├── tools/doctor/  tools/playtest/  tools/__fixtures__/{good-game,bad-game}/
│   ├── rules/<portal>.json      # rule packs with sources
│   ├── templates/{threejs,phaser,pixi,godot,shared}/
│   ├── manifest.json            # generated, single source of truth
│   ├── CHANGELOG.md  EULA.md  README.md
├── lite/                        # generated subset → public repo geraltbhai/levelwright-lite (MIT)
├── demos/putt-planet/  demos/neon-drift/   # proof games (+ evidence/)
├── site/                        # Astro + Three.js website (Vercel)
├── scripts/                     # build-manifest, build-lite, release, capture-media
├── docs/                        # SOURCES.md, LAUNCH.md, ARCHITECTURE.md, QA.md, qa/ screenshots
└── .github/workflows/ci.yml
```

---

## 4. The kit (`kit/`)

### 4.1 Roster

**Agents (17).** Each has a stage and one job.

| Agent | Stage | Job |
|---|---|---|
| `bug-detective` | Diagnose | Reproduces first, isolates the cause, writes a failing repro, then fixes the root cause |
| `perf-profiler` | Diagnose | Frame-time budget, draw calls, GC spikes, asset weight; produces measured before/after |
| `crash-triager` | Diagnose | Console and page errors, WebGL context loss, memory, device-specific failures |
| `game-feel-director` | Feel | Juice: easing, hitstop, screenshake, particles, feedback timing; works from a feel checklist |
| `ui-ux-reviewer` | Feel | HUD readability, menus, touch targets, first-minute experience, accessibility |
| `level-pacing-designer` | Feel | Difficulty curves, level flow, session-length targets for web players |
| `audio-designer` | Feel | SFX and music integration, mixing and ducking, autoplay policies, compression |
| `playtest-analyst` | Feel | Runs the playtest bot, reads the evidence, ranks fixes |
| `portal-publisher` | Business | Integrates the portal adapter; reads each portal's current official docs first |
| `monetization-designer` | Business | Ad breaks at natural pauses, opt-in rewarded ads, IAP; ethical guardrails |
| `analytics-engineer` | Business | Event taxonomy, funnels, retention, feature flags, consent |
| `licensing-agent` | Business | Sell sheet, license tiers, white-label builds, outreach drafts to portals |
| `store-page-writer` | Business | Portal listing copy, tags, thumbnail brief, trailer storyboard |
| `release-captain` | Ship | Ship/no-ship decision record with evidence, versioning, changelog, rollback |
| `web-engine-specialist` | All | Three.js, Phaser, PixiJS and Canvas implementation |
| `godot-specialist` | All | Godot 4 (GDScript), web export, COOP/COEP and thread settings, JavaScriptBridge |
| `unity-webgl-specialist` | All | Unity WebGL build size, compression, memory and loader settings |

**Skills (45)**

- **Diagnose (9):** `repro-first-bugfix`, `frame-budget-audit`, `draw-call-reduction`, `gc-and-memory-hunt`, `webgl-context-loss`, `input-latency-audit`, `physics-stability`, `save-data-hardening`, `cross-browser-matrix`
- **Feel (11):** `juice-pass`, `hitstop-and-screenshake`, `tweening-and-easing`, `particle-polish`, `camera-feel`, `ui-readability-pass`, `touch-controls`, `first-minute-onboarding`, `difficulty-curve`, `accessibility-pass`, `audio-mix-pass`
- **Assets & performance (7):** `texture-compression` (KTX2/Basis), `gltf-optimize` (glTF-Transform: meshopt/Draco), `sprite-atlas-pack`, `audio-compress`, `font-subset`, `bundle-size-budget`, `preload-and-loading-screen`
- **Business (11):** `portal-adapter-integration`, `ad-break-placement`, `rewarded-ads-design`, `iap-lifecycle`, `analytics-taxonomy`, `remote-config-flags`, `ab-experiment`, `consent-and-privacy`, `license-sell-sheet`, `white-label-build`, `portal-listing-pack` (includes the trailer storyboard)
- **Ship (7):** `release-checklist`, `ship-decision-record`, `versioning-and-changelog`, `mobile-web-pass`, `godot-web-export`, `unity-webgl-build`, `staged-rollout`

**Slash commands (8):**

| Command | What it does |
|---|---|
| `/lw-audit` | Read-only four-stage audit that produces a ranked fix list |
| `/lw-fix <issue>` | Repro-first fix using `bug-detective` |
| `/lw-polish [scene]` | Feel pass with before/after measurements |
| `/lw-port <portal>` | Adapter integration plus a rule-pack check |
| `/lw-playtest` | Runs the bot, then the analyst reviews the results |
| `/lw-license` | Builds the licensing pack |
| `/lw-ship` | Writes the ship decision record |
| `/lw-doctor` | Runs doctor and explains the findings |

**Lite subset** (`tier: "lite"` in the manifest):
- agents: `bug-detective`, `game-feel-director`
- skills: `repro-first-bugfix`, `frame-budget-audit`, `juice-pass`, `ui-readability-pass`, `bundle-size-budget`, `release-checklist`
- commands: `/lw-doctor`, `/lw-polish`
- doctor with basic rules only (no portal rule packs)

### 4.2 File formats and quality bar

- **Agent file:** frontmatter exactly as the current Claude Code docs specify. Include name, a description with clear when-to-use triggers, and the smallest tools allowlist that works. The body covers: role; the inputs it expects; the procedure; the **evidence it must produce**; hand-offs to other specialists; and boundaries (no dark patterns, no faked metrics, no edits outside the game project).
- **Skill:** `SKILL.md` frontmatter (name, plus a description with triggers). Sections: *When to use / When not to* → *Inputs* → *Procedure* (numbered) → *Verify* (measure before vs after: fps p95, ms, KB, error count, tap-target size and so on) → *Output* → *Pitfalls* → *Sources* (official links only). Put deterministic steps in `scripts/` inside the skill folder.
- **Command:** a thin orchestrator that calls specialists in order and writes `.levelwright/reports/<YYYY-MM-DD>-<command>.md` inside the user's project.
- **Ethics in the monetization skills:** ads only at natural breaks; rewarded ads are always opt-in; no deceptive UI; follow each platform's policy; include notes on audiences of children and the privacy obligations that come with them.

### 4.3 Manifest (single source of truth)

`scripts/build-manifest.mjs` writes `kit/manifest.json`. For every shipped file it records: `path`, `kind` (agent/skill/command/tool/template/rule), `name`, `stage`, `engines[]`, `tier` (lite/full), `description`, `sha256`. The **CLI, the lite builder and the website** all read this file. The site's roster and every count on the site come from it.

---

## 5. CLI (`kit/bin/levelwright.mjs`)

Use Node ≥ 20 and ESM. Aim for **zero runtime dependencies**. Playwright is an optional peer dependency, used only by `playtest` and doctor's runtime checks.

| Command | Behaviour |
|---|---|
| `init [--agents claude,codex,cursor,gemini] [--target <dir>] [--dry-run]` | Detects which agents are present (or uses the flag) and writes the kit. Writes `.levelwright/install.json` with the version, files and checksums. Adds a block between `<!-- levelwright:start -->` and `<!-- levelwright:end -->` to `CLAUDE.md` (and `AGENTS.md` / `GEMINI.md` where relevant) without touching the user's own content. |
| `update` | Three-way logic. Unchanged files are updated. **Files the user modified are left alone**: the new version is written as `<file>.levelwright-new` and listed. |
| `status` | Shows the installed version, which files were modified and which are missing. |
| `uninstall` | Removes only unmodified kit files and our marker blocks. Lists anything it kept. |
| `doctor [--portal <name>] [--runtime] [--strict] [--ci] [--json <path>]` | See §6. |
| `playtest [--config <path>]` | See §6. |
| `--help`, `--version` | |

**Install routes** (document all three in `kit/README.md` and on the site's `/docs/install` page):
1. `npx github:geraltbhai/levelwright-kit init`. This needs git access to the private repo, which Polar's GitHub benefit grants.
2. Add the private repo as a **Claude Code plugin marketplace** and install the plugin, following the current docs.
3. Clone the repo, then run `node bin/levelwright.mjs init --target <your-game>`.

**Tests (`node:test`):**
- running `init` twice is idempotent
- `update` never overwrites a modified file
- `--dry-run` writes nothing
- `uninstall` leaves modified files in place
- the marker-block merge preserves user content
- Windows-style paths work
- `init --agents codex` writes only verified Codex formats

---

## 6. Doctor and playtest (`kit/tools/`)

### Doctor

**Static checks** (fast, no browser):
- total build size and the largest files, against `levelwright.budget.json` (configurable defaults, labelled "default — check your portal's current limit")
- uncompressed audio (WAV); oversized textures (> 2048 px or > 1 MB); non-power-of-two textures where WebGL1 mipmapping matters (warning)
- missing viewport meta tag, `<title>` or favicon
- hard-coded external URLs that aren't on an allowlist (portal rules differ, so link each rule to its source)
- `localStorage` or `sessionStorage` used without a safe wrapper (sandboxed iframes can throw)
- `AudioContext` created or audio played before a user gesture
- `alert`, `confirm` or `prompt`; `window.open`; navigation of `window.top`
- no `visibilitychange` handling for pause and mute
- source maps shipped in production (warning); heavy `console.log` noise
- with `--portal`: SDK present, lifecycle calls in the right order, rule pack applied

**Runtime checks** (`--runtime`, using Playwright):
- page errors and console errors
- time to first frame
- frame time p50 and p95 over 10 s of scripted input
- long tasks
- runs inside a **sandboxed iframe harness** set up the way portals embed games (checks storage exceptions and blocked APIs)
- mobile viewport with touch emulation
- no audio before the first interaction
- the game pauses when the page is hidden

**Rule packs** live at `kit/rules/<portal>.json`. Each rule has the shape `{ id, severity, check, threshold?, message, source_url, last_verified, status: "verified" | "unverified" }`. Unverified rules show as warnings with the label "verify against official docs".

**Output:** a Markdown report and a JSON report. Exit codes: `0` pass, `1` warnings when `--strict` is set, `2` failures.

**Fixtures:** `tools/__fixtures__/good-game` and `bad-game` (broken on purpose). Tests assert that the expected findings appear.

### Playtest

Takes `playtest.config.json`:
- a URL, or a build directory that the tool serves locally
- a list of viewports
- a duration
- an input script: a sequence of key, mouse and touch actions, or `random` monkey input with a fixed **seed**

It captures:
- a screenshot every N seconds
- video, via Playwright's `recordVideo`
- frame-time samples from an injected `requestAnimationFrame` sampler (p50/p95/max)
- long tasks, through `PerformanceObserver`
- console and page errors
- memory, where the browser exposes it

It writes `.levelwright/playtest/<run-id>/report.html` and `summary.json`. Given the same seed and build, the inputs must be identical.

---

## 7. Templates and portal adapter (`kit/templates/`)

- **Templates:** `threejs`, `phaser`, `pixi` and `godot`. Each is a minimal playable "hello game" with:
  - a loading screen that shows progress
  - pause when the page is hidden
  - a mute toggle saved through a safe storage wrapper
  - the portal adapter wired in at the correct lifecycle points
  - a `levelwright.budget.json`
  - a clean `doctor` pass
- **Adapter** at `templates/shared/portal-adapter.ts`:

  ```ts
  interface PortalAdapter {
    init(): Promise<void>;
    loadingProgress(pct: number): void;
    loadingDone(): void;
    gameplayStart(): void;
    gameplayStop(): void;
    commercialBreak(): Promise<void>;           // resolves after the ad or immediately if none
    rewardedBreak(): Promise<boolean>;          // true = reward granted
    happyMoment?(): void;
    getUserLocale?(): string;
  }
  ```

  Implementations: `crazygames`, `poki`, `gamedistribution`, `gamepix`, `youtube-playables`, `itch` and `none`. Build each one **only from verified SDK docs**. If you can't verify one, ship it as a stub that throws a clear error and add a `TODO(verify)`.
- **Godot:** a GDScript autoload that calls the JS adapter through `JavaScriptBridge`, plus export-preset notes. Test a real web export **only if a Godot binary is available**. Otherwise its coverage is "guidance, untested".
- **Unity:** skills plus a WebGL build-settings checklist. No Unity project. Coverage is "guidance".

---

## 8. Demo games: the proof (`demos/`)

Build two **original**, small games in this repo:

- **Putt Planet:** Three.js low-poly mini golf with 3 holes and mouse/touch aiming.
- **Neon Drift:** Phaser 3 (or PixiJS) top-down arcade drift racer with 1 track, as a time trial.

Do this for real, because the site's before/after section shows exactly this process:

1. Build a quick, naive prototype of each game (about 1 hour each). It should work but have no polish. Commit it and tag `demo/<game>/before`.
2. Install the kit into the demo and run `/lw-audit`, `/lw-polish`, `/lw-port crazygames` (adapter in `none` mode locally), `doctor --runtime` and `playtest`. Commit the resulting reports to `demos/<game>/evidence/`. Tag `demo/<game>/after`.
3. `scripts/capture-media.mjs` checks out each tag into a temporary git worktree, builds it, and captures **identical-camera** screenshots and 6-second clips (WebM and MP4, plus a poster JPG) at the same viewport, seed and moment.
4. Write `demos/<game>/CHANGELOG.md`. Map every visible improvement to the command or specialist that made it, with metrics before → after: fps p95, bundle KB, console errors and load time in ms.

Both demos are deployed with the site at `/play/putt-planet` and `/play/neon-drift`.

---

## 9. Website (`site/`)

### 9.1 Stack and budgets

**Stack:**
- the latest stable **Astro** with static output, TypeScript in strict mode
- **Three.js** (vanilla, as a lazy-loaded island)
- plain CSS with design tokens (use Tailwind only if you justify it in `DECISIONS.md`)
- MDX guides through content collections
- sitemap and RSS
- fonts self-hosted via Fontsource
- `vercel.json` with security headers and long caching for hashed assets

**Budgets, enforced with Lighthouse CI:**

| Metric | Budget |
|---|---|
| Initial JS (excluding the deferred Three.js chunk) | ≤ 60 KB gzipped |
| Three.js hero chunk | ≤ 180 KB gzipped |
| LCP (Lighthouse mobile) | ≤ 2.0 s |
| CLS | ≤ 0.05 |
| Lighthouse mobile scores | Performance ≥ 90, Accessibility ≥ 95, Best Practices ≥ 95, SEO = 100 |

### 9.2 Visual direction: "arcade workshop" (original)

- **Tokens:**

  | Token | Value |
  |---|---|
  | ink (background) | `#0A0D12` |
  | surface | `#11161F` |
  | line | `#1F2733` |
  | text | `#E8ECF2` |
  | muted | `#8A96A8` |
  | amber (primary CTA) | `#FFB547` |
  | teal (links, secondary) | `#3FE0C5` |
  | red | `#FF5D6C` |
  | green | `#5BE38A` |

  Every pairing you use must pass WCAG AA contrast.
- **Type:** a characterful display face (e.g. Bricolage Grotesque), a clean body face (e.g. Geist), and JetBrains Mono for terminal and code. All must be OFL-licensed and self-hosted. Use a modular type scale of 1.25.
- **Motifs:** faint blueprint grid lines (about 4% opacity); a subtle CRT scanline **only** on terminal blocks; chunky 2px borders with a 10px radius; arcade-style buttons with a 2px offset shadow that "press" on click.
- **Motion:** 150–250 ms ease-out. One hero centrepiece; no parallax overload. Always respect `prefers-reduced-motion`.
- **Avoid:** purple/blue "AI" gradients, glassmorphism, emoji bullets, stock 3D blobs, generic icon grids and walls of identical cards.

### 9.3 Page flow

The order follows a proven kit-landing flow. The design and all copy must be your own.

1. **Nav:** logo (an original wordmark plus a small pixel-glyph mark you design in SVG), Kit, Demos, Specialists, Pricing, Guides, a "Get Indie — $29" button, and a skip link.
2. **Hero:** headline, a subheading, chips for engines and portals, a primary CTA (Buy Indie) and a secondary one (Get Lite free). The **playable 3D scene** (§9.4) sits beside or behind the text.
3. **Install:** one command with a copy button. Next to it, an animated terminal replays a **real** `init --dry-run` transcript captured from the CLI. Show a file counter `n / N`, where N comes from the manifest.
4. **Proof:** a gallery of the demo games using real captures, each with a "Play it" link.
5. **Before / After:** a draggable comparison slider (keyboard-operable) for each demo, with the before → after metrics table and the commands that produced the change.
6. **Specialists:** a filterable, searchable roster, filtered by stage, engine, type and tier. Data comes from `manifest.json`. Use buttons with `aria-pressed`, and update counts live.
7. **The loop:** the four stages (Diagnose → Feel → Business → Ship) as an animated SVG diagram with one sentence each.
8. **Tools that run:** showcases of doctor and playtest, using real report excerpts and a frame-time chart drawn from real `summary.json` files.
9. **Works with your agent:** Claude Code (full), and Codex, Cursor and Gemini CLI at their **verified** levels.
10. **Coverage, stated plainly:** a table of engines × areas marked Full / Partial / Guidance / Not yet, generated from a data file.
11. **Pricing:** three plans from `pricing.ts`. Show "One-time. No subscription." and the refund policy line. Note that taxes are handled at checkout by Polar as merchant of record.
12. **FAQ:** 8–10 real questions. Examples: Do I need a paid Claude plan? Does it work offline? What if I've edited the files? Can I use it in commercial games? What does Studio cover? What about refunds?
13. **Roadmap:** honest items. Asset generation is "exploring". Unreal is "not planned".
14. **Guides:** the latest 3 guides.
15. **Footer:** Terms, Privacy, Refunds, License (EULA), install docs, changelog, RSS, the Lite repo, and placeholders for support email and Discord from config.

**Other pages:**
- `/guides` and `/guides/[slug]`
- `/docs/install`
- `/changelog` (rendered from `kit/CHANGELOG.md`)
- `/play/putt-planet` and `/play/neon-drift`
- `/legal/{terms,privacy,refunds,license}`. These are templates, marked "review before launch".
- a `404` page with a tiny playable easter egg

### 9.4 Hero scene spec (Three.js)

- **Scene:** a floating low-poly island with one mini-golf hole, a flag and a few props, built procedurally (no large GLB files). The camera idles in a slow orbit.
- **Play:** drag back from the ball to aim (with a power line) and release to putt. Touch works the same way. The keyboard also works: arrows to aim, Space to charge and release. Show a stroke counter, and "Hole in N" on success. The Buy CTA gives one gentle pulse when the ball goes in.
- **The toggle: `Prototype ⇄ Polished`.** This demonstrates the product's promise, so make the difference obvious and tasteful.
  - *Prototype:* flat unlit materials, no shadows, linear motion, a hard camera, no particles, no sound.
  - *Polished:* standard materials with hemisphere and directional light and small-map soft shadows, ACES tone mapping, a ball trail, squash-and-stretch on impact, dust particles, about 60 ms of hitstop and a light screenshake on a hole-in, an eased follow camera, and synthesized WebAudio SFX (**muted by default**, with a toggle).
- **Performance:**
  - tree-shaken imports from `three`
  - DPR capped at 1.5
  - ≤ 50 draw calls
  - initialise only on idle *and* when the hero is visible
  - pause when the hero is off-screen or the tab is hidden
  - dispose everything on navigation
  - a static poster when WebGL is unavailable, the context is lost, or reduced motion is on (reduced motion still offers a "Play anyway" button)
- **Accessibility:** the canvas is decorative. Give it an aria-label and visible instructions. The page must be fully usable without it.

### 9.5 Copy

- Plain, confident and specific. Short sentences. Every claim maps to something that exists.
- Banned words: revolutionize, supercharge, unleash, seamless, game-changer, cutting-edge, 10x.
- Write a few headline options and pick the strongest. A starting direction (rewrite freely): **"Your game, studio-grade."** / *"17 specialists and real tools that find the bug, fix the feel, wire the portal, and tell you honestly when it's ready."*

### 9.6 Checkout

- Read Polar checkout URLs from `site/src/config/pricing.ts`, which reads `PUBLIC_POLAR_CHECKOUT_INDIE` and `PUBLIC_POLAR_CHECKOUT_STUDIO`.
- If they're unset, the buy buttons become "Get Lite (free)" plus "Watch on GitHub for launch".
- Never collect payment data on the site.

### 9.7 SEO and sharing

- Title and meta description per page; canonical URLs
- OG images generated at build time
- JSON-LD `SoftwareApplication` with `offers` taken from `pricing.ts`
- sitemap and `robots.txt`
- privacy-friendly, cookieless analytics (Vercel Web Analytics behind a config flag)

### 9.8 Visual QA loop (mandatory)

1. Use Playwright to screenshot every section at 375, 768, 1280 and 1920 px, and once more with reduced motion.
2. Critique the screenshots against this checklist: hierarchy; spacing rhythm; alignment; contrast; type scale; how prominent the CTAs are; dead space; tap targets ≥ 44 px; overflow and wrapping; whether the hero reads within 3 seconds.
3. Fix what you find, then repeat steps 1–2 **twice**.
4. Commit the final screenshots to `docs/qa/` and link them from `PROGRESS.md`.

---

## 10. Lite, release and CI

- **`scripts/build-lite.mjs`** generates `lite/` from the manifest (`tier: lite`) plus the basic doctor rules, with its own README and an MIT LICENSE. The full kit ships under `EULA.md`: commercial use in the buyer's games is allowed; redistributing the kit is not; seats per plan.
- **Guard:** the build **fails** if any `tier: full` file ends up in `lite/`.
- **`scripts/release.mjs`** splits `kit/` into the private repo `geraltbhai/levelwright-kit` and `lite/` into the public repo `geraltbhai/levelwright-lite` (via subtree split or a clean export). If those repos don't exist and you can't create them, **stop and ask Sam to create them**. Never push full-kit files anywhere public.
- **CI** (`.github/workflows/ci.yml`) runs:
  - the Node tests
  - doctor on the fixtures and the demos
  - the site build and a link check
  - Playwright smoke and visual tests
  - axe accessibility checks
  - Lighthouse CI against the §9.1 budgets
- **`docs/LAUNCH.md`** is Sam's step-by-step checklist:
  1. Create the Polar organization and set up payouts (India is supported through Stripe Connect Express).
  2. Create the Indie and Studio products with the **GitHub repository access** benefit pointing at `levelwright-kit`.
  3. Paste the checkout links into Vercel's environment variables.
  4. Import the project into Vercel and connect a domain.
  5. Make a sandbox test purchase.
  6. Walk through the refund flow.
  7. Studio fulfilment for v1: invite the buyer's extra GitHub usernames manually.
  8. Launch posts: Show HN; an r/gamedev post that follows its rules; a dev.to guide; an X thread; listing the free Lite skills in skill directories as the top of the funnel.

---

## 11. Phases, priorities and acceptance

Check each item off in `PROGRESS.md` as you go. Push and tag at the end of every phase.

**P0 = must ship · P1 = should ship · P2 = bonus**

| # | Phase | Priority | Acceptance |
|---|---|---|---|
| 0 | **Verify & plan.** Read the docs in §2.1 and write `docs/SOURCES.md`, `docs/ARCHITECTURE.md` and the first `DECISIONS.md` entries. Scaffold the folders. | P0 | Every external format the kit relies on has a source and a date. The plan is committed. |
| 1 | **Kit content.** All 17 agents, 45 skills and 8 commands at the §4.2 quality bar. Manifest generator. | P0 | The manifest is valid; every skill has a Verify section and official sources; a spot-check of 5 random skills by a fresh subagent passes. |
| 2 | **CLI installer** (init/update/status/uninstall, markers, plugin packaging, multi-agent adapters) and its tests. | P0 | All §5 tests pass; a manual install/update/uninstall in a scratch project is logged in PROGRESS.md. |
| 3 | **Doctor** (static and runtime), rule packs, fixtures, **playtest** bot. | P0 doctor-static · P1 runtime + playtest | Fixture tests pass; `bad-game` produces the expected findings; a playtest report renders. |
| 4 | **Templates and portal adapter.** | P1 (threejs + adapter = P0) | Each template passes `doctor`; unverified adapters are stubs with a clear error. |
| 5 | **Demo games:** before → kit → after, plus evidence and media capture. | P1 (Putt Planet) · P2 (Neon Drift) | Tags exist; evidence reports are committed; captured media is identical-camera; the CHANGELOG has metrics. |
| 6 | **Website:** tokens → layout → sections → hero 3D → guides (3 seed guides with official citations) → SEO → budgets → visual QA loop ×2. | P0 | Builds locally; meets the §9.1 budgets; QA screenshots committed; no fabricated numbers. |
| 7 | **Lite build, release scripts, CI, legal templates, `docs/LAUNCH.md`.** | P0 lite guard + LAUNCH · P1 CI | The lite guard test passes; CI is green on push. |
| 8 | **Independent verification** (§12), fix pass, final handover. | P0 | The reviewer's findings are fixed or documented; the handover is at the top of PROGRESS.md. |

If time or usage runs short, finish P0 everywhere before starting any P1 work. If you have to stop, the repo must still build and `PROGRESS.md` must say exactly where to resume.

---

## 12. Final verification (independent)

Spawn a **fresh reviewer subagent** that has seen none of the build. Give it only §1, §9 and §11 of this file plus the repo. It must check:

1. Every claim on the site maps to a working feature (list each claim → file → evidence).
2. There are no fabricated numbers, testimonials or counts. Every number traces back to the manifest or a real report.
3. Install → update (with one modified file) → uninstall works in a scratch project.
4. `doctor` and `playtest` run on a demo.
5. The Lighthouse budgets and axe checks pass.
6. The copy is original and never mentions a competitor.
7. No full-kit files appear in `lite/` or in anything that would be public.

Fix every finding, or document in `PROGRESS.md` why it's deferred. Then make the final checkpoint and push.

---

## 13. Handover

Finish by writing this at the **top of `PROGRESS.md`** and replying with the same:

- what shipped (with links to the repo and tags)
- how to run everything locally (the site, CLI, doctor, playtest and demos)
- **Sam's to-do list:** create the `levelwright-kit` and `levelwright-lite` repos if they're still missing; set up Polar; import into Vercel; pick a domain; replace the support email and Discord placeholders; get the legal templates reviewed
- known gaps and anything marked `unverified`
- hero screenshots at 1440 px and 375 px
