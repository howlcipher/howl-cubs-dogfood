# ADR R004-1: Explorer architecture

Status: accepted, 2026-10-10.

## Context

The explorer displays the published research JSON from cubs-edge-lab. It is read-only, it needs no server state, and the data is small (under 1 MB). Available here: Python 3.14, Node 22 (npm 12 warns that it does not support this Node version), and Chromium through Python Playwright.

## Options

| Option | Pros | Cons |
| --- | --- | --- |
| A. Static HTML/CSS/vanilla JS (ES modules); a Python export writes web/data/*.json; pytest plus Python Playwright | No npm or Node toolchain friction. Zero runtime dependencies and no supply-chain surface. Python-first. Hostable as plain files. Chromium is already installed. | No type checker. Hand-written DOM code. |
| B. Vite + TypeScript + Vitest + Playwright (Node) | Types and modern tooling. | npm/Node version mismatch. Workers need network for installs. A large dependency tree to audit. |
| C. Streamlit (Python server) | Fastest to build. | Needs a running server. Barely exercises frontend paths, which is the learning objective. |

## Decision

Option A. JavaScript is written as small ES modules with JSDoc types, and `node --check` serves as a syntax gate where it is available. The data export reads only research/*.json and copies exact values. Fidelity tests compare the rendered DOM with the source JSON.

## Consequences

- No build step beyond the export.
- Tests:
  - Unit: pytest for the export.
  - End-to-end: pytest with Playwright in headless Chromium, served from a local static server started by the test fixture.
- End-to-end tests are skipped with an explicit reason when Chromium is unavailable, so a clean clone without browsers still passes the fast tier. The full gate here requires them to run.
