---
name: clone-improve
description: Clone and improve any app — reverse-engineer it, rebuild what it does, fix what its users hate from real reviews, rebrand and ship it as yours. Use when Ken says "clone this app", "reverse engineer X", "make my own version of X", or names the Clone and Improve agent.
---

# Clone and Improve — app cloning, the Titain way

Rebuild what an app does, fix what its users hate, ship it as yours.
Adapted from the Replica Skill (Jake Schincariol, MIT, github.com/Jakeschincariol/replica-skill) for Ken's stack and rules. The upstream repo is the source of the method and its six Python tools; this file is how WE run it.

## The rule that governs everything

**Rebuild what an app does, never what it owns.** Features and flows, clean-room style. Never its code, logo, copy, content, trademarks, brand colours, licensed fonts, or private APIs. Never scrape behind a login, against a site's terms, or past a paywall. Only public pages and the user's own account. If the target's terms forbid using an account to build a competitor, say so and work from public sources only.

- No fake reviews, ever — not for our app, not against theirs. Illegal in the US under the FTC's 2024 rule.
- Reviewers' words are research, never testimonials. Do not put them on a landing page.
- Deploy is blocked until the rebrand sweep is clean AND Ken says go.
- Nothing public without Ken's approval of the exact words and the exact result.
- "Clone any app" means the features and the flow — not the content, the network, or the licences the app owns. You can rebuild a music app's player. You cannot clone its catalogue.

## The pipeline — eleven stages, in order

Each stage reads what the last one wrote, in a `replica/` folder in the project. Run them in order. Any one can also run alone.

| # | Stage | What it does | Output |
| --- | --- | --- | --- |
| 1 | recon | Reverse-engineers the app: screens (S01…), flows (F01…), components, inferred data model, feature matrix. From public pages, screenshots, store listings, help docs, and the user's own account. Sizes it S / M / L / XL. Lists honestly what cannot be cloned. | `replica/recon.md`, `replica/features.csv`, `replica/screens/` |
| 2 | architect | Plans the stack, database schema, and API. Orders the build: the core loop first. | `replica/architecture.md` |
| 3 | design | Rebuilds the design system as tokens: colour roles, type scale, spacing, components. Your own assets, open icons, open fonts, your own words. | `replica/design/tokens.json` |
| 4 | build | Rebuilds the app screen by screen from the recon map, ticking off `features.csv`. | the app |
| 5 | backend | Auth, database, payments, integrations. | the app |
| 6 | test | Test plan from the flows; happy paths and edge cases (time zones, double submit, slot taken mid-booking). Bugs logged by severity until no S1 or S2 is open. | `replica/test-plan.md`, `replica/bug-report.md` |
| 7 | diff | Compares the clone against the original: a parity score and what is missing. Missing must-haves means not shippable. | parity score |
| 8 | entrepreneur | Reads what the app's real users hate and turns it into fixes and a positioning angle. See below. | `replica/reviews.csv`, `replica/feedback.md`, `replica/fixes.md` |
| 9 | brand | Names it and rebrands it so it is yours: name, palette, logo brief, voice. Sweeps the codebase until nothing of the original is left. | clean sweep |
| 10 | launch | Landing page around the angle, pricing against the original's public page, store listing linted. | landing page, listing |
| 11 | deploy | Preflight (tests, parity, sweep, listing), production, DNS records, ships when Ken says go. | live on your domain |

## Entrepreneur first — the most valuable stage

A straight copy has no reason to exist. Run `/replica-entrepreneur` FIRST on any app Ken is only thinking about cloning — it finds whether there is a reason for our version to exist before any building starts.

- Aim for 100+ real reviews across at least three sources: App Store / Google Play, G2, Capterra, Trustpilot, Reddit, Hacker News, the original's own feature-request board, its changelog.
- Every quote verbatim and linked. No invented reviews, quotes, ratings, counts, or sources. If a source cannot be reached, say so. If there are 14 reviews, say 14.
- Three ranked lists: what they hate, what is missing, what is unsolved (whole jobs the app ignores — these become positioning).
- Fix plan: top 5–8 by evidence × how cheaply they can be fixed. Each fix goes into `features.csv` as a row with `original` set to `no`.
- Angle: three positioning options, each grounded in a top theme, in the form "For {who} who {hate this about the original}, {your app} {does this instead}" with the evidence. Recommend one. It drives the brand and the launch hero.
- Never put the original's name in our app name, ads, or store listing.

## The six tools (upstream repo, standard-library Python only)

Python 3.8 or newer. Nothing to pip install. None touch the network.

```bash
python3 replica-diff/imgdiff.py original.png clone.png --out diff.png   # layout diff, ignores colour
python3 replica-diff/parity.py replica/features.csv                     # parity score + missing list
python3 replica-entrepreneur/reviews.py replica/reviews.csv             # what users hate, ranked, linked
python3 replica-design/contrast.py replica/design/tokens.json           # WCAG contrast on your tokens
python3 replica-brand/sweep.py . --avoid "Original App"                 # anything of the original left?
python3 replica-launch/listing.py replica/launch/listing.json           # store limits + copycat checks
```

- `parity.py` weights must / should / could, counts partial as half, never scores what was left out on purpose or added. Missing must-haves = not shippable.
- `reviews.py` drops every review without a link. Themes with under 3 reviews or one source are marked thin.
- `sweep.py` finds the original's name, domain, and colours anywhere in the code — including inside identifiers — and blocks deploy until clean.

## Ken's stack (architect picks from this first)

TypeScript, Node.js, Next.js, React, Cloudflare Workers / Pages, Supabase, Tailwind CSS; Vercel as the deploy target. Free-tier first — no paid service load-bearing before the product earns. If the target genuinely demands a different stack, say why and get Ken's go before deviating.

## Gates (Ken's go required)

1. Ken approves the target app AND the slice ("all of Notion" is not a project; "Notion's pages, blocks and sharing" is) before recon starts.
2. Ken approves the brand (name, palette, voice) before launch.
3. Ken approves the exact launch words and the exact result before anything goes live. Deploy is blocked until the sweep is clean AND Ken says go.
4. Before selling anything built this way: run the trademark checks brand lists, read the terms of anything used, and talk to a lawyer if there is money on the line. This is not legal advice.

## Install (on Ken's PC, in Claude Code)

```bash
/plugin marketplace add Jakeschincariol/replica-skill
/plugin install replica-skill@replica-skill
```

Or clone it and copy the folders: `git clone https://github.com/Jakeschincariol/replica-skill.git`, then `cp -r replica-skill/replica-* ~/.claude/skills/`. Needs Claude Code Pro or higher. No signup, no API key.

## Scoring (Head PM rule — every idea gets scored on arrival)

Clone and Improve: speed to first dollar HIGH (a proven app rebuilt in weeks, not months), revenue LARGE (sell the improved version, or rebuild a client's clunky tool as a service), unfair advantage MEDIUM (the entrepreneur twist — fixes from real user complaints — plus our stack), build cost LOW (the skill files exist free under MIT; the work is running them well). Verdict: earns hours. First use: run entrepreneur on an app Ken is thinking about, before any build. Do not displace the Titain launch.
