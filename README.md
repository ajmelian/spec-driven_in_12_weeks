# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

## Documentos generales

- [Presentación del curso](./presentacion-curso.md)
- [Kit de plantillas SDD y Agentic Engineering](./templates/)
- [Plantilla de tarea atómica](./templates/TEMPLATE-TASK.md)


## Kit de plantillas

Las plantillas viven en `./templates/` y materializan el flujo completo cliente -> requisitos -> specs -> tareas -> implementación -> release.

- [Client Brief](./templates/TEMPLATE-CLIENT-BRIEF.md)
- [Normalized Requirements](./templates/TEMPLATE-NORMALIZED-REQUIREMENTS.md)
- [Module Spec](./templates/TEMPLATE-MODULE-SPEC.md)
- [Security Requirements](./templates/TEMPLATE-SECURITY-REQUIREMENTS.md)
- [Frontend Guide](./templates/TEMPLATE-FRONTEND-GUIDE.md)
- [Acceptance Criteria](./templates/TEMPLATE-ACCEPTANCE-CRITERIA.md)
- [Threat Model](./templates/TEMPLATE-THREAT-MODEL.md)
- [Atomic Task](./templates/TEMPLATE-TASK.md)
- [Architecture Decision Record](./templates/TEMPLATE-ADR.md)
- [Release Checklist](./templates/TEMPLATE-RELEASE-CHECKLIST.md)
- [Retrospective](./templates/TEMPLATE-RETROSPECTIVE.md)

# Índice de semanas

- [Semana 01 - Entorno local profesional con PHP 8.4, CodeIgniter 4, VS Code, GitHub y OpenCode](./unidades/semana-01.md)
- [Semana 02 - Documento cliente, normalización de requisitos y sistema documental SDD](./unidades/semana-02.md)
- [Semana 03 - Tareas atómicas, importación desde Excel/Jira y generación de Markdown](./unidades/semana-03.md)
- [Semana 04 - Bootstrap del proyecto, estructura CodeIgniter, HTML5, Bootstrap 5 y base MySQL/MariaDB](./unidades/semana-04.md)
- [Semana 05 - CodeIgniter Shield: autenticación segura](./unidades/semana-05.md)
- [Semana 06 - RBAC, permisos granulares y autorización server-side](./unidades/semana-06.md)
- [Semana 07 - Doble factor TOTP, códigos de recuperación y step-up authentication](./unidades/semana-07.md)
- [Semana 08 - Sesiones seguras, auditoría, eventos de seguridad y logging](./unidades/semana-08.md)
- [Semana 09 - OWASP Top 10 Web aplicado, validación, XSS, CSRF, inyección y subida segura de ficheros](./unidades/semana-09.md)
- [Semana 10 - OpenCode como agente de desarrollo seguro y OWASP Top 10 LLM](./unidades/semana-10.md)
- [Semana 11 - GitHub Actions, base de datos local/prod, release manual y despliegue SFTP](./unidades/semana-11.md)
- [Semana 12 - Proyecto final, documentación, demo, portfolio y retrospectiva profesional](./unidades/semana-12.md)

## Uso recomendado

Trabaja una semana cada vez. No avances a la siguiente hasta cumplir la Definition of Done semanal. Cada semana está diseñada para producir artefactos verificables: código, documentación, tests, tareas atómicas, políticas o procedimientos de despliegue.

## Orden de trabajo con OpenCode

1. Leer el documento semanal.
2. Crear o revisar los documentos `specs/` indicados.
3. Generar tareas atómicas.
4. Ejecutar una tarea cada vez.
5. Verificar tests y comandos.
6. Hacer commit.
7. Actualizar índice de tareas.

## Stack cerrado

- VS Code.
- GitHub.
- OpenCode.
- PHP 8.4.
- CodeIgniter 4.6+.
- CodeIgniter Shield.
- HTML5.
- Bootstrap 5 local.
- Vanilla JavaScript solo si es necesario.
- MySQL/MariaDB local y producción.
- GitHub Actions para validación.
- SFTP manual para producción.
