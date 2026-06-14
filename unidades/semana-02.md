# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 02 - Documento cliente, normalización de requisitos y sistema documental SDD

## Objetivo de la semana

Definir el flujo desde el documento pactado con el cliente hasta especificaciones técnicas normalizadas. Esta semana no se implementa funcionalidad. Se crea el sistema documental que permitirá trabajar de forma profesional y con bajo consumo de tokens.

## Flujo real de trabajo

```text
Documento pactado con cliente
        ↓
Requisitos normalizados
        ↓
Alcance y fuera de alcance
        ↓
Reglas de negocio
        ↓
Requisitos de seguridad
        ↓
Guía visual del frontend
        ↓
Preguntas abiertas
        ↓
Specs por módulo
        ↓
Tareas atómicas
```

## Estructura documental

```text
specs/
├── client/
│   ├── original-requirements.md
│   ├── normalized-requirements.md
│   ├── scope.md
│   ├── out-of-scope.md
│   ├── frontend-style-guide.md
│   ├── business-rules.md
│   ├── security-requirements.md
│   ├── delivery-conditions.md
│   └── open-questions.md
├── shared/
│   ├── definition-of-done.md
│   ├── security-baseline.md
│   ├── coding-standards.md
│   ├── testing-policy.md
│   ├── frontend-policy.md
│   ├── database-policy.md
│   └── deployment-policy.md
├── modules/
└── tasks/
    └── index.md
```

## Plantilla para `original-requirements.md`

Este documento es la transcripción técnica del acuerdo con el cliente. No debe ser interpretado todavía.

```md
# Original Client Requirements

## Client

## Project Name

## Business Context

## High-Level Functionalities

## Frontend Aesthetic Requirements

## Security Requirements Mentioned by Client

## Delivery Conditions

## Budget or Commercial Restrictions

## Deadlines

## Explicit Exclusions

## Notes
```

## Prompt para normalizar requisitos con OpenCode

```text
Use Plan mode.

Read specs/client/original-requirements.md.

Do not edit application code.
Do not generate implementation tasks yet.

Create or update these files:
- specs/client/normalized-requirements.md
- specs/client/scope.md
- specs/client/out-of-scope.md
- specs/client/frontend-style-guide.md
- specs/client/business-rules.md
- specs/client/security-requirements.md
- specs/client/delivery-conditions.md
- specs/client/open-questions.md

Rules:
- Do not invent requirements.
- Convert ambiguous statements into open questions.
- Separate confirmed scope from assumptions.
- Preserve client constraints.
- Identify security-sensitive areas.
- Identify high-level database impact only.
- Do not generate atomic tasks in this step.
```

## Ejemplo de requisito original

```md
El cliente necesita un portal privado para que empleados puedan acceder con usuario y contraseña, consultar documentos internos, recibir avisos y pedir ayuda desde un formulario de soporte. El sistema debe tener una estética corporativa sobria, usando azul oscuro y blanco. Debe ser seguro y permitir doble factor de autenticación para administradores. Se subirá a un hosting mediante SFTP.
```

## Ejemplo de requisito normalizado

```md
# Normalized Requirements

## Confirmed Functional Requirements

- The application must provide a private portal for authenticated employees.
- Users must authenticate with username/email and password.
- Users must access internal documents according to their permissions.
- Users must receive internal notifications.
- Users must submit support requests.
- Administrators must use two-factor authentication.
- The frontend must use a sober corporate style based on dark blue and white.
- Deployment will be performed via SFTP.

## Assumptions

- CodeIgniter Shield will be used for authentication and authorization.
- HTML5 and Bootstrap 5 will be used for the frontend.
- MySQL/MariaDB will be used as database engine.

## Open Questions

- Should non-admin users also be allowed to enable 2FA?
- What document file types are allowed?
- Are documents uploaded by administrators or imported by SFTP?
- Is email notification required or only in-app notification?
```

## `scope.md`

```md
# Scope

## Included

- Secure authentication.
- Role-based access control.
- Administrator 2FA.
- Internal document listing.
- Support request form.
- Notification panel.
- Bootstrap 5 server-rendered frontend.
- Manual SFTP deployment.

## Conditional

- Email notifications, pending confirmation.
- File uploads, pending allowed MIME types.

## Excluded

- Mobile native application.
- SPA frontend.
- Docker deployment.
- Payment gateway.
- Real-time chat.
```

## `security-requirements.md`

```md
# Security Requirements

## Authentication

- CodeIgniter Shield must be used.
- Passwords must never be stored in plain text.
- Authentication errors must not allow account enumeration.

## Authorization

- All protected actions must enforce server-side authorization.
- UI hiding is not considered authorization.

## Two-Factor Authentication

- Administrators must use TOTP-based 2FA.
- Recovery codes must be stored hashed.

## Data Protection

- Secrets must be stored in `.env`, not in code.
- Production data must not be copied to local development without anonymization.

## OWASP

- The project must map controls against OWASP Top 10 Web.
- LLM-assisted features must map controls against OWASP Top 10 LLM.
```

## Documentos compartidos mínimos

### `definition-of-done.md`

```md
# Definition of Done

A task is complete only when:

- Scope has been respected.
- Out-of-scope items have not been implemented.
- Acceptance criteria are satisfied.
- Tests have been added or updated when required.
- Security requirements have been reviewed.
- Input validation has been applied where relevant.
- Output escaping has been applied in views.
- Authorization has been enforced server-side where relevant.
- No secrets have been committed.
- Documentation has been updated when required.
- Verification commands have been executed or documented.
```

### `security-baseline.md`

```md
# Security Baseline

- Use CodeIgniter Shield for authentication and authorization.
- Keep CSRF protection enabled for forms.
- Validate all external input server-side.
- Escape all dynamic output.
- Use Models, Query Builder or prepared statements.
- Do not log passwords, tokens, TOTP codes, recovery codes or API keys.
- Do not expose stack traces in production.
- Use least privilege for database users.
- Treat AI output as untrusted.
```

## Revisión humana obligatoria

Los siguientes documentos requieren revisión humana antes de generar módulos:

| Documento | Motivo |
|---|---|
| `scope.md` | define lo contratado |
| `out-of-scope.md` | evita desviaciones y trabajo no presupuestado |
| `delivery-conditions.md` | afecta coste, fechas y responsabilidad |
| `security-requirements.md` | afecta riesgo y arquitectura |
| `open-questions.md` | evita convertir dudas en funcionalidades |

## Definition of Done semanal

- Existe `specs/client/original-requirements.md`.
- Existe documentación normalizada.
- Las preguntas abiertas están separadas del alcance confirmado.
- Los requisitos de seguridad están identificados.
- El agente no ha generado código todavía.
