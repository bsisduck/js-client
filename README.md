<p align="center">
  <a href="https://tracepath.dev"><strong>TracePath</strong></a>
</p>

<p align="center">
  <a href="https://github.com/bsisduck/js-client/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License"></a>
  <a href="https://www.npmjs.com/org/tracepathhq"><img src="https://img.shields.io/badge/npm-%40tracepath-cb3837.svg" alt="npm scope"></a>
  <a href="https://github.com/bsisduck/tracepath/actions"><img src="https://img.shields.io/github/actions/workflow/status/bsisduck/js-client/ci.yml?branch=main" alt="CI"></a>
</p>

# TracePath JavaScript SDKs

Official JavaScript / TypeScript client SDKs for [TracePath](https://tracepath.dev) — an open-source error tracking and observability platform. This repo is the storefront for every TracePath package on npm under the `@tracepath/*` scope: error tracking, session replay, tracing and metrics for browsers, Node.js, React, Vue, Svelte, jQuery and React Native.

[TracePath](https://tracepath.dev) is a completely open-source observability platform. You can [self-host](https://docs.tracepath.dev/server/docker-compose) it or use [TracePath Cloud](https://cloud.tracepath.dev).

> Source of truth: the SDK sources live in the [`sdks/js` directory](https://github.com/bsisduck/tracepath/tree/main/sdks/js) of the main [TracePath](https://github.com/bsisduck/tracepath) monorepo. This repo mirrors that content, carries the release/publish CI, and points to [docs.tracepath.dev](https://docs.tracepath.dev) for usage docs.

> Forked from the upstream Traceway js-client (MIT, © dusanstanojeviccs) and republished under the `@tracepath` npm scope.

## Quickstart (Node.js backend)

```bash
npm install @tracepath/core @tracepath/backend
```

```ts
import { init, captureException, shutdown } from "@tracepath/backend";

init("YOUR_PROJECT_TOKEN"); // reports to https://ingest.tracepath.dev/api/report
process.on("beforeExit", () => shutdown()); // flush on exit

try {
  risky();
} catch (e) {
  captureException(e);
}
```

Or with an explicit self-hosted endpoint: `init("YOUR_PROJECT_TOKEN@https://selfhost.example.com")`.

Environment chain: `TRACEPATH_TOKEN` / `TRACEPATH_URL` (legacy `TRACEWAY_TOKEN` / `TRACEWAY_URL` still accepted).

## Quickstart (browser)

```bash
npm install @tracepath/frontend
```

```ts
import { TracePathFrontendClient } from "@tracepath/frontend";

const client = new TracePathFrontendClient(
  "YOUR_PROJECT_TOKEN@https://ingest.tracepath.dev/api/report"
);
client.start();
```

Then generate a connection string on your TracePath dashboard and pass it to `init(...)` (or the framework provider equivalent). Full per-framework guides: [docs.tracepath.dev](https://docs.tracepath.dev/client).

## Packages

Every package lives in `packages/<name>/` and is published as `@tracepath/<name>` on npm. All active packages move in lockstep — one version, one release.

| Package | Description |
|---|---|
| [`@tracepath/core`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/core) | Shared types, connection-string parsing, defaults |
| [`@tracepath/frontend`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/frontend) | Browser SDK — exceptions, session replay, console/network capture |
| [`@tracepath/backend`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/backend) | Node.js SDK — tracing, exceptions, host metrics (deprecated — prefer OTel) |
| [`@tracepath/react`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/react) | React wrapper — `<TracePathProvider>`, `<TracePathErrorBoundary>`, hooks |
| [`@tracepath/react-native`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/react-native) | React Native SDK for iOS, Android and Expo |
| [`@tracepath/vue`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/vue) | Vue 3 plugin + composable |
| [`@tracepath/svelte`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/svelte) | Svelte integration |
| [`@tracepath/jquery`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/jquery) | jQuery AJAX error capture |
| [`@tracepath/sourcemap-upload`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/sourcemap-upload) | CLI for uploading source maps |
| [`@tracepath/bundler-plugin`](https://github.com/bsisduck/tracepath/tree/main/sdks/js/packages/bundler-plugin) | Vite/Rollup/webpack debug-ID plugins |
| `@tracepath/nestjs` | Deprecated — use OpenTelemetry (kept for compat) |

> Backend integrations should use [OpenTelemetry](https://opentelemetry.io/) exporting OTLP/HTTP to your TracePath instance — see the [OTel guide](https://docs.tracepath.dev/client/otel). The deprecated packages still receive security fixes.

## Ingest endpoint contract

All packages POST gzipped JSON to `<endpoint>/api/report` with `Authorization: Bearer <project token>`. The endpoint defaults to `https://ingest.tracepath.dev` and is overridable per connection string (`token@https://selfhost.example`) or via `TRACEPATH_URL`.

## Repo layout

```
js-client/
├── packages/              # npm workspaces (mirrored from tracepath sdks/js)
├── .github/workflows/
│   ├── ci.yml             # build + test on every push/PR
│   └── release.yml        # npm publish — gated by environment approval + NPM_TOKEN
├── package.json           # npm workspaces root
├── tsconfig.base.json     # Shared TS config inherited by every package
└── vitest.workspace.ts    # Test discovery — globs packages/*/vitest.config.ts
```

## Development

Requires Node.js ≥ 18 (the monorepo pins Node 26 via `.nvmrc`).

```bash
npm install
npm run build   # build all packages (tests import dist/)
npm test        # vitest across all packages
```

## Publishing

Publishing is **not** automated from this repo yet. It happens from the main monorepo (`.github/workflows/sdks-release.yml`, manual dispatch) — see [bsisduck/tracepath#10](https://github.com/bsisduck/tracepath/issues/10) for the npm publish plan. Once enabled, releases here run from the `release` workflow: manual dispatch (or tag `v*`) → `release` environment approval → `npm publish` with `NPM_TOKEN` under the `tracepathhq` org.

## License

MIT — see [LICENSE](LICENSE). Upstream work © dusanstanojeviccs (Traceway).

## Contributing

`main` is protected: all changes go through pull requests. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Links

- [TracePath](https://tracepath.dev) — main product site
- [Documentation](https://docs.tracepath.dev)
- [Main monorepo](https://github.com/bsisduck/tracepath)
- [Go SDK](https://github.com/bsisduck/tracepath/tree/main/sdks/go)
- [OTel Agent](https://github.com/bsisduck/tracepath-otel-agent)
- [Agent skills](https://github.com/bsisduck/skills)
