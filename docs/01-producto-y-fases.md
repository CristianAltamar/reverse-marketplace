# Definición de producto y plan por fases

**Versión 0.1 · 12 de noviembre de 2026 · Propuesta para validar**

## Qué es la app

Reverse Marketplace es un mercado inverso de cotizaciones para productos y servicios. El comprador publica qué necesita y los proveedores que pueden atender la solicitud envían sus condiciones. El comprador compara y elige. La primera experiencia debe mostrar cuánto cuesta en total, cuándo se puede recibir el producto o realizar el servicio y quién lo ofrece.

La hipótesis es que los compradores pueden ahorrar tiempo al publicar su necesidad una vez y comparar respuestas de varios comercios. El piloto cubrirá **todo Galapa** y comenzará con un grupo reducido de compradores y comercios invitados de distintos rubros. Cada proveedor atiende según lo que ofrece y las zonas a las que puede llegar. El piloto debe mostrar qué solicitudes aparecen, cuáles se pueden atender con el grupo inicial y dónde faltan proveedores.

| Participante | Qué puede hacer |
| --- | --- |
| Comprador | Publicar una solicitud, comparar y aceptar una oferta, confirmar la recepción del producto o la prestación del servicio y calificar. |
| Proveedor | Indicar qué ofrece y dónde atiende, ver solicitudes elegibles, cotizar, entregar el pedido o prestar el servicio y calificar. |
| Operaciones | Incorporar proveedores, atender reportes, moderar publicaciones, resolver incidencias y medir el resultado del mercado. |

## Cómo funciona el MVP

1. **Publicar.** El comprador indica qué necesita, la fecha y la zona aproximada. Puede detallar cantidad, especificaciones y modalidad de entrega, retiro o servicio según corresponda. También puede adjuntar fotos y agregar un presupuesto opcional, visible solo para él durante el piloto y el MVP propuesto.
2. **Cotizar.** Los proveedores aprobados que atienden el rubro y la zona reciben la oportunidad en su bandeja. Cada oferta indica qué cubre, disponibilidad, fecha prometida, precio con impuestos aplicables, cargos adicionales cuando correspondan, total y vigencia. Las ofertas son privadas entre su proveedor y el comprador. Cada proveedor puede actualizar su propia oferta mientras esté abierta la solicitud.
3. **Comparar y elegir.** El comprador compara total, plazo, condiciones y reputación. Si recibe una sola oferta, también puede aceptarla. La app muestra las condiciones antes de aceptar y conserva la versión elegida. Una solicitud admite un único acuerdo. Si vence o se cancela antes de aceptar, puede duplicarse como una solicitud nueva.
4. **Coordinar y confirmar.** Al aceptar, el comprador recibe el contacto comercial del proveedor elegido. Las partes coordinan directamente el pago y la entrega, el retiro o la prestación del servicio. La app registra por separado la aceptación y la confirmación de cumplimiento. Si hay una incidencia o cancelación después de aceptar, soporte registra el resultado y el motivo.
5. **Calificar.** Tras la confirmación de cumplimiento, cada parte puede dejar una valoración de 1 a 5 y un comentario opcional. Se admite una reseña por persona y acuerdo. Las reseñas se publican al recibir ambas o al vencer el plazo de 14 días propuesto. Los perfiles nuevos indican «Sin calificaciones».

Por ejemplo, para una torta el comprador puede indicar porciones, fecha y modalidad de entrega. Para reparar un electrodoméstico puede describir la falla, adjuntar una foto y comparar qué incluye cada servicio y cuándo puede atenderlo el proveedor.

### Pantallas y operación imprescindibles

- **Comprador:** crear solicitud, ver solicitudes propias, comparar cotizaciones, revisar el acuerdo y confirmar cumplimiento.
- **Proveedor:** completar perfil comercial, consultar oportunidades, enviar o actualizar cotización y consultar acuerdos.
- **Ambos:** registro, perfil y reputación, bandeja de avisos, notificaciones push de eventos relevantes, reportar contenido o usuarios, bloquear usuarios, contactar a soporte y solicitar eliminación de cuenta. El flujo principal funciona aunque la persona rechace las notificaciones push.
- **Operaciones:** un panel web interno para aprobar proveedores, atender reportes, ocultar contenido, suspender cuentas, resolver incidencias con historial y consultar métricas básicas.

## Alcance por fases

| Fase | Entrega | Criterio propuesto para avanzar |
| --- | --- | --- |
| **0. Validación** | Piloto asistido con formulario y WhatsApp en todo Galapa, con pocos compradores y comercios invitados de distintos rubros. Prototipo navegable del flujo móvil. | Las solicitudes reales permiten identificar qué rubros reciben ofertas comparables, dónde faltan comercios y qué compras o servicios se concretan. El equipo usa esa evidencia para decidir si desarrolla la beta o repite la prueba con ajustes. |
| **1. MVP móvil y beta cerrada** | Una app Android/iOS con modos de comprador y proveedor. Flujo completo de publicación a reseña, panel interno y métricas. Distribución a participantes por Google Play y TestFlight. | Usuarios de ambos roles completan el recorrido en dispositivos reales. Soporte puede tramitar reportes y cancelaciones. Los errores que impiden comprar, exponen datos o duplican acuerdos están resueltos. Se revisan las métricas del piloto en la app. |
| **2. Lanzamiento público local** | App Store y Google Play, incorporación progresiva de proveedores, atención al usuario y seguimiento semanal del mercado. La cobertura de lanzamiento se define con los resultados del piloto y la beta. | La cobertura de cotizaciones y los cierres alcanzan los objetivos acordados en la beta durante al menos cuatro semanas. Operaciones puede atender el volumen y medir compras repetidas. |
| **3. Expansión y monetización** | Ampliar una categoría o ciudad a la vez. Priorizar chat, contraofertas, solicitudes con varios renglones o logística según la fricción observada. Evaluar suscripciones, créditos o comisión. | Cada expansión cuenta con compradores y proveedores disponibles. Cualquier cobro requiere evidencia de disposición a pagar y revisión de operación, obligaciones y políticas de tiendas. |

Durante el piloto y el MVP, compradores y proveedores participan gratis. El pago, la facturación y la entrega, retiro o prestación se acuerdan directamente con el comercio. La incorporación de cobros dentro de la app depende de definir quién recauda, concilia, devuelve y responde por incidencias. Se propone evaluar una suscripción después de observar compras repetidas y una comisión si se puede atribuir el cierre y administrar el pago.

## Cómo empezar

| Momento propuesto | Trabajo | Resultado esperado |
| --- | --- | --- |
| Preparación | Nombrar a quien operará el piloto. Invitar a un grupo reducido de compradores y comercios. Registrar qué ofrece cada comercio y qué zonas de Galapa atiende. | Lista de participantes, formulario de solicitud y plantilla de cotización. |
| Prueba | Recibir solicitudes reales y compartir cada una con los comercios elegibles. Devolver ofertas con el mismo formato y hacer seguimiento al cierre. Probar el prototipo en paralelo con compradores y proveedores. | Registro de solicitudes, ofertas, cierres y motivos de pérdida, incluidos los casos sin un comercio que pueda atender. |
| Después del piloto | Revisar resultados cuando hayan vencido las ventanas de medición. Decidir si se construye la beta o se repite la prueba con ajustes en la operación, el formulario o la oferta de comercios. | Decisión documentada y alcance técnico para la beta. |

Antes de recibir solicitudes, explicar a los participantes cómo se usarán sus datos y qué información se compartirá. Para pedir ofertas, enviar las especificaciones y la zona aproximada. La dirección exacta la coordina el comprador directamente con el comercio elegido cuando corresponda.

Registrar por solicitud el rubro, la zona, la hora de publicación, los comercios avisados, la hora de cada oferta válida, el total, el plazo, la oferta elegida y el resultado. Una **oferta válida** cubre lo solicitado y especifica qué incluye, total, modalidad, plazo y vigencia. Reportar los conteos sobre el total de solicitudes reales. Con pocos participantes, revisar los motivos de cierre o pérdida por rubro y zona junto con los conteos generales.

| Indicador | Cómo se mide |
| --- | --- |
| Cobertura en 24 horas | Solicitudes con al menos una oferta válida dentro de 24 horas sobre las solicitudes reales publicadas. |
| Capacidad de comparar | Solicitudes con dos o más ofertas válidas de distintos proveedores dentro de 24 horas sobre las solicitudes reales publicadas. |
| Cierre confirmado | Solicitudes cuyo cumplimiento confirma el comprador dentro de 14 días de publicar sobre las solicitudes reales publicadas. |
| Tiempo y repetición | Mediana hasta la primera oferta entre solicitudes con respuesta y número de compradores que vuelven a publicar durante el período. |

## Decisiones de producto para cerrar antes de construir

- Cerrar la lista de invitados, los rubros que atienden los comercios y las fechas de la prueba. Registrar la cobertura de cada comercio dentro de Galapa.
- Confirmar que el formulario común, una oferta ganadora por solicitud y la coordinación directa cubren los casos del piloto. Registrar cuándo la falta de información, la necesidad de dividir un pedido o la negociación impiden cerrar.
- Designar responsables de reclutamiento, moderación y atención de acuerdos con problemas. Acordar qué información del comercio ve el comprador en cada paso y qué evidencia pide soporte.
- Probar el acceso por correo en teléfonos de los participantes y registrar cuántos usan Android y cuántos iOS para planificar la beta y el orden de publicación.

Continúa en [investigación de mercado](02-investigacion.md), [bases técnicas](03-bases-tecnicas.md) y [resumen para el equipo](04-resumen-piloto-galapa.md).
