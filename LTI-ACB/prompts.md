## PROMPT 1 — GENERACIÓN DE USER STORIES

Actúa como Product Manager senior con experiencia en metodologías ágiles.

El documento `LTI-acb/LTI-acb.md`  es el PRD completo del producto LTI (descripción, funciones, modelo de datos, casos de uso y arquitectura). Léelo en su totalidad y, basándote exclusivamente en él, genera las User Stories necesarias para cubrir todas las funcionalidades principales del sistema.

Aplica las siguientes reglas para cada historia:

**Plantilla obligatoria:**

**ID:** US-XXX
**Título:** [Título descriptivo corto]
**Épica:** [Épica a la que pertenece]

**Historia:**
Como [tipo de usuario],
quiero [realizar una acción],
para [obtener un beneficio concreto].

**Criterios de aceptación (formato BDD):**
- Dado que [contexto], cuando [acción], entonces [resultado].
- (un criterio por escenario; incluye al menos un escenario alternativo o de error)

**Notas adicionales:** [restricciones, edge cases, consideraciones UX relevantes]

**Historias relacionadas:** [IDs vinculados]

**Evaluación INVEST:**
- ✅/❌ Independent / Negotiable / Valuable / Estimable / Small / Testable — [justificación de una línea cada uno]

---

**Reglas de calidad:**
1. Perspectiva del usuario final, no técnica. Sin detalles de implementación en el cuerpo de la historia.
2. Cada historia debe ser completable en un sprint (≤ 4 días). Si es demasiado grande, descomponla en slices verticales.
3. Los criterios de aceptación deben ser coherentes con las entidades del modelo de datos del documento.
4. Verifica INVEST antes de incluir cada historia.

**Cobertura mínima:** 2 historias por épica, cubriendo los 4 tipos de usuario del sistema (recruiter, hiring manager, administrador, candidato) a lo largo del conjunto. Organiza las historias por épicas derivadas de las funcionalidades del documento.

---

## PROMPT 2 — CONSTRUCCIÓN DEL PRODUCT BACKLOG

Actúa como Product Manager senior con experiencia en metodologías ágiles.

El documento `LTI-ACB/UserStories-ACB.md` contiene las 18 User Stories del producto LTI organizadas por épicas. Usando exclusivamente esas historias, construye el Product Backlog priorizado y añadelo en dicho documento.

**Metodología de priorización:** MoSCoW (Must have / Should have / Could have / Won't have this release). Justifica cada asignación con un argumento breve basado en valor de negocio, riesgo técnico o dependencias entre historias.

**Formato de entrega:**

Hemos elegido el formato MoSCoW
Una tabla con las columnas: Prioridad MoSCoW | ID | Título | Épica | Justificación (1 línea)

Ordena la tabla de mayor a menor prioridad dentro de cada categoría MoSCoW. Al final de la tabla incluye un párrafo de conclusiones explicando los criterios que han guiado la priorización global (qué valor de negocio se entrega primero y por qué).

---

## PROMPT 3 — TICKETS DE TRABAJO

Actúa como Tech Lead con experiencia en equipos de desarrollo ágil.

El documento `LTI-ACB/UserStories-ACB.md` contiene las User Stories del producto LTI. Descompón la User Story **US-009 — Configurar una automatización de comunicación por cambio de stage** en tickets de trabajo tal y como se haría en una reunión de planificación de sprint. Se ha elegido esta historia por ser la más técnicamente diversa: involucra modelo de datos (entidad AUTOMATIZACION con JSON), lógica de eventos en el Motor de Automatización, integración con SendGrid y un builder visual en frontend.

**Plantilla obligatoria para cada ticket:**

**TK-XXX — [Título técnico del ticket]**
**Tipo:** Backend / Frontend / Base de datos / DevOps / QA
**Historia asociada:** US-XXX
**Descripción técnica:** Qué hay que implementar exactamente, con suficiente detalle para que un desarrollador pueda empezar sin preguntar. Incluye endpoint, componente, tabla o servicio afectado según el tipo.
**Criterios de aceptación técnicos:**
- [ ] Condición verificable 1
- [ ] Condición verificable 2
**Dependencias:** TK-XXX o ninguna
**Estimación:** X puntos de historia (Fibonacci: 1, 2, 3, 5, 8)
**Rol sugerido:** Backend dev / Frontend dev / Full-stack / DBA / DevOps

---

**Reglas:**
1. Cada ticket debe poder asignarse a una sola persona.
2. Separa claramente las capas: un ticket de base de datos no mezcla lógica de API; un ticket de frontend no incluye lógica de negocio.
3. Los criterios de aceptación son técnicos y verificables (tests, respuestas HTTP, estados en BD), no funcionales.
4. Respeta el modelo de datos de `LTI-acb/LTI-acb.md` al nombrar tablas, campos y relaciones.
5. Indica explícitamente las dependencias entre tickets para que el equipo pueda paralelizar el trabajo donde sea posible.
6. Al final, incluye un resumen con el total de puntos estimados y el desglose por tipo de ticket.

---

## PROMPT 4 — ESTIMACIÓN DE ESFUERZO (EXTRA)

Actúa como Scrum Master con experiencia facilitando sesiones de Planning Poker.

El documento `LTI-ACB/UserStories-ACB.md` contiene los tickets de trabajo TK-001 a TK-006 generados para la User Story US-009. Ya tienen una estimación inicial asignada. Tu tarea es simular una sesión de Planning Poker para revisar y afinar esas estimaciones.

**Metodología:** Planning Poker con escala Fibonacci en puntos de historia (1, 2, 3, 5, 8, 13, 21). Se usa esta metodología porque los puntos de historia capturan complejidad relativa e incertidumbre mejor que las horas, y la escala Fibonacci fuerza al equipo a tomar decisiones claras evitando falsas precisiones. Se elige Planning Poker (frente a T-shirt sizes o estimación individual) porque genera debate estructurado que aflora riesgos ocultos.

**Formato de entrega para cada ticket:**

**TK-XXX — [Título]**
- Estimación inicial: X pts
- Votos simulados del equipo: Backend dev → X | Frontend dev → X | QA → X | Tech Lead → X
- Resultado del debate: [qué divergencias hubo y por qué]
- Estimación final consensuada: X pts
- Riesgos que podrían inflar el esfuerzo: [1-2 líneas]

**Al final:**
1. Tabla resumen con estimación inicial vs. estimación final por ticket.
2. Total de puntos revisado y comparación con el total inicial.
3. Párrafo de conclusiones: qué tickets concentran más incertidumbre y qué haría el equipo antes de comprometerse con ellos en el sprint (spike, PoC, aclaración de requisitos, etc.).
