# Log de prompts y revisión crítica — S01-A2

**Agente:** Grok Bot  
**Objetivo:** Configurar entorno / stack del proyecto del curso sin ejecutar scaffolding de código de aplicación aún.

## Iteración 1 — Prompt amplio (rechazado / refinado)

**Prompt (borrador):**  
“Recomiéndame un stack para una app web con backend y frontend para la maestría.”

**Problema detectado:** Demasiado genérico; empuja a stacks de moda (p. ej. Next fullstack) sin mis restricciones laborales ni el catálogo del curso.

**Qué cambié:** Añadir SO (Windows), lenguajes (Python fuerte), roles (DE/Sr DE), catálogo FastAPI/Flask/Express/Nest + React/Vite/Vue/Next, y meta de portafolio.

## Iteración 2 — Prompt con restricciones

**Prompt (efectivo):**  
Describe proyecto portafolio AI eng (no O&G), opciones del curso, pide recomendación justificada por criterio, y pide contrastar con vacantes reales vía Carrera.

**Resultado:** Candidatos de producto (eval gate, retrieval workbench, data contract mapper, etc.) + stack Python/FastAPI dominante.

**Revisión:** Pedí ranking a Guild Navigator y luego investigación de mercado OSS vs pago antes de elegir.

## Iteración 3 — Verificación de mercado

**Prompt / tarea:**  
Investigar si LLM Eval Gate ya está cubierto por OSS gratis (Promptfoo, DeepEval, …) vs herramientas de pago.

**Resultado:** Categoría saturada en OSS; elegí igual el Eval Gate por empleabilidad, documentando la diferenciación (UI + Postgres + gate narrative).

**Revisión:** No fingí “innovación de categoría”; el valor es portafolio + ingeniería del curso.

## Iteración 4 — Frontend

**Prompt / tarea:**  
Rankear React+Vite vs Next vs Vue solo por vacantes.

**Resultado:** React+Vite #1; frontend casi no pesa en JDs DE, pero React gana el glance.

**Comandos del agente:** En esta fase no corrí `npm create` ni `pip install` de la app; solo se crearán carpetas y docs. Cualquier comando de scaffold futuro se revisará antes de ejecutar (no `rm -rf` ni push `--force`).

## Commit message propuesto (Conventional Commits)

```
chore(s01-a2): add env setup docs and mi-proyecto scaffold

Document FastAPI + React/Vite + Postgres + Docker stack for the
LLM Eval & Regression Gate portfolio project, agent tool comparison,
and empty folder layout with contenido.txt placeholders.
```
