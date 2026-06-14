# Formación autodidacta: Desarrollo Seguro con IA, SDD y Agentic Engineering

Stack objetivo: VS Code, GitHub, OpenCode, PHP 8.4, CodeIgniter 4.6+, CodeIgniter Shield, HTML5, Bootstrap 5, MySQL/MariaDB, PHPUnit, GitHub Actions y despliegue manual vía SFTP.

Regla principal: no se implementa código desde una idea vaga. Todo desarrollo parte de especificaciones, tareas atómicas, criterios de aceptación, pruebas y revisión de seguridad.

# Semana 10 - OpenCode como agente de desarrollo seguro y OWASP Top 10 LLM

## Objetivo de la semana

Usar OpenCode como copiloto controlado para desarrollo seguro, revisión, generación de tests, documentación y refactor. Además, definir controles para cualquier funcionalidad basada en LLM.

## Principio central

El agente puede asistir. No gobierna el alcance, no decide requisitos contractuales y no ejecuta acciones destructivas sin aprobación.

## Reglas para `AGENTS.md`

```md
## AI Security Rules

- Treat all LLM output as untrusted.
- Do not expose secrets to the model.
- Do not paste `.env` contents into prompts.
- Do not allow autonomous destructive operations.
- Do not modify authentication, authorization, cryptography or migrations without a task file.
- For security-sensitive code, produce a risk summary before implementation.
- Prefer small task context over broad repository context.
```

## Usos recomendados de OpenCode

| Uso | Modo |
|---|---|
| analizar requisitos | Plan |
| generar tareas atómicas | Plan/Edit controlado |
| implementar tarea concreta | Build |
| revisar OWASP | Plan |
| generar tests | Build con tarea específica |
| refactor | Build con tarea específica |
| revisar PR | Plan |

## Usos no recomendados

| Uso | Motivo |
|---|---|
| “hazme el módulo entero” | scope creep, alto consumo de tokens |
| “arregla todo” | ambiguo y peligroso |
| pegar secretos | fuga de información |
| permitir cambios masivos sin diff | bajo control humano |

## Prompt de revisión segura

```text
Use Plan mode.

Review TASK-2FA-005 and its required context files.
Do not edit files.
Identify:
- security assumptions
- missing acceptance criteria
- missing tests
- possible OWASP risks
- whether the task is atomic enough
```

## Prompt de implementación segura

```text
Implement TASK-2FA-005.

Rules:
- Read only required_context files declared in the task.
- Inspect code files only when necessary.
- Do not implement out-of-scope items.
- Add or update required tests.
- Do not modify unrelated files.
- After implementation, summarize changed files, risks and verification commands.
```

## OWASP Top 10 LLM aplicado

`docs/security/owasp-top-10-llm.md`:

```md
# OWASP Top 10 LLM Mapping

| Risk | Applicability | Control |
|---|---|---|
| Prompt Injection | high | never execute model output directly |
| Sensitive Information Disclosure | high | do not send secrets or private data |
| Supply Chain | medium | pin dependencies, review libraries |
| Data and Model Poisoning | medium | trusted knowledge sources only |
| Improper Output Handling | high | validate and escape all LLM output |
| Excessive Agency | high | human approval for actions |
| System Prompt Leakage | medium | avoid exposing internal prompts |
| Vector and Embedding Weaknesses | future | source validation and access control |
| Misinformation | high | cite internal sources and require fallback |
| Unbounded Consumption | medium | rate limits and cost controls |
```

## Funcionalidad IA mínima del proyecto

Ejemplo: asistente de revisión de seguridad que genera sugerencias, no ejecuta cambios.

Reglas:

- El usuario envía fragmento de código o selecciona archivo.
- El sistema genera observaciones.
- El output se muestra como sugerencia, no se aplica automáticamente.
- Se escapa todo output.
- No se envían `.env`, secretos ni datos personales.
- Se registra coste/latencia si hay API externa.

## Servicio conceptual

```php
<?php

declare(strict_types=1);

namespace App\Services\AiSecurity;

final class AiSecurityReviewService
{
    public function reviewCode(string $code): SecurityReviewResult
    {
        if ($this->containsPotentialSecret($code)) {
            throw new \InvalidArgumentException('The submitted code appears to contain secrets.');
        }

        // External LLM call would be encapsulated here.
        // Output must be treated as untrusted.
        return new SecurityReviewResult(
            summary: 'Pending external provider integration.',
            findings: []
        );
    }

    private function containsPotentialSecret(string $content): bool
    {
        return preg_match('/(password|api_key|secret|token)\s*=/i', $content) === 1;
    }
}
```

## Tests IA seguros

| Test | Objetivo |
|---|---|
| código con `api_key=` se rechaza | evita fuga de secreto |
| salida IA se escapa | evita XSS |
| usuario sin permiso no usa asistente | autorización |
| prompt injection en entrada no ejecuta acción | exceso de agencia |

## Registro de uso IA

Tabla conceptual:

```text
ai_usage_logs
- id
- user_id
- feature
- provider
- model
- input_hash
- tokens_input
- tokens_output
- latency_ms
- created_at
```

No guardar prompts completos si pueden contener datos sensibles. Preferir hash, métricas y metadatos mínimos.

## Definition of Done semanal

- `AGENTS.md` contiene reglas de seguridad IA.
- Existe matriz OWASP LLM.
- OpenCode se usa con tareas atómicas.
- Hay prompts operativos para revisión e implementación.
- Cualquier función IA trata outputs como no confiables.
