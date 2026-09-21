# Log de prompts y revisión crítica — S01-A2

**Estudiante:** Carlos Humberto Galván Perales (A01797969)  
**Agente:** Grok Bot  
**Objetivo:** Definir stack y entorno del proyecto *LLM Eval & Regression Gate* sin ejecutar scaffold de aplicación aún.  
**Fecha:** 2026-09-21

## Convención de prompting usada

En cada iteración refinada apliqué el mnemonic **RCTCF-Q** (variante práctica de CO-STAR / RISEN):

| Letra | Significado | Qué forcé en el prompt |
|---|---|---|
| **R** | Role | Quién es el agente (tutor SE / analista de mercado / ranker Carrera) |
| **C** | Context | Curso TC5062, Windows, perfil DE/AI-eng, sin O&G |
| **T** | Task | Acción concreta y verificable |
| **C** | Constraints | Catálogo permitido, no inventar, no ejecutar comandos destructivos |
| **F** | Format | Tabla, ranking 1→n, bullets, secciones fijas |
| **Q** | Quality bar | Criterios de aceptación / qué rechazar |

Reglas anti-falla que repetí: (1) no recomendar fuera del catálogo del curso, (2) contrastar con vacantes reales o con OSS, (3) si falta evidencia, decirlo en lugar de inventar.

---

## Iteración 1 — Prompt débil (rechazado)

### Prompt exacto (anti-patrón)

```text
Recomiéndame un stack para una app web con backend y frontend para la maestría.
```

### Por qué falló

- Sin **Role** ni **Constraints**: el agente puede inventar Next fullstack o Node-only.
- Sin **Format**: respuesta narrativa difícil de comparar.
- Sin **Quality bar**: no pide criterios técnicos (≥3) ni alternativas del catálogo.

### Qué aprendí

Un prompt de una línea optimiza fluidez, no decisión defendible en rúbrica ni en entrevista.

---

## Iteración 2 — Definición de producto + stack (RCTCF-Q)

### Prompt exacto

```text
R — Role:
Actúa como tutor de software engineering y coach de portafolio para un MSc Applied AI.
No eres un evangelista de frameworks: priorizas decisiones defendibles.

C — Context:
- Curso: TC5062 (Análisis, diseño y construcción de software), actividad S01-A2.
- SO: Windows 10/11. Lenguajes fuertes: Python y SQL; JS suficiente para UI.
- Meta: app web (backend + frontend) útil como evidencia de portafolio en búsqueda laboral
  (Data Engineer / Sr DE / Platform / AI-adjacent). Pocos proyectos públicos de AI engineering.
- Restricciones de dominio: alejar el tema de oil & gas; sin datos sensibles.
- Scaffolding detallado de código NO es de esta semana (backend sem 5–6, frontend 7–8).
- Catálogo permitido del curso:
  Backend: FastAPI+Python | Flask+Python | Express/NestJS+Node
  Frontend: React+Vite | Vue3+Vite | Next.js
  Más: Git, GitHub, Conventional Commits, Docker (previsto).

T — Task:
1) Propón 3–5 ideas de producto web NO cookiecutter (evitar CRUD/todo/chatbot genérico).
2) Para la idea top, recomienda un stack del catálogo.
3) Justifica con ≥3 criterios técnicos comparando alternativas del catálogo
   (p. ej. ecosistema/empleabilidad, facilidad de entrega en el curso, rendimiento I/O o DX).

C — Constraints:
- No salgas del catálogo de backend/frontend.
- No asumas que “innovador” = reinventar categorías ya cubiertas por OSS maduro;
  si la categoría está saturada, dilo y propone diferenciación honesta.
- No ejecutes instalaciones ni generes código de app; solo decisión y estructura documental.
- Si necesitas datos de vacantes reales, pide handoff a Carrera / Guild Navigator en lugar de inventar JDs.

F — Format:
## Ideas (tabla: nombre | problema | por qué no es cookiecutter | riesgo de scope)
## Stack recomendado (tabla capa → tecnología)
## Justificación (≥3 criterios; en cada uno: elegido vs alternativa rechazada)
## Diferenciación vs OSS conocidos (si aplica)
## Preguntas abiertas (máx 3)

Q — Quality bar:
Rechaza tu propia respuesta si: (a) falta un criterio técnico, (b) eliges Node sin justificar
contra mis vacantes DE, o (c) propones un chatbot PDF sin métricas/eval.
```

### Resultado

Ideas tipo eval gate / retrieval workbench / data contract mapper; stack Python/FastAPI dominante; necesidad de rankear por Carrera y de checar mercado OSS.

### Revisión crítica

El prompt ya era usable, pero aún mezclaba “elegir producto” y “elegir frontend”. Lo separé en iteraciones 3–4 para no sesgar.

---

## Iteración 3 — Market check OSS vs pago (RCTCF-Q + “verify before claim”)

### Prompt exacto

```text
R — Role:
Eres analista de mercado de tooling AI eng. Tu trabajo es matar ideas con evidencia,
no vender el proyecto.

C — Context:
Candidato fuerte de portafolio: "LLM Eval & Regression Gate"
(golden set, scores, regresión, gate pass/fail antes de promover un cambio de prompt/modelo).
Sé que existen Promptfoo, DeepEval, Ragas, Langfuse, Phoenix, LangSmith, Braintrust, etc.,
pero necesito un mapa actualizado OSS gratis/self-host vs de pago.

T — Task:
Para ESTE caso de uso (eval + regression + CI gate), investiga y responde:
1) Herramientas OSS/self-host que ya lo cubren bien.
2) Productos de pago/hosted equivalentes.
3) Qué hueco real quedaría para un proyecto FastAPI+React de estudiante
   (si el hueco es débil, dilo explícitamente).
Repite el mismo análisis breve para: Retrieval Quality Workbench y AI Data Contract Mapper
(solo para comparar white space relativo).

C — Constraints:
- Cita nombres de herramientas concretas; no uses “hay muchas”.
- Distingue licencia/self-host vs SaaS de pago.
- Prohibido concluir “es innovador” si Promptfoo/DeepEval ya resuelven el 80% del CLI/CI.
- Si una fuente es blog comparativo, márcala como secundaria vs docs/repos oficiales.

F — Format:
### Caso Eval Gate
| Tool | OSS/self-host o pago | Qué cubre | Qué no cubre |
### White space relativo (Eval vs Retrieval vs Data Contract) — ranking 1→3
### Implicación para portafolio (empleabilidad vs novelty) — 5–8 oraciones

Q — Quality bar:
Si no encuentras evidencia de saturación OSS en Eval Gate, di “evidencia insuficiente”
en lugar de afirmar white space.
```

### Resultado

Eval Gate saturado en OSS; elegí igual el proyecto por empleabilidad; diferenciación documentada (UI + Postgres + narrativa CI), no fingiendo blue ocean.

### Revisión crítica

Sin este prompt, la rúbrica de “uso responsable” quedaría en “el agente dijo que era buena idea”. El market check es la verificación.

---

## Iteración 4 — Ranking de frontend solo por vacantes (RCTCF-Q)

### Prompt exacto (handoff a Guild Navigator / Carrera)

```text
R — Role:
Ranker de stack para Carrera real. Ignora preferencias estéticas de frameworks.

C — Context:
Proyecto ya cerrado: LLM Eval & Regression Gate.
Backend cerrado: FastAPI + Postgres + Docker.
Perfil de aplicación: mid+senior Data Engineer / Sr DE / Platform / AI-adjacent
(Python, SQL, Spark/Databricks, Airflow, AWS, REST, CI/CD). Frontend rara vez es must.

T — Task:
Rankea SOLO por relevancia en vacantes a las que aplico (no por hype):
1) React + Vite
2) Next.js
3) Vue 3 + Vite
Entrega orden 1→3, 1–2 bullets por opción, pick recomendado para un dashboard ligero
contra FastAPI, y una nota explícita si “frontend casi no pesa” en mi cola.

C — Constraints:
- Prohibido rankear por “más moderno” o “mejor DX” sin anclar a JDs/ATS.
- Si React y Next empatan en señal, prefiere el de menor overhead para SPA de ops.
- No recomiendes Angular ni jQuery.

F — Format:
## Summary (orden + pick)
## Evidence (patrones de JD)
## Ranking 1→3
## Risks
## Next action

Q — Quality bar:
Si no hay menciones de UI en la mayoría de JDs, dilo en Summary; no inventes
que Vue “está creciendo” en mi segmento sin evidencia.
```

### Resultado

React + Vite #1; Next #2; Vue #3; frontend casi no filtra ATS, pero React gana el glance del GitHub.

### Revisión de comandos

En S01-A2 **no** ejecuté `npm create vite`, `pip install` ni `docker compose up` de la app. Solo docs + carpetas con `contenido.txt`. Cualquier scaffold futuro se revisa antes de correr (nada de `rm -rf`, `git push --force`, ni pegar secrets en el prompt).

---

## Checklist de calidad de prompts (para el resto del curso)

- [ ] ¿Hay Role explícito?
- [ ] ¿Context incluye restricciones reales (SO, catálogo, deadline, datos sensibles)?
- [ ] ¿Task es observable (tabla, ranking, archivo)?
- [ ] ¿Constraints dicen qué NO hacer?
- [ ] ¿Format evita pared de texto?
- [ ] ¿Quality bar permite rechazar la respuesta?
- [ ] ¿Pedí verificación contra fuente (OSS, JD, SWEBOK) cuando aplica?

## Commit (Conventional Commits)

```
chore(s01-a2): add env setup docs and mi-proyecto scaffold

Document FastAPI + React/Vite + Postgres + Docker stack for the
LLM Eval & Regression Gate portfolio project, agent tool comparison,
and empty folder layout with contenido.txt placeholders.
```
