# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 05 - CodeIgniter Shield: autenticación segura

## Objetivo de la semana

Instalar, configurar y comprender CodeIgniter Shield como base de autenticación y autorización. Esta semana crea el núcleo de seguridad del portal.

## Alcance

| Incluido | Excluido |
|---|---|
| Instalación de Shield | 2FA avanzado |
| Migraciones de Shield | Integración LDAP/OAuth |
| Login/logout | Registro público si no está contratado |
| Usuario administrador inicial | Recuperación avanzada de cuenta |
| Protección del dashboard | Personalización estética completa |

## Instalación

```bash
composer require codeigniter4/shield
php spark shield:setup
php spark migrate --all
```

Revisa qué archivos se han creado o modificado antes de aceptar el commit.

```bash
git status
git diff
```

## Configuración mínima

Revisa `app/Config/Auth.php` o el archivo equivalente generado por Shield. Documenta cualquier cambio en un ADR:

```bash
touch docs/adr/ADR-0001-use-codeigniter-shield.md
```

ADR ejemplo:

```md
# ADR-0001 - Use CodeIgniter Shield for authentication

## Status

Accepted

## Context

The application requires secure authentication, role-based authorization and extensibility for two-factor authentication.

## Decision

Use CodeIgniter Shield as official authentication and authorization package for CodeIgniter 4.

## Consequences

- Avoid custom password authentication.
- Reuse Shield migrations, entities and filters.
- Extend Shield only where the project requires 2FA and step-up authentication.
```

## Crear usuario administrador inicial

Puedes crear un seeder específico:

```bash
php spark make:seeder AdminUserSeeder
```

Ejemplo conceptual:

```php
<?php

declare(strict_types=1);

namespace App\Database\Seeds;

use CodeIgniter\Database\Seeder;
use CodeIgniter\Shield\Entities\User;

final class AdminUserSeeder extends Seeder
{
    public function run(): void
    {
        $users = auth()->getProvider();

        $email = 'admin@example.test';

        if ($users->findByCredentials(['email' => $email]) !== null) {
            return;
        }

        $user = new User([
            'username' => 'admin',
            'email' => $email,
            'password' => 'ChangeThisPassword123!',
            'active' => true,
        ]);

        $users->save($user);

        $createdUser = $users->findByCredentials(['email' => $email]);
        $createdUser?->addGroup('admin');
    }
}
```

No uses esta contraseña en producción. En producción el usuario inicial debe crearse mediante procedimiento controlado y cambiar la contraseña en el primer acceso.

## Proteger dashboard

En rutas:

```php
$routes->group('', ['filter' => 'session'], static function ($routes): void {
    $routes->get('/', 'Dashboard::index');
});
```

La protección exacta puede variar según la configuración de Shield. Lo importante es que el dashboard no sea público.

## Vista login con Bootstrap 5

`app/Views/auth/login.php`:

```php
<?= $this->extend('layouts/auth') ?>

<?= $this->section('content') ?>
<div class="card shadow-sm">
    <div class="card-body p-4">
        <h1 class="h4 mb-3">Acceso seguro</h1>

        <?= $this->include('partials/flash_messages') ?>

        <form method="post" action="<?= url_to('login') ?>" autocomplete="off" novalidate>
            <?= csrf_field() ?>

            <div class="mb-3">
                <label for="email" class="form-label">Email</label>
                <input
                    type="email"
                    class="form-control"
                    id="email"
                    name="email"
                    value="<?= esc(old('email'), 'attr') ?>"
                    required
                    maxlength="190"
                    autocomplete="username"
                >
            </div>

            <div class="mb-3">
                <label for="password" class="form-label">Contraseña</label>
                <input
                    type="password"
                    class="form-control"
                    id="password"
                    name="password"
                    required
                    autocomplete="current-password"
                >
            </div>

            <button type="submit" class="btn btn-primary w-100">Entrar</button>
        </form>
    </div>
</div>
<?= $this->endSection() ?>
```

## Tests mínimos

| Test | Objetivo |
|---|---|
| login renderiza | la ruta de login responde 200 |
| dashboard protegido | usuario anónimo no accede |
| login inválido | no revela si existe el email |
| usuario autenticado | accede al dashboard |

Ejemplo de test conceptual:

```php
public function testAnonymousUserCannotAccessDashboard(): void
{
    $result = $this->get('/');

    $result->assertRedirect();
}
```

## Riesgos OWASP cubiertos

| Riesgo | Control |
|---|---|
| Identification and Authentication Failures | Shield, hashing seguro, errores genéricos |
| Broken Access Control | filtros de rutas, grupos y permisos |
| Security Misconfiguration | `.env`, entorno dev/prod separado |
| Software and Data Integrity Failures | Composer lock, revisión de dependencias |

## Prompts OpenCode

Generar tareas:

```text
Read specs/modules/authentication/*.md and specs/shared/*.md.
Generate atomic tasks for installing and configuring CodeIgniter Shield.
Do not implement code.
```

Ejecutar tarea:

```text
Implement TASK-AUTH-001.
Read only the required_context files declared in the task.
Do not implement 2FA or RBAC beyond what the task requires.
After implementation, list changed files and verification commands.
```

## Definition of Done semanal

- Shield instalado.
- Migraciones ejecutadas.
- Login/logout disponible.
- Dashboard protegido.
- Usuario admin local creado.
- Tests básicos de autenticación añadidos.
- ADR de Shield documentado.
