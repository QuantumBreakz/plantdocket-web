# PlantDocket Web

Web planning for a factory document workspace with revision history, remarks, and departmental approvals. Staff can find the current approved revision of a drawing, policy, or office document.

Backend repository: [plantdocket-backend](https://github.com/QuantumBreakz/plantdocket-backend).

## Planning documents

- [Master Documents](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/README.md)
- [Web delivery plan](docs/frontend-delivery.md)
- [Frontend repository scaffold](docs/scaffolding/frontend.md)
- [MVP scope](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/mvp/01-scope.md)
- [Feature verification matrix](https://github.com/QuantumBreakz/plantdocket-backend/blob/main/docs/delivery/05-feature-verification.md)

## Write-ups

Design notes from this project, with tested Go code:

- [Multi-step document approval in Go: keep the old revision effective until the last step approves](https://aliahmed.si/blog/document-approval-workflow-go/)
- [Email one-time codes for guest file sharing in Go: hashing, attempt limits, expiry and races](https://aliahmed.si/blog/email-otp-guest-file-sharing-go/)
- [HTTP Range requests in Go: resumable downloads, 206 and 416, If-Range, and capping multipart ranges](https://aliahmed.si/blog/http-range-requests-go/)
