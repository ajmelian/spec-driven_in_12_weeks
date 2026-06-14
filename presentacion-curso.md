# Presentación del curso

## Desarrollo real con Spec-Driven Development, AI Engineering y Agentic Engineering

Este curso está diseñado para aprender a dirigir, documentar, implementar y desplegar un proyecto de software real usando Spec-Driven Development, AI Engineering y Agentic Engineering. El objetivo no es aprender únicamente un framework, una herramienta de IA o una forma concreta de programar. El objetivo es adquirir una metodología exportable a cualquier proyecto profesional.

El laboratorio práctico se implementará con PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, MySQL/MariaDB, HTML5, Bootstrap 5, GitHub, OpenCode y despliegue manual vía SFTP. Sin embargo, este stack es solo el vehículo de aprendizaje. La metodología aprendida puede trasladarse después a Laravel, Symfony, WordPress, Node.js, Python, Java, .NET, aplicaciones móviles, APIs, backoffices, SaaS, proyectos con IA, proyectos internos de empresa o desarrollos para cliente.

## Propósito de la formación

El propósito principal es aprender a transformar una necesidad real de cliente en un sistema software entregable, trazable, seguro y mantenible.

Durante el curso se trabaja todo el ciclo de vida del desarrollo:

1. Documento inicial pactado con el cliente.
2. Análisis de requisitos.
3. Normalización funcional.
4. Identificación de alcance y fuera de alcance.
5. Detección de ambigüedades y preguntas abiertas.
6. Diseño de especificaciones por módulo.
7. Definición de requisitos de seguridad.
8. Diseño de experiencia visual con HTML5 y Bootstrap 5.
9. Descomposición en tareas atómicas.
10. Optimización de contexto para agentes IA.
11. Implementación guiada por OpenCode.
12. Testing.
13. Revisión humana.
14. Revisión OWASP.
15. Preparación de release.
16. Despliegue en servidor remoto vía SFTP.
17. Documentación final y retrospectiva.

El curso no sigue un modelo de aprendizaje basado en consumir contenido de forma pasiva. Cada semana debe producir artefactos verificables: documentos, especificaciones, tareas, código, tests, checklists, procedimientos de despliegue o documentación de seguridad.

## Qué se va a aprender

### 1. Spec-Driven Development aplicado a proyectos reales

Se aprenderá a trabajar desde especificaciones antes de escribir código. La especificación no se tratará como documentación secundaria, sino como el contrato operativo que guía el trabajo del desarrollador y del agente IA.

Se trabajarán artefactos como:

| Artefacto | Finalidad |
|---|---|
| `original-requirements.md` | Documento inicial del cliente transformado a Markdown. |
| `normalized-requirements.md` | Requisitos ordenados, claros y sin ruido contractual. |
| `scope.md` | Funcionalidades incluidas en el proyecto. |
| `out-of-scope.md` | Funcionalidades explícitamente excluidas. |
| `open-questions.md` | Ambigüedades que requieren validación. |
| `module/spec.md` | Especificación funcional por módulo. |
| `module/security.md` | Requisitos de seguridad del módulo. |
| `module/frontend.md` | Reglas de interfaz y experiencia visual del módulo. |
| `module/acceptance.md` | Criterios de aceptación. |
| `TASK-*.md` | Tareas atómicas ejecutables por agente IA. |
| `definition-of-done.md` | Criterios comunes de cierre. |
| `release-checklist.md` | Procedimiento de puesta en producción. |

La idea central es sencilla: si una funcionalidad no está especificada, no se implementa. Si una tarea no tiene criterios de aceptación, no está preparada para ser ejecutada. Si una acción tiene impacto en seguridad, datos o despliegue, debe quedar documentada antes de tocar código.

### 2. AI Engineering orientado a ingeniería, no a generación improvisada de código

Se aprenderá a utilizar IA como una herramienta de ingeniería, no como sustituto del criterio técnico. El agente IA no decidirá el producto, el alcance ni la arquitectura por sí solo. El agente trabajará sobre documentos versionados, tareas pequeñas y reglas claras.

Se practicarán usos como:

- Extraer requisitos desde un documento cliente.
- Detectar ambigüedades y riesgos.
- Generar especificaciones normalizadas.
- Crear tareas atómicas.
- Proponer planes técnicos.
- Implementar una tarea concreta.
- Generar tests.
- Revisar código.
- Detectar posibles vulnerabilidades.
- Crear documentación técnica.
- Preparar checklists de release.

También se trabajarán límites explícitos:

- La IA no debe inventar requisitos.
- La IA no debe convertir ambigüedades en alcance confirmado.
- La IA no debe implementar tareas no aprobadas.
- La IA no debe modificar autenticación, permisos o criptografía sin revisión estricta.
- La IA no debe leer documentación innecesaria si una tarea declara contexto mínimo.

### 3. Agentic Engineering con OpenCode

El curso utiliza OpenCode como agente IA principal. El objetivo es aprender a dirigir un agente de código mediante documentación, reglas, tareas atómicas y revisión humana.

Se trabajará con:

- `AGENTS.md` como contrato operativo del agente.
- `opencode.json` como configuración del proyecto.
- Prompts de planificación.
- Prompts de implementación.
- Prompts de revisión de seguridad.
- Prompts de generación de tests.
- Prompts de refactorización.
- Prompts de documentación.

La regla principal será trabajar siempre desde una tarea atómica. Una tarea atómica debe ser suficientemente pequeña para ejecutarse en una sesión focalizada y suficientemente completa para no depender de contexto implícito.

Ejemplo de flujo:

```text
1. Seleccionar TASK-AUTH-003.
2. Leer solo los ficheros indicados en required_context.
3. Verificar dependencias.
4. Implementar únicamente el alcance declarado.
5. Añadir tests.
6. Ejecutar verificación.
7. Registrar notas de finalización.
8. Actualizar el índice de tareas.
9. Hacer commit.
```

### 4. Optimización de tokens mediante tareas atómicas

Una parte esencial del curso es aprender a reducir el consumo innecesario de contexto en LLMs. En lugar de pedir al agente que lea grandes documentos con requisitos aún no implementados, se fragmentará el trabajo en ficheros pequeños.

Se utilizará una estructura de este tipo:

```text
specs/
├── client/
│   ├── original-requirements.md
│   ├── normalized-requirements.md
│   ├── scope.md
│   ├── out-of-scope.md
│   └── open-questions.md
├── shared/
│   ├── definition-of-done.md
│   ├── coding-standards.md
│   ├── security-baseline.md
│   └── testing-policy.md
├── modules/
│   ├── authentication/
│   │   ├── spec.md
│   │   ├── security.md
│   │   ├── frontend.md
│   │   └── acceptance.md
│   └── two-factor-authentication/
└── tasks/
    ├── index.md
    ├── authentication/
    │   ├── TASK-AUTH-001-install-shield.md
    │   ├── TASK-AUTH-002-configure-groups.md
    │   └── TASK-AUTH-003-create-login-view.md
    └── two-factor/
```

Cada tarea declara sus dependencias y su contexto mínimo:

```yaml
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
  - specs/modules/authentication/spec.md
  - specs/modules/authentication/frontend.md
expected_files:
  - app/Views/auth/login.php
  - app/Views/layouts/auth.php
security_impact: true
database_impact: false
---
```

Esto permite controlar qué lee el agente y evitar que consuma tokens revisando módulos completos, futuras tareas o documentación no aplicable.

### 5. Desarrollo seguro como criterio transversal

La seguridad no se abordará como un añadido al final del proyecto. Será una restricción transversal desde el documento cliente hasta la puesta en producción.

Se trabajará con:

- OWASP Top 10 Web.
- OWASP Top 10 para aplicaciones LLM.
- Autenticación segura con CodeIgniter Shield.
- RBAC y permisos granulares.
- Autorización server-side.
- 2FA TOTP.
- Step-up authentication para operaciones críticas.
- Gestión segura de sesiones.
- Auditoría y eventos de seguridad.
- Validación de entrada.
- Escapado de salida.
- Protección CSRF.
- Prevención de XSS.
- Prevención de inyección SQL.
- Subida segura de ficheros.
- Gestión segura de secretos.
- Separación de entornos local y producción.

Cada tarea con impacto de seguridad debe declararlo explícitamente. Cada módulo sensible debe tener su propio `security.md`.

### 6. Proyecto real como laboratorio

El proyecto práctico será un portal seguro desarrollado con PHP 8.4 y CodeIgniter 4.6+. El nombre de referencia será:

```text
ci4-secure-ai-portal
```

El proyecto incluirá:

- Interfaz HTML5 y Bootstrap 5.
- Autenticación con CodeIgniter Shield.
- Gestión de usuarios.
- Roles y permisos.
- 2FA TOTP.
- Auditoría de seguridad.
- Gestión de sesiones.
- Panel administrativo.
- Base de datos MySQL/MariaDB.
- Validaciones y formularios seguros.
- Tests.
- GitHub Actions.
- Documentación técnica.
- Checklist de despliegue vía SFTP.

El proyecto no usará Docker ni frameworks JavaScript. El frontend será server-side rendered con vistas de CodeIgniter, HTML5, Bootstrap 5 y JavaScript vanilla solo cuando sea necesario.

## Metodología de trabajo

La metodología del curso se basa en un ciclo repetible:

```text
Cliente -> Requisitos -> Specs -> Tareas atómicas -> Implementación -> Tests -> Revisión -> Release
```

Cada semana seguirá una estructura común:

1. Objetivo metodológico universal.
2. Aplicación al laboratorio CodeIgniter 4.
3. Artefactos a generar.
4. Plantillas reutilizables.
5. Ejercicios prácticos.
6. Uso del agente IA.
7. Validación humana.
8. Definition of Done semanal.
9. Exportabilidad a otros proyectos.

Esto evita que el curso se convierta en un tutorial rígido de CodeIgniter. Cada semana debe responder a dos preguntas:

```text
¿Qué estoy aprendiendo que sirve para cualquier proyecto?
¿Cómo lo practico en este laboratorio concreto?
```

## Principios de la formación

### 1. La especificación gobierna el desarrollo

No se implementa desde conversaciones dispersas, ideas vagas o prompts improvisados. El código parte de specs versionadas.

### 2. El agente ejecuta, pero el ingeniero dirige

OpenCode ayuda a analizar, generar, implementar, revisar y documentar. Sin embargo, el alcance, la arquitectura, la seguridad y la aceptación final quedan bajo responsabilidad humana.

### 3. Las tareas deben ser pequeñas y verificables

Una tarea grande consume más tokens, genera más desviaciones y dificulta la revisión. Una tarea atómica permite control, trazabilidad y commits limpios.

### 4. La seguridad se diseña desde el inicio

Cada requisito debe analizarse desde la perspectiva de abuso, permisos, datos, validación, auditoría y despliegue.

### 5. La documentación forma parte del producto

La documentación no es un subproducto. Es la fuente de verdad que permite a personas y agentes IA trabajar con coherencia.

### 6. Todo debe poder auditarse

Cada decisión relevante debe poder rastrearse desde el requisito hasta la tarea, el código, el test y el despliegue.

### 7. El resultado debe ser exportable

Aunque el laboratorio use CodeIgniter, los artefactos y procesos deben poder aplicarse a otros stacks.

## Herramientas utilizadas

| Categoría | Herramienta |
|---|---|
| IDE | VS Code |
| Repositorio | GitHub |
| Agente IA | OpenCode |
| Lenguaje | PHP 8.4 |
| Framework | CodeIgniter 4.6+ |
| Seguridad | CodeIgniter Shield, OWASP |
| Frontend | HTML5, Bootstrap 5, JavaScript vanilla |
| Base de datos | MySQL/MariaDB |
| Testing | PHPUnit |
| CI | GitHub Actions |
| Despliegue | SFTP manual controlado |
| Documentación | Markdown, Docs as Code |

## Qué no persigue este curso

Este curso no pretende enseñar Docker, Kubernetes, microservicios, React, Vue, Angular, mobile, cloud avanzado ni MLOps profundo. Esas áreas pueden ser útiles en otros contextos, pero aquí se excluyen para concentrar el aprendizaje en SDD, AI Engineering, seguridad, tareas atómicas y entrega real.

Tampoco se busca aprender a “pedirle código a la IA” de forma improvisada. El objetivo es aprender a crear un sistema de trabajo en el que la IA pueda colaborar sin romper alcance, seguridad ni mantenibilidad.

## Resultado esperado

Al finalizar, se deberá disponer de tres resultados:

### 1. Una aplicación real

Un portal seguro funcional en CodeIgniter 4 con autenticación, autorización, 2FA, auditoría, interfaz Bootstrap, base de datos, tests, documentación y despliegue controlado.

### 2. Un repositorio profesional

Un repositorio con specs, tareas atómicas, documentación técnica, GitHub Actions, checklists, ADRs, matriz OWASP y evidencias de implementación.

### 3. Una metodología reutilizable

Un sistema de trabajo exportable a otros proyectos, basado en:

- Documento cliente.
- Normalización de requisitos.
- Specs por módulo.
- Tareas atómicas.
- Contexto mínimo para LLMs.
- Reglas para agentes IA.
- Validación humana.
- Testing.
- Seguridad.
- Release.
- Despliegue.

## Estructura del curso

| Semana | Tema |
|---:|---|
| 01 | Entorno local profesional para AI Engineering. |
| 02 | Documento cliente, alcance y normalización de requisitos. |
| 03 | Spec-Driven Development, sistema documental y tareas atómicas. |
| 04 | Proyecto base, CodeIgniter 4, HTML5, Bootstrap 5 y base de datos. |
| 05 | Autenticación segura con CodeIgniter Shield. |
| 06 | RBAC, permisos granulares y autorización server-side. |
| 07 | 2FA TOTP, códigos de recuperación y step-up authentication. |
| 08 | Sesiones seguras, auditoría y logging. |
| 09 | OWASP Top 10 Web y subida segura de ficheros. |
| 10 | OpenCode seguro y OWASP Top 10 LLM. |
| 11 | GitHub Actions, releases, base de datos local/prod y despliegue SFTP. |
| 12 | Proyecto final, portfolio, documentación y retrospectiva. |

## Cómo estudiar este curso

El curso debe trabajarse de forma secuencial. No conviene saltar directamente a la implementación. Las primeras semanas crean el sistema de trabajo que después permite desarrollar con control.

Por cada semana:

1. Leer el documento semanal completo.
2. Crear los artefactos indicados.
3. Ejecutar los comandos propuestos.
4. Usar OpenCode solo con prompts controlados.
5. Revisar manualmente los resultados.
6. Ejecutar tests o verificaciones.
7. Hacer commit.
8. Actualizar el índice de tareas.
9. Registrar aprendizajes y problemas.

La semana se considera terminada solo cuando se cumple su Definition of Done.

## Flujo de trabajo recomendado con OpenCode

Ejemplo de prompt para planificación:

```text
Use Plan mode.

Read the selected task file and only the files listed under required_context.

Do not edit code.

Produce:
- implementation plan
- affected files
- security considerations
- tests required
- risks
- verification commands
```

Ejemplo de prompt para implementación:

```text
Implement TASK-AUTH-003.

Read only the task file and its required_context files.
Respect Scope and Out of Scope.
Do not implement future tasks.
Add or update the required tests.
After implementation, summarize changed files and verification commands.
```

Ejemplo de prompt para revisión de seguridad:

```text
Review the changes for this task from a secure software development perspective.

Focus on:
- input validation
- output escaping
- authentication
- authorization
- CSRF
- session handling
- sensitive data exposure
- logging risks
- OWASP mapping

Do not modify files. Report findings only.
```


## Kit de plantillas reutilizables

El curso incluye un kit de plantillas en `./templates/` para que el flujo sea repetible en cualquier proyecto, no solo en el laboratorio CodeIgniter 4. Estas plantillas convierten el temario metodológico en artefactos operativos que pueden ser leídos por personas, versionados en Git y consumidos por agentes IA con contexto limitado.

| Plantilla | Uso principal | Momento de uso |
|---|---|---|
| `TEMPLATE-CLIENT-BRIEF.md` | Captura ordenada del documento o reunión inicial con cliente. | Inicio del proyecto. |
| `TEMPLATE-NORMALIZED-REQUIREMENTS.md` | Traducción técnica de requisitos, alcance, supuestos y preguntas abiertas. | Antes de diseñar módulos. |
| `TEMPLATE-MODULE-SPEC.md` | Contrato funcional y técnico de cada módulo. | Antes de generar tareas. |
| `TEMPLATE-SECURITY-REQUIREMENTS.md` | Requisitos de seguridad, OWASP, autorización, auditoría y abuso esperado. | Para módulos sensibles. |
| `TEMPLATE-FRONTEND-GUIDE.md` | Reglas de interfaz, HTML5, Bootstrap 5, formularios y accesibilidad. | Antes de construir vistas. |
| `TEMPLATE-ACCEPTANCE-CRITERIA.md` | Criterios verificables de aceptación. | Antes de implementar y probar. |
| `TEMPLATE-THREAT-MODEL.md` | Modelo ligero de amenazas, activos, entradas y controles. | Antes de implementar módulos críticos. |
| `TEMPLATE-TASK.md` | Unidad atómica ejecutable por agente IA. | Durante la planificación operativa. |
| `TEMPLATE-ADR.md` | Registro de decisiones de arquitectura. | Cada vez que se tome una decisión relevante. |
| `TEMPLATE-RELEASE-CHECKLIST.md` | Validación de release, backup, SFTP, migraciones y rollback. | Antes de producción. |
| `TEMPLATE-RETROSPECTIVE.md` | Revisión de aprendizaje, proceso, deuda y uso del agente IA. | Al cerrar una semana o release. |

La regla práctica es simple: las plantillas se copian para crear documentos concretos del proyecto; los documentos compartidos como `definition-of-done.md`, `security-baseline.md` o `agent-rules.md` se referencian para evitar duplicación y consumo innecesario de tokens.

## Criterio de éxito

El curso habrá cumplido su objetivo si, al terminar, eres capaz de enfrentarte a un nuevo proyecto y construir un sistema de trabajo como este sin depender del stack concreto:

```text
1. Recibir documento de cliente.
2. Normalizar requisitos.
3. Detectar alcance y ambigüedades.
4. Crear specs por módulo.
5. Crear tareas atómicas.
6. Dirigir un agente IA con contexto mínimo.
7. Revisar código y seguridad.
8. Probar.
9. Documentar.
10. Desplegar.
```

Ese es el aprendizaje principal: no solo programar con IA, sino dirigir ingeniería de software asistida por IA con criterio profesional.
