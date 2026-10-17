# zero2prod

A newsletter backend built while working through
[*Zero to Production in Rust*](https://www.zero2prod.com) by Luca Palmieri.

Actix-web serves HTTP endpoints; Postgres stores subscribers via SQLx.
Logging uses `tracing` with Bunyan-formatted JSON output.

## Stack

- Rust (edition 2024)
- Actix-web
- Postgres + SQLx
- `tracing` / `tracing-subscriber` / `tracing-bunyan-formatter`
- GitHub Actions CI + `cargo-deny` advisories

## Endpoints

| Method | Path             | Description              |
|--------|------------------|--------------------------|
| GET    | `/health_check`  | Liveness check           |
| POST   | `/subscriptions` | Subscribe with form body |

Example:

```bash
curl -i -X POST \
  -d 'email=ursula_le_guin@gmail.com&name=le_guin' \
  http://127.0.0.1:8000/subscriptions
```

## Prerequisites

- Rust toolchain
- Docker (for local Postgres)
- `psql`
- `sqlx-cli` (Postgres feature)

## Setup

1. Start Postgres and run migrations:

```bash
./scripts/init_db.sh
```

2. Run the app (default port from `configuration.yaml`: `8000`):

```bash
cargo run
```

Pretty-print logs:

```bash
cargo run | bunyan
```

(`cargo install bunyan` if needed.)

3. Tests:

```bash
cargo test
```

## Project layout

```
src/
  main.rs           # binary entrypoint
  lib.rs            # module root
  configuration.rs  # settings from configuration.yaml
  startup.rs        # HTTP server wiring
  telemetry.rs      # tracing subscriber setup
  routes/           # health_check, subscriptions
migrations/         # SQLx SQL migrations
tests/              # integration tests
scripts/init_db.sh  # local Postgres + migrate
```

## Status

In progress through the book’s telemetry / integration-test logging chapters.
Not a production deployment.
