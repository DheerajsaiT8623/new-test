# GitHub Copilot Code Review Checklist — Angular 20 Standalone SPA

Audience: Automated/assisted code reviews for Angular 20 standalone applications similar to this repo. Use this checklist to evaluate adherence to project standards and produce a Pass/Fail report with evidence and suggestions.

How to use
- For each item, verify the listed acceptance criteria using targeted file reads and simple searches.
- Capture small, relevant excerpts (not full files) as evidence.
- Mark Status as Pass/Fail/NA and add notes/suggestions.

Report structure (recommended)
- Summary table: ID → Verdict → Short reason
- Evidence: File path(s) + quoted snippet(s)
- Suggestions: Actionable next steps with file/line pointers

---

## Checklist

### Architecture & Bootstrap
- [ ] A1. Bootstrap & providers centralized
  - Accept: `src/main.ts` bootstraps with `bootstrapApplication(App, appConfig)`; `src/app/app.config.ts` provides router, HttpClient, global error listeners, zone coalescing.
  - Verify: Read `src/main.ts`, `src/app/app.config.ts`.
  - Evidence: Excerpts showing `bootstrapApplication(...)`, `provideRouter(routes)`, `provideHttpClient()`.

- [ ] A2. Central routing with guards and roles
  - Accept: `src/app/app.routes.ts` defines guarded routes (e.g., `/dashboard`, `/payment/:id/:qty`); supports `data: { role }`.
  - Verify: Read `app.routes.ts`, `core/guards/auth.guard.ts`.
  - Evidence: Route entries + guard logic enforcing auth and optional role.

### Models & Types
- [ ] M1. Single source models file
  - Accept: New code imports from `src/app/core/models/models.ts`; no new imports from legacy `inventory.models.ts`.
  - Verify: Search imports; scan references.
  - Evidence: Import lines; any legacy usage flagged.

### Services & Data Flow
- [ ] S1. UserService state + storage contract
  - Accept: `BehaviorSubject` state with `$` observables: `currentUser$`, `users$`, `products$`; persistence via `localStorage` keys `currentUser`, `users`, `products`, `seller_{id}_products`.
  - Verify: Read `core/services/user.service.ts`.
  - Evidence: Fields/methods excerpts.

- [ ] S2. InventoryService uses environment base URL and typed methods
  - Accept: CRUD + stock methods call `${environment.apiBaseUrl}/inventory...` with `httpOptions` and typed responses.
  - Verify: Read `core/services/inventory.service.ts`.
  - Evidence: Method signatures and constructed URLs.

### Environments & Config
- [ ] E1. No hard-coded API roots outside environments
  - Accept: All HTTP roots come from `src/environments/*.ts`.
  - Verify: Search for `http://` or `https://` in `src/app` excluding `src/environments`.
  - Evidence: Zero-hit search or exceptions documented.

- [ ] E2. Angular CLI defaults align with workflow
  - Accept: `build.defaultConfiguration = production`; `serve.defaultConfiguration = development`; `inlineStyleLanguage = scss`.
  - Verify: Read `angular.json`.
  - Evidence: Config excerpts.

### Components & Forms
- [ ] C1. Standalone components with explicit imports
  - Accept: `standalone: true`; imports include only required modules (e.g., `CommonModule`, `ReactiveFormsModule`).
  - Verify: Read representative components in `src/app/features/**` and `app.ts`.
  - Evidence: Component metadata excerpts.

- [ ] C2. Reactive Forms with validators for user inputs
  - Accept: Controls validated (email, CVV, expiry, min/max, required).
  - Verify: Read `features/auth/login.component.ts`, `features/payment/payment.component.ts` (and other forms if present).
  - Evidence: Form group definitions and validator usage.

### Routing & Guard Behavior
- [ ] R1. Unauthenticated redirect preserves returnUrl
  - Accept: Guard redirects to `/login` with `queryParams: { returnUrl }` when unauthenticated.
  - Verify: Read `core/guards/auth.guard.ts`.
  - Evidence: Guard branch creating `UrlTree`.

- [ ] R2. Role-based route enforcement
  - Accept: When `data.role` present and mismatched, guard redirects (e.g., to `/dashboard`).
  - Verify: Read `core/guards/auth.guard.ts` and routes with `data.role`.
  - Evidence: Guard logic showing role check.

### Error Handling & UX
- [ ] H1. HTTP error formatting pattern is user-friendly
  - Accept: Components format `HttpErrorResponse` to readable messages (pattern like `formatHttpError`).
  - Verify: Read `features/dashboard/dashboard.component.ts` and other API consumers.
  - Evidence: Function implementation and usage in subscriptions.

### Styling & Assets
- [ ] S3. SCSS usage is consistent
  - Accept: Global `src/styles.scss` present; components use SCSS; `angular.json` set to SCSS.
  - Verify: Read `angular.json`, spot-check components.
  - Evidence: Config + component style metadata.

### Build/Test Workflows
- [ ] B1. NPM scripts for CLI flows
  - Accept: `npm start` (serve), `npm run build`, `npm test` map to Angular CLI.
  - Verify: Read `package.json` scripts.
  - Evidence: Scripts block.

### Legacy & Duplicates
- [ ] L1. No new references to legacy files
  - Accept: Avoid adding new imports from `src/app/core/models/inventory.models.ts`.
  - Verify: Search imports.
  - Evidence: Import list; flag any violations.

- [ ] L2. Unused or duplicate artifacts are acknowledged
  - Accept: Identify files like unused `dashboard.component.html` when inline template is used; do not add new references.
  - Verify: Check component metadata vs sibling files.
  - Evidence: Note file presence and current usage.

---

## Reviewer Worksheet (for each item)
- ID: (e.g., A1)
- Verdict: Pass / Fail / NA
- Files checked: [...]
- Evidence: (short quoted excerpt)
- Notes: (context, edge cases)
- Suggestions: (specific actions with file/line pointers)
