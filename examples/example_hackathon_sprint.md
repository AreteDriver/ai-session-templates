# Example Hackathon Sprint Session

A filled-in session template for a 72-hour hackathon sprint building an EVE Frontier intelligence tool, showing deadline-driven tradeoff decisions, scope cuts, demo script planning, and submission requirements.

```md
# Hackathon Sprint Session

## Objective
Build and deploy a working EVE Frontier intelligence tool in 72 hours for the Frontier Hackathon. The tool must ingest on-chain data, present actionable intel to players, and demonstrate a viable use case for the Frontier ecosystem. Judging criteria: functionality, UX polish, technical depth, and ecosystem value.

## Deadline
April 13, 2026 23:59 UTC. Submission requires: live URL, 3-minute demo video, GitHub repo link, and a README explaining the architecture.

## Project
- Name: frontier-tribe-os
- Stack: Python 3.12 / FastAPI (backend), React 18 / Vite (frontend), SQLite, Sui testnet RPC
- Deploy targets: Fly.io (API), Vercel (frontend)
- Repos: `AreteDriver/frontier-tribe-os` (monorepo)

## What Ships (MVP -- non-negotiable)
These features must work in the demo video or we do not submit:

1. **Kill Feed** -- live-updating table of recent killmails from on-chain events. Columns: timestamp, killer, victim, solar system, ship type. Auto-refreshes every 30 seconds.
2. **System Intelligence** -- click a solar system name anywhere in the app, get a panel showing: kill count (24h/7d), active smart assemblies, fuel status of gates, threat level (high/medium/low).
3. **LLM Briefing** -- one-click "Generate Briefing" button that calls Claude Haiku to produce a 150-word tactical summary of a system's recent activity. Costs ~$0.002 per briefing. Pre-cache the 10 most active systems.
4. **Landing Page** -- single page explaining what the tool does, with a "Login to Dashboard" CTA. Must look professional in the demo video.
5. **Deployment** -- live on `frontier-tribe-os.vercel.app` (frontend) and `frontier-tribe-os.fly.dev` (API). Both must be up during judging.

## What Gets Cut If Behind Schedule
Ordered by what drops first:

1. **Alert Configuration UI** -- users configure notification rules. Cut: hardcode 3 alert rules, show them as read-only. Saves ~4 hours.
2. **Pilot/Corp Intel pages** -- detailed entity lookup. Cut: show entity name + kill count only, no drill-down. Saves ~6 hours.
3. **Battle Report generation** -- aggregate kills into battle reports. Cut entirely. Saves ~8 hours. Can describe as "planned feature" in README.
4. **Signature Resolution module** -- track exploration signatures. Cut entirely. Saves ~6 hours. Not core to the intel story.

## Acceptable Tech Debt
Document in README under "Known Limitations". Fine for a hackathon:
- No backup, no auth, no rate limiting, no test suite
- Hardcoded Sui package ID in `backend/config.py`
- `print()` instead of structured logging, inline CSS, `any` types in TS

Do NOT cut: RPC error handling (testnet drops kill the demo), mobile layout (judges use phones), loading skeletons (white screens look broken).

## Architecture (Keep Simple)
Monorepo. `frontend/` (React+Vite: Landing, Dashboard, SystemDetail pages) and `backend/` (FastAPI: ingest.py polls Sui events, routes/ serves data, services/ has intel + briefing logic, SQLite via db.py). No microservices, no message queue, no Redis.

## Sprint Schedule

### Hours 0-8: Foundation
- [ ] Scaffold FastAPI backend with health check
- [ ] Scaffold React frontend with Vite, react-router, basic layout
- [ ] Set up Sui RPC connection and verify event polling works locally
- [ ] Create SQLAlchemy models for killmails and solar systems
- [ ] Deploy skeleton to Fly.io and Vercel -- confirm both are reachable
- **Gate**: `curl https://frontier-tribe-os.fly.dev/health` returns 200

### Hours 8-24: Core Features
- [ ] Implement kill feed ingestion from Sui events
- [ ] Build Kill Feed page (table with auto-refresh)
- [ ] Implement system intelligence aggregation queries
- [ ] Build System Intelligence panel (click system name to open)
- **Gate**: Kill feed shows real killmails from testnet. System panel shows kill counts.

### Hours 24-40: LLM + Polish
- [ ] Implement briefing endpoint (Claude Haiku, 150-word max, 512 max_tokens)
- [ ] Build Briefing Card component with loading state
- [ ] Pre-cache briefings for top 10 active systems on startup
- [ ] Build landing page with clear value prop and CTA
- [ ] Add loading skeletons to all data-fetching components
- **Gate**: Full flow works end-to-end. Stranger could use it without explanation.

### Hours 40-56: Stretch Features + Hardening
- [ ] Alert Configuration (if on schedule) or hardcode and skip
- [ ] Pilot/Corp Intel (if on schedule) or stub with minimal data
- [ ] Error boundaries on all pages
- [ ] Mobile-responsive pass on all pages
- [ ] Favicon, meta tags, Open Graph image
- **Gate**: App survives Sui testnet going down for 5 minutes without crashing.

### Hours 56-72: Demo Prep + Submission
- [ ] Record 3-minute demo video (script below)
- [ ] Write README: architecture diagram, setup instructions, known limitations
- [ ] Final deploy to both platforms
- [ ] Test all submission links work
- [ ] Submit before deadline with 2-hour buffer
- **Gate**: Submission form completed. All links verified by a second person if possible.

## Demo Script (3 minutes)
1. (0:00-0:30) Open landing page. "Frontier Tribe OS gives EVE Frontier pilots real-time intelligence on what is happening in their neighborhood." Click "Go to Dashboard."
2. (0:30-1:15) Show Kill Feed. "This is a live feed of killmails from the Frontier blockchain. Every kill on-chain appears here within 30 seconds." Point out columns. Click a solar system name.
3. (1:15-2:00) Show System Intelligence panel. "Clicking any system gives you its threat profile -- kills in the last 24 hours, active smart assemblies, gate fuel status." Click "Generate Briefing."
4. (2:00-2:45) Show LLM Briefing. "The briefing is generated by Claude, summarizing recent activity into a tactical overview. It costs a fraction of a cent per request." Show the rendered briefing.
5. (2:45-3:00) "Frontier Tribe OS is open source, deployed on Fly.io and Vercel, and ingests data directly from the Sui blockchain. Thank you."

## Constraints
- Total Anthropic API spend capped at $5 for the hackathon (Haiku is cheap, but no infinite loops)
- Sui testnet RPC at `fullnode.testnet.sui.io:443` -- not mainnet
- No storing secrets in the repo -- use Fly.io secrets and Vercel env vars
- Monorepo: `flyctl deploy` from `backend/`, Vercel rootDirectory set to `frontend/`

## Known Risks
- Sui testnet instability: if RPC is down during demo recording, use cached data and note it
- Fly.io cold start: keep `min_machines_running = 1` to avoid 10-second spin-up in demo
- Anthropic rate limits: pre-cache briefings so the demo never waits on a cold LLM call
- Vercel build timeout: keep frontend dependencies minimal, no heavy UI libraries

## Submission Checklist
- [ ] Live frontend URL works and loads in under 3 seconds
- [ ] Live API URL returns health check 200
- [ ] Demo video is under 3 minutes, uploaded and accessible
- [ ] GitHub repo is public with a README
- [ ] README includes: project description, architecture, setup instructions, tech stack, known limitations
- [ ] All submission form fields completed
- [ ] Verified all links work from an incognito browser window
```
