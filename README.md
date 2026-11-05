# OmniPost

B2B social media marketing platform — one workspace where a business or agency manages its brands, creates content, runs approvals, and publishes to Facebook, Instagram and LinkedIn.

This repository currently contains the **backend**. The web app (frontend) will be added once it is ready for review.

| Folder | What's inside |
|---|---|
| [`backend/`](backend/) | FastAPI · PostgreSQL 16 · Celery/Redis · S3-compatible storage — API, database migrations, background workers, tests |
| [`docs/`](docs/) | Project documentation: approach, database, backend functionality, progress and roadmap |

**Full documentation:** [docs/README.md](docs/README.md) · **Run the backend locally:** [backend/README.md](backend/README.md)

---

## Project status — 5 November 2026

| Area | Status |
|---|---|
| Backend (Phase 1) | ✅ Done |
| Database (13 tables) | ✅ Done |
| Automated tests (59) | ✅ Written — 52 unit tests pass; 7 database tests run when PostgreSQL is up |
| Web app (frontend) | 🟡 In progress |
| Real Facebook / Instagram publishing check | 🟡 Waiting on Meta developer app access |
| Hosted review environment | ⏳ Not started |
| AI content generation | ⏳ Phase 2 |

---

## ✅ Done so far

**Backend — complete for Phase 1**

- **Sign-in and sessions** — register, login, logout, secure auto-refresh of the session, rate limiting on login.
- **Workspaces and team** — workspaces with owner / admin / member roles, add members, workspace settings, usage summary.
- **Brands** — brand profiles (industry, audience, tone of voice, language, guidelines) and logo upload.
- **Smart Templates** — 16 ready-made post templates with placeholders, suggested hashtags and call-to-action (the Phase 1 stand-in for AI).
- **Posts** — caption, hashtags, call-to-action, separate wording per platform, pre-publish validation with character limits and preview text.
- **Media** — image and video upload with file-type checks, size limits, removal of hidden location data, and carousel ordering.
- **Approval workflow** — submit, approve, reject with reason, withdraw; posts are locked while under review; every approval is tied to a fingerprint of the exact content, so nothing can change after approval without going back to draft.
- **Scheduling and publishing** — schedule or publish now to several accounts at once, per-account status, automatic retry of temporary failures, safe manual retry, protection against duplicate posts, content calendar.
- **Social accounts** — connect Facebook pages and Instagram business accounts through Meta login; mock accounts for demos and testing; warnings before access expires.
- **Security and records** — strict separation between customers' workspaces, encrypted social media credentials, full audit trail of every action, usage logging for future billing.
- **Database** — 13-table PostgreSQL schema with versioned migrations and built-in safety rules.
- **Testing** — 59 automated tests, including end-to-end tests of the full create → approve → schedule → publish journey.
- **Demo data** — one command sets up a demo agency with brands, accounts and posts in every stage.

Details: [docs/03_backend_functionality.md](docs/03_backend_functionality.md) · [docs/02_database.md](docs/02_database.md)

---

## 🟡 In progress

- **Web app (React + TypeScript).** Screens built so far: login and registration, brands, Smart Templates, posts list, Post Studio (create and edit), approval panel, publish panel with status and retry, content calendar, connected accounts and settings. Next: finishing the remaining screens, connecting every flow end to end, mobile layout and polish.
- **Real publishing check.** Proving live Facebook and Instagram publishing with a Meta developer app and a test Page / Instagram business account (needs the client's Meta Business access).
- **LinkedIn decision.** The LinkedIn publishing code is built for text posts; the connect flow (about 1.5 days) will be added if the client's LinkedIn developer app is approved. Until then LinkedIn works in demo mode only.

---

## 🔭 Future plan

**Finish Phase 1 (MVP)**

- Hosted review environment (recommended: Railway or Render + Cloudflare R2 for media) so stakeholders can try the full journey themselves.
- End-to-end test pass on the hosted environment, demo script, and a known-limitations document.
- MVP review and sign-off.

**Phase 2 and later**

- **AI content** — generate captions, hashtags, calls-to-action and post ideas from the brand profile; AI image generation; usage credits. AI output always goes through the same approval workflow.
- **Richer publishing** — Instagram carousels, Reels and Stories; LinkedIn images; video.
- **Team administration** — email invitations, remove members, change roles, password reset, profile editing.
- **Workflow** — multi-level approvals, bulk scheduling, comments on posts.
- **Insights** — post performance and calendar analytics.
- **Billing** — plans and credits built on the existing usage log.
- **Production readiness** — backups, monitoring and alerting, load testing, hardened infrastructure.

Full roadmap and known limitations: [docs/04_progress_and_roadmap.md](docs/04_progress_and_roadmap.md)
