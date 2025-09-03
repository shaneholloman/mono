# Sample AGENTS.md file

## Dev environment tips

- Locate a package path: `pnpm -w --filter <package_name> exec pwd`
- Bootstrap deps for one package: `pnpm install --filter <package_name>` (root install with `pnpm -w install` is usually enough)
- New Next.js app (under `apps/`): `pnpm dlx create-next-app@latest apps/<app_name> --ts`
- New internal library (under `packages/`): create `packages/<lib_name>` and reuse shared configs (`@workspace/typescript-config`, `@workspace/eslint-config`).
- Verify package names via each package's `package.json` `name` field (ignore the root one).

## Testing instructions

- Tests are not set up yet in this template. Recommended next steps:
  - Packages: add Vitest and a `test` script, then wire `turbo.json` with a `test` task.
  - App (`apps/web`): consider Playwright or component tests as needed.
- Current checks you can run now:
  - Lint all: `pnpm lint` (runs Turbo across packages)
  - Lint one package: `pnpm turbo run lint --filter <package_name>`
  - Type-check all packages: `pnpm typecheck`
  - Type-check one package: `pnpm turbo run typecheck --filter <package_name>`

## PR instructions

- Title format: `[<package_name>] <Title>`
- Before merging:
  - Run lint: `pnpm lint` (or `pnpm turbo run lint --filter <package_name>`)
  - Type-check all: `pnpm typecheck`
  - If/when tests are added: `pnpm turbo run test --filter <package_name>`
