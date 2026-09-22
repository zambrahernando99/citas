# GOAL 1 — guiado sencillo: autenticación JWT

## Objetivo didáctico
Aprender a escribir una condición final verificable sin abarcar todo el producto.

### Codex

```text
/goal Implementa exclusivamente HU-001 (Registrar USER) y HU-002 (Gestionar sesión JWT) en `citas-api`. La meta solo puede iniciar si ambas HU figuran formalmente como `Aprobada`; antes de editar, lee `AGENTS.md`, `README.md`, `../PRD.md`, `../RESTRICCIONES_TECNICAS.md`, `../database/REQUISITOS_NORMALIZACION_3FN.md`, `docs/wiki/llm-wiki/wiki/index.md`, el contrato y decisiones vigentes de la wiki, y los CA/DoD de ambas HU. Si alguna fuente requerida falta, contradice una decisión aprobada o alguna HU no está aprobada, no edites: reporta el bloqueo con la evidencia concreta.

Trabaja por checkpoints y conserva evidencia breve de cada uno. Como el repositorio puede no tener aplicación inicial, crea el bootstrap mínimo Java 21 / Spring Boot 3.5.x / Maven con arquitectura hexagonal, Spring Data JPA, MySQL 8.4, Flyway y Spring Security; no supongas paquetes, endpoints ni perfiles antes de inspeccionarlos. Antes del código funcional, define y documenta el contrato REST mínimo, sus errores observables y la decisión de sesión en los artefactos de `citas-api` que gobierne el orquestador; no inventes reglas de negocio fuera de las fuentes aprobadas.

El resultado debe permitir que un visitante registre una cuenta con nombres, apellidos, tipo y número de documento, email, teléfono y contraseña. Debe asignarse únicamente el rol `USER`, sin permitir su autoasignación. Rechaza sin crear la cuenta los emails o documentos duplicados. Exige los campos mínimos y formato básico de email, sin imponer políticas de longitud o complejidad de contraseña no aprobadas. Usa un hash adaptativo compatible con Spring Security y nunca persistas, devuelvas ni registres la contraseña en texto plano.

Implementa login por email y contraseña. Emite access y refresh JWT separados: el access dura 15 minutos y el refresh 7 días; ambas vigencias deben ser configurables solo por variables de entorno. Incluye roles en el contexto de autorización. El refresh es revocable, rotativo y por sesión: su estado se conserva con identificador de sesión/token, revocación y expiración, sin guardar el refresh crudo. Una renovación válida revoca atómicamente el refresh presentado y emite un nuevo par; los refresh inválidos, expirados, revocados o reutilizados después de rotar se rechazan. Logout revoca únicamente la sesión presentada; las demás sesiones del usuario permanecen válidas.

Configura CORS y autorización base explícitos. No versionas ni registras secretos, passwords, tokens o datos no sintéticos. Si el modelo exige cambios de esquema, usa una nueva migración Flyway y conserva 3FN. No implementes UI, consumo frontend, recuperación de contraseña, proveedor externo de identidad, ownership/autorizar por rol de HU-004 ni funcionalidades de HUs posteriores; entrega el contrato REST listo para que `citas-web` lo consuma y registra esa validación cross-repo como pendiente, no como evidencia completada.

La meta se cumple únicamente cuando existe evidencia de: registro USER válido; unicidad de email y documento; password protegido en persistencia y respuestas; login válido con access/refresh separados; rechazo de credenciales inválidas; rotación de refresh y rechazo de refresh inválido, expirado, revocado o reutilizado; logout y rechazo del refresh cerrado; CORS/autorización base; pruebas unitarias, de aplicación e integración REST/persistencia relevantes; y `mvn test` exitoso. Actualiza la trazabilidad y evidencia de CA/DoD sin declarar completos los elementos de UI o cross-repo no verificados.

Si el mismo criterio falla tras tres intentos diagnósticos consecutivos, pausa sin ampliar el alcance. Reporta el checkpoint, criterio, comandos ejecutados, resultados y causa o hipótesis sustentada. Al terminar, resume cambios, decisiones/contrato, pruebas ejecutadas y evidencia pendiente.
```

### Claude Code equivalente
Usa la misma condición con `/goal ...`. No necesita `/loop` para este ejercicio.
