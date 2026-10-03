# Evidencias y trazabilidad para evaluación final

La calificación se realiza al finalizar S6, pero el historial debe permitir reconstruir el progreso.

## Evidencia mínima por sesión

| Sesión | Evidencia mínima |
|---|---|
| S2 | commit backend + commit frontend; AGENTS; Scrum specs; baseline ejecutable |
| S3 | commits; tests; hook FAIL/PASS; secreto ficticio bloqueado |
| S4 | commits; logs Builder/Verifier; goal/loop; MVP |
| S5 | commit; WF-001 JSON; evidencia MCP; riesgos residuales |
| S6 | commit final; WF-002 JSON; validaciones; merge/main estable; sustentación |

## Regla Git
- desarrollo en `develop`;
- `main` representa únicamente puntos que el estudiante considera estables;
- no exigir merge por sesión;
- no hacer squash/rebase destructivo que borre el progreso antes de la evaluación.

## Registro S2 — GOAL 01 y baseline integrado

| Campo | Evidencia |
|---|---|
| Repos | `citas-api` y `citas-web`, rama `develop` |
| HU abordadas | HU-001 Registrar USER y HU-002 Gestionar sesión JWT |
| Backend | `mvn test` en Java 21: 9 pruebas, 0 fallos/errores |
| Infraestructura | MySQL 8.4 saludable; API con Flyway respondió health `200` |
| REST | CORS `http://localhost:5173`, registro `201`, login, rotación de refresh y logout `204` con datos sintéticos |
| Frontend | Cliente REST directo por `VITE_API_URL`; `npm run lint` y `npm run build` exitosos |
| Pendientes | Recuperación de contraseña, autorización por ownership y HU posteriores no pertenecen a S2 |

## Plantilla de registro

```text
Sesión:
Repo:
Branch:
Commit hash:
HU abordadas:
Criterios completados:
Pruebas ejecutadas:
Qué quedó pendiente:
Evidencia adicional:
```

## n8n evaluable
Los JSON exportados deben abrir/importar sin depender de secretos embebidos. Las credenciales se configuran en n8n y nunca deben formar parte del JSON/repositorio en texto claro.

## Registro S3 — Verificación y red que dice "no"

| Campo | Evidencia |
|---|---|
| Repos / rama | `citas-api` y `citas-web`, `develop` |
| Commits | api `1744813` test(s3)… |
| HU | HU-003..HU-028 (pruebas de reglas de slots 30/60, doble reserva, retención, autorización, ownership) |
| Red → Green | `citas-api/docs/evidence/s3-s4/01-red-green.md` (4 rojas: historial, bandeja, IDs y defecto real 500→409) |
| Hook FAIL/PASS | `citas-api/docs/evidence/s3-s4/02-hook.md` y `citas-web/docs/evidence/s3-s4/02-hook.md`: FAIL por pruebas, FAIL por secreto ficticio (nunca entró al historial), PASS |

## Registro S4 — MVP con Builder/Verifier

| Campo | Evidencia |
|---|---|
| Commits | api `0823ad3`, web `3f838ff` feat(s4)…; merge `develop → main` (`--no-ff`) en ambos |
| Loops | `citas-api/docs/evidence/loops/`: LOOP_01 defecto de reprogramación, LOOP_02 reprogramación cross-repo, LOOP_03 reconciliación de contrato (propio); PASS en la iteración 1 |
| Pruebas | backend 40/40 (`mvn test` en `citas-api-dev`); web tsc OK, Vitest 13/13, build OK (en `citas-web-dev`) |
| E2E Docker | `citas-api/docs/evidence/s3-s4/03-e2e-docker.md`: API sobre MySQL real 47/47 + recorrido UI USER/ADMIN/PROFESSIONAL |
| Decisiones | Regímenes fijos (PRD RF-05), V7; ver wiki `decisiones.md` DEC-009..012 |
| Pendiente | Prueba de concurrencia real (dos transacciones simultáneas) sobre MySQL; hoy la doble reserva está cubierta de forma secuencial y por `SELECT … FOR UPDATE` |

## Registro S5 — Agente conectado (en curso)

| Campo | Evidencia |
|---|---|
| Repo / rama | `citas-api`, `develop` |
| Commit | api `a7da6dc` feat(s5)… (backend, JSON y evidencias; sin push) |
| Contrato | `GET /api/v1/automation/reminders/due`, `POST /api/v1/automation/reminders/{id}/deliveries`, `X-Automation-Key` (wiki DEC-013..017) |
| Pruebas | Red 10/10 → Green; suite 54/54; smoke MySQL 8.4 V8 (`citas-api/docs/evidence/s5/01-red-green.md`) |
| Hook | Export n8n FAIL/PASS (`citas-api/docs/evidence/s5/02-hook-n8n-json.md`) |
| WF-001 | `citas-api/automations/n8n/WF-001-appointment-reminders.json` (`active: false`) |
| Seguridad | `citas-api/docs/evidence/s5/05-untrusted-content.md`, `citas-api/docs/security/S5-riesgos-residuales.md` |
| Entorno | BD de la app `citas_app` separada de la referencia `citas_fcv_training` (enmienda DEC-005) |
| Pendiente | Ejecución en n8n con Gmail OAuth propio, túnel ngrok, invocación MCP y demo en vivo |

## Registro S6 — WF-002 y WF-003 (en curso)

| Campo | Evidencia |
|---|---|
| Repo / rama | `citas-api`, `develop` |
| Commit | api `73f270d` feat(s6)… (sin push) |
| Backend | Outbox V9 + despachador con reintentos (WF-002); `GET /api/v1/automation/appointments/daily` sin PII (WF-003) |
| Pruebas | Red 8/8 → suite 72/72; smoke MySQL 8.4 V9 (`citas-api/docs/evidence/s6/01-wf002-wf003.md`) |
| n8n por MCP | `Hernando-WF-002-status-notifications` (`lLZQOsvQpmVLXYah`) y `Hernando-WF-003-daily-operational-summary` (`Z8YOsBjSqA39K1pu`), inactivos y sin credenciales; ejecuciones de prueba 59, 60 y 61 |
| JSON | `citas-api/automations/n8n/WF-002-status-notifications.json`, `WF-003-daily-operational-summary.json` (hook OK) |
| Seguridad | Credencial ajena autoasignada detectada y retirada (DEC-020, R13) |
| Pendiente | Credenciales propias (Gmail OAuth, Header Auth), ejecución real y activación; merge `develop → main` |

## Registro S5/S6 — Ejecución real (2026-10-03)

| Campo | Evidencia |
|---|---|
| Commits | api `77a8140` (reintento 404/401/403, en `origin/develop`) y `9b650f5` (evidencia real, local) |
| Infraestructura | API Docker + ngrok en Docker (`--profile tunnel`) con política de rutas; credenciales propias en n8n |
| WF-001 | Ejecuciones 76-79: FAILED→SENT, sin duplicado, rama de API caída |
| WF-002 | Activo; 4 eventos DELIVERED (HTTP 200) y 4 correos recibidos |
| WF-003 | Ejecución 85; resumen del día recibido |
| Evidencia | `citas-api/docs/evidence/s6/02-ejecucion-real.md` con capturas sin datos personales |

**Cierre 2026-10-03:** `citas-api` `develop` publicado y fusionado en `main` (merge `3fa6423`, `--no-ff`); `citas-web` sin cambios en S5/S6 (`main` ya contiene `develop`). WF-001, WF-002 y WF-003 activos en la instancia por decisión del estudiante.
