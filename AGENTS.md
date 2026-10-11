# AGENTS.md — Quiboot Odoo · Facturación electrónica Bolivia

Instrucciones para Codex y otros agentes que trabajen en este repositorio. El proyecto parte desde cero y desarrolla módulos reutilizables para Odoo Community 20 que permiten emitir facturas electrónicas bolivianas mediante SIAT. El desarrollo sigue especificaciones, planes y tareas en Markdown.

## Stack y estructura

- **Objetivo confirmado:** Odoo Community 20, Python 3.12, PostgreSQL y Docker Compose con la imagen oficial de Odoo.
- Python 3.12 es la versión fijada para el código propio y el entorno de ejecución y pruebas. Todavía no se han seleccionado revisión de Odoo, etiqueta/digest de imagen, versión de parche de Python 3.12, versión compatible de PostgreSQL, bibliotecas, servicios ni comandos de ejecución. Verificar que la revisión de Odoo, la imagen oficial y las dependencias elegidas sean compatibles con Python 3.12 antes de incorporarlas; no asumir `latest`, inventar etiquetas ni cambiar las versiones objetivo sin autorización. Si esa compatibilidad no puede demostrarse, registrar el bloqueo del entorno y proponer alternativas para aprobación.
- Un despliegue opera para una empresa emisora. El módulo será reutilizable para distintos clientes mediante instalaciones separadas; multiempresa no forma parte del alcance. Sí se soportarán sucursales y puntos de venta fiscales.
- El primer incremento cubre el documento sector **Compra y Venta**, modalidad **Electrónica en Línea**, partiendo de los registros de compra/venta definidos en la especificación. POS es un incremento posterior.
- La factura electrónica tendrá **modelo persistente y tabla propios**, separados de los registros comerciales y contables. Su relación con esos registros y su política de generación se definen en la spec. No implementar toda la entidad fiscal como campos añadidos a `account.move`.

Estructura inicial propuesta para crear a medida que existan tareas que la necesiten:

| Ruta | Propósito |
| --- | --- |
| `AGENTS.md` | Instrucciones operativas para los agentes; ubicar en la raíz del repositorio. |
| `docs/CONSTITUTION.md` | Constitución aprobada y principios del proyecto. |
| `docs/specs/<funcionalidad>/spec.md` | Requisitos, alcance, decisiones y criterios de aceptación. |
| `docs/specs/<funcionalidad>/plan.md` | Diseño, dependencias, contratos y estrategia de pruebas. |
| `docs/specs/<funcionalidad>/tasks.md` | Tareas pequeñas y seguimiento de su verificación. |
| `docs/adr/` | Decisiones arquitectónicas, alternativas e impactos. |
| `docs/siat/` | Referencias públicas y contratos oficiales versionados, con procedencia, fecha y huellas. Nunca credenciales. |
| `addons/qb_facturacion_siat/` | Nombre propuesto del módulo inicial; modelos, seguridad, servicios, vistas, datos, reportes y pruebas. |
| `addons/qb_facturacion_pos/` | Integración futura con POS; no crear ni implementar en el primer incremento. |
| `infra/` | Configuración Docker Compose y archivos de infraestructura una vez especificados. |
| `scripts/` | Automatizaciones locales aprobadas; sin secretos ni efectos productivos implícitos. |

No crear carpetas vacías ni infraestructura adicional por anticipación. Esta tabla describe destinos propuestos, no archivos que ya existan. Si el repositorio evoluciona, actualizar las rutas documentadas.

## Comandos

**Estado inicial: comandos operativos pendientes de definición y comprobación.** No hay todavía un entorno ejecutable ni una batería de pruebas del proyecto. No presentar comandos hipotéticos como verificados.

Antes de implementar funcionalidades, preparar la spec y el plan del entorno local. Identificar versiones, servicios, volúmenes, base de pruebas desechable, configuración de addons y mecanismos para impedir llamadas reales al SIAT. Presentar las dependencias nuevas para aprobación antes de incorporarlas o instalarlas.

Cuando el entorno esté implementado, reemplazar esta sección con los comandos **exactos y copiables**, ejecutados y comprobados, para:

1. Validar Docker Compose e iniciar el entorno local.
2. Instalar y actualizar los módulos en una base local de pruebas.
3. Ejecutar pruebas de lógica Python y pruebas de integración de Odoo.
4. Ejecutar las herramientas de lint y formato que se aprueben.
5. Consultar logs y detener el entorno conservando sus datos.

Las pruebas no deben depender de servicios reales ni compartir base o filestore con instalaciones de usuarios. Actualizar módulos en una base de pruebas desechable es parte de la verificación local autorizada; migrar datos existentes requiere aprobación. No usar comandos que borren volúmenes como forma habitual de reiniciar.

## Skills del proyecto

- Usar la skill local **`odoo-developmen-skill'`**, instalada en `.agents/skills/odoo-development-skill/SKILL.md`, para diseñar, implementar o revisar módulos Odoo: ORM, modelos, campos, vistas XML, manifiestos, seguridad, controladores y pruebas. Leer su contenido antes de aplicarla por primera vez en la conversación y consultar los recursos que indique cuando sean pertinentes.
- Aplicar sus pautas de separación de responsabilidades, extensión mediante herencia, uso del ORM, ACL, reglas de registro, validaciones y pruebas junto con la spec y el plan activos. Los ejemplos de la skill son orientativos: comprobar sus APIs, decoradores y sintaxis de vistas contra documentación oficial o código de la revisión objetivo de Odoo 20 antes de utilizarlos, incluidos ejemplos como `@api.multi`. No asumir compatibilidad por aparecer en la skill.
- Mantener las decisiones de este archivo al adaptar los ejemplos: español, prefijo `qb_`, entidad fiscal propia y Python 3.12. La skill no autoriza nuevas dependencias, cambios de alcance, instalaciones ni efectos externos. Si una pauta entra en conflicto con las instrucciones del proyecto, registrar la discrepancia y aplicar las decisiones confirmadas del propietario.
- Ejecutar las comprobaciones Python con el intérprete 3.12 del entorno aprobado y las pruebas Odoo en una base desechable. La comprobación de sintaxis no sustituye las pruebas funcionales ni de integración. Mientras el entorno no exista, documentar las comprobaciones pendientes sin presentarlas como ejecutadas.

## Convenciones

- Código propio, interfaz, comentarios, mensajes, documentación y comunicación con el usuario **en español**. Usar identificadores legibles sin tildes ni `ñ` para facilitar interoperabilidad.
- Todos los módulos y modelos **nuevos propios** comienzan con `qb_`. Propuesta de modelo: `_name = "qb_factura.electronica"`, con tabla propia; concretar nombres, relaciones y restricciones en el plan. Las clases usan PascalCase y métodos/campos `snake_case`.
- Mantener los nombres de APIs de Odoo, bibliotecas y contratos SIAT. Heredar un modelo existente no implica renombrarlo ni cambiar su tabla. Conservar identificadores oficiales como `codigoSucursal` en el adaptador de interoperabilidad.
- Respetar convenciones y mecanismos soportados por la revisión objetivo de Odoo 20. Preferir ORM, herencia y extensiones de módulos; no modificar el núcleo ni requerir Enterprise.
- Separar lógica fiscal determinista, persistencia, firma, transporte SOAP e interfaz. Mantener las reglas reutilizables para que POS pueda consumirlas posteriormente sin duplicar la emisión.
- Usar bibliotecas mantenidas para XML, SOAP y criptografía cuando sean aprobadas; no implementar criptografía propia. No introducir un framework, cola externa o microservicio sin necesidad especificada y aprobación de sus dependencias.
- La licencia del proyecto está pendiente. No elegirla ni copiar código de terceros sin comprobar y documentar compatibilidad.
- Los patrones de referencia serán los archivos del propio módulo respaldados por specs y pruebas. Mientras no existan, no inventar un archivo de referencia ni replicar APIs de versiones anteriores sin verificar.

## Reglas de dominio / trampas conocidas

- Consultar `docs/CONSTITUTION.md` y la spec activa antes de implementar. La constitución aprobada recoge las decisiones del propietario: una empresa por instalación, entidad fiscal propia, prefijo `qb_` y español también en código. No revertir estas decisiones ni modificar los principios de la constitución sin aprobación.
- **Compra y Venta es el documento sector**, no una instrucción para emitir una factura propia por cada compra a un proveedor. La spec debe distinguir factura emitida por la empresa de factura recibida del proveedor y determinar qué registros originan una emisión. Bloquear solo esa decisión funcional hasta aclararla.
- Separar estado fiscal de estado comercial/contable. Confirmar una venta o publicar un asiento no demuestra aceptación del SIN. Las transiciones fiscales deben respaldarse con respuestas interpretadas según el contrato.
- La entidad fiscal conserva una instantánea de datos, XML firmado, payload enviado, códigos, hashes, respuesta y evidencia. Cambiar contactos o productos no debe alterar emisiones anteriores. Las correcciones siguen los procesos fiscales aplicables, sin editar o eliminar documentos validados.
- Identificar explícitamente NIT, sistema, ambiente, modalidad, sucursal y punto de venta. No confundir punto de venta fiscal del SIN con una sesión o configuración POS de Odoo.
- Conservar historial y vigencia de CUIS/CUFD y el CUFD de cada evento de contingencia. No sustituir códigos históricos por los actuales ni renovar concurrentemente sin control.
- El CUF, la numeración y los reintentos deben evitar emisiones duplicadas. Un timeout puede ocurrir después de que el SIN reciba el documento: preservar su identidad y conciliar antes de decidir reenvío. Diseñar persistencia y envío recuperables; no asumir atomicidad entre Odoo y SIN.
- Validar XML y firma contra los contratos oficiales del sector y modalidad. Respetar la secuencia de firma, compresión, hash y codificación especificada para cada operación; no modificar el XML después de firmarlo. Deshabilitar entidades externas al procesar XML.
- Calcular importes fiscales con precisión decimal y redondeo documentados; definir el mapeo desde los valores monetarios de Odoo. No inventar fórmulas de IVA, códigos, plazos, límites, XSD o respuestas.
- Gestionar explícitamente hora local `America/La_Paz`, convenciones temporales de Odoo y formato de cada contrato SIAT. No depender del huso del host ni de la preferencia horaria del usuario para determinar la fecha fiscal.
- Un paquete recibido requiere el proceso de validación y conciliación correspondiente, incluida validación parcial. Contingencia requiere eventos, códigos y procedimiento fiscal; no consiste únicamente en acumular facturas en una cola.
- Un único emisor no elimina la seguridad: aplicar ACL, reglas y comprobaciones antes de operaciones fiscales. No mezclar datos entre clientes, sucursales o ambientes. Justificar cualquier uso de `sudo()`.
- Fuente inicial: <https://siatanexo.impuestos.gob.bo/index.php/informacion>. Consultar fuentes oficiales actuales para la regla afectada y registrar evidencia. Una página en construcción o ejemplo histórico no prueba vigencia. La documentación inicial no sustituye autorización del sistema ni pruebas oficiales.

## Forma de trabajar

1. Leer este archivo, `docs/CONSTITUTION.md` y `spec.md`, `plan.md`, `tasks.md` de la funcionalidad afectada. Para trabajos Odoo, leer y aplicar la skill local `odoo-development` conforme a la sección Skills del proyecto. Inspeccionar el repositorio antes de asumir archivos, dependencias o comandos.
2. Seguir el flujo **especificación → plan → tareas → implementación → verificación**. No se requiere Spec Kit ni otra herramienta SDD. Si falta un requisito, redactar la propuesta y aclarar decisiones materiales antes de implementarlo; no ampliar unilateralmente el alcance.
3. Planificar antes de cambios que afecten modelos persistentes, contratos fiscales, estados, seguridad o infraestructura. Registrar decisiones duraderas en ADR; resolver detalles rutinarios dentro de la spec sin consultas innecesarias.
4. Implementar cambios pequeños vinculados a tareas. Evitar refactorizaciones ajenas, cambios masivos de formato y sobrescribir modificaciones del usuario.
5. Ejecutar las comprobaciones apropiadas y actualizar el estado de tareas con evidencia. No marcar una tarea terminada si su validación obligatoria está bloqueada.
6. Si una acción necesita aprobación, completar primero la preparación segura: propuesta concreta, diferencias, impacto, validación disponible y recuperación. Pedir aprobación de esa acción específica y continuar el trabajo independiente que ya esté autorizado.
7. Al terminar, explicar qué cambió, archivos afectados, pruebas realizadas y resultado, pendientes y decisiones que requieren aprobación. Distinguir pruebas ejecutadas de las omitidas; nunca afirmar que se probó lo que no se ejecutó.

## Límites

### Siempre

- Siempre: actualizar `MEMORY.md` al terminar cada tarea.

### Siempre, sin consultar de nuevo

- Leer y analizar archivos, documentación pública y código necesario para la tarea.
- Crear o editar archivos de documentación y código requeridos por requisitos ya especificados.
- Implementar las tareas especificadas con las herramientas y dependencias ya aprobadas.
- Ejecutar pruebas y comprobaciones locales aisladas, con datos sintéticos y SIAT simulado; corregir fallos relacionados con la tarea.
- Mantener trazabilidad, pruebas relevantes, documentación y protección de secretos. Revisar cambios antes de entregarlos.

### Preguntar antes de ejecutar

- Incorporar o instalar **dependencias nuevas**, incluidos addons de terceros, herramientas y servicios adicionales. Presentar propósito, versión, licencia e impacto.
- Ejecutar **migraciones** sobre datos existentes, modificar irreversiblemente su formato o realizar operaciones destructivas. Preparar procedimiento, respaldo y recuperación primero.
- Crear **commits**, hacer **push**, fusionar o modificar historial Git. Puede revisar cambios y preparar el mensaje sin ejecutarlos.
- Realizar **cualquier llamada real a servicios SIAT**, también en pruebas/piloto: emisión, consultas, sincronización, códigos y verificación de comunicación. Leer documentación pública no constituye una llamada a esos servicios. Configurar endpoints no autoriza consumirlos.
- Ejecutar **despliegues**, habilitar trabajos que produzcan efectos externos o cambiar configuración de instalaciones compartidas/productivas.
- Cambiar alcance, arquitectura material, licencia, versión mayor de Odoo o principios de la constitución; enviar documentos o mensajes a clientes; usar datos reales; eliminar recursos; alterar permisos o credenciales.

Una autorización explícita vale para el alcance concedido; no pedirla nuevamente dentro de ese alcance. Si aparece otra acción crítica fuera de él, preparar y consultar por esa acción. No pedir permisos rutinarios para crear archivos o ejecutar pruebas locales ya autorizadas.

### Nunca

- Modificar el núcleo de Odoo, introducir dependencia obligatoria de Enterprise o desarrollar POS anticipadamente.
- Guardar tokens, contraseñas o claves privadas en repositorio, fixtures, documentación, imágenes o logs; solicitar que se peguen en el chat.
- Desactivar TLS, inventar datos fiscales para superar una validación o falsificar estados de aceptación.
- Borrar evidencia fiscal, alterar XML firmado o emitir nuevamente para ocultar un resultado incierto.
- Utilizar producción como entorno de prueba, compartir secretos entre ambientes o activar envíos reales desde copias de pruebas.
- Ejecutar acciones reservadas sin aprobación ni eludir una revisión fallida. Afirmar autorización del SIN basándose únicamente en pruebas locales.

## Verificación

- **Documentación:** comprobar consistencia con decisiones confirmadas, referencias y criterios de aceptación; señalar supuestos pendientes.
- **Lógica:** probar CUF, precisión y redondeo, fechas, validaciones, unicidad y comportamiento de reintentos con casos relevantes.
- **Odoo:** verificar instalación y actualización en base desechable, modelo y tabla fiscal propios, relaciones, restricciones, permisos, sucursales y puntos de venta.
- **Interoperabilidad simulada:** verificar contratos SOAP, XML/XSD, firma con material de prueba, errores, timeouts, respuestas inciertas, contingencia y resultados parciales sin red SIAT real.
- **Entorno:** validar configuración, aislamiento de datos y ausencia de secretos/envíos reales. No ejecutar despliegues como parte implícita de la verificación.
- **Integración real:** preparar un plan separado indicando operaciones, ambiente, datos, credenciales gestionadas fuera del código y efectos esperados; ejecutarlo únicamente con aprobación.

Mientras el repositorio no tenga entorno y pruebas disponibles, informar ese bloqueo y la preparación realizada. No confundir revisión de documentación con validación funcional ni relajar una comprobación fiscal por falta de acceso.
