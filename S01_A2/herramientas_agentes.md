# Comparativa de herramientas de agentes de IA — S01-A2

**Estudiante:** Carlos Humberto Galván Perales (A01797969)  
**Curso:** TC5062  
**Fecha:** 2026-09-21  
**Contexto:** Herramientas evaluadas al configurar el entorno y decidir el stack del proyecto *LLM Eval & Regression Gate*.

## Tabla comparativa

| Criterio | Grok Bot (usado aquí) | GitHub Copilot (IDE) | ChatGPT (web/app) | Cursor Agent / Composer | Claude (web/api) |
|---|---|---|---|---|---|
| Dónde corre | Chat de asistente de escritorio + tools | Dentro del editor (completions/chat) | Navegador / app | IDE Cursor (edits multiarchivo) | Navegador / API |
| Fortaleza | Orquestar investigación, memoria de curso, handoffs con otros bots (Carrera), docs de entrega | Autocompletado y parches locales rápidos | Brainstorm y explicación general | Refactors y scaffolds grandes en el repo | Razonamiento largo y prosa técnica |
| Debilidad | No sustituye revisar comandos ni el PDF/fuente primaria | Contexto acotado al archivo/repo abierto; menos “proyecto de curso” | Fácil copiar sin verificar; no ve tu filesystem a menos que lo pegues | Puede generar diffs amplios difíciles de auditar | Menos integración nativa con tu máquina local salvo API/IDE |
| Costo típico | Según plan del producto | Suscripción | Free / Plus / Team | Suscripción Cursor | Free / Pro / API |
| Mejor para esta actividad | Definir stack, documentar decisiones, contrastar mercado OSS | Cuando ya hay carpeta/código y quieres snippets | Primera lluvia de ideas | Scaffold detallado (semanas 5–8) | Redactar justificación larga o revisar textos |
| Riesgo si se usa mal | Entregar sin contrastar (p. ej. inventar KAs o stacks “de moda”) | Aceptar código sin leer | Prompt genérico → respuesta cookiecutter | Ejecutar comandos destructivos sin revisión | Texto fluido pero desconectado de tus vacantes reales |

## Evidencia de uso en esta actividad

- Usé **Grok Bot** para acotar el producto (eval gate vs otras ideas), pedir ranking a Guild Navigator (vacantes) e investigar OSS (Promptfoo, DeepEval, etc.).
- Refiné prompts: de “recomienda un stack” a restricciones reales (Windows, Python, DE roles, sin O&G, curso TC5062).
- **No ejecuté** scaffold de dependencias aún: esta entrega solo define stack y carpetas; el código llega en semanas 5–8.

## Conclusión (personal)

Para **decidir y documentar** el entorno (S01-A2), Grok Bot me sirvió más que un completador de IDE: necesitaba cruzar perfil laboral, mercado OSS y rúbrica del curso.  
Para **implementar** backend/frontend después, combinaré Cursor/Copilot en el repo con la misma regla: el agente propone, yo reviso comandos y diffs, y la fuente de verdad del producto son tests + gate de eval, no la fluidez de la respuesta.  
Ninguna herramienta reemplaza contrastar alternativas (FastAPI vs Node, React vs Next) con criterios técnicos escritos; eso es lo que califica la rúbrica y lo que puedo defender en entrevista.
