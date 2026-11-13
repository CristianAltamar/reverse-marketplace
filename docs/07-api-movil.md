# Contrato inicial de la API móvil

**Borrador técnico v0.2 · Contrato para integrar el primer recorrido con cuentas de prueba**

El [contrato OpenAPI](api-movil.openapi.json) define la API desde el borrador de una solicitud hasta la confirmación o solicitud de cancelación del acuerdo. Usa OpenAPI 3.1.1 como formato. La API se versiona bajo `/v1` y el host se definirá para cada ambiente. [Especificación OpenAPI 3.1.1](https://spec.openapis.org/oas/v3.1.1.html)

## Operaciones de la primera integración

| Método y ruta | Quién la usa | Resultado |
| --- | --- | --- |
| `POST /requests` | Comprador | Guarda un borrador con los campos base. |
| `GET /requests` | Comprador | Lista sus solicitudes con estado y datos privados propios. |
| `POST /requests/{requestId}/publish` | Comprador titular | Publica el borrador y fija su vencimiento. |
| `GET /opportunities` | Comercio aprobado | Lista las solicitudes abiertas de su rubro y zona, con una vista que omite el presupuesto y datos privados del comprador. |
| `GET /requests/{requestId}/my-offer` | Comercio titular de la cotización | Recupera su última revisión y estado, incluso después de cerrar la solicitud. |
| `PUT /requests/{requestId}/my-offer` | Comercio elegible | Crea o actualiza su cotización con control de revisión. |
| `GET /requests/{requestId}/offers` | Comprador titular | Devuelve la revisión más reciente por comercio y su estado. |
| `POST /requests/{requestId}/acceptance` | Comprador titular | Acepta una revisión exacta y devuelve el acuerdo. |
| `GET /agreements/{agreementId}` | Parte del acuerdo | Devuelve condiciones y estado. El contacto comercial aparece solo en la vista autorizada del comprador. |
| `POST /agreements/{agreementId}/complete` | Comprador titular | Confirma entrega, retiro o servicio realizado. |
| `POST /agreements/{agreementId}/cancellation-requests` | Comprador titular | Crea un caso para que soporte revise la cancelación. |

## Convenciones que comparten la app y el backend

- **Identidad.** La app verifica el código de correo con el proveedor de identidad elegido y envía `Authorization: Bearer <access_token>` a la API. La API valida el token y carga el estado, la admisión y los permisos de la cuenta. Las rutas de identidad y el manejo de correo se documentarán al elegir el proveedor. Las primeras pruebas pueden usar cuentas y catálogos de rubros y zonas precargados en beta.
- **Borrador.** La app puede conservar localmente un formulario incompleto. Para guardar el borrador del contrato se requiere tipo, rubro, zona, descripción y fecha requerida. Para clasificar una zona como `OTHER`, el comprador indica una referencia aproximada en `zoneHint`. La app y el equipo de soporte deben orientar a evitar direcciones exactas y datos personales en ese campo.
- **Montos y tiempo.** `priceCop` incluye los impuestos aplicables. `extraChargesCop` contiene la suma de cargos adicionales declarados por separado, con valor cero cuando corresponda. La API calcula `totalCop`. Los instantes se intercambian en formato RFC 3339 y la app los muestra en hora de Colombia. La vigencia se valida con la hora del servidor.
- **Listas.** `GET /requests` y `GET /opportunities` usan `limit` y cursor opaco. El cursor pertenece a la cuenta y la consulta que lo emitieron. La app sigue `nextCursor` mientras exista. La API vuelve a comprobar el acceso de cada comercio en cada lectura.
- **Escrituras y reintentos.** Cada operación de escritura requiere `Idempotency-Key`. La app conserva la clave al reintentar la misma acción tras un corte de red. La API registra la clave por cuenta y operación, junto con la ruta resuelta y un resumen del cuerpo. Si se reutiliza con otra solicitud o cuerpo, devuelve `409 IDEMPOTENCY_KEY_REUSED`. La respuesta se vuelve a proyectar con los permisos vigentes. El tiempo de retención de las claves queda por fijar antes de beta.
- **Revisiones.** Al enviar una oferta, `expectedRevision: 0` indica creación y un valor positivo identifica la versión que se va a reemplazar. Al aceptar, `offerId` y `revision` identifican los términos que el comprador vio. La app refresca y vuelve a pedir confirmación ante `OFFER_REVISION_CHANGED`. Una repetición de la misma aceptación devuelve el acuerdo existente incluso si llega con otra clave. El servidor registra la operación y sus eventos una sola vez.
- **Cancelación.** Crear un caso devuelve `status: open` y mantiene el acuerdo en `agreed`. La ficha indica si existe un caso abierto. Una nueva solicitud de cancelación mientras ya hay un caso abierto devuelve `409 CANCELLATION_PENDING`. Resolver el caso corresponde al flujo interno de soporte. La app muestra el estado del acuerdo después de la resolución.

## Respuestas de error que debe manejar la app

| HTTP | Ejemplo de código | Acción esperada |
| --- | --- | --- |
| `400` | `INVALID_REQUEST` | Corregir el formato de la petición. |
| `401` | `AUTH_REQUIRED` | Renovar sesión o volver a ingresar el código. |
| `403` | `ACCOUNT_NOT_ADMITTED`, `PROVIDER_NOT_APPROVED` | Mostrar el estado de acceso o de aprobación. |
| `404` | `RESOURCE_NOT_FOUND` | Mostrar que el recurso no está disponible para esa cuenta. |
| `409` | `OFFER_REVISION_CHANGED`, `OFFER_EXPIRED`, `REQUEST_EXPIRED`, `REQUEST_NOT_OPEN`, `AGREEMENT_NOT_ACTIVE`, `CANCELLATION_PENDING`, `IDEMPOTENCY_KEY_REUSED` | Refrescar el recurso y explicar el motivo específico. |
| `422` | `VALIDATION_FAILED` | Marcar los campos devueltos por la API. |
| `429` | `RATE_LIMITED` | Esperar el plazo indicado en `Retry-After` cuando se reciba. |

Las respuestas incluyen `code`, `message` y `requestId` para rastrear un caso de soporte. Pueden incluir errores por campo. El cliente toma decisiones a partir del código y usa textos adecuados para cada pantalla.

## Siguiente ampliación del contrato

Agregar registro abierto con admisión, perfil y aprobación comercial, catálogos, edición y cancelación de una solicitud abierta, retiro de una oferta, resolución de casos y consulta del historial de soporte. Después, añadir adjuntos, avisos, reseñas, reportes, bloqueo y eliminación de cuenta. Consultar el [inventario de rutas por entrega](10-solicitudes-rutas-y-procesos.md) y el [modelo de permisos y estados](06-dominio-y-permisos.md).
