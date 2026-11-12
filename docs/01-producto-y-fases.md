# Definición de producto y plan por fases

**Versión 0.1 · 12 de noviembre de 2026 · Propuesta para validar**

## Qué es la app

Reverse Marketplace es un mercado inverso de cotizaciones. El comprador publica qué necesita y los proveedores que pueden atender la solicitud envían sus condiciones. El comprador compara y elige. La primera experiencia debe responder tres preguntas en el teléfono: cuánto cuesta en total, cuándo puedo recibirlo o recogerlo y quién lo suministra.

La hipótesis inicial es que pequeños contratistas y talleres de Barranquilla pierden tiempo consultando a varios comercios cuando necesitan confirmar medidas, existencias y costo de entrega. La primera categoría propuesta es **madera dimensionada**, con madereras y ferreterías locales como proveedores. El piloto debe comprobar la frecuencia del problema y si hay suficiente oferta en la ciudad. La visión admite otras categorías y ciudades cuando se pueda repetir el resultado.

| Participante | Qué puede hacer |
| --- | --- |
| Comprador | Publicar una solicitud, revisar y comparar ofertas, aceptar una, confirmar que recibió el material y calificar. |
| Proveedor | Configurar su categoría y cobertura, ver solicitudes elegibles, cotizar, preparar el pedido y calificar el cumplimiento del comprador. |
| Operaciones | Incorporar proveedores, atender reportes, moderar publicaciones, resolver incidencias y medir el resultado del mercado. |

## Cómo funciona el MVP

1. **Publicar.** El comprador indica material, dimensiones, cantidad, unidad, fecha en que lo necesita, ciudad, zona aproximada y modalidad de entrega o retiro. Puede adjuntar fotos y agregar un presupuesto opcional, visible solo para él durante el piloto y el MVP propuesto.
2. **Cotizar.** Los proveedores aprobados de la categoría y ciudad reciben la oportunidad en su bandeja. Cada uno presenta una cotización por la cantidad completa, con disponibilidad, fecha prometida, precio del material con impuestos aplicables, costo de entrega, total y vigencia. Las ofertas son privadas entre su proveedor y el comprador. Cada proveedor puede actualizar su propia oferta mientras esté abierta la solicitud.
3. **Comparar y elegir.** El comprador compara total, fecha, modalidad y reputación. Si recibe una sola oferta, también puede aceptarla. La app muestra las condiciones antes de aceptar y conserva la versión elegida. Una solicitud admite un único acuerdo. Si vence o se cancela antes de aceptar, puede duplicarse como una solicitud nueva.
4. **Coordinar y confirmar.** Al aceptar, el comprador recibe el contacto comercial del proveedor seleccionado. Las partes coordinan el pago directamente y el proveedor entrega el material, o el comprador lo retira. La app registra la aceptación y la confirmación de recepción del comprador. Si hay una incidencia o cancelación posterior a la aceptación, soporte registra el resultado y el motivo. La aceptación y la recepción se presentan como acciones distintas.
5. **Calificar.** Tras la recepción confirmada, cada parte puede dejar una valoración de 1 a 5 y un comentario opcional. Se admite una reseña por persona y acuerdo. Las reseñas se publican al recibir ambas o al vencer el plazo de 14 días propuesto. Los perfiles nuevos indican «Sin calificaciones».

Ejemplo ilustrativo. Una persona pide 10 listones de pino de 2 m y especifica su sección. Una maderera cotiza COP 115.000 de material y COP 20.000 de envío para mañana. Otra cotiza COP 125.000 de material y COP 5.000 de envío para pasado mañana. La app compara COP 135.000 frente a COP 130.000 y deja la decisión en manos del comprador.

### Pantallas y operación imprescindibles

- **Comprador:** crear solicitud, ver solicitudes propias, comparar cotizaciones, revisar el acuerdo y confirmar recepción.
- **Proveedor:** completar perfil comercial, consultar oportunidades, enviar o actualizar cotización y consultar acuerdos.
- **Ambos:** registro, perfil y reputación, bandeja de avisos, notificaciones push de eventos relevantes, reportar contenido o usuarios, bloquear usuarios, contactar a soporte y solicitar eliminación de cuenta. El flujo principal funciona aunque la persona rechace las notificaciones push.
- **Operaciones:** un panel web interno para aprobar proveedores, atender reportes, ocultar contenido, suspender cuentas, resolver incidencias con historial y consultar métricas básicas.

## Alcance por fases

| Fase | Entrega | Criterio propuesto para avanzar |
| --- | --- | --- |
| **0. Validación** | Entrevistas, piloto asistido con formulario y WhatsApp, y prototipo navegable del flujo móvil. Una ciudad y una categoría. | Hay compras reales, respuesta de varios proveedores y capacidad para cerrar operaciones. Se alcanzan los objetivos del piloto o se ajusta la hipótesis y se repite. |
| **1. MVP móvil y beta cerrada** | Una app Android/iOS con modos de comprador y proveedor. Flujo completo de publicación a reseña, panel interno y métricas. Distribución a participantes por Google Play y TestFlight. | Usuarios de ambos roles completan el recorrido en dispositivos reales. Soporte puede tramitar reportes y cancelaciones. Los errores que impiden comprar, exponen datos o duplican acuerdos están resueltos. Se revisan las métricas del piloto en la app. |
| **2. Lanzamiento público local** | App Store y Google Play, incorporación progresiva de proveedores, atención al usuario y seguimiento semanal del mercado. La cobertura inicial se acota a Barranquilla. | La cobertura de cotizaciones y los cierres alcanzan los objetivos acordados en la beta durante al menos cuatro semanas. Operaciones puede atender el volumen y medir compras repetidas. |
| **3. Expansión y monetización** | Ampliar una categoría o ciudad a la vez. Priorizar chat, contraofertas, solicitudes con varios renglones o logística según la fricción observada. Evaluar suscripciones, créditos o comisión. | Cada expansión cuenta con compradores y proveedores disponibles. Cualquier cobro requiere evidencia de disposición a pagar y revisión de operación, obligaciones y políticas de tiendas. |

Durante el piloto y el MVP, compradores y proveedores participan gratis. El pago, la facturación del material y la entrega se acuerdan directamente con el comercio. La incorporación de cobros dentro de la app depende de definir quién recauda, concilia, devuelve y responde por incidencias. Se propone evaluar una suscripción después de observar compras repetidas y una comisión si se puede atribuir el cierre y administrar el pago.

## Cómo empezar

| Momento propuesto | Trabajo | Resultado esperado |
| --- | --- | --- |
| Días 1 a 3 | Nombrar a quien operará el piloto. Entrevistar a cinco compradores y cinco proveedores sobre su última compra o cotización real. Pedir ejemplos de especificaciones, canales, tiempos y compras que no se cerraron. | Formulario de solicitud, plantilla de cotización y lista inicial de participantes. |
| Días 4 a 15 hábiles | Reclutar entre 8 y 12 comercios. Recibir 20 solicitudes reales y compartir cada una con varios proveedores elegibles. Devolver ofertas con el mismo formato y hacer seguimiento al cierre. Probar el prototipo en paralelo con compradores y proveedores. | Registro de solicitudes, ofertas, motivos de pérdida y confirmaciones de compra. |
| Después del piloto | Revisar resultados cuando hayan vencido las ventanas de medición. Elegir entre construir el MVP, ajustar la categoría o la cobertura y repetir el piloto, o probar otra hipótesis. | Decisión documentada y alcance técnico para la beta. |

Antes de recibir solicitudes, explicar a los participantes cómo se usarán sus datos y qué información se compartirá. Para pedir ofertas, enviar las especificaciones y la zona aproximada. La dirección exacta la coordina el comprador directamente con el comercio elegido cuando corresponda.

Registrar por solicitud la hora de publicación, proveedores avisados, hora de cada oferta válida, total con entrega, fecha prometida, oferta elegida, recepción confirmada y motivo de cierre sin compra. Una **oferta válida** cubre la cantidad solicitada y especifica el total, la modalidad, la fecha y la vigencia. Los objetivos son hipótesis del proyecto y deben recalibrarse con lo observado.

| Indicador | Cómo se mide | Umbral inicial propuesto |
| --- | --- | --- |
| Cobertura en 24 horas | Solicitudes con al menos una oferta válida dentro de 24 horas / solicitudes reales publicadas. | 12 de 20 (60 %). |
| Capacidad de comparar | Solicitudes con dos o más ofertas válidas de distintos proveedores dentro de 24 horas / solicitudes reales publicadas. | 8 de 20 (40 %). |
| Cierre confirmado | Solicitudes cuya recepción el comprador confirma dentro de 14 días de publicar / solicitudes reales publicadas. | 5 de 20 (25 %). |
| Tiempo y repetición | Mediana hasta primera oferta entre solicitudes con respuesta. Registrar también la tasa de solicitudes sin respuesta y cuántos compradores vuelven a publicar durante el período. | Recoger línea base en el piloto. |

## Decisiones de producto para cerrar antes de construir

- Confirmar categoría, ciudad y comprador inicial con evidencia de compras reales.
- Confirmar que una solicitud de un solo material, un proveedor ganador y coordinación directa cubren la mayoría de casos del piloto. Registrar cuántas compras requieren varios materiales o negociación antes de elegir.
- Designar responsables de reclutamiento, moderación y atención de acuerdos con problemas. Acordar qué información del comercio ve el comprador en cada paso y qué evidencia pide soporte.
- Probar el acceso por correo en teléfonos de los participantes y registrar cuántos usan Android y cuántos iOS para planificar la beta y el orden de publicación.

Continúa en [investigación de mercado](02-investigacion.md) y [bases técnicas](03-bases-tecnicas.md).
