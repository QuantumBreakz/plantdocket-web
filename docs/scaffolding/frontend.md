# PlantDocket Web Repository Scaffold

**Repository.** `QuantumBreakz/plantdocket-web`

**Scope.** The employee browser workspace for factory drawings, policies and office documents. The [web PRD](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/prds/02-onsite-mvp-prd.md) and [web delivery phases](../frontend-delivery.md) define acceptance. Sharing serves the separate guest verification and download origin from the backend.

## Ownership and directory layout

Use a web runtime that supports server-side sessions and same-origin API forwarding. Confirm the web framework, runtime, lockfile and browser support matrix before bootstrap. The paths below show the intended ownership boundaries. Adapt route filenames to the framework selected.

```text
plantdocket-web/
  README.md                         onboarding and execution entry points
  docs/
    frontend-delivery.md             MVP delivery phases
    scaffolding/frontend.md          this guide
  src/
    routes/
      auth/                          sign-in, callbacks and logout
      workspace/                     document library and scoped administration
      review-queue/                  current steps assigned to the signed-in reviewer
    features/
      catalog/                       folders, metadata search and document details
      revisions/                     transfers, states, history, restore and trash
      remarks/                       document and revision discussions
      approvals/                     templates, submissions, decisions and release history
      access/                        workspace memberships and folder grants
      sharing/                       approved-revision guest grants and revocation
      audit/                         privileged search and exports
    server/
      auth/                          account mode and session lifecycle
      api/                           allowlisted same-origin API forwarding
      config/                        runtime environment validation
    generated/
      plantdocket-api/                typed consumer from pinned OpenAPI
    shared/
      ui/                            accessible reusable primitives
      errors/                        stable API errors and display-safe handling
  contracts/
    openapi.lock                     approved contract version and digest
  tests/
    unit/
    contract/
    browser/                         synthetic users, transfer and approval journeys
  .github/
    CODEOWNERS                       product and boundary review ownership
    workflows/                       PR checks and web candidate build
  Dockerfile                        signed production image input
  .gitignore                        local environment and generated build state
  .env.example                      documented keys and dummy values
```

## Request and product flow

| Concern | Scaffold contract |
| --- | --- |
| Employee session | Named site-managed account is the default. Optional site SSO uses the selected IdP. The server owns session state and server-held tokens. Browser requests use a host-only, HTTPS-only session cookie and request-forgery checks on mutations. |
| API client | Generate request and response types from the backend contract in `contracts/openapi.lock`. Browser business calls go to approved same-origin paths. The web server forwards authenticated employee context and correlation IDs. Library applies current membership, document and review-step authorization. |
| File transfer | Use the private API for upload reservation, status and signing. Transfer file bytes using scoped object URLs. Bound memory with file slices and incremental integrity checks. Persist only an opaque upload reference scoped to the user and site. On reload, require matching file reselection when necessary and resume using accepted parts from the service. |
| Revision and approval | Present the last approved revision as current. Scan clearance produces a draft. Display revision remarks, ordered departmental steps, a scoped reviewer queue, and reasons when a revision returns. Use expected state and idempotency keys for review mutations. Refresh after a conflict. |
| Controlled distribution | Internal downloads load the exact authorized version. Employee sharing controls invite or revoke access to an approved revision. Guest verification and streaming remain in Sharing. Surface permission loss and expiry in the employee workspace without revealing concealed documents. |
| Client safety | Render remarks as text. Keep credentials, signed URLs, invitation tokens, remarks and titles out of client telemetry where they could expose protected information. Redact failure traces and display the request ID for support. |

## Configuration and repository entry points

| Setting | Where it takes effect |
| --- | --- |
| Employee origin and cookie policy | The deployed server uses the allowed origin and TLS settings for the selected installation. |
| Private API endpoint and service credentials | The web server receives them at runtime through the approved secret source. |
| Site/account mode and callback | Environment configuration selects site-managed sign-in or approved SSO. |
| API contract version and digest | Pin at build time. Include the digest in CI and release evidence. |
| Stage-specific fixture roles and files | Supply through the controlled backend fixture stack. Use synthetic documents and test identities. |

Define stable repository tasks for `dev`, `verify`, `test:browser` and `build` once the toolchain is selected. `verify` runs formatting, type and unit checks and contract validation. `test:browser` starts from a known backend fixture state and records browser, web and backend artifact versions, case IDs and redacted failure evidence. `build` produces an OCI image suitable for promotion. Use the same image with destination-specific runtime hostnames, origins and account configuration.

## Acceptance for the first scaffold

1. From a clean checkout, start the web entry against the documented local stack and sign in as a synthetic employee. Verify a denied workspace and an expired session.
2. Upload, scan and register a draft, add a remark, then follow two ordered approval steps in the browser. The ordinary reader sees the previous approved revision until final approval. Validate an out-of-order step and revoked reviewer.
3. Exercise upload resume, exact-version download, and a verified guest download through a separate origin. Use the 500 MB case within the supported browser and environment matrix.
4. Build and record the web image digest and contract digest. Pair it with the backend candidate for staging. Link results to [feature verification](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/delivery/05-feature-verification.md) and [release gates](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/delivery/02-ci-cd-and-release-gates.md).

The [backend scaffold](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/scaffolding/backend.md) owns the local stack, fixture services, runtime manifests and deployment workflows.
