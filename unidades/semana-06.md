# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 06 - RBAC, permisos granulares y autorización server-side

## Objetivo de la semana

Construir una matriz de permisos real, aplicarla en servidor y probar casos positivos y negativos. El objetivo no es ocultar botones: es impedir acciones no autorizadas aunque se invoquen manualmente por URL o formulario.

## Roles propuestos

| Rol | Uso |
|---|---|
| admin | control total de la aplicación |
| security_manager | gestión de auditoría, sesiones y eventos |
| operator | gestión operativa de contenidos o tickets |
| user | acceso básico al portal |

## Permisos propuestos

```text
users.view
users.create
users.update
users.disable
security.events.view
security.sessions.view
security.sessions.revoke
documents.view
documents.upload
documents.delete
tickets.view
tickets.create
tickets.reply
settings.update
```

## Matriz de autorización

`specs/modules/rbac/authorization-matrix.md`:

```md
# Authorization Matrix

| Permission | Admin | Security Manager | Operator | User |
|---|---:|---:|---:|---:|
| users.view | yes | no | no | no |
| users.create | yes | no | no | no |
| users.update | yes | no | no | no |
| users.disable | yes | no | no | no |
| security.events.view | yes | yes | no | no |
| security.sessions.view | yes | yes | no | no |
| security.sessions.revoke | yes | yes | no | no |
| documents.view | yes | yes | yes | yes |
| documents.upload | yes | no | yes | no |
| documents.delete | yes | no | no | no |
| tickets.view | yes | yes | yes | yes |
| tickets.create | yes | yes | yes | yes |
| tickets.reply | yes | no | yes | no |
| settings.update | yes | no | no | no |
```

## Regla de seguridad

Toda acción protegida debe validarse en el backend:

```php
if (! auth()->user()->can('users.update')) {
    throw \CodeIgniter\Exceptions\PageNotFoundException::forPageNotFound();
}
```

También se puede usar filtro, servicio o policy propia. Lo importante es centralizar la decisión y probarla.

## Filtro de permiso conceptual

`app/Filters/PermissionFilter.php`:

```php
<?php

declare(strict_types=1);

namespace App\Filters;

use CodeIgniter\Filters\FilterInterface;
use CodeIgniter\HTTP\RequestInterface;
use CodeIgniter\HTTP\ResponseInterface;
use CodeIgniter\HTTP\RedirectResponse;

final class PermissionFilter implements FilterInterface
{
    public function before(RequestInterface $request, $arguments = null)
    {
        $permission = $arguments[0] ?? null;

        if ($permission === null) {
            return service('response')->setStatusCode(403, 'Permission not configured');
        }

        $user = auth()->user();

        if ($user === null || ! $user->can($permission)) {
            return service('response')->setStatusCode(403, 'Forbidden');
        }

        return null;
    }

    public function after(RequestInterface $request, ResponseInterface $response, $arguments = null): void
    {
    }
}
```

Registra el filtro según la estructura de tu versión de CodeIgniter.

## Rutas protegidas por permiso

```php
$routes->group('security', ['filter' => 'session'], static function ($routes): void {
    $routes->get('events', 'Security\EventsController::index', [
        'filter' => 'permission:security.events.view',
    ]);
});
```

## Ocultar elementos en vista no es seguridad

Correcto para UX:

```php
<?php if (auth()->user()?->can('security.events.view')): ?>
    <a href="<?= site_url('security/events') ?>" class="nav-link">Eventos de seguridad</a>
<?php endif; ?>
```

Insuficiente como seguridad si no hay validación backend.

## Tests negativos obligatorios

| Caso | Resultado esperado |
|---|---|
| usuario anónimo accede a `/security/events` | redirect/login o 403 |
| usuario básico accede a `/security/events` | 403 |
| operator accede a `users.disable` | 403 |
| admin accede a `users.disable` | 200 |

Ejemplo:

```php
public function testBasicUserCannotViewSecurityEvents(): void
{
    $user = $this->createUserWithGroup('user');

    $result = $this->actingAs($user)->get('/security/events');

    $result->assertStatus(403);
}
```

## Tareas atómicas sugeridas

| ID | Título |
|---|---|
| TASK-RBAC-001 | Define authorization matrix |
| TASK-RBAC-002 | Configure Shield groups |
| TASK-RBAC-003 | Configure Shield permissions |
| TASK-RBAC-004 | Create permission filter |
| TASK-RBAC-005 | Protect security events route |
| TASK-RBAC-006 | Add authorization negative tests |

## OpenCode prompt

```text
Implement TASK-RBAC-004.
Read only the required context files listed in the task.
Do not modify unrelated routes.
Do not add UI changes.
Add tests if required by the task.
```

## Definition of Done semanal

- Existe matriz de permisos.
- Grupos y permisos están documentados.
- Al menos una ruta sensible está protegida.
- Hay tests positivos y negativos.
- Se demuestra que ocultar botones no sustituye autorización backend.
