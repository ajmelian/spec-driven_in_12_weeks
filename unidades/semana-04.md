# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 04 - Bootstrap del proyecto, estructura CodeIgniter, HTML5, Bootstrap 5 y base MySQL/MariaDB

## Objetivo de la semana

Configurar el esqueleto real del proyecto, crear la estructura base de vistas, políticas frontend, conexión de base de datos, primera migración propia y primeras pruebas de humo.

## Módulos cubiertos

| Módulo | Resultado |
|---|---|
| Frontend base | layout principal, layout auth, partials |
| Base de datos | conexión local, migración inicial, seed básico |
| Testing | prueba de arranque y rutas |
| Documentación | políticas frontend y database |

## Estructura frontend

```text
app/Views/
├── layouts/
│   ├── main.php
│   └── auth.php
├── partials/
│   ├── navbar.php
│   ├── sidebar.php
│   ├── flash_messages.php
│   └── breadcrumbs.php
├── dashboard/
│   └── index.php
└── auth/
```

## Layout principal

`app/Views/layouts/main.php`:

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
    <?= $this->include('partials/navbar') ?>

    <div class="container-fluid">
        <div class="row">
            <aside class="col-md-3 col-lg-2 bg-light border-end min-vh-100 p-3">
                <?= $this->include('partials/sidebar') ?>
            </aside>
            <main class="col-md-9 col-lg-10 p-4">
                <?= $this->include('partials/flash_messages') ?>
                <?= $this->renderSection('content') ?>
            </main>
        </div>
    </div>

    <script src="<?= base_url('assets/vendor/bootstrap/js/bootstrap.bundle.min.js') ?>"></script>
    <script src="<?= base_url('assets/js/app.js') ?>"></script>
</body>
</html>
```

## Partial de mensajes flash

`app/Views/partials/flash_messages.php`:

```php
<?php foreach (['success', 'warning', 'danger', 'info'] as $type): ?>
    <?php if (session()->has($type)): ?>
        <div class="alert alert-<?= esc($type, 'attr') ?> alert-dismissible fade show" role="alert">
            <?= esc(session($type)) ?>
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Cerrar"></button>
        </div>
    <?php endif; ?>
<?php endforeach; ?>
```

## Controlador dashboard

`app/Controllers/Dashboard.php`:

```php
<?php

declare(strict_types=1);

namespace App\Controllers;

use CodeIgniter\HTTP\ResponseInterface;

final class Dashboard extends BaseController
{
    public function index(): string
    {
        return view('dashboard/index', [
            'title' => 'Panel principal',
        ]);
    }
}
```

`app/Views/dashboard/index.php`:

```php
<?= $this->extend('layouts/main') ?>

<?= $this->section('content') ?>
    <h1 class="h3 mb-3">Panel principal</h1>
    <p class="text-muted">Portal seguro desarrollado con CodeIgniter 4, PHP 8.4 y Bootstrap 5.</p>
<?= $this->endSection() ?>
```

## Ruta inicial

`app/Config/Routes.php`:

```php
$routes->get('/', 'Dashboard::index');
```

## Política frontend

`specs/shared/frontend-policy.md`:

```md
# Frontend Policy

- Use server-side rendered HTML5 views.
- Use Bootstrap 5 stored locally.
- Do not use frontend frameworks.
- Use vanilla JavaScript only for progressive enhancement.
- Escape all dynamic output with `esc()`.
- Use semantic HTML where possible.
- Forms must include CSRF protection.
- Validation errors must be displayed safely.
- Do not load assets from external CDNs unless explicitly approved.
```

## Política de base de datos

`specs/shared/database-policy.md`:

```md
# Database Policy

- Use MySQL/MariaDB.
- Local database and production database must be separated.
- Never hardcode credentials.
- Store credentials in `.env`.
- All schema changes must use migrations.
- Development data must use seeders.
- Production data must not be copied locally unless anonymized.
- Before production migrations, create a database backup.
```

## Primera migración de aplicación

```bash
php spark make:migration CreateApplicationSettingsTable
```

Ejemplo:

```php
<?php

declare(strict_types=1);

namespace App\Database\Migrations;

use CodeIgniter\Database\Migration;

final class CreateApplicationSettingsTable extends Migration
{
    public function up(): void
    {
        $this->forge->addField([
            'id' => [
                'type' => 'INT',
                'constraint' => 10,
                'unsigned' => true,
                'auto_increment' => true,
            ],
            'setting_key' => [
                'type' => 'VARCHAR',
                'constraint' => 120,
            ],
            'setting_value' => [
                'type' => 'TEXT',
                'null' => true,
            ],
            'created_at' => [
                'type' => 'DATETIME',
                'null' => true,
            ],
            'updated_at' => [
                'type' => 'DATETIME',
                'null' => true,
            ],
        ]);

        $this->forge->addKey('id', true);
        $this->forge->addUniqueKey('setting_key');
        $this->forge->createTable('application_settings');
    }

    public function down(): void
    {
        $this->forge->dropTable('application_settings');
    }
}
```

Ejecuta:

```bash
php spark migrate
```

## Prueba de humo

Crea `tests/app/Controllers/DashboardTest.php`:

```php
<?php

declare(strict_types=1);

namespace Tests\App\Controllers;

use CodeIgniter\Test\CIUnitTestCase;
use CodeIgniter\Test\FeatureTestTrait;

final class DashboardTest extends CIUnitTestCase
{
    use FeatureTestTrait;

    public function testDashboardRendersSuccessfully(): void
    {
        $result = $this->get('/');

        $result->assertStatus(200);
        $result->assertSee('Panel principal');
    }
}
```

Ejecuta:

```bash
vendor/bin/phpunit
```

## Tareas atómicas sugeridas

| ID | Título | Dependencias |
|---|---|---|
| TASK-BASE-001 | Create base view structure | ninguna |
| TASK-BASE-002 | Add Bootstrap 5 local assets | TASK-BASE-001 |
| TASK-BASE-003 | Create dashboard controller and route | TASK-BASE-001 |
| TASK-BASE-004 | Add application settings migration | ninguna |
| TASK-BASE-005 | Add dashboard smoke test | TASK-BASE-003 |

## Definition of Done semanal

- Layout principal creado.
- Dashboard renderiza.
- Bootstrap se carga desde local.
- Existe política frontend.
- Existe política database.
- Primera migración ejecutada.
- Prueba de humo pasa.
