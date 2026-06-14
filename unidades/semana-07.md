# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 07 - Doble factor TOTP, códigos de recuperación y step-up authentication

## Objetivo de la semana

Diseñar e implementar doble factor de autenticación basado en TOTP, con enrolamiento, verificación, recuperación y exigencia de autenticación reforzada para operaciones críticas.

## Conceptos clave

| Concepto | Definición |
|---|---|
| TOTP | código temporal basado en secreto compartido y tiempo |
| Enrolamiento | proceso en el que el usuario activa 2FA |
| Recovery codes | códigos de emergencia de un solo uso |
| Step-up authentication | exigir 2FA adicional antes de una acción sensible |

## Reglas de seguridad

- El secreto TOTP debe almacenarse cifrado o protegido según arquitectura definida.
- Los recovery codes deben almacenarse hasheados, no en texto plano.
- Los códigos TOTP no se registran en logs.
- La activación de 2FA requiere confirmar un código válido.
- La desactivación de 2FA requiere step-up authentication.
- Las acciones críticas deben exigir step-up aunque el usuario ya esté autenticado.

## Migración conceptual

```bash
php spark make:migration AddTwoFactorFieldsToUsers
```

Ejemplo conceptual:

```php
<?php

declare(strict_types=1);

namespace App\Database\Migrations;

use CodeIgniter\Database\Migration;

final class AddTwoFactorFieldsToUsers extends Migration
{
    public function up(): void
    {
        $this->forge->addColumn('users', [
            'two_factor_enabled' => [
                'type' => 'TINYINT',
                'constraint' => 1,
                'default' => 0,
            ],
            'two_factor_secret' => [
                'type' => 'TEXT',
                'null' => true,
            ],
            'two_factor_confirmed_at' => [
                'type' => 'DATETIME',
                'null' => true,
            ],
            'last_step_up_at' => [
                'type' => 'DATETIME',
                'null' => true,
            ],
        ]);
    }

    public function down(): void
    {
        $this->forge->dropColumn('users', [
            'two_factor_enabled',
            'two_factor_secret',
            'two_factor_confirmed_at',
            'last_step_up_at',
        ]);
    }
}
```

## Tabla de recovery codes

```php
$this->forge->addField([
    'id' => ['type' => 'INT', 'unsigned' => true, 'auto_increment' => true],
    'user_id' => ['type' => 'INT', 'unsigned' => true],
    'code_hash' => ['type' => 'VARCHAR', 'constraint' => 255],
    'used_at' => ['type' => 'DATETIME', 'null' => true],
    'created_at' => ['type' => 'DATETIME', 'null' => true],
]);
```

## Servicio TOTP conceptual

`app/Services/TwoFactor/TwoFactorService.php`:

```php
<?php

declare(strict_types=1);

namespace App\Services\TwoFactor;

final class TwoFactorService
{
    public function generateSecret(): string
    {
        return bin2hex(random_bytes(20));
    }

    public function verifyCode(string $secret, string $code): bool
    {
        // En una implementación real usar una librería TOTP revisada.
        // No implementar criptografía artesanal salvo como ejercicio controlado.
        return preg_match('/^[0-9]{6}$/', $code) === 1;
    }

    /**
     * @return list<string>
     */
    public function generateRecoveryCodes(int $amount = 10): array
    {
        $codes = [];

        for ($i = 0; $i < $amount; $i++) {
            $codes[] = strtoupper(bin2hex(random_bytes(5)) . '-' . bin2hex(random_bytes(5)));
        }

        return $codes;
    }

    public function hashRecoveryCode(string $code): string
    {
        return password_hash($code, PASSWORD_ARGON2ID);
    }

    public function verifyRecoveryCode(string $code, string $hash): bool
    {
        return password_verify($code, $hash);
    }
}
```

## Vista de enrolamiento

La vista debe mostrar:

- explicación clara de 2FA;
- QR o clave manual;
- campo de código de 6 dígitos;
- CSRF;
- errores genéricos;
- aviso para guardar recovery codes.

Ejemplo de input:

```php
<input
    type="text"
    inputmode="numeric"
    pattern="[0-9]{6}"
    maxlength="6"
    minlength="6"
    class="form-control"
    name="totp_code"
    id="totp_code"
    autocomplete="one-time-code"
    required
>
```

## Step-up authentication

Ejemplo de regla:

```text
Una operación crítica requiere que `last_step_up_at` sea menor de 5 minutos.
```

Operaciones críticas:

| Acción | Requiere step-up |
|---|---:|
| desactivar 2FA | sí |
| regenerar recovery codes | sí |
| cambiar email | sí |
| cambiar contraseña | sí |
| desactivar usuario | sí |
| borrar documento | sí |

## Filtro conceptual de step-up

```php
final class StepUpFilter implements FilterInterface
{
    public function before(RequestInterface $request, $arguments = null)
    {
        $user = auth()->user();

        if ($user === null) {
            return redirect()->to('/login');
        }

        $lastStepUp = $user->last_step_up_at;

        if ($lastStepUp === null || strtotime($lastStepUp) < time() - 300) {
            session()->set('intended_step_up_url', current_url());

            return redirect()->to('/security/step-up');
        }

        return null;
    }

    public function after(RequestInterface $request, ResponseInterface $response, $arguments = null): void
    {
    }
}
```

## Tests obligatorios

| Test | Resultado esperado |
|---|---|
| activar 2FA con código inválido | rechazo |
| activar 2FA con código válido | activación |
| recovery code se almacena hasheado | no aparece en claro |
| acción crítica sin step-up | redirección a verificación |
| acción crítica con step-up reciente | permitida |

## Tareas atómicas sugeridas

| ID | Título |
|---|---|
| TASK-2FA-001 | Add 2FA schema changes |
| TASK-2FA-002 | Create TwoFactorService |
| TASK-2FA-003 | Create 2FA enrollment flow |
| TASK-2FA-004 | Create recovery codes flow |
| TASK-2FA-005 | Add step-up authentication filter |
| TASK-2FA-006 | Protect critical actions with step-up |
| TASK-2FA-007 | Add 2FA tests |

## Definition of Done semanal

- Existe diseño TOTP documentado.
- Existen migraciones 2FA.
- Existe flujo de activación.
- Existen recovery codes hasheados.
- Existe step-up authentication.
- Hay tests negativos y positivos.
