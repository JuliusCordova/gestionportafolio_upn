# SDD 02 — Contratos API, entidades y eventos v1
**Contratos objetivo de piloto; comprobar compatibilidad con el OpenAPI real del producto técnico antes de codificar.** Prefix propuesto `/api/v1`; no afirmar que rutas idénticas estén ya desplegadas.

## 1. Entidades (Firestore lógico)
- `demands/{id}`: `id`, `tenant_id`, `title`, `description`, `requester_id`, `domain`, `problem`, `north_star`, `benefit_assumptions[]`, `cost_assumptions[]`, `state`, `revision`, `created_at`, `updated_at`.
- `policy_checks/{id}`: `demand_id`, `policy_id`, `policy_version`, `status` PASS/WARN/FAIL/UNKNOWN, `gap`, `evidence_refs[]`, `reviewer_id`.
- `agent_runs/{id}`: `agent_id`, `demand_id`, `model_version`, `prompt_version`, `policy_version`, `start/end`, `tokens`, `cost_estimate`, `status`, `output_hash`.
- `scores/{id}`: `demand_id`, `weights_version`, `dimensions{business,strategic,feasibility,risk}`, `overall`, `financials{npv,irr,payback_years}`, `inputs_hash`, `calculated_at`.
- `decisions/{id}`: `demand_id`, `committee`, `decision`, `reason`, `decision_maker`, `decided_at`, `approved_version`.
- `audit_events/{id}`: `aggregate_id`, `actor_id`, `role`, `action`, `timestamp`, `before_hash`, `after_hash`, `trace_id`, `reason`.
- `policy_catalog/{id}`: `policy_id`, `version`, `title`, `text_ref`, `valid_from`, `classification`, `owner`.
- `release_gates/{id}`: `build_sha`, `check_results`, `decision`, `conditions[]`, `sponsor_role`, `recorded_at`.

`tenant_id` es obligatorio en consultas multi-tenant. PII omitida en evidencia y logs; timestamps UTC ISO-8601; cantidades en PEN con unidad explícita. Revisar índices y límites Firestore al implementar.

## 2. Contratos HTTP propuestos
| ID | Método / endpoint | Rol | Entrada | Salida | CA |
|---|---|---|---|---|---|
| API-01 | POST /demands | Data Owner | CreateDemand | DemandResponse 201 | CA-03 |
| API-02 | POST /agents/intake/suggest | Data Owner | IntakePrompt | DraftSuggestion 200 | CA-02,CA-03 |
| API-03 | POST /demands/{id}/submit | Owner | expected_revision, request_id | DemandResponse | CA-03 |
| API-04 | POST /demands/{id}/policy-check | Arquitecto/Analista | policy_version | PolicyCheckResponse | CA-04 |
| API-05 | POST /demands/{id}/architecture-check | Arquitecto | architecture_version | GapReport | CA-04 |
| API-06 | POST /demands/{id}/score | Comité/Analista | ScoringInputs | ScoreBreakdown | CA-06 |
| API-07 | POST /demands/{id}/committee-decisions | Comité autorizado | decision,reason,expected_revision | DecisionRecord 201 | CA-05 |
| API-08 | GET /demands/backlog | Committee/Exec | filtros,cursor | PagedBacklog | CA-07 |
| API-09 | GET /demands/{id}/timeline | Actor autorizado | id | AuditTimeline | CA-08 |
| API-10 | POST /releases/{id}/gate | Sponsor simulado | decision,evidence_ids,conditions | GateDecision | CA-10 |
| API-11 | GET /health, /ops/readiness | Operador | ninguno | Health/Readiness | CA-09 |
| API-12 | GET /demands/{id}/business-case | Actor autorizado | id | Document metadata/download seguro | CA-07 |

**Errores:** 400 body mal formado; 401 token ausente/inválido; 403 rol/tenant denegado; 404 objeto no visible; 409 conflicto de versión/transición; 422 faltan datos/reglas; 429 cuotas LLM; 503 dependencia no disponible. JSON de error: `{code,message,trace_id,details?}`. Idempotency-Key requerido en operaciones que modifican estado.

## 3. Ejemplo request y response
```json
{
  "title": "Catálogo de clientes unificado",
  "domain": "clientes",
  "problem": "Solicitudes sin dueño ni trazabilidad",
  "north_star": {"metric": "dias_hasta_decision", "baseline": 45, "target": 20, "unit": "dias"},
  "benefit_assumptions": [{"amount": 50000, "currency": "PEN", "period": "year", "estimated": true}],
  "cost_assumptions": [{"amount": 15000, "currency": "PEN", "period": "one_time", "estimated": true}]
}
```
```json
{
  "id": "dmd-demo-001",
  "state": "DRAFT",
  "revision": 1,
  "created_at": "2026-10-24T00:00:00Z",
  "audit_event_id": "evt-demo-001"
}
```

## 4. Score determinista
Dimensiones calificadas 1..5, pesos positivos que sumen exactamente 1.0, `overall = Σ peso_i × score_i`. Los pesos son versionados y aprobados por comité simulado antes de probar. Los agentes pueden explicar el resultado pero **no modificar la fórmula ni la decisión**. VAN/TIR/payback requieren flujos por período y tasa; si faltan datos responder `not_calculable`, nunca cero ficticio. Año 0/1/2 del caso S01 no demuestra por sí mismo el payback asumido de 1.5 años.

## 5. Eventos / audit
`DemandCreated, IntakeSuggested, DemandSubmitted, PolicyChecked, ArchitectureReviewed, ScoreCalculated, CommitteeDecisionRecorded, ReleaseGateDecided`.
Envelope: `event_id, event_type, schema_version, aggregate_id, actor_id, timestamp, correlation_id, payload_hash`. Se diseñan como registros append-only, sin requerir Pub/Sub en el MVP; una adopción event-driven sería cambio de alcance.
