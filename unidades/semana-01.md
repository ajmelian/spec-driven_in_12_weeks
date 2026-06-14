# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 01 - Entorno local profesional con PHP 8.4, CodeIgniter 4, VS Code, GitHub y OpenCode

## Objetivo de la semana

Construir un entorno local reproducible, documentado y apto para desarrollar aplicaciones seguras en CodeIgniter 4 con PHP 8.4. Al finalizar tendrás un repositorio inicial, OpenCode instalado, GitHub preparado, MySQL/MariaDB local, Bootstrap 5 local y una primera estructura documental.

## Resultado esperado

Al terminar esta semana debes tener:

| Entregable | Ubicación | Criterio de aceptación |
|---|---|---|
| Proyecto CodeIgniter 4 creado | raíz del repositorio | `php spark serve` funciona |
| Repositorio Git inicializado | `.git/` | primer commit creado |
| Repositorio remoto | GitHub | push realizado correctamente |
| OpenCode operativo | terminal de VS Code | `opencode` abre el agente |
| Bootstrap 5 local | `public/assets/vendor/bootstrap/` | no se usa CDN |
| Base de datos local | MySQL/MariaDB | conexión desde CI4 validada |
| Documento de entorno local | `docs/deployment/local-environment.md` | contiene versiones y comandos |

## Decisiones técnicas

| Decisión | Motivo |
|---|---|
| PHP 8.4 | formación moderna y alineada con el ciclo actual de PHP |
| CodeIgniter 4.6+ | requisito para PHP 8.4 y framework moderno |
| MySQL/MariaDB | SGBD objetivo del proyecto |
| HTML5 + Bootstrap 5 | frontend suficiente sin introducir frameworks JavaScript |
| OpenCode | agente IA desde terminal integrado en VS Code |
| SFTP | despliegue manual controlado, sin Docker |

## Instalación base en Ubuntu

Comprueba versiones:

```bash
php -v
composer --version
git --version
mysql --version
```

Si necesitas instalar PHP 8.4, usa el mecanismo apropiado para tu Ubuntu. En una estación Ubuntu con repositorios compatibles puedes comprobar módulos instalados con:

```bash
php -m
```

Extensiones habituales para CodeIgniter 4:

```text
intl
mbstring
json
mysqlnd
curl
xml
zip
openssl
```

## Crear el proyecto

```bash
mkdir -p ~/proyectos/formacion-secure-ai
cd ~/proyectos/formacion-secure-ai
composer create-project codeigniter4/appstarter ci4-secure-ai-portal
cd ci4-secure-ai-portal
```

Arranca el servidor local:

```bash
php spark serve
```

Abre el navegador:

```text
http://localhost:8080
```

## Inicializar Git

```bash
git init
git branch -M main
git status
git add .
git commit -m "chore: bootstrap CodeIgniter 4 project"
```

Crea el repositorio en GitHub y conecta el remoto:

```bash
git remote add origin git@github.com:TU_USUARIO/ci4-secure-ai-portal.git
git push -u origin main
```

## Estructura inicial de directorios

```bash
mkdir -p specs/client specs/shared specs/modules specs/tasks docs/adr docs/deployment docs/security docs/database docs/frontend .opencode/commands public/assets/css public/assets/js public/assets/vendor/bootstrap
```

Crea archivos base:

```bash
touch README.md SECURITY.md AGENTS.md opencode.json
touch specs/tasks/index.md
touch specs/shared/definition-of-done.md specs/shared/security-baseline.md specs/shared/coding-standards.md specs/shared/testing-policy.md
touch docs/deployment/local-environment.md docs/deployment/remote-server.md docs/deployment/sftp-release-checklist.md docs/deployment/rollback.md
```

## Configuración inicial de `.env`

Copia el archivo de entorno:

```bash
cp env .env
```

Ajustes mínimos:

```ini
CI_ENVIRONMENT = development
app.baseURL = 'http://localhost:8080/'

security.csrfProtection = 'cookie'
security.tokenRandomize = true
security.tokenName = 'csrf_token_name'
security.headerName = 'X-CSRF-TOKEN'
security.cookieName = 'csrf_cookie_name'
security.expires = 7200
security.regenerate = true
security.redirect = true
security.samesite = 'Lax'

database.default.hostname = localhost
database.default.database = ci4_secure_ai_portal_dev
database.default.username = ci4_dev
database.default.password = CAMBIAR_PASSWORD_LOCAL
database.default.DBDriver = MySQLi
database.default.DBPrefix =
database.default.port = 3306
```

Nunca subas `.env` a GitHub.

## Base de datos local

```sql
CREATE DATABASE ci4_secure_ai_portal_dev CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'ci4_dev'@'localhost' IDENTIFIED BY 'CAMBIAR_PASSWORD_LOCAL';
GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, ALTER, INDEX, DROP ON ci4_secure_ai_portal_dev.* TO 'ci4_dev'@'localhost';
FLUSH PRIVILEGES;
```

Comprueba acceso:

```bash
mysql -u ci4_dev -p ci4_secure_ai_portal_dev
```

## Bootstrap 5 local

Descarga Bootstrap 5 y copia los archivos mínimos a:

```text
public/assets/vendor/bootstrap/css/bootstrap.min.css
public/assets/vendor/bootstrap/js/bootstrap.bundle.min.js
```

No uses CDN durante la formación. Esto reduce dependencias externas y permite controlar versiones.

## Primer layout HTML5 con Bootstrap 5

Crea `app/Views/layouts/main.php`:

```php
<!doctype html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <meta name="robots" content="noindex,nofollow">
    <title><?= esc($title ?? 'Secure AI Portal') ?></title>
    <link rel="stylesheet" href="<?= base_url('assets/vendor/bootstrap/css/bootstrap.min.css') ?>">
    <link rel="stylesheet" href="<?= base_url('assets/css/app.css') ?>">
</head>
<body>
    <main class="container py-4">
        <?= $this->renderSection('content') ?>
    </main>
    <script src="<?= base_url('assets/vendor/bootstrap/js/bootstrap.bundle.min.js') ?>"></script>
    <script src="<?= base_url('assets/js/app.js') ?>"></script>
</body>
</html>
```

Observa el uso de `esc()` para salida dinámica. Esto será obligatorio en toda la formación.

## Instalar OpenCode

Instalación habitual:

```bash
curl -fsSL https://opencode.ai/install | bash
```

O mediante npm si ya tienes Node instalado:

```bash
npm install -g opencode-ai
```

Desde el terminal integrado de VS Code:

```bash
opencode
```

Comandos iniciales dentro de OpenCode:

```text
/connect
/init
```

## `opencode.json` inicial

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "AGENTS.md",
    "specs/shared/definition-of-done.md",
    "specs/shared/security-baseline.md",
    "specs/shared/coding-standards.md",
    "specs/shared/testing-policy.md"
  ]
}
```

## Primer `AGENTS.md`

```md
# Agent Rules

This project uses PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, MySQL/MariaDB, HTML5, Bootstrap 5, PHPUnit, GitHub Actions and manual SFTP deployment.

Do not introduce Docker, Docker Compose, Kubernetes, React, Vue, Angular, Svelte, Vite or Webpack unless explicitly requested.

All implementation work must start from an atomic task file under specs/tasks/.

The agent must read only the context files declared in the selected task unless additional code inspection is strictly necessary.
```

## Comandos de verificación

```bash
php -v
composer validate
php spark serve
php spark routes
git status
```

## Tarea práctica

Crea `docs/deployment/local-environment.md` con:

| Apartado | Contenido mínimo |
|---|---|
| Sistema operativo | versión y arquitectura |
| PHP | versión y extensiones |
| Composer | versión |
| MySQL/MariaDB | versión |
| VS Code | extensiones usadas |
| OpenCode | método de instalación |
| Proyecto | ruta local |
| URL local | `http://localhost:8080` |

## Definition of Done semanal

La semana está terminada cuando:

- El proyecto arranca localmente.
- Git funciona y hay push en GitHub.
- OpenCode se abre desde VS Code.
- La base de datos local existe.
- Bootstrap 5 está almacenado localmente.
- `.env` no aparece en `git status` como archivo versionado.
- Existe documentación básica del entorno local.
