# Express + TypeScript Template

![Node.js](https://img.shields.io/badge/Node.js-26.x-339933?style=flat&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-5.x-000000?style=flat&logo=express&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-6.x-3178C6?style=flat&logo=typescript&logoColor=white)
[![License](https://img.shields.io/badge/License-MIT-blue.svg?style=flat)](LICENSE)

A starting point for Express APIs with TypeScript, Zod validation, Pino logging,
and integration tests. Includes CORS, security headers, compression, and rate limiting.

## Requirements

- Node.js 26.x is recommended for this setup and is configured in CI.
- npm.

## Installation

From the repository root:

```bash
npm ci
```

For custom settings, create a local `.env` using [.env.example](.env.example) as a
reference. Otherwise, the server uses the defaults listed below.

## Usage

Start the development server with automatic reload:

```bash
npm run dev
```

With the server running, check its health from another terminal:

```bash
curl http://localhost:8080/api/health
```

The response includes `success`, a message, and server uptime in `data.uptime`.
These examples use the default port, `8080`.

| Method | Route | Purpose |
| --- | --- | --- |
| GET | `/api/health` | Server status and uptime. |
| GET | `/api/app` | A greeting. |
| POST | `/api/app` | A greeting using a nonempty JSON `name`. |

Try the validated request:

```bash
curl -X POST http://localhost:8080/api/app \
  -H 'Content-Type: application/json' \
  -d '{"name":"Developer"}'
```

To compile to `dist/` and run the compiled application:

```bash
npm run build:prod
npm start
```

## Configuration

The development and start scripts load `.env` when present.

| Variable | Purpose | Default |
| --- | --- | --- |
| `PORT` | HTTP port. | `8080` |
| `CORS_ORIGIN` | Allowed origins, comma-separated. | `http://localhost:3000` |
| `LOG_LEVEL` | Pino logging level. | `info`; `silent` when `NODE_ENV=test` |

The rate limit is 100 requests per 15-minute window. Its settings are defined in
[src/configs/configs.ts](src/configs/configs.ts).

## Development

| Command | Purpose |
| --- | --- |
| `npm run build:check` | Check types without emitting files. |
| `npm run lint` | Run ESLint. |
| `npm test` | Run Vitest tests. |
| `npm run format` | Rewrite TypeScript files in `src/` and `tests/` with Prettier. |
| `npm run prepare` | Set up Husky Git hooks; also runs during installation. |

Application code is in [src/](src/), routes in [src/app/](src/app/), and integration
tests in [tests/](tests/).

Pre-commit hooks lint and format staged TypeScript files, then run tests.
Commit messages are checked with Commitlint's Conventional Commits preset.

The [CI workflow](.github/workflows/ci.yaml) runs lint, type checks, and tests on
pull requests targeting `main`, with a manual trigger also available.

## License

[MIT](LICENSE).
