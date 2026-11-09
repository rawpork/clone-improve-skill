# Clone and Improve

Clone and improve any app — reverse-engineer it, rebuild what it does, fix what its users hate from real reviews, rebrand and ship it as yours.

Adapted from the [Replica Skill](https://github.com/Jakeschincariol/replica-skill) (Jake Schincariol, MIT) for the Titain stack (TypeScript, Next.js, Cloudflare, Supabase, Tailwind, Vercel, free-tier first).

## Install

Clone it straight into Claude Code's global skills folder:

```bash
git clone https://github.com/rawpork/clone-improve-skill.git ~/.claude/skills/clone-improve
```

On Windows (PowerShell):

```powershell
git clone https://github.com/rawpork/clone-improve-skill.git $env:USERPROFILE\.claude\skills\clone-improve
```

Skills in `~/.claude/skills` load in every Claude Code session, whatever folder it is opened in. Restart Claude Code after installing.

For the full upstream eleven-stage pipeline and its six Python tools, install the Replica Skill plugin in Claude Code:

```
/plugin marketplace add Jakeschincariol/replica-skill
/plugin install replica-skill@replica-skill
```

## The rule

Rebuild what an app does, never what it owns. Features and flows, clean-room style. Never its code, logo, copy, content, trademarks, brand colours, licensed fonts, or private APIs. Only public pages and your own account. No fake reviews, ever. Deploy is blocked until the rebrand sweep is clean and the owner says go.

## Pipeline

recon → architect → design → build → backend → test → diff → entrepreneur → brand → launch → deploy. Each stage reads what the last one wrote into a `replica/` folder. Run the entrepreneur stage first on any app you are only thinking about — it reads 100+ real user reviews, verbatim and linked, and finds whether your version has a reason to exist before any building starts.
