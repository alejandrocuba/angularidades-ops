# Core Agentic Engineering Protocols

> [!NOTE]
>
> - Synced from `alejandrocuba/dynamic-scaffolding` GitHub repository. Do not edit directly; changes will be overwritten on updates.
> - Project overrides (frameworks, workflows) belong in `AGENTS.md` and inherit these rules.
> - Design, accessibility, and markup standards are defined in [DESIGN-SYSTEM.core.md](DESIGN-SYSTEM.core.md) and project `DESIGN-SYSTEM.md`.

## 1. Determinism

- **Predictability**: Use pure functions, immutable data, and framework-native reactivity. Avoid mutable shared states.
- **Error Handling**: Never swallow exceptions silently. Log with diagnostic context or bubble to explicit boundaries.

## 2. Architecture

- **SRP/DRY**: Single responsibility per module. Abstract repeated logic/styles into shared utilities.
- **Async Safety**: Prevent state updates or mutations on unmounted/destroyed components.

## 3. Documentation

- **Flag Surprises**: Comment and flag non-obvious, legacy, or confusing logic.

## 4. Dependencies

- **Tooling**: Use `pnpm` exclusively. Human approval required for `package.json`/lockfile changes.
- **Formatting**: Use Prettier for code formatting. Code must conform to the shared `.prettierrc` rules, and formatting check must pass prior to merge.

## 5. Security

- **Environment**: Stay sandboxed. No `sudo` or out-of-workspace edits.
- **XSS**: Prefer `textContent`. Sanitize `innerHTML` via DOMPurify/Trusted Types.
- **Supply Chain**: CDN links require `integrity` hashes, `crossorigin="anonymous"`, and HTTPS.
- **Source Maps**: Exclude `sourcesContent` in production (`hidden` or none).

## 6. Runtime & Memory Safety

- **SSR/SSG**: Guard browser globals (`window`, `document`) via lifecycle hooks or platform checks.
- **Leaks & Cleanup**: Prevent memory leaks. Clear timeouts, remove event listeners, dispose of subscriptions, and manage lifecycle cleanup.
- **Performance (INP)**: Avoid long, synchronous tasks that block the main thread. Yield frequently during heavy data processing to ensure a responsive Interaction to Next Paint.
- **Modern APIs**: Use `fetch`, abandon deprecated APIs (`document.write`, sync XHR).

## 7. CSS-First

- **Prefer Native CSS**: Prioritize native CSS (e.g., `@keyframes`, transitions, Scroll-Driven Animations) over JavaScript for motion, reveals, hovers, and focus states.
- **Progressive Enhancement**: Provide readable fallback/default states for modern CSS features (e.g., `animation-timeline`) using `@supports` to ensure compatibility and prevent broken layouts.
- **Compositor Optimization**: Run animations on the GPU (compositor thread) instead of CPU to prevent frame drops and minimize JS weight.

## 8. Type Safety (TypeScript)

- **Strict Mode**: No `any`. Use `unknown` with type guards. Prefer implicit inference.
- **Overrides**: Justify and document `@ts-ignore` or lint disables.
- **Object Safety**: Prevent prototype pollution. Use `structuredClone` for deep copies, `Object.create(null)` for maps.

## 9. Testing

- **Coverage**: Mandate tests (`.spec.ts`) to prevent regressions.
- **Mocks**: Simulate and assert user interactivity accurately.

## 10. I/O Token Efficiency

- **Minimization**: Favor concise instructions over verbose prose.
- **Identifiers**: Use short, deterministic IDs for inter-agent communication instead of descriptive text.
- **Payloads**: Strip superfluous metadata from API payloads or tool inputs/outputs.

## 11. Git & Release Protocols

- **Local Git Hooks (Husky)**: Repositories enforce local Git hooks via Husky (`.husky/`).
  - The repository's `package.json` must declare `"prepare": "husky"`.
  - The commit hook `.husky/commit-msg` executes `pnpm exec commitlint --edit "$1"` before any commit is created.
  - **Strict Commit Linting**: Non-compliant commit messages will be intercepted and rejected immediately by Husky. All contributors and AI agents must generate compliant messages on the first attempt to prevent commit failures.
- **Branch Naming**: Adhere strictly to `<type>/<kebab-case-description>` (e.g., `feat/dynamic-header`, `fix/grid-overlap`, `chore/cleanup-deps`, `ci/automated-releases`).
- **Conventional Commits**: Commit messages must follow the [Conventional Commits](https://www.conventionalcommits.org/) format:

  ```text
  <type>(<optional-scope>): <description in imperative mood, lowercase>

  [optional body explaining rationale]

  [optional footer(s), e.g., BREAKING CHANGE: <explanation>]
  ```
  - Permitted types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.

- **Squash-Merge & PR Titles**: Pull Request titles must strictly follow Conventional Commits syntax because GitHub Squash & Merge uses the PR title as the final commit message on `main`. Verified in CI via `.github/workflows/pr-title-check.yml`.
- **Release Please (SemVer)**:
  - `feat:` triggers a **MINOR** version bump (`0.1.0` -> `0.2.0`).
  - `fix:` and `perf:` trigger a **PATCH** version bump (`0.1.0` -> `0.1.1`).
  - `feat!:` / `fix!:` or `BREAKING CHANGE:` in the footer triggers a **MAJOR** version bump (`1.0.0` -> `2.0.0`).
  - `chore:`, `ci:`, `docs:`, `style:`, `refactor:`, `test:` do not trigger a release on their own.
- **CI Workflow Optimization**: PR workflows (such as preview deployments or tests) should skip execution on branches generated by `release-please` using:
  ```yaml
  if: "${{ !startsWith(github.head_ref, 'release-please') }}"
  ```
