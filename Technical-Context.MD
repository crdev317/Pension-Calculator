# Technical Context

Engineering-practice contract for code-writing agents in this project. Every implementation must respect what's written here. If a constraint conflicts with the user's request, flag the conflict — don't silently bypass it.

## Overriding Principles
1. **Calculation logic is pure and separate from the UI.** Every figure the page displays comes from a side-effect-free, unit-tested TypeScript function with no DOM access, so the maths is verifiable without a browser.
2. **User inputs never leave the browser.** There is no backend, no storage (including cookies and `localStorage`) and no analytics. Ages, salaries, contributions and pot values are personal data, and the product never holds any.
3. **No magic numbers.** Every assumption (growth rate, charges, inflation and the like) is a named, documented constant, and is shown to the user alongside the result. A projection is only as trustworthy as its visible assumptions.

## Frameworks
- **TypeScript 7.0** — the only application language, for both the calculation module and the page.
- **Vite 8.3** — dev server, production build (a static HTML/CSS/JS bundle) and `vite preview`.
- **No UI framework** — a single HTML page, with TypeScript modules driving the DOM directly.

## 3rd-party tech
- **None.** No external APIs, analytics or error-tracking services, and no hosted backend (Principle 2).

## Packages in use
**Runtime:** none — the shipped bundle has zero runtime dependencies. Number and currency formatting uses the browser's built-in `Intl.NumberFormat`.
**Build:** `vite`, `typescript`
**Testing:** `vitest` (unit), `@playwright/test` (end-to-end, Chromium)
**Code quality:** `eslint`, `typescript-eslint`, `prettier`
**Charting:** none — results are headline figures only.

## Toolchain pinning
- Policy: **exact-pinned**.
- Node.js: major version **24 (LTS)**, pinned in `.nvmrc` and the `engines` field of `package.json`.
- npm packages: exact versions only (`save-exact=true` in `.npmrc`; no `^` or `~` ranges). `package-lock.json` is committed and installs use `npm ci`.
- Versions current at authoring (2026-10-06): Vite 8.3.3, TypeScript 7.0.2, Vitest 5.0.3, @playwright/test 1.63.0, ESLint 10.12.0, typescript-eslint 8.71.1, Prettier 3.9.9. Once the project is scaffolded, `package-lock.json` is the source of truth, not this list.
- Bumping: each version bump is its own PR and merges only when the full test suite passes. Dependency-update PRs never auto-merge.

## Instrumentation
- No runtime telemetry: no analytics, error-tracking service, cookies or tracking pixels.
- Production code does not log. `console` calls are for local development only; ESLint's `no-console` rule fails lint on any that remain.
- No code path, error handling included, writes a user's age, salary, contributions, pot value or any other input anywhere — not to the console, the URL, storage or the network.
- The only observable signals are CI test results and, once the app is hosted, the static host's access logs (page requests only, never form data).

## User Feedback Approach
- Channel: the page itself. No email, notifications or pop-ups.
- Validation is inline, next to the field, when the field loses focus. Messages say what to fix (e.g. "Enter an age between 18 and 75"), never only "Invalid input". Errors are announced to assistive technology via `aria-live`, and the page meets WCAG 2.2 AA.
- Tone: plain UK English, friendly but not chatty. A pension term (e.g. "tax relief") gets a one-line explanation where it first appears.
- Money is shown in pounds via `Intl.NumberFormat('en-GB', { style: 'currency', currency: 'GBP' })`, rounded to the nearest pound for display. Calculations keep full precision and round only at display.
- Every result is labelled as an estimate, shown with the assumptions that produced it (Principle 3) and a short line stating it is not financial advice.

## Branching and Environments
- Model: protected `main` with a per-Feature integration branch. Full rules: `CONTRIBUTING.md`.
- Branches: `main`; `feature/<feature-id>-<slug>` per Feature; `story/<id>-<slug>` per Story, cut from its Feature branch; `bug/<id>-<slug>` for parentless work, cut from `main`.
- Merges: Story → Feature branch by squash; Feature → `main` by history-preserving merge commit; parentless work → `main` by squash.
- Environments: **local only** — the Vite dev server for development and `vite preview` for the production build. There is no hosted environment; a hosting target is chosen by ADR when the product needs a public URL.
- Protected: `main` — PR-only, no force-push, no deletion. CI, once wired, is an Azure DevOps pipeline posting the `ci` status check.

## Git Guardrails
- Never, without explicit human confirmation: force-push, `git reset --hard`, rebase a shared branch, or delete a branch (beyond the post-merge cleanup of a merged PR's own branch).
- Never commit directly to `main`; every change reaches it through a PR.
- Never bypass hooks with `--no-verify`.
- Never commit secrets or real personal data. Test fixtures use made-up figures only.
- Hook in place: `.githooks/pre-commit` blocks scratch and diff dump files. Enable it once per clone with `git config core.hooksPath .githooks`.
- **HITL delivery opens and closes its own board state — nothing else will.** No skill in the HITL pipeline (`/factory-to-stories`, `/factory-check-security-design`, `/factory-triage`) sets an assignee, an iteration, or an in-progress state at creation — they deliberately leave a HITL-tagged Story, Bug, or wholly-HITL Feature untouched for a human to clear. So: **before** making any change while hand-delivering one, set `System.AssignedTo` to yourself, `System.IterationPath` to the current sprint, and `System.State` to `Active`. For a Feature, delivery starts with its first Story; while it is planned, the planning skills set its state. **When** the delivering PR merges and the work item is done, set `System.State` to `Resolved`/`Closed` yourself before ending the session. Skipping either half leaves the item unassigned, out of sprint, and looking untouched for the entire time it's actually being worked.

## Testing & the ratchet
> Seeded from the Factory standard — **opt-out, not opt-in**. Trim a line only with a deliberate, recorded decision; don't start from nothing. Full standard: <https://github.com/enateltd/factory-skills/blob/main/docs/testing-and-the-ratchet-standard.md> · the *why*: <https://github.com/enateltd/factory-skills/blob/main/docs/agentic-e2e-testing-principles.md>.

- **Every seam gets a real-IO test on at least one side.** The form → calculation → result-display path is covered by at least one Playwright test with nothing mocked. Mock-on-both-sides only proves the mocks agree.
- **Tiers, as they apply to this product:**
  - **Tier 1 — every commit, $0:** Vitest unit tests on the calculation module, built on hand-worked reference cases (known inputs → known outputs), plus Playwright tests against the dev server in Chromium.
  - **Tier 2 — every PR, $0:** the same Playwright tests against a fresh production build served by `vite preview`, in Chromium. Each run builds from scratch, so the substrate is disposable by construction.
  - **Tier 3 — not in use:** nothing is hosted. Reinstated when a hosting target is chosen.
- **Plan test coverage at the Feature level, not the Story level.** Each Feature declares at kickoff which tiers it needs and which reference cases must exist.
- **The ratchet — every defect found in use pins a Tier-1 regression.** The fix lands with a Tier-1 test encoding the **class** of defect, **bounded by the seam** where it surfaced — not only the exact reproducer, and not a project-wide sweep. A Story that came from such a defect carries that test as an acceptance criterion.
- **Assert on observable behaviour, not internal state.** End-to-end tests assert on what the user sees — the displayed figures, messages and assumptions — not on in-memory state.

### Trimmed from the standard (recorded decision, 2026-10-06)
- *Test the contract, not the LLM* and the *live-LLM* Tier 2 — the product contains no LLM.
- *Platform matrix for OS-touching code* — the product runs in the browser and has no filesystem or subprocess code. Cross-browser testing is also out of scope: Tier 2 runs Chromium only, to keep the suite simple.
- *Tier 3 (live)* — no hosted environment exists. Revisit when a hosting target is chosen.
- *Disposable-substrate seed and teardown machinery* — not needed: each Tier-2 run builds fresh.
