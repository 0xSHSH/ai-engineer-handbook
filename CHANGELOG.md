# 📝 Changelog

All notable changes to this repository are documented here. Format loosely follows [Keep a Changelog](https://keepachangelog.com/).

## [2.0.0] — 30 July 2026
### Added
- Full interview preparation track expansion in `docs/interviews/`:
  - `03_technical_python.md` — Python & software engineering fundamentals
  - `04_ml_interview.md` — classical machine learning
  - `05_dl_interview.md` — deep learning & architectures
  - `06_llm_interview.md` — LLM-specific engineering (RAG, fine-tuning, evaluation, hallucination)
  - `07_ai_agents.md` — agent architectures, tool calling, MCP, multi-agent frameworks
  - `08_system_design.md` — building blocks + worked system designs
  - `09_coding_interview.md` — DSA patterns and practice set
  - `10_sql_guide.md` — SQL fundamentals through window functions and locking
  - `11_api_guide.md` — REST/GraphQL/WebSockets, auth, rate limiting, versioning
  - `12_cloud_guide.md` — AWS/Azure/GCP concepts, IAM, networking, containers
  - `13_devops_guide.md` — Docker, Kubernetes, CI/CD, Terraform, deployment strategies
  - `14_company_preparation.md` — interview-process patterns by company type
  - `15_hiring_preparation.md` — portfolio, networking, negotiation, offer comparison
- Root-level repository documentation: `ROADMAP.md`, `PROGRESS.md`, this `CHANGELOG.md`
- `.github/ISSUE_TEMPLATE/feature_request.md` (previously only available under `docs/08_templates/`, now also a proper GitHub issue template)

### Changed
- Rewrote the root `README.md`: refreshed navigation table, current progress snapshot, roadmap summary, and links to every new interview document
- Rewrote `docs/interviews/README.md` navigation table and usage walkthrough to reflect the full 15-document track
- Standardized formatting across interview docs: consistent TOC, callouts, tables, and cross-links between related guides

### Fixed
- Cross-references between documents validated (no dangling links to not-yet-written interview tracks)

---

## [1.0.0] — Initial release
### Added
- Core handbook: fundamentals, assistant guide, capability guide, model guide, prompt engineering, engineering roadmap, workflows, FAQ, resources
- Initial interview track: resume preparation, HR/behavioral prep
- Templates for README, API docs, system design docs, bug reports, feature requests
- Mermaid diagrams: handbook overview, fundamentals flow
- Image generation handbook
- MIT License

---
*Part of the [AI Engineer Handbook](README.md). See also: [`ROADMAP.md`](ROADMAP.md) · [`PROGRESS.md`](PROGRESS.md).*
