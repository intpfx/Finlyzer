# AGENTS

This repository is a local-first Electron desktop app.

## Stack

- React 19 + TypeScript
- Vite
- Tailwind CSS v4
- Dexie for local IndexedDB storage
- ECharts for analytics visualizations

## Working Rules

- Keep changes minimal and targeted.
- Preserve the light-only visual language and the current line-divided UI style.
- Do not reintroduce the retired counterparty-entity system.
- Prefer local-first behavior; do not add remote services or cloud assumptions without explicit approval.
- Keep amount handling in integer cents across parsing, storage, analytics, and rendering.
- When changing backup behavior, keep it aligned with the current schema version only.

## Release Notes

- Main release command: `pnpm dist`
- Windows-specific packaging: `pnpm dist:win`
- Mirror fallback packaging: `pnpm dist:win:mirror`
- Final packaged artifact is emitted into `release/`

## Repository Hygiene

- Do not commit `.mcp-debug-shell/`, `dist/`, `release/`, or `node_modules/`.
- Update `README.md` when user-facing workflows or release commands change.
- Update this file if repository-level conventions materially change.