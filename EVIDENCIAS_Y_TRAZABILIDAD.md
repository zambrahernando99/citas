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
