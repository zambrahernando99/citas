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
