# Manejo de solicitudes, rutas y procesos

**Borrador técnico v0.3 · 13 de noviembre de 2026**

## Admisión a la beta y aprobación comercial

1. Quien tenga acceso a la app de prueba puede iniciar el registro y verificar su correo por OTP. La API crea la cuenta en `pending` si es su primer ingreso y conserva su estado en ingresos posteriores. El usuario puede consultar ese estado. La API restringe solicitudes, cotizaciones y acuerdos a cuentas admitidas, según las reglas de propiedad y acceso. La instalación de la app se distribuye por los canales de prueba de las tiendas.
2. Operaciones aprueba o rechaza el ingreso. La aprobación habilita la función de comprador. Propuesta de estados de cuenta: `pending`, `admitted`, `rejected`, `suspended`. Definir cómo se reconsidera una decisión y cómo se comunica al usuario.
3. Una cuenta admitida puede pedir activar su perfil comercial. Completa nombre, contacto, rubros y cobertura. Operaciones revisa por separado ese perfil. Solo un comercio aprobado y habilitado ve oportunidades y cotiza. La cuenta conserva el modo comprador mientras se revisa el comercio.

## Flujo comercial y responsabilidad de la API

| Paso | Regla de ejecución | Estado visible |
| --- | --- | --- |
| Publicar | Recibir tipo, rubro, zona, descripción y fecha requerida. Validar catálogos activos, campos opcionales y vigencia. Encontrar comercios aprobados por rubro y cobertura. Guardar el evento de publicación en la misma transacción. | Borrador, abierta, sin cotizaciones o con cotizaciones. Una solicitud puede abrirse aunque aún falte un comercio que pueda atenderla. |
| Cotizar | Validar que siga abierta, que el comercio cubra rubro y zona y que las cuentas puedan interactuar. Calcular el total en servidor y conservar la revisión anterior. Una aclaración asistida se refleja en una actualización autorizada de los términos. | Oferta activa, retirada, vencida, aceptada o cerrada. |
| Aceptar | Verificar propiedad, vigencia y revisión. Adjudicar, crear el acuerdo y guardar el evento en una sola transacción. Un intento concurrente sobre otra oferta devuelve conflicto. | Acuerdo abierto con condiciones y contacto comercial según permisos. |
| Cumplir | El comprador confirma entrega, retiro o servicio. Las partes coordinan y pagan directamente al comercio. El estado completado habilita las reseñas. | Completado con fecha de confirmación. |
| Cancelar | Antes de adjudicar, cancela el titular. Después de aceptar, el comprador abre un caso de soporte y operaciones decide. La pausa de confirmación durante el caso sigue como propuesta. | Solicitud cancelada o acuerdo con caso pendiente y luego resuelto. |
| Vencer | La API compara con el reloj de servidor en cada escritura. El trabajador materializa vencimientos y actualiza las bandejas. | Solicitud u oferta vencida, con opción de duplicar solicitud cuando corresponda. |

Los permisos por actor, bloqueos y orden de transacciones están en el [modelo de dominio](06-dominio-y-permisos.md). El backend puede emitir avisos al publicar o cotizar y los vuelve a filtrar al entregar y al abrir. La app consulta sus bandejas incluso sin permiso de notificaciones. Los reintentos usan la misma clave de idempotencia. Si el servidor confirma un resultado comercial, la app refresca la vista con ese resultado.

## Mapa de rutas móviles

| Área | Rutas propuestas | Acceso |
| --- | --- | --- |
| Sesión y admisión | `/sign-in`, `/verify-code`, `/admission` | Visitante o sesión pendiente, según pantalla. |
| Comprador | `/requests`, `/requests/new`, `/requests/:id`, `/requests/:id/offers` | Cuenta admitida. Detalle y ofertas limitados al titular. |
| Comercio | `/business/profile`, `/business/opportunities`, `/business/opportunities/:id`, `/business/offers` | Perfil accesible a cuenta admitida. Oportunidades y cotización requieren comercio aprobado y elegible. |
| Ambas partes | `/agreements`, `/agreements/:id`, `/notifications`, `/account`, `/support` | Cuenta admitida y permiso sobre cada recurso. |

Estas son rutas de navegación de Expo Router. Las rutas internas del panel cubren admisiones, comercios, casos y moderación. Los enlaces de push llevan a un detalle autorizado y muestran un estado recuperable si la sesión venció o el recurso dejó de estar disponible.

## Inventario de rutas API por entrega

El [OpenAPI inicial](api-movil.openapi.json) conserva las 11 operaciones ya definidas. La siguiente tabla es una propuesta de ampliación de `/v1`. Cada ruta nueva se agrega al contrato y se prueba antes de conectarla con una pantalla.

| Entrega | Rutas y operaciones |
| --- | --- |
| Primer recorrido | Las 11 operaciones actuales de solicitudes, oportunidades, cotizaciones y acuerdos. Cuentas y catálogos precargados para la prueba técnica. |
| Acceso y operación | `POST /account/registration`, `GET /me`, `GET /catalog/categories`, `GET /catalog/zones`, `PUT /me/business-profile`, `POST /me/business-profile/submit`. Rutas internas para listar y resolver admisiones y aprobar o suspender comercios. OTP se verifica con el proveedor de identidad. |
| Completar el flujo | `GET` y `PATCH /requests/{requestId}`, `POST /requests/{requestId}/cancel`, `POST /requests/{requestId}/my-offer/withdraw`, `GET /me/offers`, `GET /agreements`, `GET /me/support-cases`. Rutas internas para resolver cancelaciones. Duplicar una solicitud crea un nuevo borrador. |
| Confianza y avisos | Alta y confirmación de archivos privados, bandeja y registro de dispositivo push, reseñas, reportes, bloqueos, soporte y solicitudes de eliminación de cuenta. |

Las rutas nuevas usan recursos en inglés como el contrato inicial, respuestas por actor, cursor para listados y errores con identificador de rastreo. Una sesión válida sin admisión puede usar registro y consulta de estado. La API de negocio responde con `ACCOUNT_NOT_ADMITTED` si la cuenta carece de admisión. Ver [convenciones de API](07-api-movil.md).
