# Standalone Fixed Asset Governance release

- Extracted Fixed Asset module into an independent Flask application.
- Added a dedicated login/session boundary and CSRF protection.
- Added independent `FAR_DATABASE_URL` configuration and local SQLite fallback.
- Added standalone responsive navigation, sign-in page, health endpoint and Render deployment template.
- Retained the `fa_*` namespaced data model, fixed asset workflows, reporting and exports.
- No CETTP source code, login session, database connection or portal navigation is included.
