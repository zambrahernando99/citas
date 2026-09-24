El plan debe tratar S4 y S5 como cierre del MVP, no como dos incrementos aislados: hay deuda de S2/S3 y el frontend todavía depende ampliamente de datos simulados.
Estado verificado
- Completadas con evidencia existente: HU-001, HU-002 y la base backend de HU-011 a HU-013.
- Parcialmente implementadas en backend, pero incompletas para DoD/end-to-end: HU-014, HU-017, HU-018, HU-019, HU-023 y HU-028.
- Pendientes: HU-003 a HU-010, HU-015, HU-016, HU-020 a HU-022, HU-024 a HU-027.
- El frontend conserva mockData.ts, selección ficticia de rol y pantallas con datos estáticos; no puede considerarse evidencia de las HU posteriores.
Hay una diferencia de planificación que resolveré explícitamente: Scrum sitúa HU-025 a HU-027 en S6, pero la guía de S4 exige agenda profesional y cierre de atención. Para cumplir “toda la funcionalidad descrita”, el plan las incluye en el alcance S4/S5.
Plan de ejecución
1. Cerrar decisiones y contrato transversal antes de ampliar código.
   - Aprobar la matriz rol–acción–recurso y ownership de HU-004.
   - Definir el grafo de estados: REQUESTED, APPROVED, REJECTED, CANCELLED, COMPLETED, NO_SHOW, y los estados de reprogramación.
   - Resolver los vacíos documentados: expiración de retenciones, definición temporal de “cita aplicable” para cierre, comportamiento al desactivar un profesional con citas vigentes, y entrega segura del token de recuperación.
   - Publicar el contrato REST completo, con payloads, filtros, errores problem+json, paginación si aplica y autorización por endpoint.
   - Registrar estas decisiones y el contrato en la wiki global antes de implementar.
2. Completar la deuda funcional de S2/S3: HU-003 a HU-010.
   - HU-003: recuperación con token temporal, de un uso, expiración y hash adaptativo; sin SMTP obligatorio ni exposición de tokens.
   - HU-004: autorización por rol y ownership real en API y rutas de interfaz; eliminar el selector manual de rol.
   - HU-005 y HU-006: perfil propio y afiliación EPS–plan–régimen normalizada.
   - HU-007: catálogos fijos protegidos y sembrados.
   - HU-008, HU-009 y HU-010: CRUD administrativo de EPS, planes y especialidades, con inactivación y sin borrados físicos de referencias.
   - Validar HU-011 a HU-013 contra estas dependencias y conservar sus APIs ya existentes.
3. Consolidar disponibilidad y reservas: HU-014 a HU-020 y HU-028.
   - HU-014: endurecer creación de bloques, solapamientos, ownership, sede asignada y concurrencia.
   - HU-015 y HU-016: edición/eliminación segura de bloques futuros no comprometidos y calendario propio por rango/sede.
   - HU-017: búsqueda real por sede, especialidad, profesional, fecha y tipo; excluir profesionales inactivos, slots retenidos u ocupados y exigir consecutividad para 60 minutos.
   - HU-018 y HU-019: reserva transaccional general APPROVED y solicitud especializada REQUESTED, sin doble reserva.
   - HU-020: listado y detalle de citas propias por estado/fecha, incluyendo sede, profesional, especialidad, duración y motivo de rechazo.
   - HU-028: centralizar toda transición en un servicio de estados que genere historial inmutable con actor, fuente, fecha y motivo.
4. Implementar el ciclo de vida completo: HU-021 a HU-024.
   - HU-021: cancelar únicamente citas propias, futuras y elegibles; liberar slots y auditar.
   - HU-022: crear reprogramación PENDING conservando la cita y franja originales, y reteniendo la nueva franja.
   - HU-023: decisión ADMIN de solicitud especializada; aprobación conserva slots y rechazo motivado los libera.
   - HU-024: decisión ADMIN de reprogramación; aprobación intercambia franjas atómicamente y rechazo conserva la original.
   - La persistencia debe separar cita, reserva/retención, historial y solicitud de reprogramación para conservar 3FN y evitar destruir la cita original.
5. Incluir la operación requerida por la guía de S4: HU-025 a HU-027.
   - HU-025: agenda del profesional por día/semana/sede, limitada a sus citas APPROVED.
   - HU-026: marcar COMPLETED o NO_SHOW solo en citas propias y temporalmente elegibles.
   - HU-027: bandeja ADMIN unificada de solicitudes REQUESTED y reprogramaciones PENDING, con filtros por sede, profesional, especialidad y fecha.
6. Sustituir completamente los datos simulados del frontend.
   - Eliminar el uso productivo de src/data/mockData.ts; solo podrán existir fixtures aislados de pruebas.
   - Reemplazar los estados globales ficticios de App.tsx por consultas REST, caché de pantalla y recarga tras mutaciones.
   - Conectar con la API real las pantallas de perfil, afiliación, EPS/planes, especialidades, profesionales, bloques, disponibilidad, reserva, mis citas, agenda y bandeja administrativa.
   - Derivar identidad y permisos de la sesión real; las rutas y la navegación se mostrarán según roles autorizados, no por alternancia manual.
   - Cada vista debe cubrir carga, vacío, error, éxito, expiración de sesión y acciones deshabilitadas mientras se procesa una mutación.
   - Sustituir también métricas, próximas citas, historial y datos del dashboard actualmente escritos de forma fija.
7. Validar por HU y por contrato.
   - Backend: pruebas de dominio/aplicación, integración REST y persistencia; conflictos concurrentes, ownership, transiciones, auditoría e integridad de slots.
   - Frontend: typecheck, build y pruebas de flujos críticos contra API; sin interceptores que devuelvan datos de negocio falsos.
   - Cross-repo: pruebas de reserva general, solicitud especializada, cancelación, reprogramación, decisiones ADMIN y cierre profesional.
   - Criterio adicional: búsqueda estática sin importaciones productivas de mockData ni valores operativos fijos.
   - Actualizar evidencia CA/DoD de cada HU, contrato, trazabilidad y wiki.
8. Ejecutar la evidencia autónoma de S4.
   - Un loop guiado: solicitud especializada real, desde disponibilidad hasta retención y visualización.
   - Un loop independiente: reprogramación o cierre de atención, con Builder separado de Verifier.
   - Guardar por iteración objetivo, cambios, pruebas backend, build frontend, veredicto, límite de reintentos y condición de escalamiento.
   - Solo declarar S4 terminado con todas las HU anteriores verificadas end-to-end y un commit estable en develop.
9. Completar S5: WF-001, MCP y seguridad.
   - Añadir el contrato mínimo de automatización: consulta de citas APPROVED próximas con datos mínimos, autenticación de privilegio limitado, ventana configurable e idempotencia.
   - Persistir resultados de entrega para impedir recordatorios duplicados por cita/ventana y permitir reintentos seguros.
   - Construir en n8n: Schedule Trigger → API de recordatorios → Gmail → registro de resultado.
   - Configurar OAuth individual con alcance mínimo; credenciales únicamente en n8n/entorno, nunca en el JSON ni repositorio.
   - Probar con cuentas y citas sintéticas antes de activar.
   - Exportar el flujo saneado en citas-api/automations/n8n/WF-001-appointment-reminders.json.
   - Obtener evidencia de una invocación MCP exitosa, documentar la demo de contenido no confiable y los riesgos residuales.
   - Dejar preparado el contrato de eventos para WF-002; WF-003 sigue siendo opcional según la documentación vigente.
El orden evita construir UI sobre contratos incompletos y permite retirar los mocks sin dejar ninguna pantalla operativa desconectada. No realicé cambios todavía.