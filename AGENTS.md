# Repository Guidelines

## Project Structure & Module Organization

This repository is currently in the requirements phase. Project documentation lives in `docs/`:

- `docs/requirements.md` defines the agreed Harness MVP behavior.
- `docs/feature-list.md` maps requirements to implementation-ready feature IDs.
- `docs/open-issues.md` records unresolved decisions and recommended defaults.

No application or test directories exist yet. The agreed implementation target is a cross-platform TypeScript CLI distributed through npm. When adding the initial codebase, keep runtime code, tests, and bundled templates in clearly separated directories, and document the chosen layout here.

## Build, Test, and Development Commands

There is currently no `package.json`, build script, or automated test command. Do not assume a package manager or add instructions for commands that are not committed. Useful repository-level checks are:

```sh
git diff --check          # Detect whitespace errors.
git status --short        # Review the files included in a change.
```

When scaffolding the CLI, add reproducible `build`, `test`, `lint`, and `format` scripts to `package.json`, commit the lockfile, and update this section with the exact commands.

## Coding Style & Naming Conventions

For Markdown, use descriptive headings, short paragraphs, relative links, and pipe tables where they improve comparison. Preserve the existing Japanese terminology in product documents and use backticks for commands, paths, IDs, and literal values. Feature IDs follow `F-001`; quality requirements use `Q-001`; pending decisions use `P-001`.

For future TypeScript, prefer strict typing, small modules, deterministic behavior, and platform-neutral path APIs. Keep user-facing CLI and generated template text in English, as required by the MVP specification.

## Testing Guidelines

No test framework or coverage threshold is configured. Documentation changes should be checked for working relative links, consistent IDs, and agreement across all three documents. Code contributions should add tests with the feature they implement, especially for deterministic detection, conflict handling, rollback behavior, and macOS/Linux/Windows path differences.

## Commit & Pull Request Guidelines

History uses Conventional Commit-style subjects such as `docs: add MVP feature list` and `chore: initialize repository`. Use an imperative, concise subject with an appropriate scope prefix (`docs:`, `feat:`, `fix:`, `test:`, or `chore:`).

Pull requests should explain the change and rationale, link relevant issues, identify affected feature or requirement IDs, and list validation performed. Include screenshots or terminal output when CLI interaction or rendered output changes. Keep each PR focused and update related requirements documents when a decision changes.
