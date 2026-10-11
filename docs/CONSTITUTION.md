# Constitución del sistema de facturación electrónica

**Proyecto:** Quiboot · Odoo Community 20 · Bolivia  
**Estado:** Aprobada y vigente por autorización del propietario.  
**Versión:** 1.0.0 · **Fecha de aprobación:** 2026-10-11

## Principios innegociables

1. **Stack explícito y reproducible.** Usamos Odoo Community 20, Python 3.12, PostgreSQL y Docker Compose con imagen oficial de Odoo, sin modificar el núcleo ni exigir Enterprise. Cada versión y dependencia requiere compatibilidad demostrada y aprobación antes de incorporarse. **Verificación:** el plan del entorno registra versiones exactas, evidencia de compatibilidad y aprobaciones; ningún componente depende de `latest`.

2. **La especificación precede al código.** Seguimos `spec → plan → tareas → implementación → verificación`; cada cambio responde a un requisito y a criterios de aceptación. Las decisiones arquitectónicas duraderas se registran en ADR; las ambigüedades bloquean únicamente el trabajo afectado. **Verificación:** cada tarea enlaza requisito, cambio y evidencia de aceptación.

3. **Alcance limitado y explícito.** Cada instalación atiende a una empresa emisora, con sucursales y puntos de venta fiscales. El primer incremento cubre Compra y Venta, modalidad Electrónica en Línea; POS queda para después. La spec determina qué registros originan facturas propias y distingue las recibidas de proveedores. **Verificación:** specs, modelos y pruebas respetan este alcance; toda ampliación tiene aprobación.

4. **Lógica fiscal independiente de la interfaz.** Separamos reglas deterministas, persistencia, firma, transporte SOAP e interfaz. Las reglas reciben sus datos y contexto explícitamente y pueden probarse sin vistas, red ni base de datos. **Verificación:** las pruebas de lógica pura se ejecutan aisladamente y las vistas no contienen reglas fiscales.

5. **Entidad fiscal propia y evidencia íntegra.** La factura electrónica tiene modelo persistente y tabla propios; conserva instantánea, identidad fiscal, códigos históricos, XML firmado, payload y respuestas. Los cambios comerciales no alteran emisiones anteriores y las correcciones siguen procesos fiscales trazables. **Verificación:** pruebas de persistencia e inmutabilidad confirman estas garantías y la separación del estado contable y fiscal.

6. **Reglas fiscales respaldadas por evidencia.** Cada regla y contrato SIAT utilizado identifica su fuente oficial, versión o huella y fecha de consulta; se comprueba su aplicabilidad antes de implementarlo. Precisión, redondeo y fechas fiscales —con conversión explícita a `America/La_Paz` cuando corresponda— se especifican y prueban. **Verificación:** cada regla tiene referencia y casos de prueba; las dudas de vigencia quedan registradas como bloqueos.

7. **Emisión recuperable y sin duplicados por reintentos.** Identidad, numeración e intención de envío se persisten con control de concurrencia. Un resultado incierto se concilia antes de decidir un reenvío; la aceptación fiscal exige evidencia interpretada conforme al contrato. **Verificación:** pruebas de concurrencia, timeout y reinicio conservan la identidad y no falsifican aceptación ni generan otra factura para ocultar el fallo.

8. **Pruebas como condición de cierre.** Los criterios críticos requieren pruebas automatizadas de lógica, integración Odoo e interoperabilidad simulada, incluidos permisos, errores y recuperación. Instalación y actualización se verifican en una base desechable con Python 3.12. **Verificación:** se registran comandos y resultados; ninguna tarea se cierra con validación obligatoria fallida o bloqueada, ni se presenta revisión documental como prueba funcional.

9. **Datos del usuario protegidos y efectos externos controlados.** Aplicamos permisos mínimos, comprobaciones de acceso y aislamiento entre instalaciones y ambientes; justificamos cada `sudo()`. Usamos datos sintéticos y SIAT simulado en pruebas; secretos y datos sensibles quedan fuera del repositorio y de logs técnicos. Datos reales, llamadas reales al SIAT, despliegues y migraciones requieren aprobación explícita; estas últimas incluyen respaldo y recuperación. **Verificación:** pruebas de acceso, revisión de secretos y configuración confirman aislamiento y bloqueo de envíos reales desde pruebas.

10. **Español y convenciones consistentes.** Código propio, comentarios, pruebas, interfaz, mensajes y documentación se escriben en español; los identificadores no llevan tildes ni `ñ`. Módulos y modelos nuevos propios usan el prefijo `qb_`; se conservan los nombres de APIs y contratos externos. **Verificación:** la revisión de cambios comprueba idioma, nombres y compatibilidad con la revisión objetivo de Odoo.

## Gobierno y adopción

- Esta constitución limita specs, planes y código; los detalles de cada funcionalidad permanecen en su spec y plan.
- Cada plan y revisión registra cumplimiento, evidencia o no aplicabilidad de los principios afectados.
- Cambiar un principio requiere propuesta con motivo e impacto, aprobación del propietario y registro de versión. Una spec o un cambio de código no lo modifica implícitamente.
- El propietario aprobó los principios y la sustitución del documento anterior el 2026-10-11. La referencia vigente y única es `docs/CONSTITUTION.md`, alineada con las decisiones confirmadas en `AGENTS.md`.

## Decisiones pendientes

- Revisión de Odoo, etiqueta/digest oficial, parche de Python 3.12, versión de PostgreSQL y dependencias: compatibilidad y aprobación pendientes. Las etiquetas del Compose existente no constituyen evidencia de compatibilidad.
- Spec y plan del entorno, comandos operativos y pruebas ejecutables: pendientes de preparación y verificación.
- Licencia del proyecto, contratos fiscales aplicables y política de generación desde registros comerciales: pendientes de decisiones documentadas antes del trabajo que dependa de ellos.
