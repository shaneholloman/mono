# `mono`

Reference: <https://ui.shadcn.com/docs/monorepo.md>

shadcn/ui monorepo template

This template is for creating a monorepo with shadcn/ui.

## Commands I ran

> Note that this will install a pre-release. In other words, we won't be using `latest`, so you'll get the pre-release version of shadcn with the latest versions of React and Tailwind for our bootstrapped app. So essentially you could just trash this whole repo and run these 5 commands, and you'll be basically back to the same state as this app is right now.

```sh
pnpm dlx shadcn@canary init # I called the app `mono`
pnpm dlx shadcn@canary add --all -c apps/web
pnpm install
pnpm build
pnpm dev
```

## Installation & Setup

1. **Install dependencies** (from monorepo root):

    ```sh
    pnpm install
    ```

2. **Build the project** (from monorepo root):

    ```sh
    pnpm build
    ```

3. **Start development server** (from monorepo root):

    ```sh
    pnpm dev
    ```

The app will be available at `http://localhost:3000`

Browser will display the default Next.js page:

![screenshot](assets/screenshot.png)

## Usage

This template is set up for the pre-release (canary) of `shadcn/ui` compatible with React 19. Use the canary tag for consistency with this repo:

```sh
pnpm dlx shadcn@canary init
```

## Adding components

To add components to your app, run the following command at the root of your `web` app:

```sh
pnpm dlx shadcn@canary add button -c apps/web
```

This will place the ui components in the `packages/ui/src/components` directory.

## Tailwind

Your `tailwind.config.ts` and `globals.css` are already set up to use the components from the `ui` package.

## Using components

To use the components in your app, import them from the `ui` package.

```tsx
import { Button } from "@workspace/ui/components/button"
```

## Monorepo scripts

- Build all: `pnpm build`
- Dev server: `pnpm dev` (Next.js app under `apps/web`)
- Lint all packages: `pnpm lint`
- Lint one package: `pnpm turbo run lint --filter <package_name>`
- Type-check all packages: `pnpm typecheck`
- Type-check one package: `pnpm turbo run typecheck --filter <package_name>`

## Continuous Integration

- GitHub Actions workflow runs lint and typecheck on pushes and PRs to `main`.
- File: `.github/workflows/ci.yml`

## Dependency Updates

- Automated by Renovate using `renovate.json` at repo root.
- Enable the Renovate GitHub App on this repo (or org).
- Renovate opens PRs; existing CI runs on them. Minor/patch devDependencies may auto‑merge if checks pass.

Setup Steps

- Visit <https://github.com/apps/renovate> and click Install.
- Choose your personal account (top-left selector), select “Only select repositories,” pick this repo, and confirm.
- Renovate reads `renovate.json` and opens PRs; your existing CI validates them.
- First-time users may be asked to register and then redirected to the Mend developer portal (for example: <https://developer.mend.io/github/{{Username}}>).

## Linting

- ESLint 9 flat config is used per package:
  - `apps/web/eslint.config.js` extends `@workspace/eslint-config/next-js`
  - `packages/ui/eslint.config.js` extends `@workspace/eslint-config/react-internal`
- Root has `eslint.config.js` that ignores all files to avoid accidental root linting.
- Shared config lives in `packages/eslint-config`.

## Adding apps and packages

- New Next.js app under `apps/`:
  - `pnpm dlx create-next-app@latest apps/<app_name> --ts`
  - Add `lint` and `typecheck` scripts similar to `apps/web/package.json`.
- New internal library under `packages/`:
  - Create `packages/<lib_name>` and reuse shared configs (`@workspace/typescript-config`, `@workspace/eslint-config`).
  - Add a `lint` script: `eslint . --max-warnings 0`.

## Testing

Tests are not set up yet in this template. Recommended next steps:

- Libraries (`packages/*`): add Vitest and a `test` script; wire a `test` task in `turbo.json`.
- App (`apps/web`): consider Playwright or component tests.
- For now, rely on `pnpm lint` and TypeScript checks.

## PR checklist

- Title format: `[<package_name>] <Title>`
- Before merging:
  - Lint all or targeted package: `pnpm lint` or `pnpm turbo run lint --filter <package_name>`
  - Type-check the app (and any package with a script): `pnpm --filter web run typecheck`
  - When tests exist: `pnpm turbo run test --filter <package_name>`
