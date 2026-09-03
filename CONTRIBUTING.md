# Contributing Guidelines

Thank you for considering contributing to this project! Adhering to these guidelines ensures a smooth review process and maintains code quality.

## 1. Reporting Issues

- **Bug Reports**: If you find a bug, please search existing issues first. If it is new, open a new issue describing the bug, including steps to reproduce, expected behavior, and screenshots or log files if available.
- **Feature Requests**: Open an issue describing the feature, why it is needed, and any proposed visual or architectural implementations.

_Note: For security vulnerabilities, please refer to [SECURITY.md](SECURITY.md) and do not file a public issue._

## 2. Development Setup

We recommend using the standard package manager defined in the repository.

1. Clone your fork of the repository:

   ```bash
   git clone https://github.com/[your-username]/[repository-name].git
   cd [repository-name]
   ```

2. Install dependencies:

   ```bash
   pnpm install
   ```

3. Run the development server and test suite locally to verify your setup:
   ```bash
   pnpm run dev
   pnpm run test
   ```

## 3. Creating a Branch

Before writing code, create a branch off the `main` branch with a descriptive prefix (`<type>/<kebab-case-description>`):

```bash
# For a new feature:
git checkout -b feat/dynamic-header

# For a bug fix:
git checkout -b fix/issue-123-grid-overlap

# For documentation or refactoring:
git checkout -b chore/clean-unused-imports

# For CI/CD updates:
git checkout -b ci/automated-releases
```

## 4. Commit Message & Release Conventions

This project strictly enforces the [Conventional Commits](https://www.conventionalcommits.org/) specification via commitlint and automated releases using [Release Please](https://github.com/googleapis/release-please).

### Format

```text
<type>(<optional-scope>): <description in imperative mood, lowercase>

[optional body]

[optional footer(s)]
```

### Types & SemVer Release Impact

| Type                          | SemVer Impact                  | CHANGELOG Section        | Description                                             |
| :---------------------------- | :----------------------------- | :----------------------- | :------------------------------------------------------ |
| `feat:`                       | **MINOR** (`0.1.0` -> `0.2.0`) | Features                 | A new feature                                           |
| `fix:`                        | **PATCH** (`0.1.0` -> `0.1.1`) | Bug Fixes                | A bug fix                                               |
| `perf:`                       | **PATCH**                      | Performance Improvements | Performance improvement                                 |
| `feat!:` / `BREAKING CHANGE:` | **MAJOR** (`1.0.0` -> `2.0.0`) | Breaking Changes         | Breaking API or architectural change                    |
| `chore:`                      | No bump                        | Excluded                 | Maintenance or tooling changes                          |
| `ci:`                         | No bump                        | Excluded                 | CI/CD configuration and scripts                         |
| `docs:`                       | No bump                        | Excluded                 | Documentation updates                                   |
| `refactor:`                   | No bump                        | Excluded                 | Code change that neither fixes a bug nor adds a feature |
| `test:`                       | No bump                        | Excluded                 | Adding or correcting tests                              |

> [!IMPORTANT]
> **Pull Request Titles**: Pull Requests are merged using **Squash & Merge**. The PR title becomes the final commit message on `main`. Your PR title **must** follow the Conventional Commits format (validated in CI by `amannn/action-semantic-pull-request`).

## 5. Coding Standards

- All contributions must follow the protocols in [AGENTS.md](AGENTS.md).
- Verify UI updates against the visual guidelines in [DESIGN-SYSTEM.md](DESIGN-SYSTEM.md).
- Write or update unit tests for any logic you implement or modify.
- Run code formatting and linting scripts locally before pushing:
  ```bash
  pnpm run lint
  pnpm run format
  ```

## 6. Submitting a Pull Request (PR)

1. Sync your fork with the upstream repository:
   ```bash
   git checkout main
   git pull origin main
   git checkout your-branch-name
   git rebase main
   ```
2. Push your changes:
   ```bash
   git push origin your-branch-name
   ```
3. Open a Pull Request on GitHub with a Conventional Commit title (e.g. `feat: add dynamic user preferences`).
4. Ensure your PR description clearly states:
   - What changes are introduced.
   - Which issues are resolved (e.g., `Closes #123`).
   - Visual comparisons (before/after screenshots) for UI modifications.
