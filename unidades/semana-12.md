# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 12 - Proyecto final, documentación, demo, portfolio y retrospectiva profesional

## Objetivo de la semana

Cerrar el proyecto como evidencia profesional. El resultado debe poder enseñarse a un cliente, empresa o reclutador como demostración de desarrollo seguro con IA, SDD y Agentic Engineering.

## Entregables finales

| Entregable | Ubicación |
|---|---|
| README profesional | `README.md` |
| SECURITY | `SECURITY.md` |
| Specs cliente | `specs/client/` |
| Specs módulos | `specs/modules/` |
| Tareas atómicas | `specs/tasks/` |
| ADRs | `docs/adr/` |
| Matriz OWASP Web | `docs/security/owasp-top-10-web.md` |
| Matriz OWASP LLM | `docs/security/owasp-top-10-llm.md` |
| Guía despliegue SFTP | `docs/deployment/` |
| Tests | `tests/` |
| CI | `.github/workflows/ci.yml` |

## README profesional

Estructura sugerida:

```md
# CI4 Secure AI Portal

Secure portal built with PHP 8.4, CodeIgniter 4, CodeIgniter Shield, MySQL/MariaDB, Bootstrap 5 and OpenCode-assisted Spec-Driven Development.

## Goals

- Demonstrate secure software development in PHP.
- Apply Spec-Driven Development.
- Use atomic tasks optimized for AI agents.
- Apply OWASP Top 10 Web and OWASP Top 10 LLM controls.
- Use local development and manual SFTP deployment.

## Stack

## Features

## Security Controls

## Architecture

## Local Installation

## Testing

## Deployment

## Agentic Engineering Workflow

## Screenshots

## License
```

## SECURITY.md

```md
# Security Policy

## Supported Versions

This is a training project. Security fixes are applied to the main branch.

## Security Principles

- Authentication and authorization use CodeIgniter Shield.
- 2FA is required for privileged operations.
- CSRF protection is enabled.
- Dynamic output is escaped.
- Production secrets are stored outside the repository.
- LLM output is treated as untrusted.

## Reporting a Vulnerability

Open a private security advisory or contact the maintainer.

## Out of Scope

- Social engineering.
- Denial of service testing without permission.
- Attacks against third-party providers.
```

## Documento de arquitectura

`docs/architecture/overview.md`:

```md
# Architecture Overview

## Style

Server-side rendered modular monolith.

## Layers

- Controllers: HTTP orchestration.
- Services: application use cases.
- Models: persistence.
- Views: HTML5 and Bootstrap 5.
- Filters: authentication, authorization and step-up controls.

## Security

- Shield handles authentication.
- Permissions enforce authorization.
- 2FA protects privileged operations.
- Security events provide auditability.

## Deployment

Manual SFTP release with backup and rollback procedure.
```

## Demo mínima

La demo debe cubrir:

1. Login.
2. Dashboard.
3. Usuario sin permiso intentando acceder a área protegida.
4. Admin accediendo correctamente.
5. Activación o verificación 2FA.
6. Evento de auditoría generado.
7. Ejecución de tests.
8. Explicación de tareas atómicas y OpenCode.
9. Checklist SFTP.

## Guion de vídeo de 5 minutos

```text
1. Presentación del objetivo del proyecto.
2. Stack técnico.
3. Flujo SDD: cliente -> specs -> módulos -> tasks.
4. Ejemplo de tarea atómica.
5. Seguridad: Shield, RBAC, 2FA, auditoría, OWASP.
6. Testing y CI.
7. Despliegue SFTP con checklist.
8. Qué demuestra profesionalmente este repositorio.
```

## Retrospectiva técnica

`docs/retrospective.md`:

```md
# Retrospective

## What worked well

## What was difficult

## Security decisions that improved the project

## AI agent practices that reduced token consumption

## What should be automated next

## What would change in a real client project
```

## Métricas finales

| Métrica | Valor |
|---|---|
| Specs generadas | pendiente |
| Tareas atómicas | pendiente |
| Tareas completadas | pendiente |
| Tests | pendiente |
| Cobertura aproximada | pendiente |
| ADRs | pendiente |
| Riesgos OWASP cubiertos | pendiente |

## Checklist final

```md
# Final Checklist

- [ ] README completo.
- [ ] SECURITY.md completo.
- [ ] AGENTS.md completo.
- [ ] Tareas atómicas actualizadas.
- [ ] Índice de tareas actualizado.
- [ ] Tests pasan.
- [ ] GitHub Actions en verde.
- [ ] OWASP Web documentado.
- [ ] OWASP LLM documentado.
- [ ] Release SFTP documentada.
- [ ] Demo preparada.
- [ ] Retrospectiva escrita.
```

## Definition of Done de la formación

La formación se considera completada cuando puedes explicar y demostrar:

- cómo se transforma un documento cliente en specs;
- cómo se fragmenta el trabajo en tareas atómicas;
- cómo OpenCode trabaja con contexto mínimo;
- cómo CodeIgniter Shield protege la autenticación;
- cómo funciona RBAC;
- cómo se implementa 2FA;
- cómo se registran eventos de seguridad;
- cómo se aplica OWASP Top 10 Web;
- cómo se evalúan riesgos LLM;
- cómo se valida el proyecto con tests y CI;
- cómo se despliega vía SFTP con backup y rollback.
