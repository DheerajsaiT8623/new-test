# GitHub Copilot Code Review Checklist — Fast-Path (<=10s)

Scope: Read only these files
- src/main.ts
- src/app/app.config.ts
- src/app/app.routes.ts
- src/app/core/guards/auth.guard.ts
- src/app/core/models/models.ts
- src/app/core/services/inventory.service.ts
- src/environments/environment.development.ts
- src/app/features/payment/payment.component.ts
- package.json

Checklist (Pass/Fail/NA with 2–8 line evidence)

F1. Bootstrap & Providers centralized
- Accept: main.ts uses bootstrapApplication(App, appConfig); app.config.ts provides router + HttpClient.
- Verify: src/main.ts, src/app/app.config.ts
- Evidence: bootstrapApplication(...); provideRouter(routes); provideHttpClient()

F2. Routes guarded for protected areas
- Accept: app.routes.ts defines routes for /dashboard and /payment/:productId/:quantity with canActivate: [authGuard].
- Verify: src/app/app.routes.ts
- Evidence: Guarded route entries

F3. Guard enforces auth + preserves returnUrl (+ role branch present)
- Accept: Unauthed → createUrlTree('/login', { queryParams: { returnUrl } }); if data.role present and mismatched → redirect (e.g., '/dashboard').
- Verify: src/app/core/guards/auth.guard.ts
- Evidence: createUrlTree call; optional role-check branch

F4. Models source-of-truth present
- Accept: core/models/models.ts defines User, Product, Inventory, Stock (prefer this file).
- Verify: src/app/core/models/models.ts
- Evidence: Interface/type declarations (short excerpts)

F5. InventoryService uses environment.apiBaseUrl with typed HTTP
- Accept: Methods compose `${environment.apiBaseUrl}/inventory...` and return typed responses with http options.
- Verify: src/app/core/services/inventory.service.ts
- Evidence: import { environment } ...; example GET/POST signatures + URL composition

F6. Environment base URL defined (dev)
- Accept: environment.development.ts exports apiBaseUrl (http://localhost:.../api).
- Verify: src/environments/environment.development.ts
- Evidence: apiBaseUrl value

F7. Payment form uses Reactive Forms validators
- Accept: Controls include Validators (required, length, pattern) for card, expiry, cvv, amount.
- Verify: src/app/features/payment/payment.component.ts
- Evidence: FormGroup definition + Validators usage

F8. NPM scripts map to Angular CLI
- Accept: start → ng serve; build → ng build; test → ng test.
- Verify: package.json
- Evidence: scripts block excerpts

Output format
- Summary table: ID | Title | Verdict | Notes
- For each item: Files checked | Evidence (short quotes) | Verdict | Suggestions (file:line)
