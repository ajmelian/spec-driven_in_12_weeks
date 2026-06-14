# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 11 - GitHub Actions, base de datos local/prod, release manual y despliegue SFTP

## Objetivo de la semana

Formalizar el paso de desarrollo local a producción sin Docker y sin despliegue automático. Se usará GitHub Actions para validación y SFTP para subida manual controlada.

## Flujo de release

```text
Desarrollo local
    ↓
Tests locales
    ↓
Commit
    ↓
Push a GitHub
    ↓
GitHub Actions valida
    ↓
Tag/release
    ↓
Preparar paquete local
    ↓
Backup producción
    ↓
Subida SFTP
    ↓
Migraciones producción
    ↓
Verificación
    ↓
Registro en CHANGELOG
```

## GitHub Actions básico

`.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  phpunit:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.4'
          extensions: intl, mbstring, mysqli, curl, xml, zip
          coverage: none

      - name: Validate composer
        run: composer validate --strict

      - name: Install dependencies
        run: composer install --no-interaction --prefer-dist

      - name: Run tests
        run: vendor/bin/phpunit
```

## Producción: estructura recomendada

```text
/home/usuario/apps/ci4-secure-ai-portal/
├── releases/
│   ├── 2026-01-01_001/
│   └── 2026-01-02_002/
├── current -> releases/2026-01-02_002
├── shared/
│   ├── .env
│   └── writable/
└── backups/
```

El document root del servidor debe apuntar a:

```text
/home/usuario/apps/ci4-secure-ai-portal/current/public
```

No debe apuntar a la raíz del proyecto.

## Archivos que no se suben

`.deployignore` conceptual:

```text
.git/
.github/
tests/
specs/
docs/
.env
node_modules/
writable/logs/*
writable/cache/*
planning/
tools/dev-only/
```

## Preparar release local

`scripts/prepare-release.sh`:

```bash
#!/usr/bin/env bash
set -euo pipefail

VERSION="${1:-}"

if [ -z "$VERSION" ]; then
  echo "Usage: ./scripts/prepare-release.sh v0.1.0"
  exit 1
fi

BUILD_DIR="build/${VERSION}"

rm -rf "$BUILD_DIR"
mkdir -p "$BUILD_DIR"

composer validate --strict
composer install --no-dev --optimize-autoloader
vendor/bin/phpunit
php spark config:check || true

rsync -av \
  --exclude='.git' \
  --exclude='.github' \
  --exclude='tests' \
  --exclude='specs' \
  --exclude='docs' \
  --exclude='.env' \
  --exclude='build' \
  ./ "$BUILD_DIR/"

echo "Release prepared at $BUILD_DIR"
```

Hazlo ejecutable:

```bash
chmod +x scripts/prepare-release.sh
```

## Backup de producción

```bash
mysqldump -u usuario_prod -p ci4_secure_ai_portal_prod > backups/backup_pre_release_$(date +%Y%m%d_%H%M%S).sql
```

## Checklist SFTP

`docs/deployment/sftp-release-checklist.md`:

```md
# SFTP Release Checklist

## Before Upload

- [ ] Local branch is clean.
- [ ] Tests pass locally.
- [ ] GitHub Actions is green.
- [ ] Version tag created.
- [ ] Release package prepared.
- [ ] Production backup created.
- [ ] Pending migrations reviewed.
- [ ] `.env` production values checked.

## Upload

- [ ] Upload release folder via SFTP.
- [ ] Verify file permissions.
- [ ] Ensure `writable/` is writable by web server.
- [ ] Ensure document root points to `public/`.

## After Upload

- [ ] Run migrations.
- [ ] Clear cache.
- [ ] Verify login.
- [ ] Verify 2FA.
- [ ] Verify dashboard.
- [ ] Verify security events.
- [ ] Check application logs.
- [ ] Update CHANGELOG.md.
```

## Migraciones producción

```bash
php spark migrate --all
php spark cache:clear
```

No ejecutes migraciones sin backup previo.

## Rollback

Si usas estructura `current -> release`, el rollback es cambiar el symlink a la release anterior y restaurar base de datos si hubo migración irreversible.

Documento `docs/deployment/rollback.md`:

```md
# Rollback Procedure

## Code rollback

1. Identify previous stable release.
2. Point `current` symlink to previous release.
3. Clear cache.
4. Verify application.

## Database rollback

1. Stop writes if possible.
2. Restore pre-release backup.
3. Verify migrations state.
4. Verify critical flows.
```

## Definition of Done semanal

- GitHub Actions ejecuta tests.
- Existe script de preparación de release.
- Existe checklist SFTP.
- Existe política de backup.
- Existe procedimiento de rollback.
- Producción y desarrollo tienen bases separadas.
