# SpacetimeDB AI Test Results

Generated test data from the AI app-generation benchmarks comparing SpacetimeDB against other backends.

Results viewer: https://spacetimedb.com/llms-benchmark-sequential-upgrade

## What's here

### `sequential-upgrade/`

Full data from the sequential upgrade benchmark comparing SpacetimeDB vs PostgreSQL (Express + Socket.io + Drizzle ORM). Same AI model (Claude Sonnet 4.6), same prompts, same chat app, two backends, upgraded through 12 feature levels with manual grading and OpenTelemetry cost tracking.

Each run directory contains:

- `METRICS_DATA.json` / `METRICS_REPORT.json`: aggregated cost, bug count, and LOC data
- `spacetime/` and `postgres/` subdirectories, each with:
  - `results/chat-app-.../`: the full L1-L12 AI-generated app source
    - `backend/` + `client/`: current (L12) app state
    - `level-1/` through `level-11/`: snapshots of app state before each subsequent upgrade
    - `ITERATION_LOG.md`: per-iteration fix history
    - `BUG_REPORT.md`: last bug report filed against the app
  - `telemetry/<session-id>/`: per-session OTel cost data
    - `cost-summary.json`: structured token/cost totals
    - `COST_REPORT.md`: human-readable summary
    - `metadata.json`: session metadata (start/end, level, mode)
  - `inputs/`: frozen copies of the prompts used for that run (reproducibility)

### Two runs

- **`sequential-upgrade-20260403/`**: original methodology
- **`sequential-upgrade-20260406/`**: refined methodology (domain bias removed from SpacetimeDB SDK docs, PostgreSQL instructions made feature-spec-neutral)

Both runs reach the same conclusion: SpacetimeDB apps are cheaper to build, have fewer bugs, and require fewer fix iterations.

## Reproducibility

Source code for each level is tracked. Build artifacts (`node_modules/`, `dist/`, `drizzle/`) and local `.env` files are excluded. To rebuild any level:

```bash
cd <run>/<backend>/results/chat-app-*/
cd client && npm install
cd ../server && npm install  # postgres only
cd ../backend/spacetimedb && npm install  # spacetime only
# follow CLAUDE.md in the app directory for deploy steps
```

## Tooling

The benchmark harness, grading scripts, and performance benchmark tool live in the main SpacetimeDB repo under [`tools/llm-sequential-upgrade/`](https://github.com/clockworklabs/SpacetimeDB/tree/master/tools/llm-sequential-upgrade).
