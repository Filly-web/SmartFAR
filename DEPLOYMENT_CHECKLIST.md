# Separate service deployment checklist

- [ ] Create a new repository or deploy this folder as a new service.
- [ ] Create a new PostgreSQL database dedicated to Fixed Asset Governance.
- [ ] Configure `FAR_DATABASE_URL` with that database's internal URL (never CETTP's URL).
- [ ] Configure a unique `FAR_SECRET_KEY`.
- [ ] Configure `FAR_ADMIN_EMAIL` and a unique strong `FAR_ADMIN_PASSWORD`.
- [ ] Set `FLASK_ENV=production` so session cookies are marked Secure behind HTTPS.
- [ ] Deploy with the included Procfile / Render service definition.
- [ ] Verify `/health`, login, FAR import preview, asset creation and all export endpoints.
- [ ] Configure automated PostgreSQL backups and test a restore.
- [ ] Complete role provisioning and maker-checker controls before production data migration.
