# Requisitos de la primera versión móvil

**Borrador técnico v0.2 · 12 de noviembre de 2026**

## Decisiones confirmadas

| Tema | Regla para la beta |
| --- | --- |
| Acceso | Compradores y comercios se identifican con un código de un solo uso enviado al correo. El proveedor de identidad se elegirá después de probar la entrega del correo y el manejo de sesiones. |
| Cuenta y comercio | Una cuenta admitida puede comprar. Puede activar el perfil comercial después de que operaciones lo apruebe. Cambiar de vista en el teléfono conserva la sesión y el historial. |
| Aclaraciones | La solicitud y la cotización incluyen la información para decidir. Si falta un dato, el equipo ayuda por el canal de soporte. Los participantes actualizan sus datos siguiendo las reglas de edición. |
| Cancelación después de aceptar | El comprador solicita la cancelación a soporte. Operaciones revisa el caso y registra la decisión y el motivo. |

El piloto asistido utiliza formulario y WhatsApp en todo Galapa, con pocos compradores y comercios de distintos rubros. La beta móvil usa el mismo flujo comercial y admite cuentas invitadas. El alcance geográfico corresponde al municipio. Cada comercio participa según los rubros y zonas que atiende.

## Historias y criterios de aceptación

| ID | Historia | Criterio verificable |
| --- | --- | --- |
| M01 | Entrar con código por correo. | Al verificar un código válido y tener acceso a la beta, se abre la cuenta. Un código vencido o usado requiere uno nuevo. Repetir el ingreso con el mismo correo conserva la misma cuenta. |
| M02 | Activar el perfil comercial. | La cuenta registra nombre comercial, contacto, rubros y cobertura y envía el perfil a revisión. Puede comprar mientras espera. Solo un comercio aprobado y habilitado puede ver oportunidades y cotizar. |
| M03 | Publicar una necesidad. | El comprador selecciona producto o servicio, rubro, zona aproximada y fecha requerida, y describe lo necesario. Cantidad, unidad, modalidad preferida, fotos y presupuesto son campos según el caso. Puede guardar un borrador, publicarlo y ver su estado. El presupuesto privado queda en su vista. |
| M04 | Encontrar y cotizar una solicitud. | Un comercio aprobado ve solicitudes abiertas para sus rubros y cobertura. Indica qué incluye, disponibilidad, modalidad, fecha prometida, precio, cargos aplicables y vigencia. El servidor calcula el total. El comercio ve sus revisiones y el estado de su oferta. |
| M05 | Comparar y aceptar. | El comprador compara ofertas por total, plazo, modalidad y reputación. Puede elegir una oferta vigente. Si cambió desde que la vio, la app pide revisar la nueva versión. Al aceptar obtiene un único acuerdo y puede consultar el contacto del comercio según las reglas de visibilidad. |
| M06 | Completar o pedir cancelación. | El comprador confirma entrega, retiro o servicio realizado. Si solicita cancelar un acuerdo, puede consultar su estado y la resolución de soporte. Las partes conservan el historial del acuerdo. |
| M07 | Calificar a la otra parte. | Con el acuerdo completado, cada parte puede enviar una reseña en el plazo propuesto de 14 días. Se publican al recibir ambas o al vencer el plazo. Una cuenta nueva aparece «Sin calificaciones». |
| M08 | Enterarse de los cambios. | La app muestra avisos de nuevas oportunidades, cotizaciones y cambios del acuerdo. Un aviso lleva al recurso si la sesión conserva acceso. El flujo se puede completar consultando las bandejas aunque el teléfono rechace el push. |
| M09 | Usar soporte y controles de cuenta. | Las partes pueden reportar contenido o usuarios, bloquear, pedir ayuda y solicitar eliminación de cuenta. Operaciones puede tramitar casos, moderar y registrar las acciones con autor, fecha y motivo. |

## Reglas de producto propuestas para los casos límite

- **Zonas y rubros.** Usar un catálogo administrado de rubros y zonas de Galapa. Una cuenta puede publicar con zona aproximada incluso si su sector aún requiere clasificación. En ese caso, solo los comercios con cobertura municipal completa reciben la oportunidad automáticamente. Operaciones puede clasificar la zona para ampliar el alcance antes de que llegue la primera oferta. La dirección exacta se coordina con el comercio elegido cuando sea necesaria.
- **Vigencia.** La API calcula el vencimiento de la solicitud al publicar a partir de un plazo configurable, limitado por la fecha en que se necesita. La oferta debe vencer a más tardar cuando venza la solicitud o llegue el momento prometido, lo que ocurra primero. La fecha prometida y las condiciones se muestran al comparar. Una oferta con fecha posterior a la requerida requiere acordar cómo se tratará en la beta.
- **Edición.** Un comprador puede editar el borrador o una solicitud abierta que todavía carece de ofertas. Después de la primera oferta, puede cancelar la solicitud abierta y crear otra con las condiciones actualizadas. Cada modificación de la cotización genera una revisión identificable.
- **Solicitudes sin ofertas.** Mostrar que la solicitud sigue abierta y permitir pedir ayuda. Cuando venza, ofrecer duplicarla. Registrar si faltaba un comercio elegible o si los comercios elegibles no respondieron.
- **Aclaraciones y visitas.** Soporte puede ayudar a precisar la necesidad y orientar al usuario a actualizarla. Una visita de diagnóstico con precio propio solo se puede aceptar como tal si ese es el servicio que cubre la solicitud. Una cotización que deja el precio completo pendiente de la visita sirve como información para soporte y queda fuera del comparador de ofertas válidas.
- **Cancelación.** Una solicitud abierta se puede cancelar desde la app. Una vez adjudicada, cancelar el acuerdo requiere un caso de soporte. Con un caso de cancelación abierto, se propone que la app suspenda la confirmación de cumplimiento hasta que soporte resuelva el caso, para evitar decisiones contradictorias. La adjudicación sigue en el historial si se cancela el acuerdo.

## Orden de entrega y salida a beta

1. **Recorrido con cuentas de prueba.** Acceso, un producto y un servicio, publicación, dos ofertas, comparación, aceptación y confirmación. Verificar el recorrido con Android e iOS y con notificaciones desactivadas.
2. **Operación de la beta.** Invitaciones y aprobación de comercios, adjuntos privados, panel de casos, reportes, bloqueo, avisos, reseñas y eliminación de cuenta. Acordar el horario y el responsable de soporte.
3. **Revisión previa a invitados.** Validar permisos con cuentas cruzadas, reintentos y acciones simultáneas, caducidades, enlaces de avisos, pérdida de red y restauración de respaldo en beta. Comprobar que el equipo puede tramitar una cancelación, un reporte y una eliminación de cuenta.

El equipo debe fijar antes de configurar la beta los plazos de publicación y cotización, el catálogo inicial de rubros y zonas, los datos de aprobación comercial, los tiempos de atención y el manejo de una cotización que supera el plazo requerido. El registro operativo del piloto sirve para definir esos valores y establecer una línea de medición.

Ver [modelo de dominio y permisos](06-dominio-y-permisos.md) y [contrato inicial de API](07-api-movil.md).
