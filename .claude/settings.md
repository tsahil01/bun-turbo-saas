# template-turbo

Turborepo monorepo template using Bun, Next.js 15, shadcn/ui, Tailwind CSS v4, Prisma + PostgreSQL, better-auth, and Dodo Payments.

## Architecture

```
apps/
  web/                 Next.js 15 app (App Router, Turbopack)
packages/
  auth/                better-auth server + client config
  config/              Shared app metadata
  database/            Prisma ORM + PostgreSQL (Prisma Accelerate)
  dodopayments/        Dodo Payments integration (placeholder)
  eslint-config/       Shared ESLint flat configs (base, next-js, react-internal)
  typescript-config/   Shared TypeScript configs (base, nextjs, react-library)
  ui/                  shadcn/ui component library (Tailwind CSS v4, Radix, CVA)
  zod-validator/       Shared Zod validation schemas
```

## Key Versions

- **Runtime**: Bun 1.3+
- **Next.js**: 15.5.12 (App Router, Turbopack)
- **React**: 19.2.4
- **TypeScript**: 5.9.3
- **Tailwind CSS**: 4.1.18
- **Prisma**: 7.4.0
- **better-auth**: 1.4.18
- **Turbo**: 2.8.9
- **ESLint**: 9.39.2

## Workspace Packages

### `@workspace/auth` (`packages/auth`)

Shared authentication. Two entry points:

- `@workspace/auth/server` — `betterAuth()` instance configured with Prisma adapter (PostgreSQL), email/password auth, Google + GitHub OAuth, `nextCookies()` plugin. Exports `auth`, `handlers` (Next.js route handler), and `Session` type.
- `@workspace/auth/client` — `createAuthClient()` from `better-auth/react`. Exports `authClient`, `signIn`, `signUp`, `signOut`, `useSession`.

Dependencies: `better-auth`, `@workspace/db`.

### `@workspace/config` (`packages/config`)

App metadata. Exports `appConfig` object and `AppConfig` type with fields: appName, appDescription, appUrl, githubUrl, twitterUrl, authorName, authorEmail, authorUrl, authorTwitterUrl, authorGithubUrl.

No external dependencies.

### `@workspace/db` (`packages/database`)

Prisma ORM setup. Exports a singleton `prisma` client instance (with Prisma Accelerate extension) and all generated Prisma types.

- Schema: `packages/database/prisma/schema.prisma` (PostgreSQL, output to `../generated/prisma`)
- Config: `packages/database/prisma.config.ts`
- Client singleton: `packages/database/src/client.ts` (uses global object pattern for dev hot-reload)
- Loads env from root `.env` via `dotenv`

Scripts: `db:generate`, `db:migrate`, `db:deploy`, `db:studio`.

Dependencies: `@prisma/client`, `@prisma/client-runtime-utils`, `@prisma/extension-accelerate`, `dotenv`. Dev: `prisma`.

### `@workspace/dodopayments` (`packages/dodopayments`)

Placeholder for Dodo Payments SDK integration. Currently exports a simple config object.

### `@workspace/eslint-config` (`packages/eslint-config`)

Three ESLint flat configs:

- `@workspace/eslint-config/base` — Base config with `@eslint/js`, Prettier, TypeScript-ESLint, Turbo plugin (`no-undeclared-env-vars`), `only-warn` plugin.
- `@workspace/eslint-config/next-js` — Extends base. Adds `@next/eslint-plugin-next`, React, React Hooks plugins.
- `@workspace/eslint-config/react-internal` — Extends base. Adds React flat recommended + React Hooks. For internal packages like `@workspace/ui`.

### `@workspace/typescript-config` (`packages/typescript-config`)

Three TypeScript configs (consumed via `extends`):

- `base.json` — Target ES2022, strict, declarationMap, NodeNext module resolution.
- `nextjs.json` — Extends base. Bundler resolution, JSX preserve, `next` plugin, allowJs, noEmit.
- `react-library.json` — Extends base. JSX react-jsx.

### `@workspace/ui` (`packages/ui`)

shadcn/ui component library with Tailwind CSS v4.

Exports (via package.json `exports` field):

- `@workspace/ui/styles/globals.css` — Global CSS with Tailwind v4, oklch design tokens (light/dark), JetBrains Mono font, `tw-animate-css`.
- `@workspace/ui/lib/*` — Utilities (e.g., `cn()` function using `clsx` + `tailwind-merge`).
- `@workspace/ui/components/*` — React components (e.g., `button.tsx` with CVA variants).
- `@workspace/ui/hooks/*` — Hooks (e.g., `useIsMobile()`).
- `@workspace/ui/postcss.config` — PostCSS config with `@tailwindcss/postcss`.

shadcn/ui theme config: Radix base, Lyra style, zinc base color, emerald primary, phosphor icons, JetBrains Mono font, radius 0.

Dependencies: `class-variance-authority`, `clsx`, `lucide-react`, `motion`, `next-themes`, `radix-ui`, `react`, `react-dom`, `sonner`, `tailwind-merge`, `tw-animate-css`. Dev: `tailwindcss`, `@tailwindcss/postcss`, `@turbo/gen`.

### `@workspace/zod-validator` (`packages/zod-validator`)

Shared Zod validation schemas. Two entry points:

- `@workspace/zod-validator` (or `@workspace/zod-validator/index`) — Re-exports all schemas.
- `@workspace/zod-validator/auth` — `signUpSchema` (name, email, password min 8), `signInSchema` (email, password). Types: `SignUpInput`, `SignInInput`.
- `@workspace/zod-validator/common` — `idSchema` (UUID), `paginationSchema` (page default 1, limit default 20 max 100). Types: `IdInput`, `PaginationInput`.

Dependencies: `zod`.

## apps/web

Next.js 15 application with App Router and Turbopack.

### Routes

- `/` — Home page. Renders a `Button` from `@workspace/ui`.
- `/api/auth/[...all]` — Catch-all auth API route. Delegates to `handlers` from `@workspace/auth/server`.

### Configuration

- `components.json` — shadcn/ui config. Style: lyra, base: radix, base color: zinc, icon library: phosphor. CSS source: `@workspace/ui/styles/globals.css`. Utils alias: `@workspace/ui/lib/utils`.
- `next.config.ts` — Default/empty NextConfig.
- `tsconfig.json` — Standalone config (ES2017 target, Bundler resolution, `@/*` -> `./src/*` path alias).
- `eslint.config.mjs` — Uses `@eslint/eslintrc` FlatCompat to extend `next/core-web-vitals` and `next/typescript`.
- No local `postcss.config` — relies on `@workspace/ui/postcss.config`.
- No local `globals.css` — imports from `@workspace/ui/styles/globals.css` in root layout.
- `src/lib/utils.ts` — Re-exports `cn` from `@workspace/ui/lib/utils`.

### Workspace Dependencies

dependencies: `@workspace/auth`, `@workspace/config`, `@workspace/db`, `@workspace/dodopayments`, `@workspace/ui`, `dotenv`, `next`, `react`, `react-dom`.
devDependencies: `@workspace/eslint-config`, `@workspace/typescript-config`, `eslint`, `eslint-config-next`, `typescript`, plus eslint plugins.

## Root Configuration

- `package.json` — Name: `template-turbo`. Workspaces: `apps/*`, `packages/*`. Scripts: `build`, `dev`, `lint`, `format`, `format:check`, `db:generate`, `db:migrate`, `db:studio`. Engine: Node >= 20.
- `turbo.json` — Build depends on `^build` + `db:generate`. Caches `.next/**`. Env-aware for: `DATABASE_URL`, `BETTER_AUTH_SECRET`, `BETTER_AUTH_URL`, `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET`, `DODO_PAYMENTS_API_KEY`, `DODO_PAYMENTS_WEBHOOK_SECRET`, `NEXT_PUBLIC_APP_URL`.
- `tsconfig.json` — Extends `@workspace/typescript-config/base.json`.
- `.eslintrc.js` — Root-only config extending `@workspace/eslint-config/library.js`.
- `.prettierrc.json` — Plugin: `prettier-plugin-tailwindcss`.
- `.vscode/settings.json` — `tailwindCSS.experimental.configFile` pointing to `packages/ui/src/styles/globals.css`.
- `.env.example` — Template env vars: `DATABASE_URL`, `BETTER_AUTH_SECRET`, `BETTER_AUTH_URL`, `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`, `GITHUB_CLIENT_ID`, `GITHUB_CLIENT_SECRET`, `DODO_PAYMENTS_API_KEY`, `DODO_PAYMENTS_WEBHOOK_SECRET`, `NEXT_PUBLIC_APP_URL`.

## Common Commands

```bash
bun install          # Install all dependencies
bun dev              # Start dev server (all apps)
bun build            # Build all apps and packages
bun lint             # Lint all apps and packages
bun format           # Format code with Prettier
bun db:generate      # Generate Prisma client
bun db:migrate       # Run database migrations
bun db:studio        # Open Prisma Studio
```

## Adding shadcn Components

```bash
cd packages/ui
bunx shadcn@latest add button
```

Components are installed into `packages/ui/src/components/` based on the `components.json` aliases.

## File Conventions

- Package manager: **Bun** (not pnpm/npm/yarn)
- All workspace packages use `workspace:*` protocol
- CSS: Single global stylesheet in `packages/ui/src/styles/globals.css` (Tailwind v4, oklch tokens)
- No local CSS files in apps — import from `@workspace/ui/styles/globals.css`
- Prisma generated client is gitignored — always run `bun db:generate` after clone
- Environment variables loaded from root `.env` file
