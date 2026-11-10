# Standalone Fixed Asset Governance release

- Extracted Fixed Asset module into an independent Flask application.
- Added a dedicated login/session boundary and CSRF protection.
- Added independent `FAR_DATABASE_URL` configuration and local SQLite fallback.
- Added standalone responsive navigation, sign-in page, health endpoint and Render deployment template.
- Retained the `fa_*` namespaced data model, fixed asset workflows, reporting and exports.
- No CETTP source code, login session, database connection or portal navigation is included.


## V1.1 – 2025 opening FAR and monthly depreciation
- Added a dedicated 2025 opening FAR import mode that bypasses historical accounting/depreciation validations while preserving source balances.
- Established 31 December 2025 as the opening position and restricted depreciation runs to January 2026 onward after the opening batch is committed.
- Added depreciation rate mapping for reducing-balance assets and updated import guidance.
- Kept standard subsequent imports subject to the existing validation process.

## V1.2 – Opening FAR and batch templates
- Added Downloads & Import Templates workspace in the standalone FAR portal.
- Added downloadable 2025 opening FAR workbook, plus export of current stored FAR balances after opening migration.
- Opening 2025 migration bypasses accounting/depreciation validation; preserves supplied historical numeric balances and supplies only database-safe defaults for missing required identifiers/descriptions/dates. Duplicate database asset numbers are re-numbered and duplicate tags are left blank rather than blocking the opening import.
- Added new asset batch upload and disposal batch upload pages and Excel templates.
- Disposal NBV and gain/loss are computed from the asset's portal balance at processing time.
