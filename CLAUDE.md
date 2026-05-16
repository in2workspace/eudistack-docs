# Platform Docs (Public Knowledge Base) — Repo Guide for Claude

> **Per-repo CLAUDE.md.** Loaded only when working inside this repo. The
> SDD Constitution lives in `../eudistack-platform-dev/CLAUDE.md`.

## Identity

Public-facing **Knowledge Base** for the EUDIStack platform: user
guides, API references, integration tutorials, FAQ. Distinct from
internal SDD docs (which live in `eudistack-platform-dev/docs/`).

> Audience: external developers, customers, partners. Tone: tutorial /
> reference / formal.

## Tech stack

Likely Markdown-based static site (MkDocs / Docusaurus / VitePress —
check `package.json` or `mkdocs.yml`).

## Common commands

> Check the repo's actual build tool. Patterns by stack:

| Stack | Dev server | Build |
|-------|------------|-------|
| MkDocs | `mkdocs serve` | `mkdocs build` |
| Docusaurus | `npm start` | `npm run build` |
| VitePress | `npm run docs:dev` | `npm run docs:build` |

## Conventions

- All public docs in **English** (audience is European-wide).
- Code examples must be tested and pinned to a version.
- API references generated from OpenAPI specs in the backend repos when possible.
- Screenshots: WebP preferred, captions mandatory, alt text for accessibility.

## What NOT to put here

- Internal SDD specs (those live in `eudistack-platform-dev/docs/EUDISTACK-NNN-*/`).
- Architecture decisions internal to the team (those live in SAD §ADR).
- Sensitive operational info (runbooks, credentials, internal URLs).

## Git workflow

- **Squash merge to `main`.** Conventional Commits.

## References

- Constitution: [`../eudistack-platform-dev/CLAUDE.md`](../eudistack-platform-dev/CLAUDE.md)
- Figma page **10 Knowledge Base**.
