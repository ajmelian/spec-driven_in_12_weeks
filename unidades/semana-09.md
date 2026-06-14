# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 09 - OWASP Top 10 Web aplicado, validación, XSS, CSRF, inyección y subida segura de ficheros

## Objetivo de la semana

Aplicar OWASP Top 10 Web de forma práctica sobre el proyecto. Esta semana convierte riesgos en controles verificables.

## Matriz OWASP del proyecto

`docs/security/owasp-top-10-web.md`:

```md
# OWASP Top 10 Web Mapping

| Risk | Applicability | Control | Evidence |
|---|---|---|---|
| A01 Broken Access Control | high | Shield permissions, filters, tests | RBAC tests |
| A02 Cryptographic Failures | medium | password hashing, HTTPS, no secrets in logs | Shield, SECURITY.md |
| A03 Injection | high | Query Builder, validation, no raw SQL | code review |
| A04 Insecure Design | high | SDD, threat model, abuse cases | specs |
| A05 Security Misconfiguration | high | .env, production settings, no debug | deployment docs |
| A06 Vulnerable Components | medium | Composer audit/update policy | CI |
| A07 Auth Failures | high | Shield, 2FA, session controls | auth tests |
| A08 Integrity Failures | medium | GitHub, Composer lock, release checklist | CI/release |
| A09 Logging Failures | high | Security events | audit module |
| A10 SSRF | low/medium | URL validation if remote fetch exists | pending |
```

## Validación server-side

Ejemplo de reglas para formulario de soporte:

```php
$rules = [
    'subject' => [
        'label' => 'Asunto',
        'rules' => 'required|min_length[5]|max_length[120]',
    ],
    'message' => [
        'label' => 'Mensaje',
        'rules' => 'required|min_length[20]|max_length[5000]',
    ],
];

if (! $this->validate($rules)) {
    return redirect()->back()->withInput()->with('errors', $this->validator->getErrors());
}
```

## Escaping en vistas

Correcto:

```php
<h1><?= esc($ticket->subject) ?></h1>
<p><?= nl2br(esc($ticket->message)) ?></p>
```

Incorrecto:

```php
<h1><?= $ticket->subject ?></h1>
<p><?= $ticket->message ?></p>
```

## CSRF

Todo formulario debe incluir:

```php
<?= csrf_field() ?>
```

Y usar `POST`, `PUT`, `PATCH` o `DELETE` para operaciones con efecto.

## Inyección SQL

Correcto:

```php
$tickets = $ticketModel
    ->where('status', $status)
    ->where('created_by', $userId)
    ->findAll();
```

Evitar concatenación manual:

```php
$db->query("SELECT * FROM tickets WHERE status = '{$status}'");
```

## Subida segura de ficheros

Reglas:

- validar tamaño;
- validar extensión;
- validar MIME real;
- renombrar con nombre aleatorio;
- no conservar el nombre original como nombre físico;
- almacenar fuera de `public/` si no debe servirse directamente;
- registrar evento de subida/rechazo;
- no permitir ejecución de archivos subidos.

Ejemplo conceptual:

```php
$file = $this->request->getFile('document');

if (! $file->isValid()) {
    return redirect()->back()->with('danger', 'Archivo inválido.');
}

$allowedMimeTypes = [
    'application/pdf',
    'image/jpeg',
    'image/png',
];

if (! in_array($file->getMimeType(), $allowedMimeTypes, true)) {
    return redirect()->back()->with('danger', 'Tipo de archivo no permitido.');
}

if ($file->getSizeByUnit('mb') > 5) {
    return redirect()->back()->with('danger', 'El archivo supera el tamaño máximo permitido.');
}

$newName = $file->getRandomName();
$file->move(WRITEPATH . 'uploads/documents', $newName);
```

## Cabeceras de seguridad

Documenta y aplica, según servidor:

```text
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=(), geolocation=()
Content-Security-Policy: default-src 'self'
```

Para Apache `.htaccess` o configuración de vhost:

```apache
Header always set X-Content-Type-Options "nosniff"
Header always set X-Frame-Options "DENY"
Header always set Referrer-Policy "strict-origin-when-cross-origin"
```

## Tests de seguridad

| Test | Riesgo cubierto |
|---|---|
| usuario sin permiso no accede | Broken Access Control |
| input HTML se escapa | XSS |
| formulario sin CSRF falla | CSRF |
| archivo `.php` rechazado | File upload risk |
| SQL no concatena entrada | Injection review |

## Prompt OpenCode para revisión OWASP

```text
Use Plan mode.

Review the current codebase against docs/security/owasp-top-10-web.md.
Do not edit files.
Return:
- confirmed controls
- missing controls
- risky files
- recommended atomic tasks
- tests that should be added
```

## Definition of Done semanal

- Existe matriz OWASP.
- Hay controles concretos para los riesgos aplicables.
- Se han añadido tests de seguridad.
- Subida de ficheros tiene validación defensiva.
- No hay salida dinámica sin `esc()` en vistas revisadas.
