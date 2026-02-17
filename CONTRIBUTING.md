# Contributing

## Getting Started

Follow the setup instructions in the README to get the project running locally.

## Code Style

- Avoid inline comments unless absolutely necessary
- Run `bun format` before submitting changes
- Follow existing code patterns and conventions
- Use TypeScript types properly - avoid `any`
- Ensure `bun lint` passes without errors

## Development Workflow

1. Create a branch from the main branch
2. Make your changes
3. Run `bun lint` and `bun format`
4. Run type checking with `bun typecheck` (if available)
5. Perform self-review (see Pull Request Guidelines)
6. Commit your changes
7. Push and create a pull request

## Database Changes

- Create migrations for any schema changes using Prisma
- Run `bun db:generate` after schema changes
- Test migrations locally before submitting
- Include migration files in your PR

## Pre-submission Checks

Before submitting a pull request, ensure:

- Code is formatted with `bun format`
- Linting passes with `bun lint`
- Type checking passes (if applicable)
- All tests pass (if applicable)
- Database migrations are included and tested

## Pull Request Guidelines

### Self Review

Before creating a pull request, perform a thorough self-review:

1. Review your own code changes line by line
2. Test all functionality locally
3. Verify edge cases and error handling
4. Check for unused imports or dead code
5. Ensure consistent code style with the rest of the codebase
6. Verify that all pre-submission checks pass

### PR Description

Draft a well-formatted pull request with:

- Clear title describing the change
- Summary of what was changed and why
- Reference related issues (e.g., "Fixes #123")
- List of changes made
- Testing steps for reviewers
- Any breaking changes or migration steps required

### General Guidelines

- Keep PRs focused and reasonably sized
- Respond to review feedback promptly
- Update PR description if changes are made during review

## Environment Variables

- Never commit `.env` files
- Use `.env.example` files as templates
- Document any new required environment variables in your PR
