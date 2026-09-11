# Pinterest publication runbook

## Purpose

Allow the **Useful Tech & Home Gear — Web, SEO & Amazon** project to execute the same GitHub-controlled Pinterest production/publication procedure that successfully ran on 2026-09-09, without depending on Work as the operator and without using Google Drive as project state.

## Repositories

- Canonical project repository: `usefultechhomegear/usefultechhomegear.github.io`
- Authorized Pinterest execution repository: `usefultechhomegear/pinterest-amazon-car-safety-tech`
- The execution repository is an operational dependency only. It must not replace the canonical project repository.

## Control path

All execution requests are issued by updating:

`.github/pinterest/control/command.json`

in `usefultechhomegear/pinterest-amazon-car-safety-tech`.

The external dispatcher observes the GitHub control commit, dispatches `.github/workflows/pinterest-orchestrator.yml`, and the orchestrator persists durable request/publication/monitoring state back into GitHub.

Supported commands used by the canonical runtime:

- `generate-pending-products`
- `approve-publish-monitor`
- `rebuild-pinterest-history`
- `control-plane-smoke`

## Manual chat procedure

### 1. Generate pending products

When Sebastián instructs **“Genere los productos pendientes”**:

1. Read current Pinterest operational state and full publication history from the execution repository.
2. Build a new unique request ID and daily batch ID.
3. Write `command.json` with `enabled: true`, command `generate-pending-products`, and GitHub as `sourceOfTruth`.
4. Use the production rules currently in force. The 2026-09-09 successful baseline used:
   - 10 target products.
   - research across all active boards (12 at that execution).
   - commercial hard gates.
   - deduplication against the full publication history.
   - semantic board/product matching.
   - launch balance of at least 4 automotive, 4 home-tech and 2 wildcard products by commercial score.
   - maximum 2 products per board.
   - two-day board rotation.
   - ChatGPT-only creative generation.
   - publication disabled during this stage.
5. Verify the dispatcher/orchestrator completed and read the generated batch from `.github/pinterest/creative/batches/<batchId>/batch.json`.
6. Verify assets, metadata, Amazon URLs/tracking IDs, boards and pin count.
7. Report the generated products/pins and wait for the literal manual approval phrase **“Aprobados”**.

### 2. Publish approved products

When Sebastián says **“Aprobados”** for the current generated batch:

1. Read the current batch and approved pin count from GitHub; never reconstruct these values from chat memory.
2. Update `.github/pinterest/control/command.json` with a new unique request ID and command `approve-publish-monitor`.
3. Include the exact current `batchId`, `batchPath`, `sourceRequestId`, `userApprovalPhrase: "Aprobados"`, `publicationAllowed: true` and the actual approved pin count.
4. Commit the control request.
5. Verify the external dispatcher starts the request and the GitHub orchestrator executes it.
6. Verify every approved pin reaches `publicationStatus: PUBLISHED`.
7. Verify every pin has a real Pinterest Pin ID and URL, verified board assignment and correct destination URL.
8. Verify baseline monitoring is captured immediately and persisted under `.github/pinterest/monitoring/`.
9. Verify the durable request state and publication records are committed in GitHub.
10. Only then report **“Publicación Finalizada”** and the published pin count.

## Autonomous Task procedure

For an autonomous scheduled production Task, do **not** stop at the manual approval checkpoint. If the Task policy authorizes autonomous publication, execute the full sequence in one run:

`generate → commercial gates → deduplication → board match → creative generation → publish → persist Pin IDs/URLs → baseline monitoring`.

The autonomous Task must not require the literal `Aprobados` phrase and must not leave an otherwise valid daily batch waiting for user interaction.

## Successful reference execution — 2026-09-09

Batch: `DAILY-PRODUCTION-20260909-1406-ART`

Observed result:

- 10 selected products.
- 13 generated/published Pins.
- 12 boards investigated.
- 30 candidates found.
- 714 historical duplicates rejected.
- Publication control request: `APPROVE-PUBLISH-20260909-184723Z`.
- Control commit: `cfedc8597ec26a990843a5052063085abe21e779`.
- Execution commit: `7317de2e6509510cf385e24d847da0ac52259afe`.

That execution published all 13 approved Pins, persisted publication records, verified board assignments, and captured baseline Pinterest metrics.

## Fail-closed rules

- GitHub is the only durable source of truth.
- Do not use Google Drive as fallback, memory, audit source or project state.
- Do not invent batch IDs, pin counts, Pinterest IDs, URLs or completion status.
- If the dispatcher, GitHub Actions, Pinterest authentication or repository permissions fail, persist/read the failure state when possible and report the blocker instead of claiming completion.
