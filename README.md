# Quiboot · Facturación electrónica Bolivia

Proyecto de módulos para Odoo Community 20, guiado por especificaciones.
La constitución vigente está en [docs/CONSTITUTION.md](docs/CONSTITUTION.md)
y las instrucciones de trabajo en [AGENTS.md](AGENTS.md).

## Estado del entorno

El Compose y la plantilla `config/odoo.conf.example` son preparación inicial:
las versiones, compatibilidad con Python 3.12 y comandos operativos todavía
requieren especificación y verificación. No hay pruebas funcionales ejecutables
del proyecto ni autorización implícita para llamadas reales al SIAT.
La configuración local `config/odoo.conf`, `.env`, base y filestore quedan
excluidos de Git. El marcador de contraseña de la plantilla debe sustituirse
únicamente en la configuración local.

## Referencia de desarrollo incluida

La skill ya instalada se conserva en `.agents/skills/odoo-development-skill/`,
con su licencia MIT original en `LICENSE`. Procede de
`fhidalgodev/odoo-development-skill`; `skills-lock.json` registra la fuente y
huella de instalación. Se incluye como documentación para agentes, sin añadir
dependencias de ejecución ni elegir la licencia del proyecto.

Sus ejemplos y afirmaciones de compatibilidad requieren verificación contra
la revisión objetivo de Odoo. Las decisiones del propietario, `AGENTS.md` y
la constitución prevalecen sobre sus pautas, incluido el idioma del código.
