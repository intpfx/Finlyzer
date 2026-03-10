# Finlyzer

Finlyzer is a local-first Electron desktop app for reconciling personal or
small-business cashflow across Alipay, WeChat, and supported bank exports. It
keeps the workflow centered on one transaction table: import, classify, inspect
mirrors, review accruals, and run chart analysis without sending data to a
server.

Current production version: `1.0.0`

## What It Does

- Imports Alipay and WeChat CSV statements plus supported bank statement
  workbooks.
- Detects source format automatically during import.
- Stores transactions, category trees, import jobs, and UI metadata locally in
  IndexedDB via Dexie.
- Lets you classify transactions inline in the main table with direction-aware
  category choices.
- Supports manual mirror grouping and ungrouping so duplicate cash movements do
  not double-count in analytics.
- Supports manual accrual records for future payable or receivable items, then
  later manual settlement review against real cash transactions.
- Supports per-row amount spread analysis from the amount cell.
- Includes charts for trend, category distribution, unified calendar heatmap,
  and category-by-time matrix.
- Supports JSON backup export and full restore.

## Main Workflow

1. Import one or more statement files.
2. Clear the uncategorized rows at the top of the table.
3. Review mirror relations and settlement candidates where needed.
4. Use the chart workbench to focus the table by date, category, or month.
5. Export a backup when the local dataset reaches a stable state.

## Storage Model

Finlyzer is local-first.

- Data lives on the current machine.
- The app stores records in IndexedDB tables managed by Dexie.
- Backup export writes a JSON snapshot of the current data model.
- Backup restore accepts only backups produced by the current schema version.

The current local stores are:

- `transactions`
- `importJobs`
- `categoryTrees`
- `appMeta`

## Tech Stack

- Electron
- React 19
- TypeScript
- Vite
- Tailwind CSS v4
- Dexie
- ECharts

## Development

Install dependencies:

```bash
pnpm install
```

Start renderer + Electron shell:

```bash
pnpm dev
```

Start with remote debugging exposed for DevTools/MCP inspection:

```bash
pnpm dev:debug
```

Run lint:

```bash
pnpm lint
```

Build renderer:

```bash
pnpm build
```

Run Electron against the latest local build:

```bash
pnpm start
```

## Packaging

Create the Windows portable release:

```bash
pnpm dist
```

Equivalent explicit command:

```bash
pnpm dist:win
```

If Electron download mirrors are needed during packaging:

```bash
pnpm dist:win:mirror
```

Successful packaging keeps the final artifact in `release/`.

Example current artifact name:

```text
release/Finlyzer-1.0.0-windows-portable.exe
```

## Repository Notes

- `.mcp-debug-shell/` is temporary local debug-shell output and should not be
  committed.
- Build output in `dist/` and packaged artifacts in `release/` are ignored.
- The repository no longer carries a local `mcp-debug-shell` skill copy; use the
  global personal skill instead.

## Limitations

- No cloud sync or collaboration.
- Backup restore is strict to the current backup format.
- Very large datasets may need future query/pagination optimization before
  storage capacity itself becomes a problem.
- Bank import currently supports the known workbook layout already validated in
  this repo, not arbitrary bank formats.
