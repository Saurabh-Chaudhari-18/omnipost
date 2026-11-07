# OmniPost

B2B social media marketing platform — one workspace where a business or agency manages its brands, creates content, runs approvals, and publishes to Facebook, Instagram and LinkedIn.

| Folder | What's inside |
|---|---|
| [`backend/`](backend/) | FastAPI · PostgreSQL 16 · Celery/Redis · S3-compatible storage — API, database migrations, background workers, tests |
| [`frontend/`](frontend/) | React 19 · TypeScript · Vite · Tailwind CSS 4 · shadcn/ui — the web app, in the product owner's design (dark by default) |
| [`docs/`](docs/) | Project documentation: approach, database, functionality, progress and roadmap, system design, **developer setup** |

**New developer? Start here:** [docs/07_setup_guide.md](docs/07_setup_guide.md) — install, configure and run everything on your laptop, step by step.

**All documentation:** [docs/README.md](docs/README.md)

---

## Project status — 7 November 2026

| Area | Status |
|---|---|
| Backend (Phase 1) | ✅ Done |
| Database (13 tables, migrations 0001–0004) | ✅ Done |
| Automated backend tests (66) | ✅ Passing |
| Web app (Phase 1 screens) | ✅ Done, in the owner's design |
| Full journey in the browser (write → approve → publish to test accounts) | ✅ Verified |
| Live Facebook / Instagram publishing | 🟡 Waiting on the client's Meta developer app |
| LinkedIn connect flow | 🟡 Depends on LinkedIn developer app approval |
| Hosted review environment | ⏳ Planned |
| AI content generation | ⏳ Phase 2 |

---

## ✅ Done so far

**Web app**

- **Sign-in and session** — register, sign in, stays signed in across reloads; light / dark / match-device theme saved to the account (dark by default).
- **Navigation** — icon rail with brand switcher on desktop, bottom bar on phones.
- **Home** — counts, what needs attention, what's coming up, account health, brands, recently published.
- **Post Studio** — write once or per platform, Smart Templates with fill-in blanks, hashtags, call to action, photos and videos (upload, reorder, remove), live Instagram / Facebook / LinkedIn previews with character-limit rings, pre-publish checks per account.
- **Approval** — send for approval (the content is sealed with a fingerprint), approve, request changes, withdraw; approval record with the seal; printable **approval certificate**.
- **Publishing** — post now or schedule, live status per account, try again for failures.
- **Calendar**, **Smart Templates**, **Connected accounts** (health, expiry warnings, test accounts, Facebook connect), **Brands**, **Settings** (team, self-approval rule, 30-day usage).

**Backend**

- Authentication with rotating refresh tokens, workspaces and roles, brands, posts with per-platform wording, media with file checks and location-data removal, hash-bound approval, background publishing that never posts twice, audit trail, usage logging, per-user preferences.

Details: [docs/03_backend_functionality.md](docs/03_backend_functionality.md) · [docs/02_database.md](docs/02_database.md) · [frontend/README.md](frontend/README.md)

---

## 🟡 In progress

- **Real publishing check** with a Meta developer app and a test Page / Instagram business account (needs the client's Meta Business access).
- **LinkedIn** connect flow if the client's LinkedIn developer app is approved; until then LinkedIn works with test accounts only.
- **Polish and testing:** automated browser tests for the main journey, accessibility and phone-layout passes.

## 🔭 Future plan

**Finish Phase 1:** hosted review environment (Railway or Render + Cloudflare R2), end-to-end test pass there, demo script, known-limitations document, sign-off.

**Phase 2 and later:** AI captions, hashtags and images (always through the approval workflow), Instagram carousels / Reels / Stories, team administration, multi-level approvals, analytics, billing, production hardening.

Full roadmap and known limitations: [docs/04_progress_and_roadmap.md](docs/04_progress_and_roadmap.md) · System design: [docs/06_system_design.md](docs/06_system_design.md)

---

## Quick start

Full instructions: [docs/07_setup_guide.md](docs/07_setup_guide.md).

**Quickest:** install Git, Python 3.12, Node 20+ and Docker Desktop, then from the repository root:

| | Windows (PowerShell) | macOS / Linux |
|---|---|---|
| Once | `powershell -ExecutionPolicy Bypass -File scripts\setup.ps1` | `bash scripts/setup.sh` |
| Every day | `powershell -ExecutionPolicy Bypass -File scripts\start.ps1` | `bash scripts/start.sh` |

Or by hand, once the tools are installed:

```bash
# backend — first time: python -m venv .venv, activate it, pip install -r requirements.txt, copy .env.example to .env and fill the two keys
docker compose up -d postgres redis
python -m alembic upgrade head
python -m seeds.seed
python -m uvicorn app.main:app --reload --port 8000                              # Terminal 1
python -m celery -A app.workers.celery_app worker --loglevel=info --pool=solo    # Terminal 2 (drop --pool=solo on macOS/Linux)
python -m celery -A app.workers.celery_app beat --loglevel=info                  # Terminal 3

# frontend — first time: npm ci
npm run dev                                                                      # Terminal 4
```

Open http://localhost:5173 and sign in with the demo account `demo@example.com` / `DemoPassword123!` (local development only).
