# eventcatalog pnpm workspace test

This repository tests eventcatalog with automatic package manager detection in a pnpm workspace.
It validates the changes from [PR #1957](https://www.github.com/event-catalog/eventcatalog/pull/1957) which replaces hardcoded `npx` with detected package executor commands.

## Prerequisites

- Node.js 18+ (recommended: use the version specified in `.nvmrc` if present)
- pnpm 9.x (`corepack enable && corepack prepare pnpm@9.15.4 --activate`)

## Repository structure

```
test-eventcatalog-pnpm/
├── package.json              # Root workspace configuration
├── pnpm-workspace.yaml       # pnpm workspace definition
└── packages/
    └── eventcatalog/         # EventCatalog instance
        ├── package.json      # References @eventcatalog/core from GitHub release
        ├── eventcatalog.config.js
        └── ...
```

## Workflow commands

All commands should be run from the repository root.

### Clean previous artifacts

Remove node_modules, build artifacts, and cached files before a fresh install.

```bash
rm -rf node_modules packages/eventcatalog/node_modules packages/eventcatalog/.eventcatalog-core packages/eventcatalog/dist
```

### Install dependencies

```bash
pnpm install
```

This fetches `@eventcatalog/core` from the GitHub release tarball specified in `packages/eventcatalog/package.json`.

### Development server

Start the development server with hot reload.

```bash
pnpm --filter eventcatalog dev
```

Or using the root script alias:

```bash
pnpm dev
```

### Build for production

Generate a static production build.

```bash
pnpm --filter eventcatalog build
```

Or using the root script alias:

```bash
pnpm build
```

### Preview production build

Serve the production build locally to verify it works correctly.

```bash
pnpm --filter eventcatalog preview
```

## Troubleshooting

If the build fails, verify that the GitHub release tarball is accessible:

```bash
curl -I https://github.com/cameronraysmith/eventcatalog/releases/download/v3.2.2-e475461b/eventcatalog-core-3.2.2.tgz
```

If you see connection issues or 404 errors, the release may have been removed or the URL may have changed.
