# MEMORY.md — Quiboot Odoo · Facturación electrónica Bolivia
Memoria del proyecto entre sesiones. Máximo ~100 líneas: resume o elimina lo que ya no
aporte.
## Estado actual
- El commit complementario `76c1f57` se subió a `planning-001`. El propietario
  informa que el PR se fusionó en `main`; la copia local sigue en `planning-001`.
- Propuesta de orden de 12 specs en `docs/PLANIFICACION.md`; todavía no hay specs
  funcionales ni implementación autorizada en esta etapa de planificación.
- 2026-10-11: `docs/CONSTITUTION.md` aprobada y vigente, versión 1.0.0.
- Se leyeron `AGENTS.md`, esta memoria, `qb_facturacion/docs/constitution.md` y
  `docker-compose.yaml`. No hay specs, planes ni tareas funcionales identificados.
## Decisiones (y por qué)
- `AGENTS.md` recoge las decisiones confirmadas: Odoo Community 20, Python 3.12,
  una empresa por instalación, entidad fiscal propia, prefijo `qb_` y español.
- El propietario aprobó el borrador y la sustitución del documento anterior.
  Se retiró `qb_facturacion/docs/constitution.md` y se actualizaron las referencias
  de `AGENTS.md` a `docs/CONSTITUTION.md`.
## Aprendizajes y errores a evitar
- Se leyó la skill local para planificar; su pauta de código en inglés discrepa
  con el proyecto: prevalece el español confirmado. Sus APIs requieren verificación.
- Consulta pública del índice SIAT con timeout; contratos y vigencia no verificados
  en la planificación. No se realizaron llamadas a servicios fiscales.
- La constitución sustituida contenía discrepancias de idioma, alcance y multiempresa;
  la constitución vigente está alineada con `AGENTS.md`.
- Adopción verificada mediante revisión documental y comprobación de referencias;
  no se ejecutaron pruebas funcionales por tratarse de cambios de documentación.
- La skill de terceros se conserva sin modificar, con licencia original; no fija
  la licencia del proyecto ni demuestra compatibilidad con Odoo 20. Se retiró del
  lock la entrada antigua de `odoo-development`, ausente del repositorio local.
- Se sustituyó la contraseña de ejemplo por un marcador; `.env`, configuración
  local, datos de Odoo y `.DS_Store` están excluidos de Git. Se revisaron patrones
  de secretos antes de preparar el commit; esto no equivale a una auditoría completa.
- Las etiquetas `odoo:20` y `postgres:18` del Compose no demuestran compatibilidad
  ni un entorno aprobado. No se inició Docker ni se ejecutaron pruebas funcionales.
## Próximos pasos
- Revisar el orden propuesto y redactar primero la spec del entorno; concretar
  el origen y momento de la emisión mediante ejemplos funcionales, sin asumirlos.
- Preparar spec y plan del entorno con evidencia de compatibilidad antes de
  incorporar dependencias o implementar funcionalidades.
