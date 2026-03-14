# Identity

> Project identity, stakeholders, and positioning.

## Project
- **Name:** Acme Dashboard
- **Purpose:** Internal analytics dashboard for the sales team
- **Users:** 12 account executives, 3 managers

## Stakeholders
- **Owner:** [name] - makes final decisions on features
- **Lead dev:** [name] - code review authority
- **Design:** [name] - provides mockups in Figma

## Positioning
- Internal tool, not customer-facing
- Speed over polish - ship fast, iterate based on feedback
- Data accuracy is non-negotiable - wrong numbers erode trust

## Key Decisions
- Chose Next.js over Remix (team familiarity, existing patterns)
- PostgreSQL over MongoDB (relational queries dominate the use case)
- No external auth provider - internal SSO handles everything
