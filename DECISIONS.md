# DECISIONS

Format for each entry: context → options → choice → why.

## D-001 · Pricing below the category, with a free tier (2026-11-10)
- **Context:** The reference product in this category sells a single plan for $89 one-time. We have no audience yet.
- **Options:** match at $89 · undercut with a single $49 plan · free Lite plus $29 Indie plus $69 Studio.
- **Choice:** free Lite plus **$29 Indie** plus **$69 Studio** (5 seats), all one-time through Polar.
- **Why:** The free Lite tier is how people find us (skill directories, GitHub). $29 sits below the point where a buyer needs approval. The trade-off: we need about 3× the sales volume of an $89 product to earn the same revenue, so the extra features in PROMPT.md §1 have to make the paid tier obviously worth it.

## D-002 · Dev monorepo stays private; buyer and free repos are split out (2026-11-10)
- **Choice:** Develop in private `levelwright`. Release `kit/` to private `levelwright-kit` (Polar GitHub-access benefit) and `lite/` to public `levelwright-lite`.
- **Why:** Buyers get only the kit, not the site or demo sources. A build guard stops full-kit files from leaking into the public repo.
