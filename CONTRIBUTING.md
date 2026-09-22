# Contributing to TracePath js-client

Thanks for your interest in contributing to the TracePath JavaScript SDKs!

> **Note:** the SDK source of truth lives in the [`sdks/js` directory](https://github.com/bsisduck/tracepath/tree/main/sdks/js) of the [TracePath monorepo](https://github.com/bsisduck/tracepath). Substantial SDK changes should land there first; this repo mirrors the sources, holds the release CI, and is the npm storefront. Docs, typo and packaging fixes are welcome directly here.

## Development setup

Requires Node.js ≥ 18 and npm ≥ 9.

```bash
npm install          # npm workspaces — provisions every package
npm run build        # tsup builds each package to dist/ (ESM + CJS + .d.ts)
npm test             # vitest across all packages
```

Tests import from `dist/`, so run a build before testing your changes.

## Branch protection

`main` is protected: **all changes go through pull requests**. Direct pushes are not merged. CI (build + test) must pass before merge.

## Commit style

Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`, …) keep the history release-friendly.

## Publishing

Releases are owner-driven only (npm names are irreversible). The `release` workflow is gated behind the `release` GitHub environment (manual approval) and requires the `NPM_TOKEN` secret. Do not publish from a local machine for any package under the `@tracepath` scope. See [issue #10](https://github.com/bsisduck/tracepath/issues/10) for the publish rollout.

## License

By contributing you agree that your contributions are licensed under the MIT License of this repository.
