# Rendimiento, optimización y operación

**Borrador técnico v0.3 · 13 de noviembre de 2026**

## Objetivos iniciales para beta

Estos valores son presupuestos de ingeniería propuestos. La primera medición con la infraestructura elegida sirve para confirmarlos o ajustarlos. Las métricas del piloto comercial, como recibir una oferta dentro de 24 horas, se llevan por separado.

| Indicador | Objetivo inicial | Cómo comprobarlo |
| --- | --- | --- |
| Lecturas de solicitudes, oportunidades y ofertas | p95 ≤ 400 ms. | Tiempo HTTP desde un generador externo en una región cercana a la API. Registrar aparte el tiempo que percibe el dispositivo desde Galapa. |
| Escrituras de publicación, cotización y aceptación | p95 ≤ 700 ms. | Carga mixta con base y trabajador activos. Las operaciones de correo, push y carga de fotos se miden por separado. |
| Errores de servidor | Menos del 1 % en el escenario nominal. | Contar HTTP 5xx y fallas de transporte del generador. Medir por separado los 409 esperados en escenarios de concurrencia. |
| Apertura de la bandeja en móvil | p95 ≤ 3 s en arranque con sesión existente. | Desde tocar el icono hasta poder usar la primera lista en compilación de distribución. Medir Android representativo e iPhone con red móvil controlada, y reportar dispositivo y red. |
| Avisos de adjudicación | p95 ≤ 30 s desde confirmar el acuerdo hasta aparecer en la bandeja de servidor. | Medir atraso del trabajador y registrar aparte el envío y recibo del proveedor push. La entrega del aviso al teléfono depende de la red y la plataforma. |
| Recuperación de base | Propuesta inicial de RPO ≤ 24 h y RTO ≤ 4 h. | Restaurar un respaldo en un entorno aislado, comprobar acuerdos y archivos asociados y medir el tiempo. Confirmar objetivos con el plan contratado. |

Escenario de referencia para reproducir. Sembrar datos sintéticos de varios rubros y zonas con 1.000 solicitudes, 5.000 revisiones de ofertas y cuentas con coberturas distintas. Reservar solicitudes y ofertas válidas para las escrituras del escenario. Ejecutar 15 minutos a 5 peticiones por segundo con una mezcla aproximada de 80 % lecturas y 20 % escrituras, sin incluir verificación real de correo ni envío masivo de push. Reportar p95 y tamaño de muestra por operación. Correr además un escenario específico de aceptación concurrente, otro de reintentos tras cortes y otro de recuperación del trabajador con eventos pendientes. Documentar región, versión, recursos asignados y si hubo arranque en frío. La carga se ejecuta sobre entornos del proyecto autorizados para pruebas. [Umbrales de k6](https://grafana.com/docs/k6/latest/using-k6/thresholds/)

## Optimización por componente

| Componente | Qué hacer desde el inicio | Cuándo ampliar |
| --- | --- | --- |
| API | Paginación por cursor estable con desempate por ID, proyecciones pequeñas y presupuestos de tiempo por operación. Mantener las transacciones cortas y el trabajo externo en el trabajador. | Revisar trazas y perfiles antes de cambiar código o subir capacidad. Escalar si CPU, latencia o saturación de conexiones se sostienen después de optimizar consultas. |
| PostgreSQL | Probar índices para bandeja del comprador, filtro de oportunidades por rubro y zona, cotizaciones por solicitud, casos por acuerdo y eventos pendientes. Medir con datos representativos y revisar planes. Presupuestar conexiones entre API, trabajador y migraciones. | Añadir índices según consultas medidas. Considerar réplica o caché distribuida cuando exista un cuello de botella identificado y se pueda preservar la consistencia. [PostgreSQL EXPLAIN](https://www.postgresql.org/docs/current/using-explain.html) |
| App móvil | Virtualizar listas largas, paginar, usar miniaturas, comprimir fotos antes de subir y dar estados de carga y recuperación. Refrescar consultas al volver a primer plano. Conservar borradores ante cortes y confirmar cambios comerciales con el servidor. | Medir navegación, tiempos de render y memoria en una build de distribución. Cambiar componentes o estrategias de caché cuando la medición muestre el cuello de botella. [Rendimiento de React Native](https://reactnative.dev/docs/performance) |
| Panel web | Cargar pantallas internas por ruta, paginar y evitar descargar expedientes completos para mostrar listados. | Medir tamaño del bundle y tiempos de interacción cuando haya pantallas y datos reales de beta. |
| Trabajador | Tomar lotes acotados, registrar intentos y próximo reintento, deduplicar por evento y destinatario, recuperar trabajo abandonado y revisar recibos de push. | Aislar colas o introducir un servicio de mensajería dedicado si el retraso y el volumen lo justifican. [Envío y recibos de Expo Push](https://docs.expo.dev/push-notifications/sending-notifications/) |

## Observabilidad y despliegue

- **API y trabajador.** Logs estructurados con `requestId`, operación, latencia, resultado y ambiente. Métricas de duración por ruta, errores, conexiones activas, cola de eventos, reintentos y eventos fallidos. OpenTelemetry es candidato para trazas de backend, empezando por publicar, cotizar y aceptar. [OpenTelemetry JavaScript](https://opentelemetry.io/docs/languages/js/)
- **App.** Registrar versión, plataforma, tiempo hasta pantalla utilizable y fallos de arranque. Evitar información de compradores, comercios y contenido de solicitudes en la telemetría. Contrastar métricas por versión antes de ampliar distribución.
- **Alertas operativas.** Proponer alertas por indisponibilidad de API, aumento sostenido de 5xx, cola atrasada y fallo de respaldo. El equipo define responsable, ventana y canal de atención antes de invitar compradores. Ante incidentes, detener la expansión de la beta, revisar trazas y resolver con el procedimiento acordado.
- **CI y publicación.** La [estrategia de pruebas](14-estrategia-de-pruebas.md) define qué corre por PR y antes de publicar. El [procedimiento de despliegue e incidentes](16-despliegue-e-incidentes.md) define promoción manual, compatibilidad con la app instalada, migraciones y recuperación.

Revisar semanalmente métricas técnicas junto con cotizaciones, cierres y casos de soporte. Una optimización entra al plan cuando corrige un problema medido o habilita una fase del proyecto.
