# Claude Code: Que Ver, Que Implementar, Que Ignorar
**Periodo:** 20-27 marzo 2026 | **Generado:** 27 marzo 2026
**Fuente:** NotebookLM (8 videos con transcripcion completa) + metadata de 10 videos

---

## TL;DR - Las 5 cosas que debes hacer YA

1. **Usa Git como "save points"** antes de cada cambio de agente. Un dev senior perdio su base de datos de produccion por no tener versionado. *(Nate B Jones, 106K views)*
2. **Crea/reescribe tu `claude.md`** como "employee handbook". No lo escribas perfecto - empieza con 3 lineas y anade cada vez que Claude haga algo mal. *(Nate B Jones, 106K views)*
3. **Instala skills especializadas** en vez de usar Claude como generalista: Terraform skill para infra, sales skills para prospeccion. *(DevOps video 3.9K + Zubair 16K views)*
4. **Habilita Agent Teams** con `CLAUDE_AGENT_TEAMS_ENABLED: true` en `settings.local.json`. Permite spawneo de agentes paralelos (frontend, backend, QA). *(Nate Herk, 78K views)*
5. **Decide Code vs Cowork** para cada tarea: Code para ingenieria (terminal, APIs), Cowork para admin (emails, docs). *(Brock Mesarich, 14K views)*

---

## Ranking por Views (con veredicto del analista)

| # | Video | Canal | Views | Veredicto | Que hacer |
|---|-------|-------|------:|-----------|-----------|
| 1 | Build & Sell with Claude Code | Nate Herk (443K) | 285,613 | ALTA | No disponible en NotebookLM (10h). Buscar clips en canal |
| 2 | Claude Code Essentials (12h) | freeCodeCamp (11M) | 135,806 | REFERENCIA | No disponible en NotebookLM (12h). Usar como enciclopedia |
| 3 | Claude Code Wiped 2.5 Years of Data | Nate B Jones (127K) | 106,039 | **IMPRESCINDIBLE** | Ver completo. Las 5 "skills" son oro puro |
| 4 | How to Build Claude Agent Teams | Nate Herk (443K) | 77,862 | **IMPRESCINDIBLE** | Ver seccion de configuracion + T-Mux + Dos/Don'ts |
| 5 | Safer OpenClaw Alternative | Cole Medin (175K) | 69,864 | ALTA | Ver los 4 componentes magicos + heartbeat script |
| 6 | I Gave Claude Code My Whole Genome | Nick Saraev (264K) | 49,180 | BAJA | Entretenimiento. Skip salvo interes en genomica |
| 7 | 3D Websites for $10K | AI Chris Lee | 24,176 | MEDIA | Ver estrategia de outreach + prompt de scroll 3D |
| 8 | $80K Sales Rep with Claude Code | Zubair Trabzada | 16,213 | ALTA | Ver instalacion de sales skills + demo pipeline |
| 9 | Claude Code vs Cowork Explained | Brock Mesarich (56K) | 14,389 | **IMPRESCINDIBLE** | Ver completo. Decision framework esencial |
| 10 | The Only Claude Skill for DevOps | AWS Fundamentals | 3,922 | NICHO | Solo si usas Terraform. Skill de Anton Babenko |

---

## Que Implementar (con datos de NotebookLM)

### 1. GIT SAVE POINTS - Prioridad maxima
**Por que (cita textual del video):**
> *"Say your agent broke something last week and you couldn't get back to the version that worked... you're 3 hours deep in a conversation that's going in circles... This is one of the most common disasters in vibe coding in 2026"* - Nate B Jones

**Que hacer:**
- `git commit` antes de cada tarea compleja del agente
- `git stash` si necesitas experimentar
- Nunca dejes que un agente trabaje sin un commit previo como checkpoint

**Fuente:** Video #3 (106K views), seccion "Skill 1: Save Points"

---

### 2. RULES FILE (`claude.md`) - Quick win inmediato
**Por que (cita textual):**
> *"Every session seems to start from zero... every major AI coding tool now supports a rules file. It's a simple text document that the agent reads at the start of every single session. Think of it as your employee handbook."* - Nate B Jones

**Como construirlo (insight clave del video):**
> *"You don't sit down and write a perfect rules file. You start with almost nothing... then every time your agent does something wrong you add a line to prevent it. Over a few weeks the file becomes a very precise reflection of exactly what your particular project needs."*

**Que hacer:**
1. Crea `CLAUDE.md` con 3 lineas: que es el proyecto, stack, 1 regla
2. Cada vez que Claude haga algo mal, anade una linea
3. Revisa semanalmente: elimina lo que ya no aplica

**Fuente:** Video #3 (106K views), seccion "Skill 3: Standing Orders"

---

### 3. AGENT TEAMS - Alto impacto para proyectos complejos
**Como activar (del video):**
```json
// settings.local.json
{
  "CLAUDE_AGENT_TEAMS_ENABLED": true
}
```

**Patron de uso (de la transcripcion):**
- Asigna roles especificos: frontend dev, backend dev, QA agent
- Usa T-Mux para ver sus "pensamientos" en split-pane
- Regla critica: asigna **file owners** - un agente no debe tocar archivos de otro
- Activa **Plan Approval Mode** para que aprueben planes antes de ejecutar

**Fuente:** Video #4 (78K views), secciones config setup + prompting patterns + dos/don'ts

---

### 4. SKILLS ESPECIALIZADAS - Multiplicador de capacidad
**Ejemplo concreto - Sales Rep ($80K value):**
- Un comando de terminal clona el repo e instala 14 slash commands de ventas
- `/sales-prospect` audita una empresa
- `/sales-contact` mapea stakeholders (Economic Buyer, Champion, Technical Evaluator)
- `/sales-outreach` genera secuencias de email personalizadas
- Genera PDF con pipeline scoring

**Ejemplo concreto - Terraform/DevOps:**
- Skill de Anton Babenko en `~/.claude/skills/`
- Previene "monolithic main files" - fuerza modular architecture
- Integra TFSec, TFLint, Infracost automaticamente
- Slash command: `/terraform`

**Fuente:** Video #8 (16K views) + Video #10 (3.9K views)

---

### 5. SEGUNDO CEREBRO (Obsidian + Claude Code + Claude Agent SDK)
**Los 4 componentes magicos (Cole Medin):**
1. **Memory** (`soul.md`, `memory.md`) - markdown en Obsidian, searchable
2. **Heartbeat** - script Python con Claude Agent SDK, corre cada 30 min
3. **Adapters** - interfaces custom (Slack, email)
4. **Skills** - workflows automatizados

**Que hacer:**
- Usa Claude Agent SDK para crear un script Python que:
  - Corre cada 30 min con cron
  - Revisa tu calendario
  - Drafts emails proactivamente
- Almacena memoria como markdown en Obsidian (SQLite para RAG)

**Fuente:** Video #5 (70K views), seccion "Four Magical Components"

---

## Contradicciones Detectadas por NotebookLM

### Code vs Cowork: destinos opuestos
| Posicion | Video | Argumento |
|----------|-------|-----------|
| "Cowork reemplazara a Code en 3-12 meses" | Brock Mesarich (#9) | Interface superior, mas seguro, sandbox |
| "Code es irreemplazable" | Multiples videos | Terminal access, APIs, agent teams, full machine control |

**Mi veredicto:** Usa ambos. Code para engineering, Cowork para admin. No son competidores.

### OpenClaw: heroe o villano
| Posicion | Video | Argumento |
|----------|-------|-----------|
| "El asistente personal AI del momento" | Varios | Adopcion masiva, community marketplace |
| "Security nightmare" | Cole Medin (#5) | Remote code execution, packages maliciosos |

**Mi veredicto:** NO uses OpenClaw directamente. Replica su logica (memory, heartbeat) en tu propio setup con Claude Code, como muestra Cole Medin.

---

## Warnings Comunes (mencionados en 3+ videos)

### 1. "Blast radius" de tareas grandes
> *"Instead of asking for a massive redesign at once, give the agent focused, medium-sized tasks and validate the results at each step to prevent compounded errors"*

**Regla:** Nunca pidas un cambio que toque mas de 3-5 archivos a la vez.

### 2. Nunca pegues secrets en chat
> *"Never paste secret keys, API tokens, or SSH credentials directly into a chat. Malicious packages can lead to attackers stealing these plain-text keys"*

**Regla:** Usa env vars. Siempre `.env` + `.gitignore`.

### 3. Context decay a partir del mensaje ~30
> *"Around message 30 it just starts ignoring things you've told it three times. It rewrites code it already wrote. It introduces bugs into features that were working. It feels like it forgot everything. Well it did."*

**Regla:** Sesiones cortas. Commit, nueva sesion, continua. Tu `claude.md` persiste el contexto.

### 4. Permisos que frenan el flow
> *"A common pitfall in agent teams is agents stopping constantly to ask for permissions"*

**Regla:** Pre-aprueba tools comunes en `settings.json`. O usa Auto Mode (nuevo, 24 marzo).

---

## Casos de Uso Extraidos (con implementacion concreta)

### Negocio
| Caso | Implementacion | Video |
|------|---------------|-------|
| Sales rep automatizado | Clonar repo skills + `/sales-prospect` | Zubair #8 (16K) |
| Webs 3D premium $10K | Nano Banana 2 + Three.js + Vercel + outreach local | Chris Lee #7 (24K) |
| Morning briefing automatico | Scheduled task con Cowork + connectors | Brock #9 (14K) |

### Ingenieria
| Caso | Implementacion | Video |
|------|---------------|-------|
| Agent teams QA | `CLAUDE_AGENT_TEAMS_ENABLED` + T-Mux | Nate Herk #4 (78K) |
| Terraform infra-as-code | Skill Anton Babenko + TFSec + TFLint | DevOps #10 (3.9K) |
| Safer personal AI | Replicar OpenClaw con Claude Agent SDK + Obsidian | Cole Medin #5 (70K) |

### Productividad
| Caso | Implementacion | Video |
|------|---------------|-------|
| Segundo cerebro proactivo | Heartbeat Python cada 30min + Obsidian memory | Cole Medin #5 (70K) |
| Zapier bridge para apps sin MCP | Zapier MCP server en Claude Code | Brock #9 (14K) |
| Genomic health protocol | genome.txt + ClinVar + PharmGKB pipeline | Nick Saraev #6 (49K) |

---

## Que Ignorar

- **Video del genoma (#6, 49K)** - Fascinante pero no accionable para la mayoria. Privacy trade-offs significativos.
- **Computer Use** - Research preview, inestable. Esperar 2-3 semanas.
- **Voice Mode** - Funciona, pero no cambia workflows. Nice-to-have.
- **freeCodeCamp completo (#2, 136K)** - 12 horas. Usalo como referencia, no como curso lineal.

---

## Releases de la semana

| Version | Fecha | Que te importa |
|---------|-------|---------------|
| v2.1.85 | 26 Mar | MCP env vars, conditional hooks |
| v2.1.84 | 26 Mar | PowerShell preview, startup 30ms mas rapido |
| v2.1.83 | 25 Mar | Plugin startup desde cache |
| v2.1.81 | 21 Mar | Mobile approval workflows |
| v2.1.80 | 20 Mar | 80MB menos de RAM en repos grandes |

**Accion:** `claude update` a v2.1.85.

---

## NotebookLM

Notebook disponible para consultas adicionales: `dbeb5e95-2251-4a8d-a411-eba0c79e82e3`
8 de 10 videos con transcripcion completa indexada. Los 2 videos de 10h+ (freeCodeCamp, Nate Herk masterclass) exceden el limite de NotebookLM.
