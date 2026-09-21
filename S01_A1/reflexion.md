# Reflexión y análisis crítico — S01-A1 SWEBOK

Curso: TC5062 Análisis, diseño y construcción de software  
Estudiante: Carlos Humberto Galván Perales (A01797969)  
Fuente contrastada: SWEBOK Guide V4.0a (IEEE Computer Society)  
Agente usado en Parte 1: Grok Bot  
Fecha: 2026-09-20

---

## ¿En qué aspectos el agente fue útil para comprender los fundamentos del SWEBOK?

El agente aceleró el mapeo inicial del Guide: en una sola pasada me devolvió la definición de software engineering alineada con SEVOCAB, los cinco objetivos de la Introduction y la distinción entre KAs de práctica (1–15) y foundation KAs (16–18). Eso me evitó empezar a leer las ~370 páginas sin un marco. También fue útil para armar la comparación waterfall / iterativo / Scrum en tabla: forzó criterios explícitos (flexibilidad, visibilidad, cambio de requisitos, documentación) y me dio un vocabulario compartido (predictive vs Agile mindset, document-driven, backlog, sprint) que después pude buscar en el PDF. Por último, al pedirle límites (“qué NO es el Guide”), el agente me recordó algo que suele olvidarse en resúmenes de blog: el body of knowledge está en las referencias, no en el texto corto de cada KA. En mi caso usé Grok Bot sobre el PDF local; el roce concreto fue abrir Table I.1 y comprobar que la lista del chat coincidía palabra por palabra con Requirements → Engineering Foundations antes de dar por buena la respuesta.

## ¿En qué aspectos fue insuficiente o impreciso? ¿Cómo lo detectaste?

Detecté imprecisiones al contrastar con el PDF, no por “sentido común”. Tres casos concretos:

1. **Lista de KAs / versión.** En un borrador previo (prompt sin “V4.0a” ni “Table I.1”), un resumen genérico tiende a mezclar la estructura de V3 (históricamente 15 KAs) o a renombrar áreas. Lo detecté abriendo Table I.1 de la Introduction: V4 lista 18 KAs en un orden fijo (Requirements, Architecture, Design, Construction, Testing, Operations, Maintenance, SCM, Management, Process, Models and Methods, Quality, Security, Professional Practice, Economics, Computing / Mathematical / Engineering Foundations). Cualquier respuesta que omita Software Architecture, Software Engineering Operations o Software Security, o que fusione Architecture con Design, queda descartada frente a Table I.1.

2. **Iterativo ≠ Agile.** El agente, si el prompt es vago, iguala “iterativo” con Scrum. En Chapter 10 (Software Engineering Process), el Guide explica que un life cycle incremental puede seguir siendo predictivo si los requisitos se cierran antes de las otras fases. Scrum aparece además como método ágil concreto (Models and Methods, Agile Methods), no como sinónimo de “cualquier iteración”. Lo detecté buscando “incremental life cycle” y “Scrum” en el texto extraído del PDF.

3. **Documentación en Agile.** Respuestas populares dicen que Agile “elimina documentación”. El SWEBOK lo marca explícitamente como misconception: el Agile Manifesto no dice que no se necesiten documentos; sí se necesitan, pero el énfasis cambia hacia entregas frecuentes de valor y comunicación. También aclara que Agile no es un método en sí. Lo contrasté en la sección 2.5 del Process KA (waterfall document-driven vs Agile mindset) y en 4.4 Agile Methods (Scrum: sprint ≤ 30 días, product backlog, Daily Scrum).

En resumen: el agente es bueno para esqueleto y vocabulario; es insuficiente cuando inventa detalle de página, confunde versiones o repite mitos de internet. La detección fue mecánica: prompt → respuesta → búsqueda en el PDF → aceptar, corregir o descartar.

## ¿Cómo planeas usar el agente durante el resto del curso para maximizar su utilidad sin depender ciegamente de él?

Voy a usarlo en tres modos acotados. Primero, como generador de esquemas y glosarios (qué pregunta, qué tabla, qué términos buscar en SWEBOK, ISO 12207 o el material de la semana). Segundo, como revisor de mis borradores: le pido que señale afirmaciones no citables y yo las verifico en la fuente primaria antes de entregar. Tercero, como apoyo para comparar enfoques (por ejemplo waterfall vs Scrum) siempre con criterios fijos que yo defino, nunca con “explícame Agile” a secas. No lo usaré como autoridad final ni para fabricar evidencia de lecturas que no hice. En trabajo profesional (MLOps / datos) el mismo hábito aplica: el modelo sugiere; el estándar, el log y el experimento confirman. Si hay duda entre dos formulaciones, gana el PDF del SWEBOK o el estándar citado, no la respuesta más fluida del agente.

## Opcional — Contraste adicional y segundo agente

**Contraste directo con SWEBOK v4.0a (discrepancias documentadas):**

| Afirmación típica del agente (si el prompt es flojo) | Qué dice el SWEBOK v4.0a | Dónde |
|---|---|---|
| “El SWEBOK tiene 15 áreas” / lista V3 | Hay **18** KAs en Table I.1 | Introduction, Table I.1 |
| “Agile es un método más rápido porque casi no documentas” | Agile no es un método; documentos sí se necesitan; misconception explícito | Process KA §2.5 |
| “Iterativo = Scrum” | Incremental puede ser predictivo; Scrum es un método ágil con sprint, backlog, Daily Scrum | Process KA (incremental); Models and Methods §4.4 |
| “Waterfall permite volver libremente entre fases” | Definición clásica: orden de fases con poca o nula iteración; document-driven | Process KA §2.5 |

**Segundo agente (prueba rápida):** repetí el Prompt 02 en un chat genérico sin anclar V4. La respuesta mezcló nombres cercanos pero alteró el orden y habló de “15 áreas principales”. La coincidencia útil fue el vocabulario (requirements, testing, maintenance). La diferencia crítica fue la fidelidad a Table I.1. Conclusión: dos agentes pueden sonar convincentes y divergir en el detalle que califica; por eso la rúbrica pide verificación contra la fuente primaria, no solo “usar IA”.
