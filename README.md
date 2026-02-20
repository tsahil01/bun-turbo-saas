# Next.js 15 Turbo Starter

A production-ready Turborepo starter template with Next.js 15, shadcn/ui, Tailwind CSS v4, Prisma, and better-auth.

## Features

- **Turborepo** - Monorepo with efficient caching and parallel execution
- **Next.js 15** - App Router, React 19, Turbopack
- **shadcn/ui** - Accessible components with Radix primitives
- **Tailwind CSS v4** - Next-gen CSS with performance optimizations
- **Prisma** - Type-safe ORM with PostgreSQL
- **better-auth** - Modern authentication with OAuth support
- **Dodo Payments** - Payment integration ready

## Quick Start

```bash
# Clone the template
gh repo create my-app --template yourusername/bun-turbo-saas
cd my-app

# Install dependencies
bun install

# Start PostgreSQL (or use any hosted DB)
docker run --name postgres -e POSTGRES_PASSWORD=mysecretpassword \
  -p 5432:5432 -d postgres:latest

# Setup environment
cp .env.example .env

# Generate Prisma client and migrate
bun db:generate
bun db:migrate

# Start development
bun dev
```

## Tech Stack

| Category  | Technology                 |
| --------- | -------------------------- |
| Runtime   | Bun                        |
| Monorepo  | Turborepo                  |
| Framework | Next.js 15                 |
| UI        | shadcn/ui, Tailwind CSS v4 |
| Database  | PostgreSQL, Prisma         |
| Auth      | better-auth                |
| Payments  | Dodo Payments              |

## Project Structure

```
apps/
  web/              Next.js application
packages/
  ui/               Shared UI components
  database/         Prisma schema and client
  auth/             better-auth configuration
  dodopayments/     Payment integration
  config/           Shared configuration
  eslint-config/    ESLint config
  typescript-config/ TS config
```

## Available Scripts

| Command           | Description            |
| ----------------- | ---------------------- |
| `bun dev`         | Start dev servers      |
| `bun build`       | Build all packages     |
| `bun lint`        | Lint code              |
| `bun format`      | Format with Prettier   |
| `bun db:generate` | Generate Prisma client |
| `bun db:migrate`  | Run migrations         |
| `bun db:studio`   | Open Prisma Studio     |

## Adding Components

```bash
cd packages/ui
bunx shadcn@latest add button
```

## Environment Variables

Copy `.env.example` to `.env` and configure:

| Variable                       | Description                                  | Required |
| ------------------------------ | -------------------------------------------- | -------- |
| `DATABASE_URL`                 | PostgreSQL connection string                 | Yes      |
| `NEXT_PUBLIC_APP_URL`          | Public app URL (e.g., http://localhost:3000) | Yes      |
| `BETTER_AUTH_SECRET`           | Auth secret key (min 32 chars)               | Yes      |
| `BETTER_AUTH_URL`              | Auth base URL                                | Yes      |
| `GOOGLE_CLIENT_ID`             | Google OAuth client ID                       | Optional |
| `GOOGLE_CLIENT_SECRET`         | Google OAuth client secret                   | Optional |
| `GITHUB_CLIENT_ID`             | GitHub OAuth client ID                       | Optional |
| `GITHUB_CLIENT_SECRET`         | GitHub OAuth client secret                   | Optional |
| `DODO_PAYMENTS_API_KEY`        | Dodo Payments API key                        | Optional |
| `DODO_PAYMENTS_WEBHOOK_SECRET` | Dodo Payments webhook secret                 | Optional |
