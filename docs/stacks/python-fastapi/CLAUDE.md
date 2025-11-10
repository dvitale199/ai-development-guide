## Project Scope

Claude is used as an AI coding assistant for this Python project (mainly FastAPI APIs, Streamlit apps, and genetics data pipelines). All code and pipeline changes must satisfy these standards for correctness, privacy, security, and reproducibility.

---

## Core Requirements

- **Empirical Validation:** All changes must be tested and show empirical correctness (with tests or data samples) before merge.
- **Human Review:** AI-generated code requires human/domain expert review—scientific accuracy, privacy, and security must be double-checked.
- **Incrementalism:** Submit only small, atomic, and reversible changes. Do not create large PRs or multi-feature branches.
- **Testing:** Use pytest. All endpoints, major functions, and data flows require unit/integration tests; synthetic data is acceptable if real data is private.

---

## Key Conventions

- **Python 3.10+ required**
- Use [FastAPI](https://fastapi.tiangolo.com/), [Streamlit](https://streamlit.io/)
- Lint (`flake8`), format (`black`), and type-check (`mypy`) all code
- Organize modules: `/app` (API), `/streamlit_app` (UI), `/services` (business logic), `/schemas` (Pydantic validation), `/tests`
- Store API/database/env config in `.env` files (never commit secrets)
- Document all endpoints in `/docs/API_REFERENCE.md`

---

## Docker & CI

- Use `docker compose up --build` for local/dev environment (API, Streamlit, DB)
- Run `docker compose run app pytest` for containerized test execution
- All environment and secret variables configured with `.env` (see `.env.example`)
- Edit `/Dockerfile` and `/docker-compose.yml` as needed; validate on Mac + Linux

---

## AI/Claude-Specific Instructions

- Read relevant project docs before proposing code
- Mark all substantial AI changes as `[claude-assisted]` or `[claude-generated]`
- Inline-comment your intent, expected inputs/outputs, and biology/statistics assumptions for major code blocks
- Never expose sensitive data, patient info, or secrets in code or documentation
- All new dependencies or non-standard approaches must be documented and justified in your PR

---

## What Not To Do

- Do not edit multiple pipeline stages/features in one PR
- Do not skip tests, linting, or type checks
- Do not bypass human review on PRs
- Do not push directly to main/master

---

For deeper coding standards, project layout, or human process rules, see `/docs/ARCHITECTURE.md` and `CONTRIBUTING.md`.

*This file must be respected by all AI or human contributors for code and pipeline safety, scientific integrity, and regulatory compliance in this repository.*

---