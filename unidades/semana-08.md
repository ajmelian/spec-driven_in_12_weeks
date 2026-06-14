# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 08 - Sesiones seguras, auditoría, eventos de seguridad y logging

## Objetivo de la semana

Implementar una capa de auditoría y gestión de eventos de seguridad. El sistema debe registrar acciones relevantes sin almacenar secretos y permitir trazabilidad operativa.

## Eventos mínimos

| Evento | Severidad | Datos permitidos |
|---|---|---|
| login_success | info | user_id, ip, user_agent |
| login_failed | warning | ip, user_agent, username/email normalizado si aplica |
| logout | info | user_id, ip |
| two_factor_enabled | info | user_id |
| two_factor_failed | warning | user_id, ip |
| step_up_success | info | user_id |
| permission_denied | warning | user_id, route, permission |
| user_disabled | warning | actor_id, target_user_id |
| file_upload_rejected | warning | user_id, reason |

## Datos prohibidos en logs

```text
passwords
TOTP codes
recovery codes
API keys
session IDs completos
tokens CSRF
secretos TOTP
contenido sensible de documentos
```

## Migración de eventos

```bash
php spark make:migration CreateSecurityEventsTable
```

```php
<?php

declare(strict_types=1);

namespace App\Database\Migrations;

use CodeIgniter\Database\Migration;

final class CreateSecurityEventsTable extends Migration
{
    public function up(): void
    {
        $this->forge->addField([
            'id' => ['type' => 'BIGINT', 'unsigned' => true, 'auto_increment' => true],
            'event_type' => ['type' => 'VARCHAR', 'constraint' => 80],
            'severity' => ['type' => 'VARCHAR', 'constraint' => 20],
            'actor_user_id' => ['type' => 'INT', 'unsigned' => true, 'null' => true],
            'target_user_id' => ['type' => 'INT', 'unsigned' => true, 'null' => true],
            'ip_address' => ['type' => 'VARCHAR', 'constraint' => 45, 'null' => true],
            'user_agent' => ['type' => 'VARCHAR', 'constraint' => 255, 'null' => true],
            'route' => ['type' => 'VARCHAR', 'constraint' => 255, 'null' => true],
            'metadata_json' => ['type' => 'JSON', 'null' => true],
            'created_at' => ['type' => 'DATETIME', 'null' => true],
        ]);

        $this->forge->addKey('id', true);
        $this->forge->addKey('event_type');
        $this->forge->addKey('severity');
        $this->forge->addKey('actor_user_id');
        $this->forge->addKey('created_at');
        $this->forge->createTable('security_events');
    }

    public function down(): void
    {
        $this->forge->dropTable('security_events');
    }
}
```

## Modelo

```php
<?php

declare(strict_types=1);

namespace App\Models;

use CodeIgniter\Model;

final class SecurityEventModel extends Model
{
    protected $table = 'security_events';
    protected $primaryKey = 'id';
    protected $allowedFields = [
        'event_type',
        'severity',
        'actor_user_id',
        'target_user_id',
        'ip_address',
        'user_agent',
        'route',
        'metadata_json',
        'created_at',
    ];

    protected $useTimestamps = false;
}
```

## Servicio de auditoría

```php
<?php

declare(strict_types=1);

namespace App\Services\Audit;

use App\Models\SecurityEventModel;
use CodeIgniter\HTTP\IncomingRequest;

final class SecurityAuditService
{
    public function __construct(
        private readonly SecurityEventModel $events = new SecurityEventModel(),
    ) {
    }

    public function record(string $eventType, string $severity, ?int $actorUserId = null, array $metadata = []): void
    {
        $request = service('request');

        $this->events->insert([
            'event_type' => $eventType,
            'severity' => $severity,
            'actor_user_id' => $actorUserId,
            'ip_address' => $request instanceof IncomingRequest ? $request->getIPAddress() : null,
            'user_agent' => $request instanceof IncomingRequest ? substr((string) $request->getUserAgent(), 0, 255) : null,
            'route' => current_url(),
            'metadata_json' => json_encode($this->sanitizeMetadata($metadata), JSON_THROW_ON_ERROR),
            'created_at' => date('Y-m-d H:i:s'),
        ]);
    }

    private function sanitizeMetadata(array $metadata): array
    {
        $blockedKeys = ['password', 'token', 'secret', 'totp', 'recovery_code', 'csrf'];

        foreach ($metadata as $key => $value) {
            foreach ($blockedKeys as $blockedKey) {
                if (str_contains(strtolower((string) $key), $blockedKey)) {
                    $metadata[$key] = '[redacted]';
                }
            }
        }

        return $metadata;
    }
}
```

## Panel de eventos

La ruta `/security/events` debe requerir permiso `security.events.view`.

Tabla mínima:

| Fecha | Evento | Severidad | Usuario | IP | Ruta |
|---|---|---|---|---|---|

No muestres `metadata_json` completo por defecto. Debe tener vista de detalle controlada.

## Gestión de sesiones

Funciones esperadas:

- mostrar sesiones activas propias;
- permitir cerrar otras sesiones propias;
- permitir que `security_manager` vea sesiones agregadas;
- registrar evento de cierre de sesión;
- no exponer IDs de sesión completos.

## Tests obligatorios

| Test | Resultado esperado |
|---|---|
| registrar evento simple | fila creada |
| metadata con password | valor redactado |
| usuario sin permiso ve eventos | 403 |
| security_manager ve eventos | 200 |

## Definition of Done semanal

- Existe tabla de eventos.
- Existe servicio de auditoría.
- Eventos sensibles no guardan secretos.
- Panel protegido por permiso.
- Hay tests de auditoría y autorización.
