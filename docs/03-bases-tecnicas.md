# Bases de la documentación técnica

**Versión 0.1 · 12 de noviembre de 2026 · Arquitectura y reglas propuestas para la beta**

## Arquitectura inicial

| Componente | Propuesta | Motivo y validación |
| --- | --- | --- |
| App de compradores y proveedores | React Native, Expo y TypeScript. Una app para Android e iOS con navegación según el rol. | Compartir el flujo y probar registro, fotos, enlaces y notificaciones en dispositivos de ambos sistemas desde la primera iteración. Usar builds de desarrollo. [Expo Development Builds](https://docs.expo.dev/develop/development-builds/introduction/) |
| API y operaciones | Node.js, Express y TypeScript. API REST modular y un panel web interno básico con acceso de administrador. | Mantener solicitudes, cotizaciones, acuerdos, reputación y moderación en un mismo backend. La API aplica permisos y transiciones para la app y el panel. |
| Base de datos, identidad y archivos | PostgreSQL y servicios administrados. Supabase es un candidato para base de datos, acceso por código de correo y almacenamiento privado de fotos. | Reducir operación inicial. Probar entrega del correo, verificación de tokens en la API, migraciones, respaldos y acceso privado a adjuntos antes de elegir proveedor. [Supabase Auth](https://supabase.com/docs/guides/auth) · [Supabase Storage](https://supabase.com/docs/guides/storage/security/access-control) |
| Avisos | Bandeja persistente en la app y push con Expo Notifications como opción inicial. | El aviso se registra en servidor. Un proceso en segundo plano envía y reintenta push sin duplicar el aviso. Validar credenciales, tokens y apertura del detalle en Android e iOS. [Configuración de push en Expo](https://docs.expo.dev/push-notifications/push-notifications-setup/) |

App y panel consultan una API autenticada. Los permisos se comprueban por recurso y actor en servidor. Si se adopta Supabase, los clientes usan Auth para iniciar sesión y la API controla el acceso al dominio y los enlaces temporales a archivos. Las credenciales privilegiadas viven en servidor. Cada ambiente tiene su propia configuración y sus propios datos. Se proponen entornos local, beta y producción.

## Modelo de datos y reglas

| Entidad | Datos y restricciones principales |
| --- | --- |
| Cuenta y perfil de proveedor | Identidad de acceso, rol, estado, nombre comercial, categoría, ciudad de cobertura y aprobación operativa. Los datos legales de identificación se guardan con acceso restringido según la política que se apruebe. |
| Solicitud | Comprador, categoría, ciudad, zona aproximada, especificaciones, cantidad y unidad, modalidad, fecha requerida, vencimiento, adjuntos y presupuesto privado opcional. |
| Cotización | Solicitud, proveedor, revisión, disponibilidad, modalidad, fecha prometida, precio del material, costo de entrega, total y vencimiento. Una cotización vigente por proveedor y solicitud. |
| Acuerdo | Una fila única por solicitud aceptada, comprador, proveedor y copia de las condiciones aceptadas. Fechas de aceptación y recepción, o motivo de cancelación. El acuerdo conserva su trazabilidad si cambia el perfil del comercio. |
| Reseña | Acuerdo, autor, destinatario, puntuación, comentario y estado de publicación. Restricción única por acuerdo y autor. |
| Aviso, reporte y bloqueo | Destinatario y evento del aviso, usuario o contenido reportado, estado y resolución, usuarios que bloquearon y registro de acciones de moderación. |

### Estados del flujo principal

| Entidad | Transiciones propuestas |
| --- | --- |
| Solicitud | Borrador → abierta → adjudicada, vencida o cancelada. Borrador y abierta son editables hasta la primera oferta. Con ofertas, un cambio de condiciones requiere cerrar y duplicar la solicitud. |
| Cotización | Vigente → aceptada, retirada, vencida o descartada. Solo el proveedor puede actualizarla o retirarla mientras esté vigente y la solicitud siga abierta. Adjudicar la solicitud descarta las demás ofertas. |
| Acuerdo | Acordado → completado cuando el comprador confirma recepción, o cancelado tras intervención de soporte. Se conserva el historial de la solicitud adjudicada aunque luego se cancele el acuerdo. |

- **Aceptar una oferta.** El comprador envía el identificador de la cotización y la revisión que leyó. La API comprueba titularidad, vigencias, estado, revisión y elegibilidad vigente del proveedor. En una transacción actualiza solicitud y cotizaciones, crea el acuerdo único y registra los eventos para avisar a ambas partes. Un reintento de la misma aceptación devuelve el acuerdo existente. Una oferta que cambió obliga a refrescar y confirmar de nuevo.
- **Precios y fechas.** Guardar precios del MVP en pesos colombianos enteros. El servidor calcula total como precio del material más entrega. Para retiro, la entrega cuesta cero. Usar UTC para instantes y zona `America/Bogota` para mostrarlos. La API valida vigencias al intentar publicar, cotizar o aceptar. Un proceso programado actualiza los estados vencidos.
- **Visibilidad.** El comprador ve sus ofertas. Cada proveedor ve las suyas y solicitudes abiertas de su cobertura y categoría, con ubicación aproximada. El contacto comercial del proveedor se muestra al comprador después de la aceptación, según los datos y reglas aprobados. La app usa selección manual de ciudad y zona.
- **Bloqueos y moderación.** Impedir nuevas interacciones entre las cuentas bloqueadas. En acuerdos previos, conservar acceso de las partes a su historial y derivar la coordinación necesaria a soporte, con controles sobre la exposición de datos de contacto. En operaciones, registrar actor, motivo y fecha al ocultar contenido, suspender o resolver un reporte.
- **Recepción y reseña.** El botón de recepción debe explicar su efecto sobre el acuerdo. Tras la confirmación, cada parte puede reseñar durante 14 días. Las reseñas se publican cuando ambas partes han enviado la suya o se cumple el plazo, con soporte para reportarlas. El promedio solo cuenta reseñas publicadas y visibles.

## Entrega móvil, privacidad y soporte

- **Distribución.** Definir titular de las cuentas de Apple Developer y Play Console, identificadores, firma y acceso del equipo. Probar en TestFlight y en el canal cerrado de Google Play. Si se usa una cuenta personal de Play Console creada después del 13 de noviembre de 2023, Google exige al menos 12 testers inscritos de forma continua durante 14 días antes de solicitar acceso a producción. Preparar fichas, declaraciones de privacidad y accesos de revisión para probar ambos roles. [TestFlight](https://developer.apple.com/testflight/) · [Pruebas de Google Play](https://support.google.com/googleplay/android-developer/answer/14151465) · [Guías de App Store](https://developer.apple.com/app-store/review/guidelines/)
- **Contenido de usuarios.** Diseñar filtros de publicación para textos y fotos, reglas de uso, reporte de contenido y de cuentas, bloqueo, contacto público de soporte y un proceso para atender reportes. En beta, probar con operaciones el recorrido desde el reporte hasta la resolución. [Apple, sección 1.2](https://developer.apple.com/app-store/review/guidelines/#user-generated-content) · [Política de Google Play](https://support.google.com/googleplay/android-developer/answer/18265346?hl=en)
- **Privacidad y eliminación.** Inventariar datos, finalidades, acceso y retención. Definir política de tratamiento y autorización aplicable, limitar el acceso a adjuntos y excluir contactos y fotos de eventos analíticos. Diseñar la solicitud de eliminación en la app y un recurso web para Google Play. Documentar el tratamiento de acuerdos abiertos, el borrado de datos asociados y las retenciones justificadas. [Políticas de tratamiento de la SIC](https://sedeelectronica.sic.gov.co/publicaciones/boletin-juridico/concepto/politicas-de-tratamiento-de-datos-personales) · [Guías de App Store, sección 5.1](https://developer.apple.com/app-store/review/guidelines/#privacy) · [Política de datos de Google Play](https://support.google.com/googleplay/android-developer/answer/10144311?hl=en-GB)
- **Rol de la plataforma en Colombia.** Solicitar revisión jurídica antes de publicar para determinar las obligaciones del flujo concreto, incluida la aplicabilidad del artículo 53 de la Ley 1480, qué información del proveedor debe recabarse y cómo se facilita a quien pueda solicitarla. Resolver con esa revisión los términos, la atención de reclamos y el efecto de aceptar una oferta en la app. [Estatuto del Consumidor](https://www1.funcionpublica.gov.co/eva/gestornormativo/norma.php?i=44306) · [Concepto de la SIC sobre portales de contacto](https://sedeelectronica.sic.gov.co/publicaciones/boletin-juridico/concepto/los-portales-de-contacto-no-hacen-parte-de-la-relacion-de-consumo-quien-responde-ante-los-consumidores)
- **Cobros futuros.** Diseñar por separado los pagos del material y cualquier compra de funciones digitales, como créditos o suscripciones. Cada flujo requiere revisar políticas de cobro de tiendas y el modelo de operación en Colombia antes de implementarse. [Apple, secciones 3.1.1 y 3.1.3(e)](https://developer.apple.com/app-store/review/guidelines/#payments) · [Política de pagos de Google Play](https://support.google.com/googleplay/android-developer/answer/9858738?hl=en)

## Qué documentar a continuación

| Orden | Documento | Decisiones que debe cerrar |
| --- | --- | --- |
| 1 | Historias de usuario y criterios de aceptación | Roles por cuenta, campos obligatorios para madera, vigencias por defecto, cambios con ofertas activas y pantalla para solicitudes sin respuesta. |
| 2 | Prototipo móvil y reglas de operación | Flujo de compra y cotización, alcance de la aprobación del comercio, qué contacto se comparte, cancelación, reportes y tiempos de atención. |
| 3 | Diagrama entidad-relación, estados y matriz de permisos | Identidad, restricciones, eventos, concurrencia al aceptar, historial y datos visibles por rol y etapa. |
| 4 | Contrato OpenAPI y decisiones de arquitectura | Recursos, autenticación, paginación, errores, reintentos, cargas de fotos y la elección de proveedores de identidad, archivos y push tras la prueba técnica. |
| 5 | Plan de pruebas, medición y publicación | Pruebas en ambos sistemas, telemetría de la fase 0, restauración de respaldos, responsables de soporte y preparación de tiendas. |

### Primera entrega técnica tras validar el piloto

Construir un recorrido completo con cuentas de prueba. Un comprador publica una solicitud, dos proveedores cotizan, el comprador compara y acepta, el proveedor elegido consulta el acuerdo y el comprador confirma recepción. Usar un dispositivo Android y uno iOS para comprobar los flujos de ambos roles y la navegación desde los avisos. Incorporar reputación, moderación, eliminación de cuenta y atención de incidencias antes de invitar a participantes externos a la beta.

La primera entrega se puede verificar con cinco casos: un proveedor ajeno a la categoría o ciudad queda fuera, los proveedores carecen de acceso a ofertas ajenas, una versión vencida o reemplazada no se puede aceptar, dos aceptaciones concurrentes producen un solo acuerdo y el comprador completa el recorrido con las notificaciones desactivadas. Registrar eventos de publicación, oferta válida, aceptación, recepción y cancelación para medir la operación con la misma definición del piloto.
