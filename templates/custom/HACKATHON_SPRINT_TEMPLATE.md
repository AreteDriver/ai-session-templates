# Hackathon Sprint Template

For time-boxed delivery with a hard demo deadline. Optimizes for shipping a working demo over
complete features or clean code. Forces explicit cut decisions upfront so you spend time
building, not debating scope mid-sprint.

```md
# Hackathon Sprint Session

## Deadline
- Demo time: [date and time, with timezone]
- Time remaining: [hours]
- Submission method: [live demo / video / deployed URL / repo link]
- Submission requirements: [list mandatory deliverables — README, video length, deploy URL, etc.]

## Demo Target
[One sentence: what will the demo show? This is the North Star. Every decision filters through this.]

### Demo Script Outline
1. [Open/navigate to — first thing the audience sees]
2. [Show feature A — the hook, most impressive thing first]
3. [Show feature B — demonstrates depth or breadth]
4. [Show feature C — if time permits]
5. [Close with — final impression, call to action, or summary slide]

Total demo time: [minutes]

## Judging Criteria (if applicable)
| Criterion | Weight | Our Strategy |
|-----------|--------|-------------|
| [innovation / technical depth / UX / completeness / impact] | [%] | [how we win on this] |

## Feature Tiers

### Must-Ship (demo breaks without these)
- [ ] [feature — brief description, estimated time]
- [ ] [feature — brief description, estimated time]
- [ ] [feature — brief description, estimated time]
- [ ] [feature — brief description, estimated time]

### Nice-to-Have (improves demo, not critical)
- [ ] [feature — brief description, estimated time]
- [ ] [feature — brief description, estimated time]
- [ ] [feature — brief description, estimated time]

### Cut List (explicitly NOT doing)
- [feature — why cut: too risky / too slow / not demo-visible]
- [feature — why cut]
- [feature — why cut]

## Tech Stack
- Frontend: [framework / "none"]
- Backend: [framework / "none"]
- Database: [SQLite / Postgres / in-memory / "none"]
- Deployment: [Fly.io / Vercel / local / "demo from localhost"]
- External APIs: [list with auth status — "key obtained" / "need to sign up"]

## Tech Debt Budget
Acceptable shortcuts for this sprint. Be explicit so future-you knows what to fix.

| Shortcut | Why Acceptable | Cleanup Cost |
|----------|---------------|-------------|
| [hardcoded config instead of env vars] | [demo only, not production] | [30 min] |
| [no auth — open endpoints] | [demo environment, no real data] | [2 hours] |
| [no error handling on API calls] | [happy path demo only] | [1 hour] |
| [inline styles / no design system] | [function over form] | [2 hours] |
| [no tests] | [ship speed, manual verification] | [half day] |

## Sprint Plan

### Hour-by-Hour (adjust to your timeline)
| Time Block | Task | Deliverable |
|-----------|------|-------------|
| 0:00-0:30 | Scaffold project, deploy hello world | Live URL or running local |
| 0:30-2:00 | Must-ship feature 1 | [working, not polished] |
| 2:00-3:30 | Must-ship feature 2 | [working, not polished] |
| 3:30-5:00 | Must-ship feature 3 | [working, not polished] |
| 5:00-6:00 | Integration, smoke test full flow | [end-to-end demo path works] |
| 6:00-7:00 | Nice-to-haves (if ahead of schedule) | [bonus features] |
| 7:00-7:30 | Polish: landing page, copy, visual cleanup | [demo-ready appearance] |
| 7:30-8:00 | Final deploy, test submission, record video | [submitted] |

### Checkpoint: Halfway Mark
At the halfway point, answer these honestly:
- [ ] Can I demo the core flow end-to-end right now? (even ugly)
- [ ] If no: drop everything else and make the core flow work
- [ ] If yes: proceed to nice-to-haves or polish

## Risk Mitigation
| Risk | Mitigation |
|------|-----------|
| External API goes down | [mock data fallback / cached responses] |
| Deploy fails | [demo from localhost as backup] |
| Feature takes 3x longer than estimated | [cut it, move to next must-ship] |
| Scope creep ("one more thing") | [check: does it make the demo better? no = skip] |

## Constraints
- Ship over polish. A working demo with rough edges beats a polished fragment
- Working demo over complete feature set. Show 3 things that work, not 6 half-built things
- Deploy early, deploy often. First deploy should happen in the first 30 minutes
- No refactoring during the sprint. If it works, move on
- If a feature takes more than 1.5x its estimate, cut scope or simplify approach
- Commit frequently. Losing work to a bad state is not recoverable in a sprint
- Test the demo flow, not individual units. End-to-end path matters most

## Submission Checklist
- [ ] Demo URL live and accessible (or video recorded)
- [ ] README covers: what it does, how to run it, tech stack
- [ ] Repo is clean (no secrets, no node_modules, .gitignore correct)
- [ ] Submission form filled out with all required fields
- [ ] Demo script rehearsed at least once
- [ ] Backup plan ready (localhost demo / pre-recorded video)

## Required Output
1. Deployed demo (URL or video link)
2. Repo link (clean, with README)
3. Features shipped vs. planned (what made it, what was cut)
4. Tech debt inventory (shortcuts taken, cleanup estimates)
5. What went well / what to do differently next time
```
