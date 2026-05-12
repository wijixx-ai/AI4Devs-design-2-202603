# User Stories — LTI ATS

---

## Product Backlog — Priorización MoSCoW

| Prioridad MoSCoW | ID | Título | Épica | Justificación |
|---|---|---|---|---|
| 🔴 Must have | US-001 | Crear y publicar una oferta en múltiples canales | EP-01 | Sin publicación de ofertas no hay candidatos; es la puerta de entrada al sistema. |
| 🔴 Must have | US-002 | Configurar el pipeline de stages de una oferta | EP-01 | Base estructural del pipeline; todas las demás funcionalidades dependen de que existan stages. |
| 🔴 Must have | US-003 | Ver el ranking IA de candidatos de una oferta | EP-02 | Diferenciador #1 del producto; sin él LTI es un ATS convencional sin propuesta de valor propia. |
| 🔴 Must have | US-011 | Consultar el estado de mi candidatura como candidato | EP-06 | Propuesta de valor explícita al candidato y diferenciador frente a ATS tradicionales; además reduce comunicaciones entrantes al recruiter. |
| 🔴 Must have | US-017 | Gestionar usuarios y roles de la empresa | EP-09 | Requisito operativo para que cualquier empresa pueda configurar y operar LTI de forma autónoma. |
| 🔴 Must have | US-018 | Gestionar consentimientos GDPR y derecho al olvido | EP-09 | Requisito legal obligatorio para operar en la UE; no es negociable ni diferible. |
| 🟠 Should have | US-005 | Mover candidatos en el pipeline con sincronización en tiempo real | EP-03 | Diferenciador #2 (colaboración en tiempo real); alto valor pero con riesgo técnico (WebSockets) que justifica posponerlo respecto a los must. |
| 🟠 Should have | US-007 | Completar un scorecard estructurado tras una entrevista | EP-04 | Necesario para estandarizar la evaluación y tomar decisiones conjuntas; depende de que existan entrevistas programadas. |
| 🟠 Should have | US-008 | Ver el resumen agregado de evaluaciones de una candidatura | EP-04 | Depende de US-007; una vez completados los scorecards, el agregado es imprescindible para la toma de decisión. |
| 🟠 Should have | US-009 | Configurar una automatización de comunicación por cambio de stage | EP-05 | Diferenciador #3 (automatización no-code); elimina el mayor volumen de trabajo repetitivo del recruiter. |
| 🟠 Should have | US-012 | Enviar un mensaje manual a un candidato | EP-06 | Comunicación básica necesaria para cubrir casos que las automatizaciones no contemplan. |
| 🟠 Should have | US-013 | Programar una entrevista con propuesta automática de horarios | EP-07 | Alto impacto en la reducción de fricción, pero la integración de calendarios añade riesgo técnico; se pospone respecto a los must. |
| 🟡 Could have | US-004 | Ajustar los pesos del scoring IA por oferta | EP-02 | Personalización avanzada que mejora el ranking, pero el sistema ya aporta valor con los pesos por defecto. |
| 🟡 Could have | US-006 | Comentar sobre un candidato en el workspace colaborativo | EP-03 | Mejora la colaboración, pero los comentarios pueden canalizarse por mensajes manuales (US-012) en una primera versión. |
| 🟡 Could have | US-010 | Configurar descarte automático por umbral de score IA | EP-05 | Automatización avanzada valiosa en alto volumen; no es bloqueante para el flujo básico de selección. |
| 🟡 Could have | US-014 | Confirmar o solicitar cambio de entrevista desde el portal | EP-07 | Mejora la experiencia del candidato, pero en un primer ciclo la confirmación puede gestionarse por email. |
| 🟡 Could have | US-015 | Consultar el dashboard de métricas del proceso de selección | EP-08 | Los datos de proceso se acumulan con el uso; el dashboard tiene mayor valor una vez que la plataforma lleva varios sprints en producción. |
| 🔵 Won't have (v1) | US-016 | Consultar el rendimiento por fuente de captación | EP-08 | Requiere el campo `fuente` en la entidad CANDIDATURA, actualmente ausente del modelo de datos; se difiere a v2 junto con el diseño del campo. |

### Conclusiones de priorización

La priorización se ha guiado por tres criterios aplicados en cascada: **valor de negocio entregable de forma autónoma**, **dependencias entre historias** y **riesgo técnico**.

Los seis **Must have** forman el MVP mínimo viable: permiten a una empresa publicar ofertas, recibir candidatos rankeados por IA, operar el sistema con sus propios usuarios y cumplir con la normativa legal. Sin ninguna de estas seis historias el producto no puede desplegarse en producción.

Los seis **Should have** incluyen los dos restantes diferenciadores del producto (colaboración en tiempo real y automatización no-code) junto con las funcionalidades de evaluación y comunicación que completan el flujo de selección de extremo a extremo. Se posponen respecto al MVP por su mayor complejidad técnica o por depender de otras historias previas.

Los cinco **Could have** son mejoras significativas que añaden profundidad al producto pero no son bloqueantes: se planifican para sprints posteriores al primer ciclo completo de producción, cuando ya existe retroalimentación real de uso.

US-016 queda fuera del primer release por un **gap en el modelo de datos** (campo `fuente` no modelado): incluirla sin resolver ese gap generaría deuda técnica desde el inicio.

---

## EP-01: Gestión de ofertas y publicación multicanal

---

**ID:** US-001
**Título:** Crear y publicar una oferta en múltiples canales
**Épica:** EP-01

**Historia:**
Como recruiter,
quiero crear una oferta de trabajo con descripción, requisitos y modalidad, y publicarla simultáneamente en LinkedIn, Indeed e InfoJobs,
para captar candidatos en múltiples canales sin duplicar trabajo manual.

**Criterios de aceptación (formato BDD):**
- Dado que estoy en el panel de ofertas, cuando relleno el formulario de nueva oferta y selecciono los canales de publicación, entonces la oferta se publica en todos los canales seleccionados y su estado cambia a "publicada".
- Dado que intento publicar una oferta sin título o descripción, cuando hago clic en "Publicar", entonces el sistema muestra un error de validación y no envía la oferta a ningún canal.
- Dado que la oferta está publicada, cuando accedo a la lista de ofertas, entonces veo el estado de publicación por canal (publicada / error / pendiente).

**Notas adicionales:** El estado de la OFERTA en base de datos debe actualizarse a "publicada" y registrar la fecha_publicacion. Los errores de publicación en un canal no deben bloquear los demás.

**Historias relacionadas:** US-002, US-003

**Evaluación INVEST:**
- ✅ Independent: No depende de otras historias para publicar.
- ✅ Negotiable: Los canales exactos y el orden del formulario son negociables.
- ✅ Valuable: Reduce el trabajo operativo del recruiter de forma directa.
- ✅ Estimable: Alcance claro y acotado.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Criterios de aceptación verificables con datos reales.

---

**ID:** US-002
**Título:** Configurar el pipeline de stages de una oferta
**Épica:** EP-01

**Historia:**
Como recruiter,
quiero definir y reordenar las etapas del proceso de selección para cada oferta,
para adaptar el pipeline a los requisitos específicos de cada posición.

**Criterios de aceptación (formato BDD):**
- Dado que estoy editando una oferta, cuando añado, renombro o reordeno stages, entonces los cambios se reflejan en el pipeline de candidaturas de esa oferta respetando el campo orden de cada STAGE.
- Dado que intento eliminar un stage que tiene candidaturas activas, cuando confirmo la acción, entonces el sistema me advierte del número de candidaturas afectadas y requiere reasignarlas antes de continuar.

**Notas adicionales:** El campo tipo del STAGE determina comportamientos especiales (p.ej. stage de descarte dispara notificación automática).

**Historias relacionadas:** US-001, US-009

**Evaluación INVEST:**
- ✅ Independent: Puede desarrollarse sin US-001 terminada.
- ✅ Negotiable: El número máximo de stages y los tipos disponibles son negociables.
- ✅ Valuable: Permite que cada proceso de selección tenga su propio flujo.
- ✅ Estimable: Funcionalidad CRUD con lógica de orden.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Criterios verificables en la base de datos y la UI.

---

## EP-02: Cribado y ranking IA de candidatos

---

**ID:** US-003
**Título:** Ver el ranking IA de candidatos de una oferta
**Épica:** EP-02

**Historia:**
Como recruiter,
quiero ver los candidatos de una oferta ordenados por puntuación IA multidimensional,
para centrar mi revisión en los perfiles con mayor fit y no tener que leer todos los CVs en orden de llegada.

**Criterios de aceptación (formato BDD):**
- Dado que accedo al pipeline de una oferta, cuando selecciono la vista de ranking IA, entonces veo la lista de CANDIDATURAS ordenada por ai_score descendente con el desglose de dimensiones (técnico, experiencia, cultural).
- Dado que el CV de un candidato no pudo parsearse, cuando accedo al ranking, entonces esa candidatura aparece marcada como "revisión manual necesaria" y no bloquea la visualización del resto.
- Dado que ningún candidato supera el umbral mínimo de score configurado, cuando accedo al ranking, entonces LTI muestra una alerta sugiriendo revisar la descripción de la oferta.

**Notas adicionales:** Los datos de ai_score y ai_analisis se almacenan en la entidad CANDIDATURA. El desglose debe ser legible sin tecnicismos.

**Historias relacionadas:** US-004, US-010

**Evaluación INVEST:**
- ✅ Independent: Solo requiere que existan candidaturas con score calculado.
- ✅ Negotiable: La representación visual del desglose es negociable.
- ✅ Valuable: Es la funcionalidad diferencial principal del producto.
- ✅ Estimable: Frontend de listado con datos ya disponibles en BD.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Criterios de ordenación y estados son verificables.

---

**ID:** US-004
**Título:** Ajustar los pesos del scoring IA por oferta
**Épica:** EP-02

**Historia:**
Como recruiter,
quiero configurar el peso relativo de cada dimensión del scoring IA (técnica, experiencia, cultural) para una oferta concreta,
para que el ranking refleje las prioridades reales del puesto sin necesidad de modificar el modelo de IA.

**Criterios de aceptación (formato BDD):**
- Dado que estoy en la configuración de scoring de una oferta, cuando modifico los pesos y guardo, entonces el ranking se recalcula automáticamente aplicando los nuevos pesos y la lista se actualiza en pantalla.
- Dado que los pesos configurados no suman 100%, cuando intento guardar, entonces el sistema muestra un error indicando el ajuste necesario y no persiste los cambios.

**Notas adicionales:** El ajuste de pesos no debe requerir reentrenar el modelo. La configuración se aplica en el Scoring Engine en tiempo de cálculo.

**Historias relacionadas:** US-003

**Evaluación INVEST:**
- ✅ Independent: El ajuste de pesos es independiente de otras funcionalidades de UI.
- ✅ Negotiable: La granularidad del ajuste (sliders, porcentajes) es negociable.
- ✅ Valuable: Permite personalización por puesto sin intervención técnica.
- ✅ Estimable: Lógica de recálculo acotada.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Resultado del recálculo es verificable con datos de prueba.

---

## EP-03: Pipeline colaborativo recruiter-manager

---

**ID:** US-005
**Título:** Mover candidatos en el pipeline con sincronización en tiempo real
**Épica:** EP-03

**Historia:**
Como recruiter,
quiero mover candidatos entre stages del pipeline desde una vista Kanban compartida,
para que el hiring manager vea los cambios al instante sin necesidad de comunicarse fuera de LTI.

**Criterios de aceptación (formato BDD):**
- Dado que recruiter y hiring manager tienen el pipeline abierto simultáneamente, cuando el recruiter arrastra una candidatura a otro stage, entonces el hiring manager ve el cambio reflejado en menos de 2 segundos sin recargar la página.
- Dado que se produce el movimiento, cuando se actualiza la CANDIDATURA, entonces se registra el nuevo stage_id y se actualiza ultima_actualizacion con la fecha y hora del cambio.

**Notas adicionales:** La sincronización en tiempo real se apoya en el Servicio de Colaboración vía WebSockets. Si hay pérdida de conexión, el cliente debe reconectarse y resincronizar el estado.

**Historias relacionadas:** US-006, US-007

**Evaluación INVEST:**
- ✅ Independent: Puede desarrollarse antes que los scorecards.
- ✅ Negotiable: El mecanismo visual (drag & drop vs. menú) es negociable.
- ✅ Valuable: Elimina la necesidad de coordinación externa entre recruiter y manager.
- ✅ Estimable: Requiere WebSocket pero el alcance es claro.
- ⚠️ Small: Podría requerir dos sprints si se implementa el WebSocket desde cero; se recomienda hacer spike técnico.
- ✅ Testable: El tiempo de sincronización y el estado en BD son verificables.

---

**ID:** US-006
**Título:** Comentar sobre un candidato en el workspace colaborativo
**Épica:** EP-03

**Historia:**
Como hiring manager,
quiero dejar comentarios sobre un candidato directamente en LTI,
para compartir mi opinión con el recruiter sin salir del sistema ni usar email o mensajería externa.

**Criterios de aceptación (formato BDD):**
- Dado que estoy en el perfil de una candidatura, cuando escribo y envío un comentario, entonces el recruiter lo ve en tiempo real en el mismo panel sin necesidad de recargar.
- Dado que hay comentarios existentes, cuando accedo a la candidatura, entonces los comentarios aparecen ordenados cronológicamente con el nombre del autor y la fecha.

**Notas adicionales:** Los comentarios pueden implementarse como MENSAJE con un tipo especial ("nota interna") o como entidad separada. Decisión de implementación negociable.

**Historias relacionadas:** US-005, US-008

**Evaluación INVEST:**
- ✅ Independent: Funcionalidad de comentarios desacoplada del movimiento de stages.
- ✅ Negotiable: La forma de persistencia (MENSAJE vs. entidad propia) es negociable.
- ✅ Valuable: Centraliza la comunicación del equipo de selección en LTI.
- ✅ Estimable: Alcance claro y tecnología conocida.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Sincronización y orden de comentarios son verificables.

---

## EP-04: Evaluación y scorecards

---

**ID:** US-007
**Título:** Completar un scorecard estructurado tras una entrevista
**Épica:** EP-04

**Historia:**
Como hiring manager,
quiero completar un scorecard estructurado después de entrevistar a un candidato,
para registrar mi valoración de forma objetiva y comparable con la de otros entrevistadores.

**Criterios de aceptación (formato BDD):**
- Dado que he realizado una entrevista, cuando abro el scorecard de la candidatura y lo relleno, entonces se crea una EVALUACION con mi scorecard, puntuacion_total y recomendacion vinculados a esa candidatura y a mi usuario.
- Dado que no completo el scorecard en el plazo definido, cuando se cumple el plazo, entonces recibo un recordatorio automático por notificación in-app y email.
- Dado que hay dos scorecards con puntuaciones divergentes en la misma candidatura, cuando el recruiter accede a la vista de evaluaciones, entonces ve un indicador visual resaltando la divergencia.

**Notas adicionales:** El scorecard es un JSON flexible por tipo de rol. La plantilla la define el recruiter al configurar la oferta. La puntuacion_total se calcula automáticamente según los pesos definidos en la plantilla.

**Historias relacionadas:** US-008

**Evaluación INVEST:**
- ✅ Independent: No depende de US-008 para ser entregable.
- ✅ Negotiable: El formato visual del scorecard y los campos son negociables.
- ✅ Valuable: Estandariza la evaluación y reduce sesgos.
- ✅ Estimable: CRUD con cálculo de puntuación.
- ✅ Small: Completable en un sprint si la plantilla es fija; negociar si es configurable.
- ✅ Testable: Creación de EVALUACION y recordatorio son verificables.

---

**ID:** US-008
**Título:** Ver el resumen agregado de evaluaciones de una candidatura
**Épica:** EP-04

**Historia:**
Como recruiter,
quiero ver un resumen agregado de todas las evaluaciones de una candidatura,
para facilitar la toma de decisión conjunta sin consolidar datos manualmente fuera del sistema.

**Criterios de aceptación (formato BDD):**
- Dado que hay al menos dos EVALUACION para una candidatura, cuando accedo a la vista de decisión, entonces veo la puntuación media, las recomendaciones individuales y un indicador de consenso o divergencia.
- Dado que solo hay una evaluación completada, cuando accedo a la vista, entonces veo la evaluación disponible con un aviso de evaluaciones pendientes y quién las debe completar.

**Notas adicionales:** El indicador de divergencia debe ser visualmente inmediato (p.ej. icono de alerta en rojo si la diferencia entre puntuaciones supera un umbral configurable).

**Historias relacionadas:** US-007

**Evaluación INVEST:**
- ✅ Independent: Consume datos ya creados por US-007.
- ✅ Negotiable: El cálculo de consenso y los umbrales de divergencia son negociables.
- ✅ Valuable: Facilita decisiones más rápidas y fundamentadas.
- ✅ Estimable: Agregación sobre datos existentes.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Resultados del agregado son verificables con datos de prueba.

---

## EP-05: Automatización de flujos de selección

---

**ID:** US-009
**Título:** Configurar una automatización de comunicación por cambio de stage
**Épica:** EP-05

**Historia:**
Como recruiter,
quiero configurar una regla automática que envíe un email personalizado al candidato cada vez que avanza de stage,
para mantenerle informado en cada etapa sin dedicar tiempo manual a cada comunicación.

**Criterios de aceptación (formato BDD):**
- Dado que estoy en el builder de automatizaciones, cuando defino un trigger "cambio a stage X" y una acción "enviar email con plantilla Y", entonces la AUTOMATIZACION queda guardada con activa=true para esa oferta.
- Dado que un candidato avanza al stage configurado, cuando se produce el cambio, entonces recibe el email en menos de 5 minutos y el MENSAJE queda registrado con automatico=true y el timestamp en enviado_at.
- Dado que desactivo la automatización, cuando se produce el trigger, entonces el email no se envía y el MENSAJE no se crea.

**Notas adicionales:** La AUTOMATIZACION almacena el trigger y las acciones como JSON para máxima flexibilidad. El Motor de Automatización evalúa los triggers en cada cambio de stage.

**Historias relacionadas:** US-010, US-012

**Evaluación INVEST:**
- ✅ Independent: No requiere otras automatizaciones para funcionar.
- ✅ Negotiable: La interfaz del builder y los tipos de acción disponibles son negociables.
- ✅ Valuable: Elimina trabajo repetitivo y mejora la experiencia del candidato.
- ✅ Estimable: Alcance definido; el Motor de Automatización ya está en la arquitectura.
- ✅ Small: Completable en un sprint con un tipo de trigger y un tipo de acción.
- ✅ Testable: Creación del MENSAJE y timing son verificables.

---

**ID:** US-010
**Título:** Configurar descarte automático por umbral de score IA
**Épica:** EP-05

**Historia:**
Como recruiter,
quiero definir un umbral mínimo de score IA por debajo del cual los candidatos sean descartados automáticamente,
para no tener que revisar manualmente candidaturas que claramente no cumplen los mínimos del puesto.

**Criterios de aceptación (formato BDD):**
- Dado que configuro un umbral mínimo de ai_score en la automatización de la oferta, cuando un candidato aplica y su ai_score es inferior al umbral, entonces la CANDIDATURA cambia a estado "descartado" y se dispara la comunicación de cierre.
- Dado que ningún candidato supera el umbral en los primeros 5 días de publicación, cuando se cumple ese plazo, entonces LTI envía una alerta al recruiter sugiriendo revisar la descripción de la oferta o ajustar el umbral.

**Notas adicionales:** El descarte automático debe quedar trazado en el historial de la candidatura para auditorías. El recruiter siempre puede revertir manualmente un descarte automático.

**Historias relacionadas:** US-003, US-009

**Evaluación INVEST:**
- ✅ Independent: Puede implementarse como un tipo de AUTOMATIZACION adicional.
- ✅ Negotiable: El mecanismo de alerta a los 5 días y el umbral por defecto son negociables.
- ✅ Valuable: Reduce drásticamente la carga en ofertas de alto volumen.
- ✅ Estimable: Extensión del Motor de Automatización con nueva lógica de trigger.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Estado de la CANDIDATURA y envío del mensaje son verificables.

---

## EP-06: Comunicaciones y portal del candidato

---

**ID:** US-011
**Título:** Consultar el estado de mi candidatura como candidato
**Épica:** EP-06

**Historia:**
Como candidato,
quiero acceder a un portal personalizado donde ver en qué etapa se encuentra mi candidatura,
para no tener incertidumbre sobre el proceso sin necesidad de contactar al equipo de selección.

**Criterios de aceptación (formato BDD):**
- Dado que accedo al portal con mi token de candidatura, cuando se carga la página, entonces veo el nombre de la oferta, el stage actual y la fecha de ultima_actualizacion de mi CANDIDATURA.
- Dado que mi candidatura ha sido descartada, cuando accedo al portal, entonces veo el estado "proceso cerrado" y el contenido del mensaje de cierre que se me envió.

**Notas adicionales:** El acceso al portal es por token único por candidatura (sin registro de usuario). El token se envía al candidato en el email de confirmación de recepción.

**Historias relacionadas:** US-012, US-014

**Evaluación INVEST:**
- ✅ Independent: El portal puede existir aunque otras funcionalidades no estén completas.
- ✅ Negotiable: El diseño visual y la cantidad de información mostrada son negociables.
- ✅ Valuable: Mejora directamente la experiencia del candidato y reduce comunicaciones entrantes.
- ✅ Estimable: Página de solo lectura con autenticación por token.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Datos mostrados y restricción por token son verificables.

---

**ID:** US-012
**Título:** Enviar un mensaje manual a un candidato
**Épica:** EP-06

**Historia:**
Como recruiter,
quiero enviar mensajes personalizados a un candidato fuera del flujo automático,
para gestionar situaciones específicas que las automatizaciones no contemplan.

**Criterios de aceptación (formato BDD):**
- Dado que estoy en el perfil de una candidatura, cuando redacto y envío un mensaje manual, entonces se crea un MENSAJE con automatico=false, el timestamp en enviado_at, y el candidato lo recibe por email.
- Dado que el candidato responde al email, cuando llega la respuesta, entonces aparece en el hilo de mensajes de esa candidatura dentro de LTI para que el recruiter pueda leerla.
- Dado que el email rebota, cuando se produce el rebote, entonces LTI notifica al recruiter dentro del sistema para que verifique los datos de contacto del candidato.

**Notas adicionales:** Los mensajes manuales y automáticos comparten la entidad MENSAJE; el campo automatico los distingue.

**Historias relacionadas:** US-009, US-011

**Evaluación INVEST:**
- ✅ Independent: Funciona con o sin automatizaciones activas.
- ✅ Negotiable: El editor de texto (enriquecido vs. plano) es negociable.
- ✅ Valuable: Cubre los casos que las reglas automáticas no pueden anticipar.
- ✅ Estimable: Integración con SendGrid ya prevista en la arquitectura.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Creación del MENSAJE y entrega del email son verificables.

---

## EP-07: Programación de entrevistas

---

**ID:** US-013
**Título:** Programar una entrevista con propuesta automática de horarios
**Épica:** EP-07

**Historia:**
Como recruiter,
quiero que LTI proponga franjas horarias compatibles entre todos los participantes de una entrevista,
para acordar la cita sin intercambio de emails entre el candidato y los entrevistadores.

**Criterios de aceptación (formato BDD):**
- Dado que selecciono los entrevistadores y la duración, cuando LTI consulta los calendarios vinculados, entonces propone al menos 3 franjas horarias disponibles en los próximos 5 días laborables.
- Dado que confirmo una franja, cuando se crea la ENTREVISTA, entonces todos los participantes reciben invitación de calendario y un enlace de videoconferencia generado automáticamente.
- Dado que no hay franjas comunes en los próximos 5 días, cuando LTI termina la búsqueda, entonces me notifica y amplía la búsqueda a 10 días laborables.

**Notas adicionales:** La integración con Google Calendar y Outlook se realiza a través del Servicio de Integraciones. La ENTREVISTA queda vinculada a la CANDIDATURA con los entrevistadores en la tabla ENTREVISTADOR.

**Historias relacionadas:** US-014

**Evaluación INVEST:**
- ✅ Independent: Puede desarrollarse de forma aislada con mocks de los calendarios.
- ✅ Negotiable: El número de franjas propuestas y el horizonte de búsqueda son negociables.
- ✅ Valuable: Elimina uno de los mayores cuellos de botella en la coordinación.
- ⚠️ Estimable: Requiere aclarar el alcance de las integraciones de calendario antes de estimar.
- ⚠️ Small: Puede requerir dividirse en: (a) propuesta manual y (b) propuesta automática por calendario.
- ✅ Testable: Franjas propuestas e invitaciones enviadas son verificables.

---

**ID:** US-014
**Título:** Confirmar o solicitar cambio de entrevista desde el portal
**Épica:** EP-07

**Historia:**
Como candidato,
quiero confirmar o solicitar un cambio de horario para una entrevista programada desde mi portal,
para gestionar mi disponibilidad sin tener que llamar o escribir directamente al recruiter.

**Criterios de aceptación (formato BDD):**
- Dado que tengo una entrevista pendiente de confirmación, cuando accedo al portal y hago clic en "Confirmar", entonces el estado de la ENTREVISTA cambia a "confirmada" y el recruiter recibe una notificación en LTI.
- Dado que solicito cambio de horario con una nota explicativa, cuando envío la solicitud, entonces el recruiter la recibe en LTI y puede proponer nuevas franjas desde el sistema.

**Notas adicionales:** El cambio de estado de la ENTREVISTA debe ser trazable. La notificación al recruiter puede implementarse como notificación in-app o email, según la configuración de la automatización.

**Historias relacionadas:** US-013

**Evaluación INVEST:**
- ✅ Independent: El portal del candidato puede operar con o sin integración de calendarios activa.
- ✅ Negotiable: El canal de notificación al recruiter es negociable.
- ✅ Valuable: Mejora la experiencia del candidato y reduce la carga del recruiter.
- ✅ Estimable: Interacción acotada sobre la entidad ENTREVISTA.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Cambio de estado y notificación son verificables.

---

## EP-08: Analytics y reporting

---

**ID:** US-015
**Título:** Consultar el dashboard de métricas del proceso de selección
**Épica:** EP-08

**Historia:**
Como recruiter,
quiero ver un dashboard con el time-to-hire y las tasas de conversión por stage de mis ofertas activas,
para identificar cuellos de botella en el proceso y actuar sobre ellos con datos objetivos.

**Criterios de aceptación (formato BDD):**
- Dado que accedo al módulo de analytics, cuando selecciono una oferta o un rango de fechas, entonces veo el time-to-hire medio, el número de candidaturas por stage y la tasa de conversión entre stages consecutivos.
- Dado que hay menos de 5 candidaturas en la oferta seleccionada, cuando accedo al dashboard, entonces el sistema muestra un aviso indicando que los datos son insuficientes para un análisis representativo.

**Notas adicionales:** El Servicio de Analytics agrega datos de las entidades CANDIDATURA, OFERTA y STAGE. Las métricas deben calcularse bajo demanda o actualizarse con una frecuencia razonable (p.ej. cada hora).

**Historias relacionadas:** US-016

**Evaluación INVEST:**
- ✅ Independent: El dashboard puede construirse sobre datos existentes sin nuevas entidades.
- ✅ Negotiable: El tipo de gráficas y el nivel de detalle son negociables.
- ✅ Valuable: Permite mejora continua del proceso basada en datos.
- ✅ Estimable: Consultas de agregación sobre datos ya estructurados.
- ✅ Small: Completable en un sprint con un conjunto inicial de métricas.
- ✅ Testable: Los valores del dashboard son verificables contra los datos de las entidades.

---

**ID:** US-016
**Título:** Consultar el rendimiento por fuente de captación
**Épica:** EP-08

**Historia:**
Como recruiter,
quiero ver qué fuentes de captación generan candidatos de mayor calidad y menor coste,
para optimizar la inversión en cada canal y concentrar esfuerzos donde el ROI es mayor.

**Criterios de aceptación (formato BDD):**
- Dado que accedo al reporte de fuentes, cuando lo visualizo, entonces veo por canal (LinkedIn, Indeed, portal propio, etc.): número de candidaturas recibidas, ai_score medio y tasa de avance a entrevista.
- Dado que filtro por oferta, cuando aplico el filtro, entonces los datos del reporte se recalculan mostrando únicamente las candidaturas de esa oferta.

**Notas adicionales:** La fuente de captación debe quedar registrada en la CANDIDATURA en el momento de la aplicación. Requiere añadir el campo fuente a la entidad si no está contemplado.

**Historias relacionadas:** US-015

**Evaluación INVEST:**
- ✅ Independent: Reporte adicional independiente del dashboard de métricas de proceso.
- ✅ Negotiable: Las métricas concretas por canal y las visualizaciones son negociables.
- ✅ Valuable: Impacta directamente en decisiones de inversión en canales.
- ⚠️ Estimable: Requiere confirmar que el campo fuente está en el modelo de datos.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Los valores del reporte son verificables contra los datos de CANDIDATURA.

---

## EP-09: Administración y configuración

---

**ID:** US-017
**Título:** Gestionar usuarios y roles de la empresa
**Épica:** EP-09

**Historia:**
Como administrador,
quiero crear, editar y desactivar usuarios de mi empresa y asignarles roles,
para controlar quién tiene acceso a LTI y con qué permisos, sin depender del soporte técnico.

**Criterios de aceptación (formato BDD):**
- Dado que estoy en el panel de administración, cuando creo un nuevo usuario con nombre, email y rol, entonces recibe un email de bienvenida con instrucciones de acceso y su USUARIO queda con activo=true en la base de datos.
- Dado que desactivo un usuario, cuando guardo el cambio, entonces activo=false y el usuario pierde el acceso inmediatamente sin poder iniciar sesión.
- Dado que intento crear un usuario con un email ya registrado en la empresa, cuando envío el formulario, entonces el sistema muestra un error indicando que el email ya existe.

**Notas adicionales:** El rol determina los permisos dentro del sistema (recruiter, hiring manager, administrador). El Servicio de Autenticación es responsable de validar activo=true en cada sesión.

**Historias relacionadas:** US-018

**Evaluación INVEST:**
- ✅ Independent: Funcionalidad de administración autónoma.
- ✅ Negotiable: La interfaz de gestión y el flujo de invitación son negociables.
- ✅ Valuable: Necesario para que cualquier empresa pueda operar LTI de forma autónoma.
- ✅ Estimable: CRUD estándar con lógica de rol.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Estado del USUARIO y restricción de acceso son verificables.

---

**ID:** US-018
**Título:** Gestionar consentimientos GDPR y derecho al olvido de candidatos
**Épica:** EP-09

**Historia:**
Como administrador,
quiero que LTI registre el consentimiento GDPR de cada candidato y permita ejecutar solicitudes de derecho al olvido,
para garantizar el cumplimiento legal y poder responder a auditorías o requerimientos de privacidad.

**Criterios de aceptación (formato BDD):**
- Dado que un candidato completa el formulario de aplicación, cuando envía la solicitud, entonces debe aceptar explícitamente la política de privacidad y el consentimiento queda registrado con fecha, versión del texto y candidato_id.
- Dado que proceso una solicitud de derecho al olvido, cuando ejecuto la anonimización del CANDIDATO, entonces sus datos personales (nombre, email, teléfono, cv_url, linkedin_url) quedan sustituidos por valores anonimizados y la acción queda registrada en el log de auditoría.
- Dado que intento procesar candidaturas de un candidato sin consentimiento registrado, cuando el sistema detecta la ausencia, entonces bloquea el procesamiento y alerta al administrador.

**Notas adicionales:** La anonimización debe preservar el ai_score_global y los datos estadísticos para que los reportes históricos no se vean afectados. Solo se eliminan los datos de identificación personal.

**Historias relacionadas:** US-017

**Evaluación INVEST:**
- ✅ Independent: Puede implementarse como módulo independiente en el panel de administración.
- ✅ Negotiable: El mecanismo concreto de anonimización y el log de auditoría son negociables.
- ✅ Valuable: Requisito legal no negociable para operar en la UE.
- ✅ Estimable: Alcance claro con operaciones de UPDATE sobre entidad CANDIDATO.
- ✅ Small: Completable en un sprint.
- ✅ Testable: Estado de los campos tras anonimización y registro en log son verificables.

---

## Tickets de trabajo — US-009: Configurar una automatización de comunicación por cambio de stage

Se ha elegido US-009 por ser la historia técnicamente más diversa: requiere migración de base de datos, lógica de eventos en el Motor de Automatización, integración con SendGrid y un builder visual en frontend.

---

**TK-001 — Migración de base de datos: tabla AUTOMATIZACION y campo automatico en MENSAJE**
**Tipo:** Base de datos
**Historia asociada:** US-009
**Descripción técnica:** Crear la tabla `AUTOMATIZACION` con los campos definidos en el modelo de datos: `id` (PK), `oferta_id` (FK → OFERTA), `nombre` (varchar), `trigger` (jsonb), `acciones` (jsonb), `activa` (bool, default true). Verificar que la tabla `MENSAJE` ya dispone del campo `automatico` (bool) y `enviado_at` (timestamp); añadirlos si no existen. Crear índice en `AUTOMATIZACION(oferta_id, activa)` para optimizar las consultas del Motor de Automatización.
**Criterios de aceptación técnicos:**
- [ ] La migración se ejecuta sin errores en entorno de desarrollo y de CI.
- [ ] La tabla `AUTOMATIZACION` existe con todos los campos y restricciones de FK correctas.
- [ ] Los campos `automatico` y `enviado_at` existen en `MENSAJE`.
- [ ] El índice `(oferta_id, activa)` está creado y aparece en el plan de consulta.
**Dependencias:** Ninguna
**Estimación:** 1 punto
**Rol sugerido:** DBA / Backend dev

---

**TK-002 — API REST: CRUD de automatizaciones por oferta**
**Tipo:** Backend
**Historia asociada:** US-009
**Descripción técnica:** Implementar los siguientes endpoints en el Servicio de Candidaturas (o en un nuevo Servicio de Automatizaciones según la decisión de arquitectura):
- `POST /api/ofertas/{oferta_id}/automatizaciones` — crea una AUTOMATIZACION; valida que el JSON de `trigger` tenga el campo `tipo` y `stage_id`, y que `acciones` tenga al menos un elemento con `tipo` y `plantilla_id`.
- `GET /api/ofertas/{oferta_id}/automatizaciones` — lista todas las automatizaciones de la oferta.
- `PATCH /api/automatizaciones/{id}` — actualiza nombre, trigger, acciones o el flag `activa`.
- `DELETE /api/automatizaciones/{id}` — elimina la automatización si no hay eventos pendientes asociados.

Todas las operaciones requieren autenticación JWT con rol `recruiter` o `admin`.
**Criterios de aceptación técnicos:**
- [ ] `POST` devuelve `201 Created` con el objeto creado; devuelve `422` si el JSON de trigger o acciones es inválido.
- [ ] `GET` devuelve `200` con array vacío si no hay automatizaciones; `404` si la oferta no existe.
- [ ] `PATCH` devuelve `200` con el objeto actualizado; `404` si el id no existe.
- [ ] `DELETE` devuelve `204`; `403` si el usuario no tiene permisos; `409` si hay eventos pendientes.
- [ ] Sin token válido todos los endpoints devuelven `401`.
**Dependencias:** TK-001
**Estimación:** 3 puntos
**Rol sugerido:** Backend dev

---

**TK-003 — Motor de Automatización: listener de cambios de stage en CANDIDATURA**
**Tipo:** Backend
**Historia asociada:** US-009
**Descripción técnica:** Implementar un listener de eventos en el Motor de Automatización que se dispare cada vez que el campo `stage_id` de una CANDIDATURA cambia. El listener debe:
1. Consultar las AUTOMATIZACIONES activas (`activa=true`) de la oferta asociada a la candidatura.
2. Evaluar si algún trigger del tipo `cambio_stage` coincide con el nuevo `stage_id`.
3. Si hay coincidencia, encolar la acción correspondiente para su ejecución asíncrona (cola de mensajes o tarea programada).

El evento de cambio puede publicarse desde el endpoint de actualización de CANDIDATURA mediante un evento interno (pub/sub o llamada directa al Motor de Automatización).
**Criterios de aceptación técnicos:**
- [ ] Al actualizar `stage_id` de una CANDIDATURA, el listener recibe el evento en menos de 1 segundo.
- [ ] Solo se evalúan automatizaciones con `activa=true` de la oferta correcta.
- [ ] Si el trigger coincide, la acción se encola; si no coincide, no se genera ninguna acción.
- [ ] Si la AUTOMATIZACION está `activa=false`, no se encola ninguna acción aunque el trigger coincida.
- [ ] El listener registra en log la evaluación (trigger evaluado, resultado, acción encolada o descartada).
**Dependencias:** TK-001, TK-002
**Estimación:** 5 puntos
**Rol sugerido:** Backend dev

---

**TK-004 — Motor de Automatización: ejecutor de la acción "enviar email" con SendGrid**
**Tipo:** Backend
**Historia asociada:** US-009
**Descripción técnica:** Implementar el ejecutor de acciones de tipo `enviar_email` dentro del Motor de Automatización. El ejecutor debe:
1. Recuperar la plantilla de email indicada en el campo `acciones[].plantilla_id`.
2. Resolver las variables de la plantilla (nombre del candidato, nombre de la oferta, stage actual) con los datos de la CANDIDATURA y el CANDIDATO.
3. Llamar a la API de SendGrid para enviar el email al `email` del CANDIDATO.
4. Crear un registro en la tabla MENSAJE con: `candidatura_id`, `remitente_id` (id del sistema), `asunto`, `contenido` renderizado, `canal=email`, `automatico=true`, `enviado_at=now()`.
5. En caso de rebote o error de SendGrid, marcar el MENSAJE con estado de error y publicar una notificación al recruiter.
**Criterios de aceptación técnicos:**
- [ ] El email se envía al candidato en menos de 5 minutos desde que se encola la acción.
- [ ] El MENSAJE se crea en BD con `automatico=true` y `enviado_at` rellenado.
- [ ] Las variables de la plantilla se resuelven correctamente (sin placeholders vacíos en el email enviado).
- [ ] Si SendGrid devuelve error 4xx o 5xx, el MENSAJE queda con estado `error` y el recruiter recibe una notificación in-app.
- [ ] Si el email rebota (webhook de SendGrid), el MENSAJE se actualiza y se notifica al recruiter.
**Dependencias:** TK-001, TK-003
**Estimación:** 3 puntos
**Rol sugerido:** Backend dev

---

**TK-005 — Frontend: builder visual de automatizaciones**
**Tipo:** Frontend
**Historia asociada:** US-009
**Descripción técnica:** Implementar el componente de builder de automatizaciones dentro de la página de configuración de una oferta. El componente debe permitir:
- Listar las automatizaciones existentes de la oferta (llamada a `GET /api/ofertas/{id}/automatizaciones`).
- Crear una nueva automatización mediante un formulario con: campo nombre, selector de trigger (tipo `cambio_stage` + selector de stage disponible en la oferta), selector de acción (tipo `enviar_email` + selector de plantilla), y toggle de activación.
- Editar y desactivar automatizaciones existentes.
- Mostrar confirmación visual tras guardar o error si la API responde con fallo.

No debe incluir lógica de negocio: toda la validación del JSON ocurre en el backend (TK-002).
**Criterios de aceptación técnicos:**
- [ ] El listado de automatizaciones se carga correctamente al abrir la sección de configuración.
- [ ] El formulario de creación llama a `POST` y añade la nueva automatización al listado sin recargar la página.
- [ ] El toggle de activación llama a `PATCH` con `{activa: true/false}` y actualiza el estado visual.
- [ ] Si la API devuelve error, se muestra un mensaje de error legible al usuario.
- [ ] El componente es accesible por teclado y tiene los atributos ARIA mínimos.
**Dependencias:** TK-002
**Estimación:** 8 puntos
**Rol sugerido:** Frontend dev

---

**TK-006 — QA: tests de integración del flujo completo de automatización**
**Tipo:** QA
**Historia asociada:** US-009
**Descripción técnica:** Implementar tests de integración que cubran el flujo end-to-end de US-009:
1. Crear una AUTOMATIZACION vía API para una oferta con trigger `cambio_stage` al stage X.
2. Cambiar el `stage_id` de una CANDIDATURA de esa oferta al stage X.
3. Verificar que se crea un MENSAJE con `automatico=true` y `enviado_at` no nulo.
4. Verificar que el email llega al candidato (mock de SendGrid en entorno de test).
5. Desactivar la automatización (`activa=false`) y repetir el cambio de stage; verificar que no se crea ningún MENSAJE nuevo.
**Criterios de aceptación técnicos:**
- [ ] Todos los escenarios del happy path pasan en CI.
- [ ] El escenario de automatización desactivada no genera MENSAJE ni llamada a SendGrid.
- [ ] El escenario de error de SendGrid genera MENSAJE con estado `error` y notificación al recruiter.
- [ ] La cobertura de los endpoints de TK-002 alcanza el 100% de ramas en los tests.
**Dependencias:** TK-001, TK-002, TK-003, TK-004
**Estimación:** 3 puntos
**Rol sugerido:** QA / Backend dev

---

### Resumen de estimación — US-009

| Ticket | Tipo | Estimación |
|---|---|---|
| TK-001 | Base de datos | 1 punto |
| TK-002 | Backend — API REST | 3 puntos |
| TK-003 | Backend — Motor de Automatización (listener) | 5 puntos |
| TK-004 | Backend — Motor de Automatización (ejecutor) | 3 puntos |
| TK-005 | Frontend — Builder visual | 8 puntos |
| TK-006 | QA — Tests de integración | 3 puntos |
| **Total** | | **23 puntos** |

**Desglose por tipo:** Base de datos: 1 pt · Backend: 11 pts · Frontend: 8 pts · QA: 3 pts

El camino crítico es: TK-001 → TK-002 → TK-003 → TK-004 → TK-006. TK-005 (frontend) puede desarrollarse en paralelo con TK-003 y TK-004 una vez que TK-002 está disponible, lo que permite distribuir el trabajo entre dos desarrolladores simultáneamente.

---

## Estimación de esfuerzo — Planning Poker (US-009)

**Metodología:** Planning Poker con escala Fibonacci en puntos de historia (1, 2, 3, 5, 8, 13, 21). Los puntos de historia capturan complejidad relativa e incertidumbre mejor que las horas; la escala Fibonacci evita falsas precisiones forzando al equipo a tomar posiciones claras. Planning Poker se elige frente a T-shirt sizes porque genera debate estructurado que aflora riesgos ocultos antes de comprometer el sprint.

---

**TK-001 — Migración de base de datos**
- Estimación inicial: 1 pt
- Votos: Backend dev → 1 | Frontend dev → 1 | QA → 2 | Tech Lead → 1
- Debate: QA vota 2 argumentando que hay que validar la migración en entornos de CI y staging con datos preexistentes en MENSAJE. El resto considera que el esquema está completamente definido y las migraciones son rutinarias. QA acepta 1 al confirmar que no hay datos legacy que requieran backfill.
- Estimación final consensuada: **1 pt**
- Riesgos: Si MENSAJE ya tiene filas en producción sin el campo `automatico`, podría necesitar backfill con valor por defecto; verificar antes del despliegue.

---

**TK-002 — API REST: CRUD de automatizaciones**
- Estimación inicial: 3 pts
- Votos: Backend dev → 3 | Frontend dev → 3 | QA → 5 | Tech Lead → 3
- Debate: QA vota 5 argumentando que la validación del JSON de `trigger` y `acciones` implica muchos escenarios negativos que hay que testear. Backend dev responde que la estructura JSON está especificada y la validación se puede resolver con un JSON Schema. Consenso en 3 dado que el esquema de validación está acotado.
- Estimación final consensuada: **3 pts**
- Riesgos: Si en el futuro se añaden nuevos tipos de trigger o acción, la validación del JSON crecerá; conviene diseñar el schema de forma extensible desde el inicio.

---

**TK-003 — Motor de Automatización: listener de cambios de stage**
- Estimación inicial: 5 pts
- Votos: Backend dev → 8 | Frontend dev → 5 | QA → 8 | Tech Lead → 8
- Debate: Backend dev y Tech Lead votan 8 porque la decisión arquitectónica de pub/sub vs. llamada directa no está tomada y tiene implicaciones de ordenación de eventos, idempotencia y manejo de fallos. Frontend dev revisa su voto a 8 al entender el alcance real. El Tech Lead propone hacer un spike de medio día antes del sprint para decidir el patrón de integración y así reducir la incertidumbre.
- Estimación final consensuada: **8 pts**
- Riesgos: Sin una decisión previa sobre el patrón de eventos (pub/sub vs. llamada síncrona), el ticket podría crecer a 13. Se recomienda spike técnico previo.

---

**TK-004 — Motor de Automatización: ejecutor email + SendGrid**
- Estimación inicial: 3 pts
- Votos: Backend dev → 3 | Frontend dev → 3 | QA → 5 | Tech Lead → 5
- Debate: QA y Tech Lead votan 5 porque el manejo del webhook de rebote de SendGrid (actualizar MENSAJE con estado error y notificar al recruiter) es alcance adicional no trivial que implica configurar un endpoint público, validar la firma del webhook y gestionar el estado del MENSAJE. Backend dev acepta la revisión al constatar que el webhook no estaba bien dimensionado en la estimación inicial.
- Estimación final consensuada: **5 pts**
- Riesgos: La configuración del webhook de SendGrid varía entre entornos (dev necesita túnel tipo ngrok, staging y producción requieren registro en el panel de SendGrid); puede añadir fricción operativa.

---

**TK-005 — Frontend: builder visual de automatizaciones**
- Estimación inicial: 8 pts
- Votos: Backend dev → 8 | Frontend dev → 13 | QA → 8 | Tech Lead → 13
- Debate: Frontend dev vota 13 argumentando que el selector de stage es dinámico (depende de los stages configurados para cada oferta), el selector de plantilla puede implicar una llamada adicional a la API, y la UX de un builder no-code tiene muchos estados intermedios (sin automatizaciones, con automatizaciones activas/inactivas, error al guardar). Tech Lead apoya el 13. QA y Backend dev revisan al alza. Sin consenso entre 8 y 13 en primera ronda; se vota de nuevo y la mayoría elige 13.
- Estimación final consensuada: **13 pts**
- Riesgos: Si se decide añadir una previsualización del email generado por la plantilla, el scope podría crecer a 21. Acordar el alcance exacto del builder antes de empezar el ticket.

---

**TK-006 — QA: tests de integración del flujo completo**
- Estimación inicial: 3 pts
- Votos: Backend dev → 3 | Frontend dev → 2 | QA → 5 | Tech Lead → 3
- Debate: QA vota 5 señalando que los tests de flujos asíncronos (el email se envía con latencia) requieren estrategias de polling o waiters que hacen los tests frágiles si no se diseñan bien. El equipo decide mantener 3 condicionado a usar un mock síncrono de SendGrid en el entorno de tests y no depender de la latencia real.
- Estimación final consensuada: **3 pts**
- Riesgos: Si no se usa mock síncrono de SendGrid, los tests pueden ser flaky por timeouts; acordar la estrategia de mock antes de empezar.

---

### Comparativa estimación inicial vs. final

| Ticket | Tipo | Estimación inicial | Estimación final | Variación |
|---|---|---|---|---|
| TK-001 | Base de datos | 1 pt | 1 pt | — |
| TK-002 | Backend — API REST | 3 pts | 3 pts | — |
| TK-003 | Backend — Listener de eventos | 5 pts | 8 pts | +3 pts |
| TK-004 | Backend — Ejecutor email | 3 pts | 5 pts | +2 pts |
| TK-005 | Frontend — Builder visual | 8 pts | 13 pts | +5 pts |
| TK-006 | QA — Tests de integración | 3 pts | 3 pts | — |
| **Total** | | **23 pts** | **33 pts** | **+10 pts (+43%)** |

### Conclusiones

La sesión de Planning Poker ha revelado una subestimación del 43% respecto a la estimación inicial. Los tres tickets que concentran la mayor incertidumbre son:

- **TK-003** (listener de eventos, +3 pts): la decisión arquitectónica sobre el patrón de integración (pub/sub vs. llamada directa) no estaba tomada. Acción antes del sprint: **spike técnico de medio día** para elegir el patrón y definir el contrato de eventos.
- **TK-004** (ejecutor email, +2 pts): el webhook de rebote de SendGrid estaba infravalorado. Acción: aclarar en refinamiento qué estados de error maneja el sistema y cuáles se difieren a una iteración posterior.
- **TK-005** (builder frontend, +5 pts): la UX de un builder no-code tiene más estados de los que aparenta. Acción: acordar el alcance exacto del builder (¿previsualización del email sí o no?) antes de comprometer el ticket en el sprint; si se incluye previsualización, escalar a 21 pts y dividir en dos tickets.
