# Fases, objetivos y tareas técnicas

**Borrador técnico v0.3 · 13 de noviembre de 2026**

| Fase | Objetivo | Evidencia para pasar a la siguiente |
| --- | --- | --- |
| 0. Piloto asistido | Validar cotizaciones de productos y servicios en todo Galapa con pocos compradores y comercios. | Solicitudes reales, primera y segunda oferta, cierres, casos sin proveedor y motivos de pérdida. [Resumen del piloto](04-resumen-piloto-galapa.md) |
| 1A. Prueba de arquitectura | Validar Node 26, pnpm, Prisma y PostgreSQL, correo y builds móviles. | Matriz de compatibilidad de Prisma 8 y contingencia Prisma 7, migraciones en beta, OTP y sesión, builds en Android e iOS y decisión de infraestructura. |
| 1B. Recorrido móvil | Construir el flujo desde publicar hasta completar con datos de prueba. | Un producto y un servicio probados de extremo a extremo. Un acuerdo por solicitud. Una oferta reemplazada o vencida devuelve un conflicto claro. |
| 1C. Beta con usuarios aprobados | Operar altas, comercios, soporte, reseñas, moderación y avisos con un grupo reducido. | El equipo puede aprobar admisiones, activar comercios y resolver incidencias. Permisos verificados, respaldo restaurado, carga de referencia medida y métricas comerciales visibles. |
| 2. Lanzamiento público local | Publicar en tiendas, ampliar incorporación y mantener soporte con datos de beta. | Revisiones de tiendas, privacidad y roles completadas. Indicadores acordados estables en beta y equipo con capacidad para atender el volumen. La cobertura de lanzamiento se define según resultados. |
| 3. Expansión | Extender zonas, oferta o funciones según evidencia de uso. | El equipo prioriza cada cambio con base en solicitudes sin atender, conversión, repetición y costo operativo. Pagos integrados y monetización tienen decisiones propias. |

## Backlog inicial

| ID | Tarea | Dependencia | Entregable |
| --- | --- | --- | --- |
| T01 | Fijar Node 26 y pnpm y preparar el workspace con apps y paquetes. | Selección de versiones verificadas. | Instalación reproducible en CI y contenedor de backend. |
| T02 | Probar Prisma 8 y evaluar Prisma 7 estable en Node 26. | T01 y PostgreSQL de prueba. | Resultados de instalación, migración, bloqueos, idempotencia, rollback, conexiones y decisión registrada. |
| T03 | Comparar servicios administrados y diseñar ambientes. | Presupuesto por definir. | Comparación de región, costos, conexión, correo, almacenamiento, copias, logs y recuperación. |
| T04 | Probar OTP, admisión y sesión en ambos sistemas. | T01 y proveedor candidato de identidad. | Acceso por correo, estado pendiente, aprobación manual y renovación de sesión probados. |
| T05 | Modelar solicitudes, comercios y zonas. | T02 y hallazgos del piloto. | Migraciones, catálogos de prueba, permisos por actor y criterios para la cobertura de cada comercio. |
| T06 | Implementar cotización versionada y aceptación. | T05 y [contrato inicial](07-api-movil.md). | Adjudicación atómica, acuerdo y outbox probados contra reintentos y concurrencia. |
| T07 | Implementar las pantallas del comprador y del comercio. | T04 a T06. | Crear, cotizar, comparar, aceptar y confirmar con un producto y un servicio en Android e iOS. |
| T08 | Completar panel interno y flujo de casos. | T04 a T07. | Admisión, aprobación comercial, cancelación, moderación y auditoría. |
| T09 | Completar adjuntos, avisos, reseñas y cuenta. | T05 a T08. | Carga privada, bandeja y push, publicación de reseñas, reportes, bloqueo y eliminación. |
| T10 | Preparar pruebas, observabilidad y salida a beta. | T03 y T06 a T09. | Pruebas de permisos, carga y restauración, métricas, revisión de tiendas y responsables operativos. |
| T11 | Configurar GitHub Actions y proteger `main`. | T01 y acceso administrativo al repositorio. | PR de prueba que demuestra que se requiere una revisión ajena y que un fallo en una comprobación impide integrar. [Forma de trabajo](15-forma-de-trabajo.md) |
| T12 | Preparar la matriz de regresión crítica y pruebas por versión. | T02, T05 y T06 para backend. T07 para móvil. | PostgreSQL de pruebas, casos de concurrencia y permisos, contrato y recorrido móvil con la versión anterior soportada. [Estrategia de pruebas](14-estrategia-de-pruebas.md) |
| T13 | Ensayar despliegue, reversión y respuesta en beta. | T03, T08, T10 a T12. | Registro de ensayo con corte simulado, tiempos de recuperación, verificación del flujo, titular, suplente y ventana de cobertura. [Despliegue e incidentes](16-despliegue-e-incidentes.md) |

El piloto asistido y las pruebas de arquitectura pueden correr en paralelo. Empezar T11 después de T01 para que los controles estén disponibles al construir el recorrido. El equipo puede usar cuentas y catálogos precargados para la primera integración. La beta con datos reales empieza una vez que se ha elegido Prisma con evidencia, se han ensayado restauración y reversión y soporte puede operar el flujo.

## Decisiones de equipo que alimentan el backlog

| Decisión pendiente | Cuándo se necesita |
| --- | --- |
| Presupuesto mensual y titular de los servicios, dominio, correo y cuentas de tiendas. | Al comparar y contratar infraestructura. |
| Versión de Prisma tras pruebas, framework HTTP y proveedores de identidad, archivos y observabilidad. | Al cerrar la prueba de arquitectura. |
| Criterios para admitir compradores, aprobar comercios y notificar el resultado, y quién operará cada revisión. | Antes de habilitar registro abierto en beta. |
| Catálogos iniciales de rubros y zonas, vigencia de ofertas, cotizaciones que exceden el plazo y confirmación durante un caso de cancelación. | Antes de configurar el flujo de beta. |
| Dispositivos de referencia, umbrales técnicos y objetivos comerciales para pasar a lanzamiento público. | Al establecer la línea base y revisar la beta. |
| Titular y suplente de beta, horario en Colombia, canal de alertas y personas autorizadas para promover y revertir. | Antes del ensayo de publicación y de abrir la beta. |
