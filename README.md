### Make sure to create a `.env` file with following variables -

```markdown
# Career Pilot — AI Career Coach

Lightweight Next.js application that helps users build resumes, generate cover letters, practice interview questions, and receive weekly industry insights using AI.

## Quick overview

- Framework: Next.js (App Router)
- UI: Tailwind CSS + shadcn-style components
- Auth: Clerk
- Database: Prisma (configured via `DATABASE_URL`)
- Background jobs / scheduling: Inngest
- AI: Gemini / Google Generative AI (via `GEMINI_API_KEY`)

This repository contains the frontend and server actions for an AI-powered career assistant called Career Pilot.

## Quick start (development)

1. Install dependencies

```powershell
npm install
```

2. Create a `.env` in the project root and set the required environment variables (example below).

3. Run the dev server

```powershell
npm run dev
```

4. Open http://localhost:3000

Build for production

```powershell
npm run build
npm run start
```

## Environment variables

Create a `.env` file in the project root and set at minimum the following:

```env
DATABASE_URL=postgresql://user:pass@host:5432/dbname

NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=
CLERK_SECRET_KEY=

NEXT_PUBLIC_CLERK_SIGN_IN_URL=/sign-in
NEXT_PUBLIC_CLERK_SIGN_UP_URL=/sign-up
NEXT_PUBLIC_CLERK_AFTER_SIGN_IN_URL=/onboarding
NEXT_PUBLIC_CLERK_AFTER_SIGN_UP_URL=/onboarding

GEMINI_API_KEY=
```

Notes:
- `DATABASE_URL` is used by Prisma. Run Prisma migrations after setting it up.
- Clerk keys are required for authentication to function.
- `GEMINI_API_KEY` (or whichever provider key you use) enables AI generation in server actions.

## Features

- Resume builder and editor with AI improvement suggestions
- Cover letter generation, preview and management
- Interview quizzes and performance tracking with charts
- Weekly industry insights generated via an Inngest scheduled function
- Authentication and user onboarding powered by Clerk

## Project structure (high level)

- `app/` — Next.js App Router pages, layouts and route handlers
	- `app/(main)/resume` — resume builder UI and components
	- `app/(main)/ai-cover-letter` — cover letter generator UI and preview
	- `app/api/inngest/route.js` — API route used by Inngest
- `actions/` — server-side actionable functions (resume, cover-letter, dashboard, interview, user)
- `components/` — shared React UI components and `components/ui` primitives
- `lib/` — helpers, Prisma client, Inngest client and functions
- `hooks/` — client hooks (e.g. `use-fetch.js`)
- `prisma/` — Prisma schema and migrations

Key files to check when contributing or debugging:

- `app/layout.js` — app root layout and providers
- `actions/resume.js` — server actions for resume CRUD and AI improvements
- `actions/cover-letter.js` — cover letter generation and management
- `lib/inngest/function.js` — scheduled functions that generate industry insights
- `lib/inngest/client.js` — Inngest client setup
- `lib/prisma.js` — Prisma client wrapper

## How AI is used

- Server actions in `actions/` call the AI model using the `GEMINI_API_KEY` environment variable.
- Cover letters, resume improvements, and dashboard insights are produced server-side and stored in the database.

## Database & Migrations

This project uses Prisma. After creating and setting `DATABASE_URL`, run:

```powershell
npx prisma migrate deploy
# or for development:
npx prisma migrate dev
```

## Deployment

- Deploy to Vercel (recommended) or another Next.js-friendly host.
- Make sure to set environment variables in the hosting platform (Clerk keys, `DATABASE_URL`, `GEMINI_API_KEY`, etc.).

## Tests and quality checks

There are currently no automated tests in the repo (additions welcome). Before opening a PR:

- Run the dev server and smoke-test core flows (auth, resume, cover letter, quiz)
- Run formatting/linting if applicable (project uses ESLint and Tailwind — see `eslint.config.mjs`)

## Contributing

1. Fork and branch from `master`.
2. Follow the existing code style in `components/ui` and `app/`.
3. Add tests for new behavior where feasible.
4. Open a PR with a clear description and screenshots if UI changes.

## Troubleshooting

- Authentication errors: verify Clerk keys and redirect URLs in `.env`.
- AI generation failures: confirm `GEMINI_API_KEY` is valid and has quota.
- Database issues: ensure `DATABASE_URL` points to a reachable DB and migrations were applied.

## License

MIT

## Acknowledgements

Built with Next.js, Prisma, Clerk, Inngest and Tailwind. UI patterns inspired by shadcn/ui.

