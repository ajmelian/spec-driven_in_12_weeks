# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 03 - Tareas atómicas, importación desde Excel/Jira y generación de Markdown

## Objetivo de la semana

Crear el sistema de tareas atómicas optimizado para LLMs. La finalidad es reducir tokens, evitar contexto innecesario y permitir que OpenCode trabaje tarea a tarea, con dependencias y contexto explícitos.

## Principio operativo

```text
No se implementa desde épicas.
No se implementa desde documentos cliente.
No se implementa desde un tasks.md gigante.
Se implementa desde un TASK-*.md atómico.
```

## Estructura de tareas

```text
specs/tasks/
├── index.md
├── authentication/
│   ├── TASK-AUTH-001-install-shield.md
│   ├── TASK-AUTH-002-configure-user-entity.md
│   └── TASK-AUTH-003-create-login-view.md
├── rbac/
├── two-factor/
├── audit/
├── database/
└── deployment/
```

## Índice ligero con checks

`specs/tasks/index.md`:

```md
# Task Index

## Authentication

- [ ] TASK-AUTH-001 - Install and configure CodeIgniter Shield
- [ ] TASK-AUTH-002 - Configure custom user entity
- [ ] TASK-AUTH-003 - Create Bootstrap 5 login view
- [ ] TASK-AUTH-004 - Add login feature tests

## Two-Factor Authentication

- [ ] TASK-2FA-001 - Add TOTP fields migration
- [ ] TASK-2FA-002 - Create TOTP enrollment service
- [ ] TASK-2FA-003 - Create TOTP verification form
- [ ] TASK-2FA-004 - Generate recovery codes
```

El índice no contiene detalles de implementación. Solo sirve para navegación humana y control de avance.

## Plantilla de tarea atómica

```md
---
id: TASK-AUTH-003
title: Create Bootstrap 5 login view
module: authentication
status: pending
priority: high
depends_on:
  - TASK-AUTH-001
  - TASK-AUTH-002
required_context:
  - specs/shared/definition-of-done.md
  - specs/shared/security-baseline.md
  - specs/shared/coding-standards.md
  - specs/modules/authentication/spec.md
  - specs/modules/authentication/frontend.md
expected_files:
  - app/Views/auth/login.php
  - app/Views/layouts/auth.php
  - app/Views/partials/flash_messages.php
database_impact: false
security_impact: true
---

# TASK-AUTH-003 - Create Bootstrap 5 login view

## Objective

Create the login view for CodeIgniter Shield using HTML5 and Bootstrap 5.

## Scope

- Create the login form view.
- Integrate the form with the authentication layout.
- Display validation errors safely.

## Out of Scope

- Do not modify authentication logic.
- Do not implement registration.
- Do not implement 2FA.
- Do not modify database schema.
- Do not introduce JavaScript frameworks.

## Security Requirements

- CSRF token must be present.
- Dynamic output must be escaped.
- Authentication errors must not reveal whether the account exists.
- No credentials may be logged.
- No CDN assets may be used.

## Acceptance Criteria

- Login page renders correctly.
- Form uses Bootstrap 5 classes.
- Form includes CSRF protection.
- Validation errors are displayed safely.
- The page does not load external frontend assets.

## Tests Required

- Feature test for rendering the login page.
- Feature test confirming CSRF field exists.

## Verification Commands

```bash
vendor/bin/phpunit
php spark routes
```

## Completion Notes

Pending.
```

## CSV/XLSX de planificación

Columnas recomendadas:

| Columna | Uso |
|---|---|
| id | identificador único, ejemplo `TASK-AUTH-001` |
| title | título corto |
| module | módulo funcional |
| status | pending, in_progress, blocked, done |
| priority | low, medium, high, critical |
| depends_on | IDs separados por coma |
| required_context | rutas separadas por coma |
| expected_files | rutas separadas por coma |
| database_impact | true/false |
| security_impact | true/false |
| objective | objetivo único |
| scope | alcance resumido |
| out_of_scope | exclusiones |
| acceptance_criteria | criterios separados por punto y coma |
| tests_required | pruebas separadas por punto y coma |

## Ejemplo CSV

```csv
id,title,module,status,priority,depends_on,required_context,expected_files,database_impact,security_impact,objective
TASK-AUTH-001,Install and configure CodeIgniter Shield,authentication,pending,critical,,specs/shared/security-baseline.md;specs/modules/authentication/spec.md,composer.json;app/Config/Auth.php,true,true,Install Shield and apply base configuration
TASK-AUTH-002,Configure custom user entity,authentication,pending,high,TASK-AUTH-001,specs/modules/authentication/spec.md;specs/shared/coding-standards.md,app/Entities/User.php;app/Models/UserModel.php,true,true,Configure application user entity
```

## Generador PHP simple de tareas Markdown

Crea `tools/generate-tasks-from-csv.php`:

```php
<?php

declare(strict_types=1);

if ($argc < 3) {
    fwrite(STDERR, "Usage: php tools/generate-tasks-from-csv.php tasks.csv specs/tasks\n");
    exit(1);
}

$csvPath = $argv[1];
$outputBase = rtrim($argv[2], DIRECTORY_SEPARATOR);

if (!is_readable($csvPath)) {
    fwrite(STDERR, "CSV file not readable: {$csvPath}\n");
    exit(1);
}

$handle = fopen($csvPath, 'rb');
$headers = fgetcsv($handle);

if ($headers === false) {
    fwrite(STDERR, "Empty CSV file.\n");
    exit(1);
}

while (($row = fgetcsv($handle)) !== false) {
    $data = array_combine($headers, $row);

    if ($data === false || empty($data['id']) || empty($data['module'])) {
        continue;
    }

    $moduleDir = $outputBase . DIRECTORY_SEPARATOR . $data['module'];
    if (!is_dir($moduleDir) && !mkdir($moduleDir, 0775, true) && !is_dir($moduleDir)) {
        throw new RuntimeException("Cannot create directory: {$moduleDir}");
    }

    $slug = strtolower(str_replace('_', '-', preg_replace('/[^a-zA-Z0-9]+/', '-', $data['title'])));
    $filePath = $moduleDir . DIRECTORY_SEPARATOR . $data['id'] . '-' . trim($slug, '-') . '.md';

    $dependsOn = splitList($data['depends_on'] ?? '');
    $requiredContext = splitList($data['required_context'] ?? '');
    $expectedFiles = splitList($data['expected_files'] ?? '');
    $acceptanceCriteria = splitList($data['acceptance_criteria'] ?? '');
    $testsRequired = splitList($data['tests_required'] ?? '');

    $markdown = buildTaskMarkdown($data, $dependsOn, $requiredContext, $expectedFiles, $acceptanceCriteria, $testsRequired);
    file_put_contents($filePath, $markdown);

    echo "Generated {$filePath}\n";
}

fclose($handle);

function splitList(string $value): array
{
    $items = preg_split('/[;,]/', $value) ?: [];

    return array_values(array_filter(array_map('trim', $items)));
}

function yamlList(array $items): string
{
    if ($items === []) {
        return "  []\n";
    }

    return implode('', array_map(static fn (string $item): string => "  - {$item}\n", $items));
}

function bulletList(array $items): string
{
    if ($items === []) {
        return "- Pending definition\n";
    }

    return implode('', array_map(static fn (string $item): string => "- {$item}\n", $items));
}

function buildTaskMarkdown(array $data, array $dependsOn, array $requiredContext, array $expectedFiles, array $acceptanceCriteria, array $testsRequired): string
{
    $id = $data['id'];
    $title = $data['title'];
    $module = $data['module'];
    $status = $data['status'] ?: 'pending';
    $priority = $data['priority'] ?: 'medium';
    $databaseImpact = strtolower($data['database_impact'] ?? 'false') === 'true' ? 'true' : 'false';
    $securityImpact = strtolower($data['security_impact'] ?? 'false') === 'true' ? 'true' : 'false';
    $objective = $data['objective'] ?? 'Pending definition';
    $scope = splitList($data['scope'] ?? '');
    $outOfScope = splitList($data['out_of_scope'] ?? '');

    return <<<MD
---
id: {$id}
title: {$title}
module: {$module}
status: {$status}
priority: {$priority}
depends_on:
{$depends = yamlList($dependsOn)}required_context:
{$context = yamlList($requiredContext)}expected_files:
{$files = yamlList($expectedFiles)}database_impact: {$databaseImpact}
security_impact: {$securityImpact}
---

# {$id} - {$title}

## Objective

{$objective}

## Scope

{$scopeText = bulletList($scope)}
## Out of Scope

{$outText = bulletList($outOfScope)}
## Security Requirements

Apply the shared security baseline and any module-specific security requirements listed in required context files.

## Acceptance Criteria

{$acceptance = bulletList($acceptanceCriteria)}
## Tests Required

{$tests = bulletList($testsRequired)}
## Verification Commands

```bash
vendor/bin/phpunit
php spark routes
```

## Completion Notes

Pending.
MD;
}
```

## Comando de generación

```bash
mkdir -p tools
php tools/generate-tasks-from-csv.php planning/tasks.csv specs/tasks
```

## Prompt para que OpenCode genere tareas desde un módulo

```text
Use Plan mode first.

Read only:
- specs/modules/authentication/spec.md
- specs/modules/authentication/security.md
- specs/modules/authentication/frontend.md
- specs/modules/authentication/acceptance.md
- specs/shared/definition-of-done.md
- specs/shared/security-baseline.md
- specs/shared/testing-policy.md

Generate atomic task files under specs/tasks/authentication/.
Update specs/tasks/index.md.

Rules:
- One task must fit in one focused implementation session.
- Each task must have one objective.
- Declare dependencies.
- Declare required_context.
- Declare expected_files.
- Do not duplicate the shared DoD.
- Do not implement code.
```

## Definition of Done semanal

- Existe plantilla de tarea atómica.
- Existe índice de tareas.
- Existe CSV de ejemplo o mecanismo equivalente.
- Existe generador de Markdown o prompt formal para generarlo.
- `AGENTS.md` obliga a implementar desde tareas atómicas.
