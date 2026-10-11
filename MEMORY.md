# MEMORY.md — Quiboot Odoo · Facturación electrónica Bolivia
Memoria del proyecto entre sesiones. Máximo ~100 líneas: resume o elimina lo que ya no
aporte.
## Estado actual
- Rama de trabajo: `planning-001`, creada desde `main` conservando los cambios
  locales. Commit y subida al repositorio pendientes.
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
- La constitución sustituida contenía discrepancias de idioma, alcance y multiempresa;
  la constitución vigente está alineada con `AGENTS.md`.
- Adopción verificada mediante revisión documental y comprobación de referencias;
  no se ejecutaron pruebas funcionales por tratarse de cambios de documentación.
- Las etiquetas `odoo:20` y `postgres:18` del Compose no demuestran compatibilidad
  ni un entorno aprobado. No se inició Docker ni se ejecutaron pruebas funcionales.
## Próximos pasos
- Revisar y seleccionar los archivos del commit de planificación; excluir archivos
  locales como `.DS_Store`. Crear el commit y subir la rama cuando se indique.
- Preparar spec y plan del entorno con evidencia de compatibilidad antes de
  incorporar dependencias o implementar funcionalidades.
