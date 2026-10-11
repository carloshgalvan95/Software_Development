# Revisión REST del borrador generado por el agente

Autor: Carlos Humberto Galván Perales (A01797969)
Curso: TC5062, actividad S04-A2

El primer `openapi.yaml` de Cobra360 lo generó Grok Bot, el agente de IA con el que trabajo, a partir del SRS. Ese borrador tiene 15 operaciones y no pasa la validación con openapi-spec-validator por un error en un subesquema. Aquí reviso el borrador contra buenas prácticas REST. Para cada observación anoto qué hace el borrador, por qué lo considero un problema y qué cambia en el `openapi.yaml` final. En dos casos la corrección más obvia no me sirve y explico por qué elijo otra.

## Observación 1. Verbos en las rutas

Qué hace el borrador: `POST /cash/payments/{paymentId}/void`, `POST /cash/cash-closings/{closingId}/reconcile` y `POST /portfolio/import` llevan el verbo en la URL.

Mi valoración: el problema es real, pero la salida obvia de cambiar la anulación por `DELETE /payments/{paymentId}` no encaja. RF-11 dice que el pago se anula sin borrarse y conserva su folio, y RF-08-AC-4 pide seguir mostrando el recibo con la leyenda ANULADO. Un `DELETE` que deja el recurso consultable contradice el significado del verbo. Tampoco me convence un `PATCH` sobre el estado del corte para conciliar, porque la conciliación trae datos propios (efectivo recibido, comentario, supervisor, fecha) y reglas que no aplican a otros campos del corte.

Acción en el yaml: la anulación y la conciliación son subrecursos únicos, `PUT /payments/{paymentId}/annulment` y `PUT /cash-closings/{closingId}/reconciliation`, y la importación es la colección `POST /portfolio-imports`.

## Observación 2. Nombres de recursos

Qué hace el borrador: mezcla prefijos de módulo (`/cash`, `/field`, `/portfolio`, `/insights`) con recursos, y tiene rutas en singular como `/field/route` y `/sync/payments`.

Mi valoración: los prefijos reflejan las carpetas del backend de ADR-001 y no tienen por qué ser parte del contrato. Si mañana muevo el corte a otro módulo, la URL no debería cambiar.

Acción en el yaml: todas las colecciones son sustantivos en plural, en minúsculas y con guion (`cash-closings`, `portfolio-imports`, `sync-batches`, `audit-events`, `reconciled-payments`). Los módulos quedan como `tags`. La ruta del gestor es `GET /collectors/me/route`, un recurso singular que pertenece al gestor autenticado.

## Observación 3. Idempotencia

Qué hace el borrador: usa un encabezado `Idempotency-Key` solo en `/sync/payments`; el registro en línea de visitas y pagos no tiene protección contra reintentos, y la anulación y la conciliación son POST.

Mi valoración: un reintento por mala señal puede duplicar un pago en línea igual que uno sin conexión. Podría exigir `Idempotency-Key` en todos los POST, pero un encabezado genérico obliga a guardar respuestas por llave en una tabla aparte. En Cobra360 ya existe un identificador natural, el UUID que el dispositivo asigna a cada registro (RF-13), así que lo pongo en el cuerpo (`clientUuid`) con restricción única en la base. Para anulación y conciliación la idempotencia viene de usar PUT: si el supervisor reintenta, el saldo no se revierte dos veces.

Acción en el yaml: `clientUuid` es obligatorio en `VisitCreate`; `POST /visits` responde 201 al crear y 200 con la visita original si el UUID ya existía; `POST /sync-batches` reporta `DUPLICADO` con el folio original (RF-13-AC-3).

## Observación 4. Códigos de estado

Qué hace el borrador: `POST /portfolio/import` responde 200 aunque crea un recurso; casi ninguna operación declara 401 ni 500; las reglas de negocio se reparten entre 409 y 422 sin criterio claro; el login solo declara 401, incluso para el usuario inactivo.

Mi valoración: hay un punto donde otra práctica común no aplica. Muchas APIs responden 404 cuando alguien pide un recurso ajeno, para no revelar que existe. RF-05-AC-3 pide explícitamente el mensaje "No tienes permiso para realizar esta acción" y el evento ACCESO_DENEGADO, así que uso 403.

Acción en el yaml: hay respuestas reutilizables (`BadRequest`, `Unauthorized`, `Forbidden`, `NotFound`, `UnprocessableEntity`, `InternalError`) aplicadas en las 27 operaciones. Las creaciones responden 201 (`/sessions`, `/users`, `/portfolio-imports`, `/assignments`, `/visits`, `/cash-closings`). Uso 400 para errores de forma, 422 para reglas del SRS (horario, montos, comentario, corte pendiente), 409 solo para el correo duplicado y 403 para el usuario inactivo en el login (RF-01-AC-4), porque ahí las credenciales sí son correctas. También declaro 413 para archivos y lotes que exceden el límite.

## Observación 5. Montos como número

Qué hace el borrador: define los montos como `number` con formato `double`. En JavaScript eso introduce errores de redondeo y no permite validar los dos decimales de RD-02 ni el rechazo de 150.555 de RF-07-AC-3.

Acción en el yaml: el esquema `Money` es una cadena con patrón de exactamente dos decimales y lo uso en todos los montos.

## Resultado

El yaml final valida sin errores con openapi-spec-validator, tiene 27 operaciones y cada una lleva `x-acceptance-criteria` con IDs reales del SRS. Los 61 criterios quedan referenciados al menos una vez; el detalle está en `decisiones_y_trazabilidad.md`.
