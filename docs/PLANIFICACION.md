# Orden propuesto de especificaciones iniciales

**Estado:** Propuesta para revisión; no aprueba dependencias, arquitectura nueva ni implementación.  
**Base:** `AGENTS.md` y `docs/CONSTITUTION.md`.  
**Fecha:** 2026-10-11

El objetivo es comprender y verificar cada incremento antes de construir el siguiente.
Los números indican orden de trabajo y dependencias; no son fechas ni estimaciones.
Las rutas siguientes son propuestas: crear cada carpeta cuando se empiece su spec.

| Orden y carpeta en `docs/specs/` | Pregunta que resuelve | Resultado verificable esperado al implementar |
| --- | --- | --- |
| 001 · `entorno-local` | ¿Cómo ejecutamos y probamos el proyecto sin afectar instalaciones de usuarios? | Versiones compatibles y aprobadas, base y filestore de pruebas desechables, addons configurados, bloqueo de red SIAT real y comandos reproducibles. |
| 002 · `alcance-y-origen-emision` | ¿Qué operación del negocio produce una factura propia y quién la autoriza? | Ejemplos de venta, factura recibida y corrección; registro origen, momento de generación y emisión, relaciones y exclusiones expresamente definidos. |
| 003 · `contratos-y-reglas-siat` | ¿Qué documentos oficiales respaldan cada comportamiento fiscal? | Inventario de contratos, XSD, perfil de firma, catálogos, respuestas y reglas aplicables, con procedencia, fecha y huellas en `docs/siat/`; vacíos y discrepancias identificados. |
| 004 · `entidad-fiscal-y-seguridad` | ¿Qué conservamos y quién puede realizar cada operación? | Modelo y tabla fiscal propios, contexto emisor/sucursal/punto/ambiente, instantánea, estados y evidencia; restricciones y permisos probados desde ORM e interfaz mínima. |
| 005 · `catalogos-y-codigos` | ¿Cómo obtenemos y conservamos los datos necesarios para emitir? | Homologaciones y catálogos versionados; historial y vigencia de CUIS/CUFD; renovación coordinada y errores simulados, sin sobrescribir códigos históricos. |
| 006 · `preparacion-fiscal` | ¿Cómo convertimos el registro comercial en datos fiscales correctos? | Mapeo de datos, cálculos decimales, redondeo, fechas, numeración y CUF con pruebas y trazabilidad; reservas seguras frente a concurrencia y reintentos. |
| 007 · `xml-firma-y-payload` | ¿Cómo producimos y verificamos el documento exacto que se enviará? | XML validado, firma con material sintético, verificación local y payload conforme a cada contrato; bytes y hashes conservados sin modificar el XML firmado. |
| 008 · `emision-y-conciliacion` | ¿Cómo enviamos y sabemos qué ocurrió cuando algo falla? | Emisión completa contra un simulador, interpretación de respuestas, persistencia recuperable, timeout, reinicio y conciliación sin duplicados; pantalla con estado y acciones autorizadas. |
| 009 · `representacion-y-consulta` | ¿Cómo consulta el usuario la factura y su evidencia? | Representación gráfica, QR y leyendas conforme al contrato; consulta y descarga con permisos y estado fiscal visible, sin publicar ni enviar documentos reales a clientes. |
| 010 · `correcciones-fiscales` | ¿Cómo resolvemos errores sin editar evidencia ni confundirlos con cambios contables? | Procesos aplicables de anulación, reversión o ajuste definidos por fuente y alcance; permisos, referencias, resultados inciertos y coordinación contable probados mediante simulación. |
| 011 · `contingencia-y-paquetes` | ¿Qué hacemos ante indisponibilidad y cómo conciliamos cada factura después? | Eventos y códigos históricos, procedimientos y límites respaldados por fuentes, paquetes y validación parcial; escenarios de recuperación simulados. |
| 012 · `operacion-y-piloto` | ¿Cómo mantenemos el sistema y qué falta para habilitar su uso real? | Procedimientos de diagnóstico, respaldo y restauración, retención, vencimientos y actualizaciones; plan separado de pruebas oficiales con operaciones, ambiente y efectos para aprobación. |

## Dependencias y puntos de control

- **001 y 002:** pueden redactarse en paralelo documentalmente. El entorno es la
  primera condición técnica; definir el origen de emisión es la primera decisión
  funcional. Una compra a un proveedor no implica una emisión propia.
- **003:** comienza en cuanto el alcance permite identificar contratos. Su inventario
  se completa y revisa durante todo el proyecto; no es una investigación única.
  Toda implementación fiscal necesita la evidencia aplicable, incluso si el resto
  del inventario sigue pendiente.
- **004:** requiere 002 y los contratos de 003 que afecten identidad, evidencia y
  estados. Su implementación requiere 001. Diseñar el ciclo de vida completo aquí
  evita rehacer la persistencia al añadir correcciones o contingencia.
- **005–007:** se construyen sobre 004 y los contratos aplicables. Numeración, CUF,
  reserva y recuperación se diseñan conjuntamente con 008; no se asigna una nueva
  identidad por cada intento de transporte.
- **008:** integra lo anterior y constituye el primer recorrido completo de emisión
  simulada. Los estados contables y fiscales siguen separados.
- **009–011:** se detallan después de ese recorrido, pero sus necesidades de datos
  y estados se anticipan en 003–004. Diferir su implementación no las excluye del
  análisis ni habilita producción sin los procedimientos aplicables.
- **012:** la protección de datos y estrategia de recuperación empiezan en 001–004;
  aquí se verifica su operación de conjunto. Ninguna llamada real al SIAT ni
  despliegue queda autorizado por completar las pruebas locales.

## Cómo escribir y trabajar cada especificación

Cada `spec.md` debe contener problema, usuarios y casos concretos, alcance y
exclusiones, requisitos numerados, decisiones y dudas, dependencias, fuentes y
criterios de aceptación observables. Debe poder entenderse sin leer el código.

Después, `plan.md` describe componentes, modelos, contratos, permisos, errores,
pruebas y razones del diseño. Evalúa primero las capacidades de Odoo Community
y referencias existentes compatibles; toda dependencia nueva se presenta con
propósito, versión, licencia e impacto antes de incorporarse. La licencia del
proyecto sigue pendiente y debe resolverse antes de distribuir módulos propios.

Finalmente, `tasks.md` divide el plan en cambios pequeños y verificables. Cada
tarea enlaza requisito y validación; registra resultado, evidencia y bloqueo.
Una prueba obligatoria bloqueada impide cerrar la tarea afectada.

Las pruebas y la interfaz mínima pertenecen a cada incremento. No se reservan
para una fase final: cada nuevo comportamiento incorpora casos normales,
permisos y fallos relevantes. Cada incremento debe permitir una demostración
con datos sintéticos y explicar dónde revisar el comportamiento si falla.

Para mantenimiento, cada spec identifica las fuentes y componentes afectados
por cambios, cómo verificar una modificación y qué procedimientos de diagnóstico
o recuperación necesita el operador. Las decisiones duraderas van a `docs/adr/`;
los comandos comprobados, al lugar indicado en `AGENTS.md`. `MEMORY.md` conserva
el estado y las decisiones entre sesiones.

## Primer trabajo recomendado

Redactar `docs/specs/entorno-local/spec.md` y cerrar su plan antes de implementar infraestructura.
No adoptar las etiquetas actuales del Compose como versiones verificadas.
La referencia para la imagen oficial es el [repositorio de Odoo Docker](https://github.com/odoo/docker);
se requiere comprobar la revisión concreta y su Python, PostgreSQL y dependencias.
Si no se demuestra compatibilidad con Python 3.12, registrar el bloqueo y presentar
alternativas para aprobación sin cambiar el objetivo unilateralmente.

Al mismo tiempo, preparar los ejemplos de 002 para decidir el registro origen y
si la emisión se solicita manualmente o se vincula a una acción comercial/contable.
Este documento no elige ese comportamiento.

Se intentó consultar el [índice público SIAT](https://siatanexo.impuestos.gob.bo/index.php/informacion)
el 2026-10-11, pero la consulta terminó en timeout. No se verificaron contratos
fiscales en esta tarea ni se realizaron llamadas a servicios SIAT.
