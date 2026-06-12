# LRS_Coalesce — Legacy Pipeline (being replaced by lrs_dbt)

> **⚠️ DEPRECATION NOTICE (2026-06-11): this Coalesce project is being migrated to dbt.**
> The replacement lives in the **`lrs_dbt`** repo (Azure DevOps, `Data Pipeline` project:
> `https://dev.azure.com/lrsrecycles/Data%20Pipeline/_git/lrs_dbt`). Per-node migration
> status is tracked in `lrs_dbt/seeds/migration_tracker.csv`.
>
> **Before building or changing a node here, check the tracker** — if the node is already
> rebuilt in dbt, make the change there instead. This repo stays live only until the dbt
> migration completes; changes here should be limited to keeping still-live nodes running.

## High-consequence nodes — do not modify casually

The FINANCE engines (e.g. `TRUX_REVENUE_CHANGE`, AR/GL/AP transaction nodes such as
`TRUX_AR_TRANSACTIONS`, `GL_Transactions`, `VW_CUSTOMER_SITE_REVENUE`) feed finance
reporting and the GL bridge. They are intentionally **not** being wrapped or naively
rewritten during the dbt migration — coordinate any change with the data team first.

## Workflows

- `Master_Deploy.yml` — deploys via `coa deploy` (push-to-`main` trigger / manual dispatch;
  note this repo's default branch is `Master`, so in practice runs are manual).
- `Workflow_Scheduler.yml` / `DEV REFRESH.yml` — scheduled data refreshes (cron).
- `Sage_Refresh_Dev.yml` / `Sage_Refresh_Prod.yml` — dispatch-only; Orchestra owns scheduling.

Credentials live in GitHub Actions secrets (`COA_CONFIG`, Snowflake key secrets) — never in YAML.
