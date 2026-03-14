# Completed Work

> Archive of finished initiatives and key decisions. Reference when someone asks "why did we do X?"

## 2026-03-10: Dashboard v2 Redesign
- Rebuilt the main dashboard with card-based layout
- Moved from server-side rendering to client-side with SWR for real-time updates
- Decision: kept chart.js instead of switching to d3 (simpler API, team knows it)

## 2026-03-05: API Rate Limiting
- Added express-rate-limit to all API routes
- 100 requests per 15 minutes per user
- Decision: per-user, not per-IP (users share office IPs)

## 2026-02-28: Auth Migration
- Migrated from custom JWT to internal SSO
- Removed all local password storage
- Decision: no fallback auth - SSO is the only path. Simpler security surface.
