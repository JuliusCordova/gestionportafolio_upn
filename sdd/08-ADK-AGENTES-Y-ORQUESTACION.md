# SDD 08 — Implementación agéntica con Google ADK (baseline DataGob)
**Decisión:** agentes programados con Google Agent Development Kit (ADK), sobre GCP y arquitectura interna Hexagonal Slice. No es una simulación de agentes basada únicamente en diagramas.

## Fuentes de implementación comprobadas
- [Orquestador ADK real](https://github.com/JuliusCordova/portafoliodatagob/blob/main/apps/api/src/atlas_datagob/agents/agent.py): clase `google.adk.agents.Agent`, `root_agent=atlas_intake_orchestrator`, tres subagentes especializados en single-turn.
- [Extractor de hechos de negocio](https://github.com/JuliusCordova/portafoliodatagob/blob/main/apps/api/src/atlas_datagob/agents/business_fact_extractor_agent.py): agente aislado con schema Pydantic `TurnBusinessFacts`.
- [Clasificación](https://github.com/JuliusCordova/portafoliodatagob/blob/main/apps/api/src/atlas_datagob/agents/semantic_classification_tool.py) y [tools gobernadas](https://github.com/JuliusCordova/portafoliodatagob/blob/main/apps/api/src/atlas_datagob/agents/governed_specialist_tools.py).
- [Shell determinista anterior](https://github.com/JuliusCordova/portafoliodatagob/blob/main/apps/api/src/atlas_datagob/agents/policy_intake_agent.py): es un flujo determinista, no confundir con subagente ADK.

## Mermaid 1 — Topología ADK real como referencia
```mermaid
flowchart TB
 U["Usuario / Data Owner"] --> UX["Next.js Intake"]
 UX --> API["FastAPI / Intake API"]
 API --> EX["Extractor ADK aislado: atlas_business_fact_extractor"]
 EX --> FACT["TurnBusinessFacts / contexto canónico"]
 API --> ROOT["ADK root_agent: atlas_intake_orchestrator"]
 ROOT --> CTX["Tool: update_business_context"]
 ROOT --> CLF["Tool: classify_project_capabilities"]
 ROOT --> SNAP["Tool: build_governed_business_case_snapshot"]
 ROOT --> DR["ADK: atlas_data_readiness_agent"]
 ROOT --> ARC["ADK: atlas_architecture_validation_agent"]
 ROOT --> POL["ADK: atlas_policy_controls_agent"]
 DR --> DRTOOL["evaluate_data_readiness_governed"]
 ARC --> ARCTOOL["validate_gcp_architecture_governed"]
 POL --> POLTOOL["evaluate_governance_policies_governed"]
 CTX --> S["Estado de sesión / servicios canónicos"]
 CLF --> S
 DRTOOL --> S
 ARCTOOL --> S
 POLTOOL --> S
 SNAP --> S
 ROOT --> RESPONSE["Business case vivo / brechas / preguntas"]
 RESPONSE --> UX
 UX --> CONFIRM{"Confirmación explícita usuario"}
 CONFIRM -->|Sí| GOVAPI["Endpoint gobernado de persistencia"]
 CONFIRM -->|No| UX
```
El **root_agent** no persiste la demanda: genera recomendaciones/snapshot; el endpoint gobernado registra únicamente tras confirmación. El extractor opera en runner separado. Subagentes ejecutan tools y reportan resultados, pero no aprueban ni deciden.

## Mermaid 2 — Relación ADK con hexágonos
```mermaid
flowchart LR
 ADK["ADK Runner / Root / Specialists"] --> AT["Tool inbound adapter"]
 WEB["API HTTP inbound adapter"] --> UC["Application Use Cases"]
 AT --> UC
 UC --> DOM["Domain Rules: approval, scoring, state"]
 UC --> PORT["Outbound Interfaces / Ports"]
 PORT --> FR["Firestore Repository Adapter"]
 PORT --> GC["Governance Catalog Adapter"]
 PORT --> GEM["Gemini / Vertex Adapter"]
 PORT --> AUD["Audit / AgentOps Adapter"]
```
Regla: ADK es orquestación/adaptador, nunca sustituto de reglas de dominio. Agente y tool deben respetar identidad y autorización; el contrato tool→use case debe ser tipado, auditable e idempotente cuando escribe estado.

## Mermaid 3 — Secuencia de intake gobernado
```mermaid
sequenceDiagram
 actor User as Solicitante
 participant Web as Next.js
 participant API as FastAPI
 participant Extract as ADK Extractor
 participant Root as ADK Orchestrator
 participant Spec as Especialistas ADK
 participant Core as Domain/Application
 participant Store as Firestore
 User->>Web: Describe necesidad
 Web->>API: Mensaje y contexto autenticado
 API->>Extract: Extraer hechos TurnBusinessFacts
 Extract-->>API: Hechos confirmados / faltantes
 API->>Root: Contexto del turno
 Root->>Core: Contexto y clasificación
 Root->>Spec: Delegar readiness / arquitectura / políticas
 Spec->>Core: Tool gobernada
 Core-->>Spec: Resultado versionado
 Spec-->>Root: Brechas y evidencias
 Root->>Core: Snapshot business case
 Core-->>Root: ready_to_register y controles
 Root-->>Web: Resumen y preguntas / confirmación
 User->>Web: Confirmar registro
 Web->>API: POST de registro gobernado
 API->>Core: Autorizar y validar transición
 Core->>Store: Guardar demanda y evento
 Store-->>API: Identificador y revisión
 API-->>Web: Confirmación de registro
```

## Mapa de agentes: real vs catálogo conceptual
| Categoría | Componente | Estado de evidencia |
|---|---|---|
| ADK root | atlas_intake_orchestrator | **Definido en código** |
| ADK especialista | atlas_data_readiness_agent | **Definido en código** |
| ADK especialista | atlas_architecture_validation_agent | **Definido en código** |
| ADK especialista | atlas_policy_controls_agent | **Definido en código** |
| ADK auxiliar | atlas_business_fact_extractor | **Definido en código como runner separado** |
| Funcionalidad | clasificación y snapshot | **Tools del orquestador, no subagentes ADK independientes** |
| Catálogo conceptual | Scoring Agent, Committee Pack, MVP Gate, Portfolio Insights | **Diseño de capacidades en docs/specs/02-agent-map.md; no afirmar que sean subagentes ADK del root** |

## Contratos del estado agéntico
Input/output del extractor: `TurnBusinessFacts {business_problem,desired_outcome,business_area,impacted_process,current_situation,stakeholders[],success_metrics[],data_sources[]}`. Especialistas deben emitir resultado estructurado con tipo, versión de catálogo, evidencias y brechas. Los campos exactos de persistencia se validan contra código y contrato de DataGob, no se inventa endpoint.

## Configuración y ejecución
El código fuente usa `ATLAS_INTAKE_MODEL` con valor predeterminado `gemini-2.5-flash` y un `ATLAS_INTAKE_EXTRACTOR_MODEL` opcional. No hardcodear proyecto ni credenciales. Desplegar con IAM, secretos y configuración apropiada de Vertex. La selección de modelo efectivo se verificará en runtime.

## Verificación para Semana 6
- [ ] Runner ADK y tools integrados con API.
- [ ] LLM prompts/versiones registrados.
- [ ] Cuotas, timeouts, protección prompt injection y tests de permisos.
- [ ] Evals de extracción, readiness, arquitectura y políticas, con casos positivos/negativos.
- [ ] Demanda persistida solo tras confirmación.
- [ ] Evidencia de Cloud Run revision, Git SHA y evaluación con datos sintéticos.
