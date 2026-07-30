# Contributing to the AI Engineer Handbook

We welcome contributions from the community! This document outlines how you can help improve the handbook.

## How to Contribute

1. **Fork the repository**
   ```bash
   git clone https://github.com/0xSHSH/ai-engineer-handbook.git
   cd ai-engineer-handbook
   ```
2. **Create a new branch** for your work:
   ```bash
   git checkout -b my-feature-branch
   ```
3. **Make your changes** – edit or add Markdown files, diagrams, or code examples.
4. **Run the pre‑commit checks** (if you have the dev environment set up):
   ```bash
   pip install pre-commit
   pre-commit run -a
   ```
   The repository includes a simple CI that lints Markdown and validates Mermaid diagrams.
5. **Commit and push** your changes:
   ```bash
   git add .
   git commit -m "feat: describe your change"
   git push origin my-feature-branch
   ```
6. **Open a Pull Request** on the original repository.  Provide a clear description of what you changed and why.

## Code of Conduct
Please also review the [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) file.  We expect all participants to follow it.

## Style Guidelines
- Use **GitHub Flavored Markdown**.  Keep line length under 120 characters.
- Headings should follow a logical hierarchy (`#`, `##`, `###`).
- Diagrams must be written in **Mermaid** and placed in the `diagrams/` folder. Reference them in Markdown with:
  ```markdown
  ```mermaid
  !include ../diagrams/your_diagram.mmd
  ```
  ```
- When adding tables, keep them readable in plain text.
- Cite sources using links; for papers include a DOI or arXiv identifier.

## Review Process
- A maintainer will review your PR for accuracy, style, and completeness.
- Minor edits may be merged automatically; larger changes may require discussion.

Thank you for helping make this handbook better!