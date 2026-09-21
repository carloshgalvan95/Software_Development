# mi-proyecto — LLM Eval & Regression Gate

Consola web (backend + frontend) para **evaluar cambios de prompts/modelos**, guardar runs, comparar scores y aplicar un **gate pass/fail** antes de promover un cambio. Pensado como proyecto de TC5062 y evidencia de portafolio (AI eng / MLOps-lite), no como reemplazo de Promptfoo/DeepEval.

## Stack

- Backend: FastAPI (Python)
- Frontend: React + Vite
- DB: PostgreSQL
- Ops: Docker Compose
- Repo curso: `CLA-TC5062-SN2026/TC5062-A01797969` → carpeta `S01_A2/`

## Estructura

```
mi-proyecto/
├── backend/src/   # API, servicios de eval, persistencia
├── frontend/src/  # UI de runs, diffs y gate
├── docker/        # Compose y Dockerfiles (semanas posteriores)
└── docs/          # Arquitectura, ADRs, capturas
```

Cada carpeta incluye `contenido.txt` describiendo qué irá ahí. El código detallado se construye en semanas 5–8 del curso.

## MVP (alcance consciente)

1. Una tarea de eval + golden set pequeño  
2. Dos o tres métricas (exact match / heuristic / judge simple)  
3. Historial de runs + comparación  
4. Botón/API de gate pass/fail  

## Cómo levantar (más adelante)

Cuando existan Dockerfiles y deps:

```bash
docker compose up --build
```

Hoy esta carpeta solo fija el esqueleto y la decisión de stack (`../stack_decision.md`).
