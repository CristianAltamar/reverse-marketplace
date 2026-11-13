# Plan de arranque del proyecto

**Para la primera reunión del equipo · 13 de noviembre de 2026**

Empezar con dos frentes en paralelo. **Operaciones** prepara el piloto asistido con compradores y comercios de distintos rubros en todo Galapa. **Desarrollo** prepara el repositorio y valida la arquitectura con datos de prueba. El primer objetivo compartido es comprobar que el equipo puede tramitar una solicitud real y que la app podrá reproducir ese recorrido de forma confiable.

## Qué decidir en la primera reunión

| Decisión | Qué debe quedar escrito |
| --- | --- |
| Responsables | Quién prioriza el producto y decide el alcance, quién opera el piloto y quién coordina desarrollo. Una persona puede asumir más de una función. Asignar quién administrará GitHub y quién revisará el primer PR. |
| Participantes y fechas | Lista corta de compradores con necesidades reales y comercios dispuestos a cotizar. Registrar por comercio sus rubros y las zonas de Galapa que atiende. Fijar una fecha de inicio, una ventana de seguimiento y la fecha de revisión de resultados. |
| Accesos de trabajo | Titular del repositorio de GitHub, integrantes con acceso y canal donde se tomarán y registrarán decisiones. Identificar un Android y un iPhone disponibles para las pruebas. |
| Gasto inicial | Decidir si la prueba de arquitectura arranca con recursos locales y de prueba, o si hay un tope autorizado para contratar servicios de validación. Identificar quién puede aprobar el gasto. |
| Primera entrega | Confirmar el recorrido de prueba con **un producto y un servicio**. El comprador publica, dos comercios cotizan, el comprador compara, acepta una oferta y confirma el cumplimiento. Dos ofertas permiten probar la comparación. La app también permite aceptar una sola oferta válida. |

La fecha del piloto y la lista final de participantes se pueden cerrar mientras desarrollo prepara el entorno. Las decisiones de infraestructura para usuarios reales se toman con la evidencia de la prueba técnica.

## Tareas que podemos iniciar

| Responsable a asignar | Tarea | Entregable |
| --- | --- | --- |
| Producto | Registrar los acuerdos de la reunión y abrir las tareas iniciales del backlog en GitHub. Definir con operaciones cómo se registrarán respuesta, comparación y cierre. | Tareas con responsable, criterio de aceptación y fecha de revisión. |
| Operaciones | Invitar a la primera cohorte de compradores y comercios. Preparar formulario, formato de cotización y registro interno. Explicar el uso de los datos y compartir con los comercios la zona aproximada al solicitar ofertas. | Lista de participantes, formulario probado con una solicitud simulada y una persona asignada al seguimiento. [Resumen del piloto](04-resumen-piloto-galapa.md) |
| Diseño o producto | Dibujar el recorrido del comprador y del comercio. Incluir los estados sin ofertas, oferta vencida, confirmación y solicitud de cancelación a soporte. | Prototipo o bocetos revisados con ambos roles. [Requisitos móviles](05-requisitos-moviles.md) |
| Administración de GitHub y desarrollo | Preparar el workspace Node 26 y pnpm, configurar GitHub Actions y proteger `main`. Ensayar un PR que demuestre que la revisión de otra persona y las pruebas automáticas se exigen. | Instalación reproducible, primer PR verificado y accesos para el equipo. [T01 y T11](12-fases-objetivos-y-tareas.md#backlog-inicial) |
| Desarrollo de backend | Probar Prisma 8 y Prisma 7 estable como contingencia con Node 26 y PostgreSQL. Verificar migraciones, una adjudicación simultánea, reintento y reversión de la transacción. Comparar los servicios administrados candidatos. | Evidencia de compatibilidad, costos estimados y decisión de ORM e infraestructura de beta. [T02 y T03](12-fases-objetivos-y-tareas.md#backlog-inicial) |
| Desarrollo móvil | Preparar una build Android y otra iOS. Probar el acceso por código al correo, la sesión y la pantalla de admisión pendiente con el proveedor candidato. | Una cuenta de prueba puede entrar en ambos sistemas y consultar su estado. [T04](12-fases-objetivos-y-tareas.md#backlog-inicial) |

## Cuándo pasar a la siguiente etapa

**Antes de recibir solicitudes reales del piloto.** Tener un responsable de seguimiento, compradores y comercios confirmados, el formulario y el formato de cotización ensayados, y el registro de resultados preparado. Medir cuántas solicitudes reciben al menos una oferta válida en 24 horas, cuántas reciben dos o más y cuántas se completan dentro de 14 días. Registrar también los casos sin comercio elegible y los motivos de cierre sin compra.

**Antes de consolidar el flujo de la beta.** Elegir Prisma y servicios con evidencia, confirmar las pantallas y campos que funcionaron en el piloto y acordar vigencias, datos para aprobar comercios, criterios de admisión, ofertas fuera de plazo y manejo de cancelaciones pendientes. Los catálogos iniciales deben reflejar los rubros y zonas observados. [Tecnologías y arquitectura](08-tecnologias-y-arquitectura.md) · [Solicitudes, rutas y procesos](10-solicitudes-rutas-y-procesos.md)

**Antes de habilitar usuarios reales en la app.** Completar el recorrido en Android y iOS con un producto y un servicio, probar permisos y concurrencia, ensayar restauración y reversión, y nombrar a la persona responsable de beta y su suplente. Dejar definido el horario en Colombia, el canal de alertas y quién puede publicar o revertir. Revisar privacidad, reportes y eliminación de cuenta con el flujo real. [Estrategia de pruebas](14-estrategia-de-pruebas.md) · [Despliegue e incidentes](16-despliegue-e-incidentes.md)

## Respuestas para empezar a asignar

- **Producto / piloto / desarrollo:** quién coordina cada frente y quién administra GitHub y revisa el primer PR.
- **Galapa:** qué compradores y comercios podemos invitar primero y cuándo revisaremos los primeros resultados.
- **Recursos:** si ya existe un repositorio compartido, qué Android y iPhone están disponibles y si hay un tope para servicios de prueba.
