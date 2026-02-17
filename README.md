# template-turbo

A Turborepo template with Next.js, shadcn/ui, Tailwind CSS v4, Prisma + PostgreSQL, and better-auth.

## Tech Stack

- **Runtime**: Bun
- **Monorepo**: Turborepo
- **Framework**: Next.js 15
- **UI**: shadcn/ui (Radix + Lyra style), Tailwind CSS v4
- **Database**: PostgreSQL with Prisma ORM
- **Auth**: better-auth
- **Payments**: Dodo Payments

## Project Structure

```
apps/
  web/             Next.js application
packages/
  ui/              Shared UI components (shadcn/ui, Tailwind CSS)
  database/        Prisma schema and database client
  config/          Shared app configuration
  dodopayments/    Dodo Payments integration
  eslint-config/   Shared ESLint configuration
  typescript-config/ Shared TypeScript configuration
```

## Getting Started

### Prerequisites

- [Bun](https://bun.sh) >= 1.0
- [Node.js](https://nodejs.org) >= 20
- [Docker](https://www.docker.com) (for PostgreSQL)

### Setup

```bash
# Install dependencies
bun install

# Start PostgreSQL
docker run --name postgres -e POSTGRES_PASSWORD=mysecretpassword \
  -p 5432:5432 -d postgres:latest

# Create env file
cp .env.example .env

# Generate Prisma client and run migrations
bun db:generate
bun db:migrate

# Start development server
bun dev
```

## Scripts

| Command           | Description                        |
| ----------------- | ---------------------------------- |
| `bun dev`         | Start all apps in development mode |
| `bun build`       | Build all apps and packages        |
| `bun lint`        | Lint all apps and packages         |
| `bun format`      | Format code with Prettier          |
| `bun db:generate` | Generate Prisma client             |
| `bun db:migrate`  | Run database migrations            |
| `bun db:studio`   | Open Prisma Studio                 |

## Adding shadcn Components

```bash
cd packages/ui
bunx shadcn@latest add button
```

## Environment Variables

Copy `.env.example` to `.env` and fill in the values:

| Variable                       | Description                  |
| ------------------------------ | ---------------------------- |
| `DATABASE_URL`                 | PostgreSQL connection string |
| `BETTER_AUTH_SECRET`           | Secret for better-auth       |
| `BETTER_AUTH_URL`              | Base URL of your app         |
| `GOOGLE_CLIENT_ID`             | Google OAuth client ID       |
| `GOOGLE_CLIENT_SECRET`         | Google OAuth client secret   |
| `GITHUB_CLIENT_ID`             | GitHub OAuth client ID       |
| `GITHUB_CLIENT_SECRET`         | GitHub OAuth client secret   |
| `DODO_PAYMENTS_API_KEY`        | Dodo Payments API key        |
| `DODO_PAYMENTS_WEBHOOK_SECRET` | Dodo Payments webhook secret |
| `NEXT_PUBLIC_APP_URL`          | Public app URL               |
