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

## CI

- GitHub Actions workflow runs on pushes and PRs to `main`:
  - Location: `.github/workflows/ci.yml`
  - Steps: install, lint, typecheck

## CI Learnings

- Order matters: set up Node, then pnpm.
  - Use Corepack to activate the pnpm version from `package.json` (`packageManager`).
  - Example: `corepack enable && corepack prepare pnpm@10.4.1 --activate`.
- Install from the repo root: `pnpm install --frozen-lockfile`.
  - This installs all workspace packages so package-level bins (like `eslint`) exist.
- Local bins only: any package running a tool in its scripts must declare it.
  - Example: `packages/ui` runs `eslint`, so it declares `devDependencies.eslint`.
- Keep lockfile in sync: commit `pnpm-lock.yaml` whenever deps change or CI with `--frozen-lockfile` will fail.
- Minimal, reliable workflow (no caching needed):

  ```yaml
  name: CI
  on:
    push: { branches: [ main ] }
    pull_request: { branches: [ main ] }
  jobs:
    build:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-node@v4
          with: { node-version: '20' }
        - name: Setup pnpm
          run: |
            corepack enable
            corepack prepare pnpm@10.4.1 --activate
        - run: pnpm install --frozen-lockfile
        - run: pnpm lint
        - run: pnpm typecheck
  ```

- Optional caching: cache the pnpm store to speed up installs; it’s safe to skip for simplicity.

## PR instructions

- Title format: `[<package_name>] <Title>`
- Before merging:
  - Run lint: `pnpm lint` (or `pnpm turbo run lint --filter <package_name>`)
  - Type-check all: `pnpm typecheck`
  - If/when tests are added: `pnpm turbo run test --filter <package_name>`
