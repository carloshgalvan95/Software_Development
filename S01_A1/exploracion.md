# Exploración del SWEBOK v4.0a con agente de IA

**Estudiante:** Carlos Humberto Galván Perales  
**Matrícula:** A01797969  
**Curso:** TC5062 Análisis, diseño y construcción de software  
**Actividad:** S01-A1 — Exploración del SWEBOK con Agente de IA  
**Fecha:** 2026-09-20

## Configuración documentada

| Campo | Detalle |
|---|---|
| Fuente primaria | SWEBOK Guide V4.0a (IEEE Computer Society) |
| Agente usado | Grok Bot |
| Fecha de la sesión | 2026-09-20 |
| Idioma de prompts | Español, con términos técnicos en inglés cuando el SWEBOK los usa así |
| Objetivo de la sesión | Familiarizarme con el SWEBOK y practicar prompts verificables contra la fuente primaria |

### Pasos seguidos

1. Descargué y abrí el PDF oficial SWEBOK v4.0a (recurso de la actividad).
2. Revisé la Introduction to the Guide (objetivos del SWEBOK, Table I.1 con las 18 KAs) y el Chapter 10 Software Engineering Process (life cycles, waterfall, Agile).
3. Redacté prompts propios (no copié el texto de las instrucciones). Empecé con borradores generales y los refiní para pedir estructura, versión exacta y criterios de comparación.
4. Ejecuté cada prompt en Grok Bot y guardé la respuesta tal cual.
5. Contrasté cada respuesta con el PDF (páginas de introducción y KA 10 / KA 11) y anoté discrepancias en `reflexion.md`.

### Problemas y soluciones

| Problema | Cómo lo resolví |
|---|---|
| El primer borrador del Prompt 02 pedía "áreas del SWEBOK" sin versión. El agente podría mezclar V3 (15 KAs) con V4. | Refiné el prompt para exigir "SWEBOK Guide V4.0a" y "las 18 knowledge areas de Table I.1". |
| En un ensayo mental del Prompt 03, un agente genérico trataba Agile/Scrum como sinónimos del modelo iterativo. | Pedí tabla con criterios fijos y pedí distinguir waterfall, iterative/incremental y Scrum según el SWEBOK. |
| El PDF es largo (~370+ páginas). Buscar "Scrum" a ojo es lento. | Usé búsqueda de texto en el PDF (`pdftotext` + búsqueda) para localizar Chapter 10 y la sección 4.4 Agile Methods en Models and Methods. |

---

# Prompts y respuestas

## Prompt 01

Actúa como tutor de ingeniería de software. Explica qué es el SWEBOK Guide V4.0a de la IEEE Computer Society y cuál es su propósito dentro de la ingeniería de software profesional.

Responde en español. Incluye:
1) definición breve de SWEBOK,
2) cómo se relaciona con la definición de software engineering (ISO/IEC/IEEE),
3) los cinco objetivos del Guide según su introducción,
4) qué NO intenta ser el Guide (límites),
5) un cambio relevante de V4 respecto a V3 (Agile/DevOps u otro).

Si algo no está en el Guide, dilo explícitamente en lugar de inventarlo. Cita secciones o tablas cuando puedas (por ejemplo Table I.1, Introduction).

### Respuesta

El SWEBOK Guide V4.0a (Software Engineering Body of Knowledge) es una guía publicada bajo los auspicios del Professional and Educational Activities Board de la IEEE Computer Society. Organiza el conocimiento generalmente aceptado de la ingeniería de software en 18 knowledge areas (KAs), más apéndices con especificaciones de las KAs, estándares anotados y referencias.

Según la Introduction to the Guide, software engineering se define (citando ISO/IEC/IEEE SEVOCAB) como la aplicación de un enfoque sistemático, disciplinado y cuantificable al desarrollo, operación y mantenimiento del software; es decir, la aplicación de ingeniería al software. El SWEBOK no inventa esa definición: la usa como punto de partida y luego caracteriza el contenido de la disciplina.

Los cinco objetivos del Guide, según la introducción, son:

1. Promover una visión consistente de la ingeniería de software a nivel mundial.
2. Especificar el alcance de la disciplina y aclarar su lugar respecto a disciplinas relacionadas (computer science, project management, etc.).
3. Caracterizar los contenidos de la ingeniería de software.
4. Dar acceso tópico al body of knowledge (el conocimiento está en el material de referencia; el Guide es el mapa).
5. Servir de base para desarrollo curricular y para materiales de certificación y licenciamiento.

Límites importantes: el Guide no pretende ser exhaustivo en citas; no intenta caracterizar a fondo las disciplinas relacionadas (solo las reconoce, Table I.2); y el criterio de inclusión es conocimiento "generally accepted", no conocimiento avanzado de investigación ni conocimiento hiper-especializado de un dominio.

Un cambio relevante en V4 frente a V3 es la incorporación de prácticas Agile y DevOps de forma transversal en casi todas las KAs, porque esos modelos se consolidaron después de V3. V4 también presenta 18 KAs (Table I.1), con capítulos 1–15 como KAs de ingeniería de software y 16–18 como foundation KAs (Computing, Mathematical, Engineering Foundations).

---

## Prompt 02

Usando exclusivamente el SWEBOK Guide V4.0a, lista las 18 knowledge areas de Table I.1 en el orden oficial del Guide.

Para cada KA escribe exactamente una oración que describa su foco, basada en el propósito de esa área (no inventes KAs de versiones anteriores).

Formato de salida:
- número. Nombre oficial: una oración.

Al final, indica en una línea si Chapters 1–15 vs 16–18 tienen roles distintos según la Introduction.

### Respuesta

1. Software Requirements: Cubre la elicitación, análisis, especificación, validación y gestión de requisitos a lo largo del ciclo de vida.
2. Software Architecture: Trata la estructura de alto nivel del sistema, decisiones arquitectónicas, vistas y propiedades que deben preservarse.
3. Software Design: Aborda el diseño de la solución de software (estructura interna, interfaces, calidad de diseño) a partir de requisitos y arquitectura.
4. Software Construction: Se centra en la creación detallada del software ejecutable (código, integración de componentes, prácticas de construcción).
5. Software Testing: Agrupa técnicas, niveles y procesos para verificar y validar que el software cumple lo esperado y revela defectos.
6. Software Engineering Operations: Incluye la operación del software en producción, su soporte operativo y prácticas alineadas con entrega continua/DevOps.
7. Software Maintenance: Cubre la modificación del software después de la entrega para corregir, adaptar o mejorar el producto.
8. Software Configuration Management: Gestiona identificación, control de cambios, status accounting, auditoría y liberación de configuraciones.
9. Software Engineering Management: Aplica planificación, medición, seguimiento y control a proyectos y trabajo de ingeniería de software.
10. Software Engineering Process: Define y adapta procesos y life cycle models para desarrollar, operar y mejorar el software de forma sistemática.
11. Software Engineering Models and Methods: Describe modelos y métodos (incluyendo ágiles y plan-based) usados para representar y construir software.
12. Software Quality: Trata atributos de calidad, procesos de aseguramiento y evaluación de la calidad del producto y del proceso.
13. Software Security: Incorpora prácticas y preocupaciones de seguridad a lo largo del ciclo de vida del software.
14. Software Engineering Professional Practice: Aborda ética, profesionalismo, comunicación y responsabilidades del ingeniero de software.
15. Software Engineering Economics: Aplica análisis económico y de valor a decisiones de ingeniería de software.
16. Computing Foundations: Proporciona fundamentos de cómputo necesarios para practicar ingeniería de software.
17. Mathematical Foundations: Resume bases matemáticas relevantes para modelar y razonar sobre software.
18. Engineering Foundations: Ofrece fundamentos de ingeniería (medición, incertidumbre, trade-offs) aplicables al software.

Según la Introduction: Chapters 1–15 se consideran las software engineering KAs; Chapters 16–18 son foundation KAs.

---

## Prompt 03

Compara el modelo en cascada (waterfall), un modelo iterativo/incremental y Scrum, usando el SWEBOK Guide V4.0a (sobre todo Software Engineering Process KA y la sección de Agile Methods en Models and Methods).

Entrega una tabla Markdown con filas = los tres enfoques y columnas = estos criterios exactos:
- flexibilidad
- visibilidad del progreso
- adecuación para requisitos cambiantes
- documentación requerida

Después de la tabla, añade 4–6 oraciones de matices: (a) waterfall como life cycle predictivo y document-driven; (b) que iterative/incremental no es lo mismo que “ser Agile”; (c) que Agile no es un método único y Scrum se describe más orientado a project management; (d) un misconception que el propio SWEBOK menciona sobre Agile vs waterfall.

### Respuesta

| Enfoque | Flexibilidad | Visibilidad del progreso | Adecuación para requisitos cambiantes | Documentación requerida |
|---|---|---|---|---|
| Waterfall | Baja: fases estrictas; en la forma clásica una fase no arranca hasta terminar la anterior | Progreso visible por hitos de fase (requisitos, diseño, código, pruebas), pero el software usable suele llegar tarde | Baja: asume requisitos relativamente estables; poca o nula iteración entre fases | Alta: el SWEBOK lo describe como document-driven; énfasis en especificaciones antes de construir |
| Iterativo / incremental | Media: el producto crece por ciclos o incrementos; las fases pueden solaparse | Media-alta: hay versiones parciales o incrementos que se pueden demostrar antes del final | Media: permite refinar con feedback, pero un incremental aún puede ser predictivo si los requisitos se cierran al inicio | Variable: más que waterfall puro en cuanto a entregas parciales, pero no implica “cero documentos” |
| Scrum | Alta dentro del marco ágil: sprints cortos, backlog priorizado y ajuste continuo del alcance del incremento | Alta: cada sprint (≤ 30 días según el Guide) entrega una versión trabajada/probada; Daily Scrum alinea el trabajo diario | Alta: el product owner reordena el backlog; el mindset ágil acepta cambiar requisitos si cambian las necesidades del usuario | Moderada y orientada a valor: el Manifesto no elimina documentos; el SWEBOK insiste en que sí se necesitan, pero el peso recae menos en documentación exhaustiva upfront |

Matices según el Guide:

El waterfall es un ejemplo claro de life cycle predictivo: actividades (concepto, requisitos, diseño, implementación, prueba, instalación) en ese orden, con poca o nula iteración. Fue útil porque sistematizó el desarrollo, pero es rígido frente al cambio.

Un modelo iterativo/incremental no es automáticamente Agile. El SWEBOK señala que un incremental puede seguir siendo predictivo si los requisitos se definen y cierran antes de las demás fases. La iteración mejora feedback; Agile añade valores/principios (apertura al cambio, entregas frecuentes de valor, comunicación).

Agile no es “un método”. Es un mindset con valores y prácticas; métodos concretos incluyen XP, Scrum, FDD, Lean, etc. Scrum se presenta como más friendly a project management: roles (Scrum master, product owner), sprints, backlog y Daily Scrum.

Un misconception que el propio SWEBOK menciona: creer que Agile es “más rápido” que waterfall porque “no hay que producir documentos”. El Guide aclara que Agile no es un método en sí, que los documentos sí se necesitan, y que la discusión waterfall vs Agile debe entenderse en perspectiva histórica y con evidencia empírica, no solo con opiniones.
