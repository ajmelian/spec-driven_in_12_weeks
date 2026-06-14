# Release Checklist: [Version]

## Metadata

| Field | Value |
|---|---|
| Version | |
| Release date | |
| Environment | Production |
| Release owner | |
| Status | Planned / In Progress / Completed / Rolled back |

## 1. Release Scope

List features, fixes and migrations included.

## 2. Pre-Release Validation

- [ ] All tasks included in the release are completed.
- [ ] Tests pass locally.
- [ ] GitHub Actions pipeline passes.
- [ ] No unresolved critical security issue remains.
- [ ] `.env` changes have been documented.
- [ ] Database migrations have been reviewed.
- [ ] Rollback plan exists.

## 3. Build Preparation

```bash
composer validate
composer install --no-dev --optimize-autoloader
vendor/bin/phpunit
php spark config:check
```

## 4. Database Backup

```bash
mysqldump -u USER -p DATABASE_NAME > backup_pre_release_YYYYMMDD_HHMM.sql
```

- [ ] Backup created.
- [ ] Backup location verified.
- [ ] Restore procedure known.

## 5. SFTP Upload

- [ ] Excluded `.git/`.
- [ ] Excluded local `.env`.
- [ ] Excluded tests if not needed in production.
- [ ] Uploaded application files.
- [ ] Verified `public/` document root.
- [ ] Verified `writable/` permissions.

## 6. Production Commands

```bash
php spark migrate --all
php spark cache:clear
```

## 7. Smoke Tests

- [ ] Home page loads.
- [ ] Login works.
- [ ] 2FA works if enabled.
- [ ] Dashboard loads.
- [ ] Critical protected action is authorized correctly.
- [ ] Logs are being written.

## 8. Rollback Plan

Describe exact rollback steps.

## 9. Post-Release Notes

Document incidents, deviations and follow-up tasks.
