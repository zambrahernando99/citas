# AGENTS.md — Orquestación cross-repo

## Alcance

Este workspace contiene exactamente dos repositorios Git independientes:

- `citas-api`: backend Java 21 / Spring Boot / Maven / MySQL.
- `citas-web`: frontend TypeScript React o Angular.

La raíz solo coordina; no contiene implementación de producto ni se convierte en un tercer repositorio Git.

## Fuente de verdad y prioridad

1. Historias de usuario aprobadas y Definition of Done en `citas-api/docs/wiki/scrum/`.
2. `PRD.md`, `RESTRICCIONES_TECNICAS.md` y `database/REQUISITOS_NORMALIZACION_3FN.md`.
3. Contratos y decisiones aprobadas de la LLM Wiki.
4. Código, migraciones y pruebas de ambos repositorios.

No inventar requisitos fuera de estas fuentes. Ante un conflicto o vacío que afecte el comportamiento, registrar una pregunta abierta y pedir decisión antes de implementar.

## Separación de responsabilidades

- Lógica de negocio, seguridad, persistencia, migraciones, API REST y n8n: `citas-api`.
- UI, accesibilidad, estado de pantalla y consumo REST: `citas-web`.
- No se usa Express ni BFF: el frontend consume directamente Spring Boot.
- Un cambio REST requiere contrato actualizado, evidencia backend, evidencia frontend y validación de compatibilidad en ambos repositorios.

## Git

- Trabajar en `develop`; `main` representa incrementos estables.
- No reescribir el historial para ocultar progreso.
- No inicializar ni usar Git en la raíz.
- Preservar cambios ajenos; no leer ni exponer archivos `.env`.

## Datos y seguridad

- Usar exclusivamente datos sintéticos, salvo información pública explícitamente incluida en los requisitos.
- Nunca persistir contraseñas, tokens, credenciales, secretos ni PII innecesaria en documentación, código o automatizaciones.
- Los ejemplos de entorno no contienen secretos reales.

## LLM Wiki

La única wiki global está en `citas-api/docs/wiki/llm-wiki/`.

- Leer `wiki/index.md` antes de usarla.
- `raw/` conserva fuentes curadas e inmutables; `wiki/` contiene síntesis mantenida.
- Actualizar `wiki/index.md` y anexar a `wiki/log.md` en cada INGEST, LEARN o LINT.
- Persistir solo conocimiento durable y verificado; nunca transcripciones completas.
- Clasificar conocimiento como HECHO, DECISIÓN, PREFERENCIA o PREGUNTA ABIERTA.

## Automatizaciones

Los workflows n8n se versionan exclusivamente como JSON en `citas-api/automations/n8n/`. No incluir credenciales ni activar un flujo sin una prueba controlada.

## Protocolo cross-repo

Antes de modificar ambos repositorios:

1. Declarar HU y alcance.
2. Enumerar repositorios y archivos previstos.
3. Definir o actualizar el contrato.
4. Implementar con pruebas.
5. Validar backend, frontend y compatibilidad REST.
6. Registrar evidencia o decisión durable en la wiki.
