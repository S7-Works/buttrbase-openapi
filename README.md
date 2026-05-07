# ButtrBase API — OpenAPI + Postman

Curated coverage of the ButtrBase backend (Rust + Axum). Focuses on the
auth, admin, identity, and signing surfaces. Some lower-traffic surfaces
(billing/teams/help/analytics proxy routes) are intentionally omitted to
keep the spec readable; consult `src/routes/mod.rs` for the full router.

## Use with Postman

1. Open Postman → **Import** → select `buttrbase.postman_collection.json`.
2. Create a new environment with:
   - `baseUrl` = `https://stagingapi.buttrbase.com` (or your prod host)
   - `apiKey` = your bearer JWT or `sk_*` API key
   - `orgUuid` = your organization UUID
   - `scimToken` = (optional) per-org SCIM bearer token for `/scim/v2/*`
3. Send any request — the collection-level pre-request script falls back
   to staging if `baseUrl` is unset.

The `/api/stream/query` endpoint is a **WebSocket**; Postman's WS workspace
can be used directly with `wss://stagingapi.buttrbase.com/api/stream/query`.

## Use with OpenAPI tools

`openapi.yaml` is OpenAPI 3.0.3. Suggested viewers / generators:

- **Stoplight Studio** — drag the YAML in for an interactive doc UI.
- **Swagger UI / Redoc** — `npx @redocly/cli preview-docs openapi.yaml`.
- **openapi-generator** — `openapi-generator-cli generate -i openapi.yaml -g typescript-fetch -o ./client`.

Two security schemes are declared: `bearerAuth` (default JWT/sk_*) and
`scimBearerAuth` (separate per-org SCIM token). Public endpoints
(`/api/coupons/validate`, gift-card validate, `.well-known/*`,
admin-portal exchange, magic-link send/verify, OIDC/SAML callbacks)
override security with `security: []`.
