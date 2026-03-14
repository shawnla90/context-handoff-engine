# Infrastructure

> Models, services, paths, deployment, and runtime details.

## Models in Use
- Claude Code (Opus) - primary development agent
- GPT-4o - fallback for specific API integrations
- Local Ollama (Llama 3) - high-frequency scripting, free tier

## Key Paths
- **App:** `src/app/` (Next.js app router)
- **API routes:** `src/app/api/`
- **Database:** `prisma/schema.prisma`
- **Tests:** `__tests__/`
- **Scripts:** `scripts/`

## Services
- **Database:** PostgreSQL on Supabase (connection string in `.env`)
- **Hosting:** Vercel (auto-deploy on push to main)
- **CI:** GitHub Actions (`.github/workflows/ci.yml`)
- **Monitoring:** PostHog for analytics, Sentry for errors

## Deployment
- Push to `main` triggers Vercel auto-deploy
- Preview deployments on PRs
- Database migrations run via `npx prisma migrate deploy` in CI

## Cron Jobs
- `scripts/sync_data.py` - runs daily at midnight via GitHub Actions
- `scripts/cleanup.py` - runs weekly, removes stale sessions
