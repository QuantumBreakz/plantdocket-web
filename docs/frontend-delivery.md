# PlantDocket Web Delivery Phases

The employee website provides factory document control in a desktop browser, with revision history, remarks and approvals. The public guest website offers invitation, email verification and download. Both installation profiles ship these same flows with separate session cookies and route policies per origin. The [web MVP PRD](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/prds/02-onsite-mvp-prd.md) owns product scope.

| Phase | Product slice | Build and acceptance evidence |
| --- | --- | --- |
| Web 0. Foundations | Reach the private website → named-account sign-in → workspace | Set up the web server, same-origin control API, site-managed sign-in or selected SSO, server-held credentials and scoped session cookies. Verify HTTPS, account provisioning, CSRF defenses, session expiry and logout. |
| Web 1. Document library | Workspace → folders or search → document facts and actions | Deliver folders, metadata search, pagination and revision details. Provide authorized administration for workspace memberships and folder grants. Verify loading, denied access, refresh and empty results. |
| Web 2. Transfers and revisions | Select document → upload → scan status → draft, restore or trash | Use a browser file picker and bounded part uploads. Resume after network loss or reload from confirmed parts when the same file is available. Ordinary readers retrieve the effective approved version. Contributors and reviewers inspect eligible drafts within scope. |
| Web 3. Remarks and review | Draft → remarks → ordered departmental decisions → effective version | Provide document and revision remarks, a reviewer queue, current-step action, return reason and full approved history. Verify no intermediate approval makes the draft effective. |
| Web 4. Approved guest access and audit | Choose approved revision → invite → recipient verifies → download → revoke | Employees manage grants from the workspace. Sharing delivers guest pages on the public origin. Provide authorized audit search and export. Verify invalid codes, retries, revocation, audience separation and guest reachability. |
| Web 5. Site readiness | Deploy and verify both origins | Run employee and guest journeys on the signed release set. Test keyboard navigation, focus, screen-reader labels, low connectivity, session expiry, the 500 MB workload and API compatibility. |

## Integration rules

- Use the backend's versioned OpenAPI contract for business operations. The employee browser calls the web server on the same origin. The web server forwards allowed API requests to private services with authenticated employee context. Keep authorization policy in the backend services.
- Terminate the browser session at the web server. Keep credentials and any configured provider tokens server-side. Use host-only, Secure, HttpOnly session cookies, check the expected origin and CSRF token on mutations, and keep employee and guest cookies scoped to their separate hostnames. [OIDC Core](https://openid.net/specs/openid-connect-core-1_0.html), [OAuth security best practices](https://www.rfc-editor.org/rfc/rfc9700).
- Send approved file parts directly to the signed object endpoint using bounded chunks. Configure exact employee origins, relevant methods and exposed part receipts on the selected provider. Save only opaque resume references, bound to the signed-in user and site. Reauthorize on resume, reselect when browser file access was lost and verify content before reuse. [Amazon S3 CORS configuration](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ManageCorsUsing.html).
- Let the browser download manager save attachments. Offer a fresh authorization for the same version if a retry needs a new URL. Validate actual downloaded bytes and checksum in test evidence.
- Keep invitation secrets out of referrers, analytics and logs. Verify keyboard operation and accessible error messages for both entry points.

Record the web image digest, supported browser versions, backend manifest, API contract hash and environment configuration in the [feature verification matrix](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/delivery/05-feature-verification.md). The [backend plan](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/mvp/06-backend-delivery.md) defines service contracts and handoffs.
