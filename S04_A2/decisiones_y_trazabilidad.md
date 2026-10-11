# Decisiones de diseño y trazabilidad de la API de Cobra360

Autor: Carlos Humberto Galván Perales (A01797969)
Curso: TC5062, actividad S04-A2
Contrato: `openapi.yaml` (OpenAPI 3.0.3, 27 operaciones, validado con openapi-spec-validator)

## Sección A. Trazabilidad de criterios de aceptación

La tabla incluye los 61 criterios Dado-Cuando-Entonces del SRS de la actividad S02-A1 (RF-01-AC-1 a RF-13-AC-7). Los IDs son los mismos del SRS; no agregué ni renombré ninguno. Cada operación del `openapi.yaml` declara estos IDs en `x-acceptance-criteria`, y verifiqué con un script que no haya IDs inexistentes ni criterios sin operación.

| ID Criterio | Descripción resumida | Endpoint | Método | ¿Cubierto? |
|---|---|---|---|---|
| RF-01-AC-1 | Inicio de sesión válido crea sesión con rol y registra LOGIN_OK | /sessions<br>/collectors/me/route<br>/audit-events | POST<br>GET<br>GET | Sí |
| RF-01-AC-2 | Contraseña incorrecta no crea sesión y no dice qué dato falló | /sessions<br>/audit-events | POST<br>GET | Sí |
| RF-01-AC-3 | Gestor que intenta importar cartera o administrar usuarios recibe rechazo | /users<br>/users<br>/portfolio-imports<br>/audit-events | GET<br>POST<br>POST<br>GET | Sí |
| RF-01-AC-4 | Usuario desactivado no puede iniciar sesión | /sessions | POST | Sí |
| RF-02-AC-1 | Gerente crea gestor con supervisor responsable en estado Activo | /users<br>/users<br>/users/{userId} | GET<br>POST<br>GET | Sí |
| RF-02-AC-2 | Alta con correo duplicado se rechaza | /users | POST | Sí |
| RF-02-AC-3 | Desactivar gestor conserva pagos y deja sus créditos sin asignar | /users<br>/users/{userId}<br>/credits<br>/payments | GET<br>PATCH<br>GET<br>GET | Sí |
| RF-02-AC-4 | Gestor sin supervisor responsable se rechaza | /users<br>/users/{userId} | POST<br>PATCH | Sí |
| RF-02-AC-5 | No se desactiva un gestor con efectivo no entregado | /users/{userId}<br>/dashboard | PATCH<br>GET | Sí |
| RF-03-AC-1 | Importación válida crea 480 y actualiza 20 créditos | /portfolio-imports | POST | Sí |
| RF-03-AC-2 | Filas inválidas se rechazan con reporte CSV fila, campo, motivo | /portfolio-imports<br>/portfolio-imports/{importId}/rejected-rows | POST<br>GET | Sí |
| RF-03-AC-3 | Falta de columna obligatoria rechaza el archivo completo | /portfolio-imports | POST | Sí |
| RF-03-AC-4 | Se descuentan pagos Registrados posteriores a la fecha de corte | /portfolio-imports | POST | Sí |
| RF-03-AC-5 | Créditos ausentes no cambian y se conserva el gestor asignado | /portfolio-imports | POST | Sí |
| RF-04-AC-1 | Asignar créditos sin asignar a un gestor a cargo, con bitácora | /credits<br>/assignments<br>/audit-events | GET<br>POST<br>GET | Sí |
| RF-04-AC-2 | Reasignar pendientes mueve solo créditos sin visita hoy | /assignments | POST | Sí |
| RF-04-AC-3 | No se asigna a gestores inactivos | /assignments | POST | Sí |
| RF-04-AC-4 | No se asigna a gestores de otro supervisor | /assignments | POST | Sí |
| RF-05-AC-1 | Ruta ordenada por atraso y monto, sin créditos al corriente | /collectors/me/route | GET | Sí |
| RF-05-AC-2 | Créditos con promesa vigente al final con su etiqueta | /collectors/me/route | GET | Sí |
| RF-05-AC-3 | Gestor no ve ni abre créditos de otro gestor | /credits/{creditId}<br>/collectors/me/route | GET<br>GET | Sí |
| RF-06-AC-1 | Visita con ubicación se guarda y aparece como última visita | /collectors/me/route<br>/visits | GET<br>POST | Sí |
| RF-06-AC-2 | Sin permiso de ubicación se guarda "Sin ubicación" | /visits | POST | Sí |
| RF-06-AC-3 | Visita a las 22:00:01 se rechaza | /visits | POST | Sí |
| RF-06-AC-4 | Resultado OTRO exige comentario | /visits | POST | Sí |
| RF-07-AC-1 | Pago válido recibe folio consecutivo y actualiza saldo | /visits<br>/payments/{paymentId} | POST<br>GET | Sí |
| RF-07-AC-2 | Pago que cubre el vencido deja el crédito fuera de la ruta | /collectors/me/route<br>/visits | GET<br>POST | Sí |
| RF-07-AC-3 | Montos mayores al saldo, cero o con 3 decimales se rechazan | /visits | POST | Sí |
| RF-07-AC-4 | Pagos de días anteriores sin corte bloquean nuevos pagos | /visits | POST | Sí |
| RF-08-AC-1 | Recibo público muestra folio y crédito enmascarado sin datos de contacto | /receipts/{token} | GET | Sí |
| RF-08-AC-2 | Compartir recibo por menú del sistema y QR | /visits<br>/payments/{paymentId} | POST<br>GET | Parcial (cliente) |
| RF-08-AC-3 | Token inexistente o alterado responde "Recibo no encontrado" | /receipts/{token} | GET | Sí |
| RF-08-AC-4 | Recibo de pago anulado muestra ANULADO y fecha | /payments/{paymentId}/annulment<br>/receipts/{token} | PUT<br>GET | Sí |
| RF-09-AC-1 | Promesa válida queda Vigente y etiqueta la ruta | /collectors/me/route<br>/visits | GET<br>POST | Sí |
| RF-09-AC-2 | Fecha fuera de 1 a 15 días o promesa duplicada se rechaza | /visits | POST | Sí |
| RF-09-AC-3 | Promesa pasa a Cumplida al completar el monto | /visits<br>/promises | POST<br>GET | Sí |
| RF-09-AC-4 | Proceso de las 00:05 marca Incumplida y la lista al supervisor | /promises | GET | Sí (proceso programado + GET) |
| RF-10-AC-1 | Corte agrupa pagos sin corte y deja efectivo no entregado en 0 | /cash-closings<br>/cash-closings/{closingId} | POST<br>GET | Sí |
| RF-10-AC-2 | No se genera corte con registros sin sincronizar | /cash-closings | POST | Sí (con dato del cliente) |
| RF-10-AC-3 | Conciliación sin diferencia deja el corte Conciliado | /cash-closings<br>/cash-closings/{closingId}/reconciliation | GET<br>PUT | Sí |
| RF-10-AC-4 | Diferencia exige comentario de 10 a 200 caracteres | /cash-closings/{closingId}/reconciliation | PUT | Sí |
| RF-10-AC-5 | Corte de ayer sin conciliar no bloquea pagos de hoy | /visits<br>/cash-closings | POST<br>GET | Sí |
| RF-10-AC-6 | Exportación CSV de pagos Registrados de cortes conciliados | /reconciled-payments | GET | Sí |
| RF-11-AC-1 | Anulación conserva folio y revierte saldo y total del corte | /payments/{paymentId}/annulment<br>/cash-closings/{closingId}<br>/audit-events | PUT<br>GET<br>GET | Sí |
| RF-11-AC-2 | Gestor no puede anular pagos | /payments/{paymentId}/annulment | PUT | Sí |
| RF-11-AC-3 | Pago de corte conciliado no se anula | /payments/{paymentId}/annulment | PUT | Sí |
| RF-11-AC-4 | Motivo de menos de 10 caracteres se rechaza | /payments/{paymentId}/annulment | PUT | Sí |
| RF-11-AC-5 | Anular un pago devuelve la promesa a Vigente | /collectors/me/route<br>/payments/{paymentId}/annulment | GET<br>PUT | Sí |
| RF-12-AC-1 | "Cobrado hoy" suma solo pagos Registrados con retraso máximo de 60 s | /dashboard | GET | Sí (el límite de 60 s es RNF) |
| RF-12-AC-2 | Alerta cuando el efectivo no entregado supera el límite | /dashboard<br>/settings/cash-limit | GET<br>GET | Sí |
| RF-12-AC-3 | Supervisor ve solo a sus gestores; Gerente ve a todos | /dashboard | GET | Sí |
| RF-12-AC-4 | Cartera vencida por tramo de atraso | /dashboard | GET | Sí |
| RF-12-AC-5 | Porcentaje de promesas cumplidas con un decimal | /dashboard | GET | Sí |
| RF-12-AC-6 | Solo el Gerente cambia el límite y queda en bitácora | /settings/cash-limit<br>/audit-events | PUT<br>GET | Sí |
| RF-13-AC-1 | Sin conexión el dispositivo guarda en cola y muestra comprobante provisional | /sync-batches | POST | Parcial (cliente) |
| RF-13-AC-2 | Al reconectar se procesan en orden de captura y se asignan folios | /sync-batches | POST | Sí |
| RF-13-AC-3 | Reenvío del mismo UUID no duplica el pago | /visits<br>/sync-batches | POST<br>POST | Sí |
| RF-13-AC-4 | Pago que excede el saldo al sincronizar queda En revisión | /sync-batches<br>/payments | POST<br>GET | Sí |
| RF-13-AC-5 | Visita sin pago fuera de horario se rechaza al sincronizar | /sync-batches | POST | Sí |
| RF-13-AC-6 | Pago fuera de horario queda En revisión con marca | /sync-batches<br>/payments | POST<br>GET | Sí |
| RF-13-AC-7 | Registro sobre crédito reasignado se acepta y se marca en bitácora | /sync-batches<br>/audit-events | POST<br>GET | Sí |

### Criterios con cobertura parcial o compartida

- RF-13-AC-1 es comportamiento del dispositivo: guardar en IndexedDB, mostrar el comprobante provisional y aumentar el contador ocurre sin red, así que ninguna petición HTTP puede cubrirlo. Lo asocié a `POST /sync-batches` porque ese es el destino de la cola, pero su prueba será de la PWA (Playwright con red desactivada).
- RF-08-AC-2 depende del menú de compartir del sistema operativo y del QR que dibuja la PWA. La API solo aporta `receiptUrl` en la respuesta del pago.
- RF-10-AC-2 exige conocer cuántos registros hay en la cola local, que el servidor no puede ver. Decidí que la PWA envíe `pendingOfflineCount` al crear el corte y que la API rechace con 422 si es mayor a 0. Es una validación que depende de un cliente honesto; la acepto porque el objetivo del criterio es evitar cortes incompletos por descuido, no por fraude, y el corte siguiente recogería los pagos sincronizados tarde.
- RF-09-AC-4 lo ejecuta el proceso programado de las 00:05 (ver diagrama de despliegue de S04-A1). La API solo expone el resultado con `GET /promises?status=INCUMPLIDA`.
- RF-12-AC-1 incluye una condición de frescura de 60 segundos que es de rendimiento; el endpoint calcula al momento, así que la cumple mientras no agregue caché.

## Sección B. Decisiones de diseño

### Por qué estos recursos

Partí de los sustantivos del SRS y no de las pantallas. Los recursos principales son `users`, `portfolio-imports`, `credits`, `assignments`, `visits`, `payments`, `promises`, `cash-closings` y `sync-batches`, más `receipts` para el acceso público y `audit-events` para la bitácora. Algunas decisiones que vale la pena explicar:

- `visits` es el punto de entrada para registrar pagos y promesas. El SRS dice que una visita PAGO exige capturar el pago en la misma operación, así que no tiene sentido un `POST /payments` separado que permita pagos sin visita. Los pagos sí tienen colección propia para consultar, filtrar y anular.
- `sync-batches` es un recurso aparte de `visits` porque el lote tiene semántica propia: orden de captura, resultado por registro y respuesta 200 aunque algunos registros se rechacen. Cada registro del lote usa el mismo esquema `VisitCreate`, de modo que la PWA no mantiene dos formatos.
- La anulación y la conciliación son subrecursos únicos (`/payments/{id}/annulment`, `/cash-closings/{id}/reconciliation`) con PUT. Así evito verbos en la URL y la operación es idempotente: si la red falla y el supervisor reintenta, el saldo no se revierte dos veces.
- `assignments` se modela como recurso creado con POST porque una asignación es un evento con efectos (bitácora, ruta del gestor) y puede tocar varios créditos a la vez.
- `collectors/me/route` usa el alias `me` para que el gestor nunca envíe su propio id, lo que cierra de raíz el acceso cruzado de RF-05-AC-3.

### Qué quedé fuera de alcance

- Cierre de sesión en el servidor. Uso JWT de vida corta (RNF-02) y la PWA borra el token; una lista de revocación agrega estado que no pide ningún criterio.
- Edición de visitas y pagos. El SRS solo permite anular pagos, y cualquier corrección de un pago conciliado se hace en el core (RF-11-AC-3).
- Integración en línea con el core y notificaciones al socio; el SRS las excluye de la versión 1.
- Recuperación de contraseña por correo; no hay criterio que la defina.

### Paginación, versionado y filtrado

- Paginación por `page` y `pageSize` (máximo 100) con un objeto `meta` que trae el total. Para 20,000 créditos y un millón de pagos (RNF-08) basta con desplazamiento; si la bitácora creciera mucho cambiaría `audit-events` a paginación por cursor. La ruta del gestor no se pagina a propósito, porque la PWA necesita descargarla completa para trabajar sin conexión.
- Versionado en la URL (`/api/v1`). Es el esquema más fácil de ver en los registros del proxy y de probar con Schemathesis. Un cambio incompatible abriría `/api/v2` sin romper PWA ya instaladas en los celulares, que pueden tardar en actualizarse.
- Filtrado con parámetros de consulta sobre las colecciones (`status`, `date`, `collectorId`, `assigned`, `q`). Las listas que el SRS nombra, como "Pagos en revisión" o "Promesas incumplidas", son filtros de una colección y no endpoints nuevos.
- Exportaciones por negociación de contenido: `GET /reconciled-payments` y el reporte de filas rechazadas responden `text/csv` o JSON según el encabezado `Accept`.

### Dónde el agente rompió prácticas REST y cómo lo detecto

El primer borrador del `openapi.yaml` lo generó Grok Bot, el agente de IA con el que trabajo. El detalle de cada observación está en `revision_rest.md`; aquí explico cómo se detecta cada problema.

- Verbos en la URL (`/payments/{id}/void`, `/cash-closings/{id}/reconcile`) y rutas agrupadas por módulo interno (`/cash/...`, `/field/...`). Salta a la vista al comparar las rutas con los sustantivos del SRS: la URL expone la estructura de carpetas del backend.
- Falta de idempotencia. El borrador usa POST para anular y conciliar. Aparece al recorrer el flujo de RF-13-AC-3, donde el problema central es el reintento, y aplicar la misma pregunta al supervisor que reintenta una anulación.
- Códigos de estado. El borrador responde 200 al crear la importación y casi no declara 401, 403 ni 500. Se encuentra revisando operación por operación contra la lista de códigos que pide la actividad y buscando en el SRS cada mensaje "No tienes permiso".
- Trazabilidad débil. Los criterios están en `description` como texto libre, a veces con rangos o solo con el RF. Se nota en cuanto se quiere medir la cobertura con un script: no hay forma de extraer los IDs de manera confiable.
