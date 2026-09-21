# Decisión de stack tecnológico — S01-A2

**Estudiante:** Carlos Humberto Galván Perales  
**Matrícula:** A01797969  
**Curso:** TC5062 Análisis, diseño y construcción de software  
**Proyecto:** LLM Eval & Regression Gate (consola web para evaluar prompts/modelos, comparar runs y aplicar un gate pass/fail antes de “promover” un cambio)  
**Agente usado:** Grok Bot  
**Fecha:** 2026-09-21  
**SO de desarrollo:** Windows 10/11 (DESKTOP-GJC2LOH) + Git + Docker Desktop (previsto)

## Contexto del producto

Aplicación web con backend y frontend para portafolio y búsqueda laboral (Data Engineer / Sr DE / Platform / AI-adjacent). El núcleo no es un chatbot: es **evaluación reproducible + regresión + gate**, consciente de que existen herramientas OSS maduras (Promptfoo, DeepEval, Ragas). La diferenciación buscada es una UI de ops + historial en Postgres + narrativa “CI for LLM changes”, no reinventar todo el CLI.

## Stack elegido

| Capa | Tecnología |
|---|---|
| Backend | Python 3.11+ + **FastAPI** |
| Frontend | **React + Vite** (TypeScript opcional en semanas 7–8) |
| Base de datos | **PostgreSQL** |
| Contenedores | **Docker** + Docker Compose |
| Control de versiones | **Git** + **GitHub** + Conventional Commits |
| CI (previsto) | GitHub Actions (gate de eval en PR; detalle en semanas posteriores) |

## Restricciones reales usadas en la recomendación

- Lenguajes fuertes hoy: Python, SQL; JS suficiente para UI, no es el eje del CV.
- Vacantes a las que aplico: DE / Sr DE / Platform (Python, APIs, Docker, CI/CD); frontend rara vez es must.
- Curso: scaffolding backend semanas 5–6, frontend 7–8; esta entrega solo define stack y carpetas.
- Evitar tema oil & gas y datos sensibles.
- No elegir Node-only / Angular / Vue como stack principal (poco overlap con mi cola de vacantes).

## Alternativas consideradas (catálogo del curso)

**Backend:** FastAPI vs Flask vs Express/NestJS  
**Frontend:** React+Vite vs Vue 3+Vite vs Next.js  

## Justificación con ≥3 criterios técnicos

### 1) Alineación con el dominio y el ecosistema (ecosistema / empleabilidad)

- **FastAPI** vive en el ecosistema Python que ya uso (MLOps, datos, RAG). Documentación OpenAPI automática ayuda a exponer runs, scores y gates como API clara.
- **Flask** también es Python, pero para una API de producto con validación (Pydantic), async y docs nativas, FastAPI reduce glue code.
- **Express/NestJS** obligan a un backend Node: útil en mercado full-stack, pero **no es lo que piden** la mayoría de vacantes a las que aplico; duplicaría curva sin ROI en entrevistas DE/Platform.
- **React** es el UI framework con más reconocimiento en glances de GitHub frente a Vue en mi segmento. **Vite** mantiene el frontend como SPA ligera contra la API; **Next.js** añade SSR/App Router que no necesito para un dashboard interno (overhead sin ganancia en mis JDs).

### 2) Facilidad de uso y velocidad de entrega en el curso

- Separar **FastAPI** y **React+Vite** encaja con el plan del curso (backend y frontend en bloques distintos) y con la rúbrica de estructura `backend/` + `frontend/`.
- Postgres + Compose permite levantar API + DB + UI con un flujo reproducible; evita “solo corre en mi laptop”.
- Un monolito Next fullstack mezclaría concerns y complicaría el scaffolding por semanas.

### 3) Rendimiento y forma de trabajo de la API

- FastAPI (Starlette/Uvicorn) es adecuado para I/O bound: disparar evals, guardar resultados, consultar historial.
- React+Vite entrega UI rápida en desarrollo (HMR); el cuello de botella real serán llamadas a modelos/judges, no el render del dashboard.
- Postgres soporta consultas de historial de runs, diffs y auditoría del gate mejor que un JSON suelto en disco cuando el proyecto crezca.

### 4) Criterio extra: diferenciación consciente vs OSS (no es “porque no conocía Promptfoo”)

Promptfoo/DeepEval ya cubren CLI + CI. Este proyecto **no pretende reemplazarlos**. El stack web (FastAPI + React + Postgres) se elige para practicar ingeniería de software del curso y mostrar un **producto opinionado** (comparar runs, promote/block, historial), reutilizando ideas de esas herramientas.

## Qué queda fuera del MVP (on purpose)

- Cluster Kafka, lakehouse, microfrontends.
- SSR/SEO (Next) y app móvil.
- Reimplementar red-teaming completo tipo Promptfoo enterprise.

## Decisión final

**FastAPI + React (Vite) + PostgreSQL + Docker Compose + Git/GitHub.**  
Justificación resumida: ecosistema Python/empleabilidad DE, entrega por capas alineada al curso, API performante para I/O de evals, UI React reconocible sin peso de Next/Vue, y Compose para reproducibilidad.
