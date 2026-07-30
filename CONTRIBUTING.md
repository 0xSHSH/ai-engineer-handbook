# 🤝 Contributing to the AI Engineer Handbook

Thanks for considering a contribution — this handbook gets better through real-world corrections, additions, and fresh examples from people actually building with AI.

## 📑 Contents
- [Ways to Contribute](#ways-to-contribute)
- [Before You Start](#before-you-start)
- [Making Changes](#making-changes)
- [Style Guide](#style-guide)
- [Submitting a Pull Request](#submitting-a-pull-request)
- [Reporting Issues](#reporting-issues)

## Ways to Contribute
- **Fix errors**: typos, outdated info, broken links, incorrect code.
- **Add examples**: a worked example under [`docs/09_examples/`](docs/09_examples/README.md) is one of the highest-value contributions — real, runnable code beats abstract explanation.
- **Expand a guide**: add missing interview questions, patterns, or edge cases to any file in [`docs/interviews/`](docs/interviews/README.md).
- **Propose a new section**: use the [feature request template](.github/ISSUE_TEMPLATE/feature_request.md) to discuss scope before writing a large addition.

## Before You Start
1. Check open issues and the [`ROADMAP.md`](ROADMAP.md) to avoid duplicating in-progress work.
2. For anything larger than a typo fix, open an issue first so the direction can be agreed on before you invest time writing it.
3. Fork the repo and create a branch named `fix/<short-description>` or `add/<short-description>`.

## Making Changes
- Keep one logical change per pull request — a typo fix and a new 500-line guide should be separate PRs.
- If you're adding a new document, follow the structure of an existing sibling file (TOC, headers, tables, a closing "Part of the AI Engineer Handbook" footer link).
- Use the templates in [`docs/08_templates/`](docs/08_templates/README.md) as a starting point for new READMEs, API docs, or system design docs.
- Update the relevant navigation table (root `README.md` and/or `docs/interviews/README.md`) when you add a new file — an unlinked file is effectively invisible to readers.

## Style Guide
- Markdown only, no HTML unless there's no other way to achieve the layout.
- Use tables for comparisons, Mermaid for diagrams/flows, and fenced code blocks with a language tag for all code.
- Write in plain, direct language — explain the *why*, not just the *what*, especially for anything framed as interview prep.
- Prefer runnable, minimal code examples over pseudocode where the concept allows it.
- Keep line length reasonable for diff readability; hard-wrapping isn't required.

## Submitting a Pull Request
1. Ensure all internal links you touched still resolve (relative paths, correct filenames).
2. Update `CHANGELOG.md` under an `[Unreleased]` heading (create one if it doesn't exist yet) describing your change.
3. Open the PR against `main` with a clear title and a short description of what changed and why.
4. Be responsive to review feedback — most PRs need at least one round of edits before merging.

## Reporting Issues
Use the appropriate template under [`.github/ISSUE_TEMPLATE/`](.github/ISSUE_TEMPLATE/):
- [`bug_report.md`](.github/ISSUE_TEMPLATE/bug_report.md) for errors, broken links, or incorrect information.
- [`feature_request.md`](.github/ISSUE_TEMPLATE/feature_request.md) for proposing new content or structural changes.

---
*Part of the [AI Engineer Handbook](README.md).*
