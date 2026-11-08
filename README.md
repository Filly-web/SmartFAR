# Britam Fixed Asset Governance, Reconciliation & Audit Portal

A standalone Flask application for fixed asset and intangible asset governance. It is deliberately separated from CETTP: it has its own application entry point, login, configuration, deployment service and database connection. It does not import CETTP code or reuse CETTP sessions/tables.

## Included modules

- Executive fixed asset dashboard and master FAR
- Asset creation, automatic asset numbering, asset details and QR tags
- Excel/CSV FAR import with mapping, staging and validation
- Depreciation processing and units-of-production usage capture
- Trial Balance imports, GL mappings and FAR/TB reconciliation
- Reconciliation investigations
- Asset transfers and disposals with retained history
- CWIP projects and capitalization
- Physical verification
- Insurance register and claims
- Tax WDV and deferred tax schedule capture
- Period locks and reopening controls
- Master FAR Excel/CSV/PDF exports
- Twelve-sheet audit pack generator and audit log

## Local setup

```bash
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

Set the environment variables in `.env` in your shell or hosting dashboard. For local development, if `FAR_DATABASE_URL` is not set, the application uses a separate `far_governance_dev.db` SQLite file in this project directory.

```bash
python app.py
```

Open `http://127.0.0.1:5000`.

## Production deployment (separate Render service)

1. Create a **new Render Web Service** from this project folder/repository. Do not replace or modify the CETTP service.
2. Create a **new, dedicated PostgreSQL database** for Fixed Asset Governance. Do not paste CETTP's database URL.
3. Set `FAR_DATABASE_URL` to the new database's internal connection string. The app normalizes standard PostgreSQL URL schemes to psycopg 3.
4. Set `FAR_SECRET_KEY`, `FAR_ADMIN_EMAIL` and `FAR_ADMIN_PASSWORD` to strong unique values.
5. Use `pip install -r requirements.txt` as the build command and the included Procfile start command.
6. Confirm `/health` returns `{"status":"ok",...}` and complete the smoke tests below before loading the official FAR.

The included `render.yaml` is a deployment template. Review the plan and attach a persistent PostgreSQL database before production use. Do not use ephemeral local SQLite for production.

## Database and separation

All fixed asset tables retain the `fa_` prefix. The standalone app creates these tables in the database supplied through `FAR_DATABASE_URL`. This application does not read or write CETTP's database unless an administrator deliberately misconfigures the connection string. Keep the database credentials separate.

## Security notes

- The included login is an initial single-administrator credential gate configured through environment variables; it is not a complete enterprise identity and multi-user RBAC implementation.
- Enable HTTPS at the hosting layer, use a strong secret, restrict database network access, and rotate credentials.
- Before go-live, implement named-user provisioning, granular roles, maker-checker approvals, password reset/MFA, rate limiting, secure object storage for documents, backups and restore tests.
- CSRF protection is enabled for form submissions. Upload size is capped at 25 MB.
- Run the application behind Gunicorn; do not enable Flask debug mode in production.

## Pre-go-live smoke tests

1. Sign in using the configured administrator credentials.
2. Open the dashboard and register one test asset.
3. Import a small FAR test workbook; inspect mapping and exceptions before committing.
4. Export Master FAR to XLSX, CSV and PDF.
5. Import a test TB and configure cost/accumulated depreciation GL mappings.
6. Run reconciliation and inspect a deliberately seeded variance.
7. Run depreciation for a test period and verify the run cannot be duplicated.
8. Test transfer, disposal, QR tag, verification, CWIP, insurance and tax schedule workflows.
9. Lock a test period and verify blocked transactions; test authorized reopening.
10. Verify audit history and database backup/restore.

## Important accounting deployment gate

Do not migrate the official company FAR or rely on outputs for statutory reporting until the organization's accounting policies, useful lives, residual values, depreciation conventions, tax pools/rates, GL mappings, opening balances, reconciliation tolerances and approval matrix have been configured and independently tested. This package is a working application foundation; the listed governance/security enhancements and user acceptance testing remain production prerequisites.
