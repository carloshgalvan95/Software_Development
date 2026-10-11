# ADR-001: Estilo arquitectónico de Cobra360

Autor: Carlos Humberto Galván Perales (A01797969)
Curso: TC5062 Análisis, diseño y construcción de software, actividad S04-A1
Proyecto: Cobra360, gestión de cobranza en campo (SRS de la actividad S02-A1)

## Fecha

2026-10-10

## Estado

Aceptada. La revisaré cuando el equipo cierre el stack definitivo en la semana 5; si cambiara el lenguaje del backend, la decisión de estilo se mantiene y solo cambia la tecnología de cada contenedor.

## Contexto

Cobra360 es una aplicación nueva que vive al lado del sistema core de una caja de ahorro. El core sigue siendo la fuente oficial de saldos y solo intercambia archivos CSV con nosotros (R-4). Los usuarios son gestores en campo con celulares Android de gama baja y señal intermitente, y supervisores y gerentes en oficina. El proyecto lo construye un equipo de cuatro personas en doce semanas, aunque cada uno debe poder levantar y modificar el sistema completo por su cuenta, así que evalué las alternativas pensando también en lo que puede sostener un solo desarrollador.

Los requerimientos que más condicionan la arquitectura son estos:

| Requerimiento | Qué le exige a la arquitectura |
|---|---|
| RF-13 y RNF-07 | El gestor captura sin conexión; la cola local guarda 300 registros por 72 horas y sincroniza 100 registros en 60 s o menos sin duplicar pagos (RF-13-AC-3). |
| RF-07, RF-10, RF-11 y RD-04 | El pago, el folio consecutivo C360-NNNNNNNN, el saldo del crédito y el corte de caja tienen que cambiar juntos o no cambiar; un folio nunca se reutiliza. |
| RNF-03 y RNF-04 | Autorización en el servidor por rol y por relación con el registro; bitácora de solo inserción por 5 años. |
| RNF-05 y RNF-08 | p95 de 2 s en ruta, visita y pago con 25 usuarios concurrentes, 20,000 créditos y 1,000,000 de pagos históricos. Es un volumen modesto para una sola base relacional. |
| RNF-09 | Vista del gestor como PWA en Chrome para Android 10 con 2 GB de RAM y 2 MB o menos en la primera carga. |
| R-5 y R-6 | Equipo pequeño, curso de doce semanas, API documentada en OpenAPI antes de implementarse. |

## Decisión

Adopto un monolito modular desplegado con Docker Compose en un solo servidor en la nube. Hay tres contenedores propios y uno de soporte:

1. Una PWA en React con Vite y TypeScript, servida como archivos estáticos, con Service Worker y una cola en IndexedDB para la operación sin conexión.
2. Una API única en FastAPI organizada por módulos de dominio (`auth`, `cartera`, `campo`, `efectivo`, `sincronizacion`, `indicadores`, `bitacora`). Los módulos solo se comunican por interfaces de servicio internas, nunca leyendo tablas de otro módulo directamente.
3. PostgreSQL 16 como única fuente de verdad de Cobra360, con el folio generado por una secuencia dentro de la misma transacción que inserta el pago y actualiza el saldo.
4. Un proxy inverso (Caddy) que termina TLS y redirige HTTP a HTTPS, y un proceso programado que ejecuta el cierre diario de promesas (RF-09-AC-4).

## Alternativas

### A. Monolito modular (elegida)

Un solo proceso backend con módulos internos, una base de datos y una PWA.

### B. Microservicios con cola de mensajes

Tres servicios (identidad y usuarios, cartera y campo, efectivo) con su propia base y una cola (RabbitMQ o Redis Streams) para propagar eventos de pago y de sincronización.

### C. Backend como servicio (Supabase o Firebase)

La PWA habla directamente con un backend administrado: autenticación, base de datos y reglas de acceso del proveedor, con funciones en la nube para la lógica de folio y conciliación.

### Comparación

| Criterio | A. Monolito modular | B. Microservicios | C. Backend como servicio |
|---|---|---|---|
| Escalabilidad | Escala vertical y, si hiciera falta, réplicas de la API detrás del proxy. Sobra para RNF-05 y RNF-08 (25 usuarios, 20,000 créditos). | La mejor en teoría; cada servicio escala por separado. Para 25 usuarios es capacidad que no vamos a usar. | Buena y automática, pero dependo de los límites del plan gratuito y de la latencia de las funciones en frío para el p95 de 2 s. |
| Mantenibilidad | Un repositorio, un contrato OpenAPI y migraciones centralizadas con Alembic. El riesgo es que los módulos se acoplen; lo mitigo con la regla de interfaces internas y pruebas por módulo. | Fronteras fuertes entre servicios, pero el contrato se reparte en varias specs y cada cambio que cruza el pago y el saldo toca dos o tres repositorios. | Poco código propio, aunque las reglas de autorización por relación (RNF-03) quedan repartidas entre políticas de la base y funciones, más difíciles de probar y versionar. |
| Complejidad | Baja. Una transacción de base de datos resuelve pago, folio, saldo y promesa (RF-07-AC-1, RF-09-AC-3, RF-11-AC-1). | Alta. El folio y el saldo viven en servicios distintos, así que necesitaría sagas o compensaciones para mantener RD-04 y RF-11. Agrega cola, observabilidad distribuida y más contenedores. | Media. La idempotencia de la sincronización (RF-13-AC-3) y la bitácora de solo inserción (RNF-04) hay que construirlas sobre primitivas del proveedor. |
| Adecuación a un desarrollador | Alta. Una persona levanta todo con `docker compose up`, depura en un solo proceso y lo despliega en una VM pequeña. | Baja. Un desarrollador pasaría más tiempo operando infraestructura que implementando los 13 RF. | Media. Arranca rápido, pero amarra el proyecto a un proveedor y deja fuera la práctica de diseño de API que pide el curso (R-6). |
| Seguridad y auditoría (RNF-03, RNF-04) | Autorización centralizada en dependencias de FastAPI; bitácora en una tabla con permisos de solo inserción. | Hay que propagar identidad y bitácora entre servicios. | Depende de reglas del proveedor; la bitácora inmutable no es nativa. |

## Consecuencias

Positivas:

- Las operaciones que mueven dinero son atómicas sin coordinación distribuida. Esto cubre directamente RD-04 (folio único sin huecos por colisión), RF-11-AC-1 (anulación que revierte saldo y total del corte) y RF-13-AC-3 (reenvío idempotente usando el UUID del dispositivo como restricción única).
- Hay un solo `openapi.yaml` que sirve de contrato para la PWA y para las pruebas con Schemathesis, alineado con R-6 y con la actividad S04-A2.
- El despliegue cabe en una VM de 2 vCPU y 4 GB, lo que deja margen para RNF-05 y RNF-08 y simplifica la disponibilidad de 99.0 % de RNF-07 (un monitor externo y respaldos diarios de PostgreSQL).

Negativas y riesgos que acepto:

- Un fallo del proceso de la API deja sin servicio a todos los módulos. Lo mitigo con reinicio automático de contenedores y con la cola sin conexión de la PWA, que permite seguir cobrando mientras el servidor vuelve (RF-13).
- Si los módulos no respetan sus fronteras, el monolito se vuelve difícil de mantener. Lo controlo con la estructura de carpetas por módulo y revisión en cada PR.
- Escalar horizontalmente exige que la API no guarde estado en memoria; por eso la sesión es un JWT de vida corta (RNF-02) y la idempotencia vive en la base, no en caché.
- Si en una versión futura la institución pidiera integración en línea con el core, agregaría un módulo adaptador dentro del monolito antes de pensar en separar servicios.

## Relación con los RNF del SRS

| RNF | Cómo lo atiende la decisión |
|---|---|
| RNF-01, RNF-02 | Módulo `auth` con hash Argon2id, bloqueo por intentos y JWT con expiración distinta por rol. |
| RNF-03 | Caddy fuerza HTTPS con TLS 1.2 o superior; la API verifica rol y relación con el registro en cada endpoint. |
| RNF-04 | Tabla `evento_bitacora` de solo inserción, escrita por el módulo `bitacora` dentro de la misma transacción de cada operación auditada. |
| RNF-05, RNF-08 | Una base relacional con índices por gestor, crédito y fecha; volumen pequeño para PostgreSQL. |
| RNF-06, RNF-09 | PWA ligera servida como estáticos, sin app nativa. |
| RNF-07 | Cola en IndexedDB en el cliente y reinicio automático de contenedores en el servidor. |

## Revisión crítica de la propuesta del agente

Para esta actividad usé a Grok Bot, el agente de IA con el que trabajo, para generar un primer borrador de la arquitectura a partir del SRS. Su propuesta coincidió conmigo en el estilo (monolito modular con PWA y PostgreSQL) y en generar el folio en el servidor. Lo que sigue son las partes que revisé y cambié, con el motivo de cada cambio.

### Sobre el ADR y los diagramas de arquitectura

- El agente comparó solo dos alternativas (monolito y backend partido con cola) y las evaluó con ventajas y desventajas genéricas. Agregué la alternativa de backend como servicio, porque para un equipo pequeño es la opción más tentadora y merecía descartarse con argumentos, y construí la tabla con los cuatro criterios que pide la actividad ligados a RNF concretos.
- El agente dibujó un diagrama `C4Context` con contenedores dentro y sin proxy ni proceso programado. Lo cambié a un C4 de contenedores real. El proxy tiene que aparecer porque es donde se cumple el TLS y la redirección de RNF-03, y el proceso programado tiene que aparecer porque RF-09-AC-4 exige que algo cambie promesas a Incumplida a las 00:05 sin que nadie haga una petición.
- El agente no propuso diagrama de despliegue. Lo hice yo con una VM y Docker Compose, y le agregué el respaldo diario de PostgreSQL, porque RNF-04 pide conservar la bitácora 5 años y eso no se sostiene con un volumen local sin respaldo.

### Decisiones de diseño propias en el diagrama de clases

El borrador del agente no traía diagrama de clases, así que comparo contra el modelo de datos implícito en su `openapi.yaml` y en su documento de arquitectura. Ahí aparecen cuatro supuestos que no comparto.

D1. Herencia de usuarios. En el borrador el usuario es un solo esquema con un campo `role` que puede valer Gestor, Supervisor o Gerente. Prefiero modelar `Usuario` como clase abstracta con las subclases `Gestor`, `Supervisor` y `Gerente`, porque cada rol tiene relaciones que los otros no tienen: solo un gestor tiene supervisor responsable (RF-02-AC-4), créditos asignados y efectivo no entregado, y solo el gerente administra el límite de efectivo (RF-12-AC-6). Con un campo de texto esas reglas quedan como validaciones sueltas; con herencia, las multiplicidades del diagrama ya las expresan. En la base usaré una sola tabla con discriminador, así que la herencia no complica las consultas.

D2. La visita compone al pago y a la promesa. El borrador registra el pago con su propio `POST /cash/payments` y deja `visitId` como opcional (`nullable`) en el pago, o sea, permite pagos sin visita. El SRS dice que un resultado PAGO exige capturar el pago en la misma operación (RF-06 y RF-07) y que la promesa nace de una visita PROMESA (RF-09-AC-1). Por eso uso `Visita *-- Pago [0..1]` y `Visita *-- Promesa [0..1]`: un pago no existe sin su visita, y el UUID del dispositivo vive en la visita, lo que hace natural la deduplicación de RF-13-AC-3.

D3. Anulación como clase propia. En el borrador anular un pago solo cambia su estado a Anulado y pide un motivo de al menos un carácter; no queda registrado quién lo autorizó ni cuándo. RF-11 pide conservar el pago, su folio, el motivo y quién lo anuló, con motivo de al menos 10 caracteres (RF-11-AC-4), y RF-08-AC-4 pide mostrar la fecha de anulación en el recibo. Por eso `Anulacion` es una clase compuesta en `Pago` (0..1) con motivo, fecha y usuario que autoriza, y el pago pasa al estado ANULADO sin borrarse.

D4. Registro sobre crédito reasignado. El documento del agente dice que sincronizar una visita sobre un crédito que ya se reasignó debe terminar en error de dominio. RF-13-AC-7 pide lo contrario: aceptar el registro, porque el gestor sí estuvo con el socio y quizá recibió dinero, y dejar la marca en bitácora. Agregué el atributo `fueraDeAsignacion` en `Visita` y la relación con `EventoBitacora`, de modo que el pago no se pierde y el supervisor puede revisarlo.

### Detalles menores que corregí

- El agente escribió "Gerente" como quien importa la cartera en la secuencia de importación; el SRS asigna esa función al Supervisor (RF-03).
- El agente nombró al sistema externo "Core SIC" y le dibujó una relación directa hacia la API. El core no se conecta con Cobra360: un usuario de oficina mueve los archivos CSV (R-4). Corregí la relación en el diagrama de contenedores.
