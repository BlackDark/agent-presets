# Tech stack: shared types and API contracts

- Go structs → TS types, one repo, plain JSON: **tygo** (via mise).
- Go API as a contract: OpenAPI first, **oapi-codegen** (Go) + **Hey API** `@hey-api/openapi-ts` (client, Zod schemas, TanStack Query hooks). Types only: `openapi-typescript` + `openapi-fetch`.
- Rust structs → TS types: **ts-rs**; **specta** when functions are exported too.
- Rust API as a contract: OpenAPI from code (`utoipa`) + **Hey API**.
- Tauri commands: **tauri-specta**, exact version pinned (RC **(watch)**).
- TS on both sides: shared Zod schema package, no codegen.

Rules:
- One source of truth per type; the other side is generated.
- Generated output is committed or built in CI, with a "regenerate, fail on diff" step.
- Generated types don't validate at runtime: parse responses with Zod. Exception: client and server in one repo, generated from the same spec, with the fail-on-diff step, and shipped as one unit (one binary or image serves both, so versions never differ at runtime); record it in `AGENTS.md`. Rolling deploys or separately updated clients keep parsing.
