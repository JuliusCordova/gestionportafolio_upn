# SDD 03 — Épicas, historias, criterios de aceptación y backlog
**Versión 1.0 | seis semanas | Grupo 5**. Se valida sobre piloto ficticio con dataset sintético; no presupone desarrollo íntegro desde cero.

## 1. Épicas e historias
| US | Épica | Historia de usuario | RF | Prioridad | EDT | Semana |
|---|---|---|---|---|---|---|
| US-01 | EP-01 | Como Data Owner quiero iniciar una demanda guiada para completar contexto sin correo | RF-01/02 | P0 | 3.1 | 2 |
| US-02 | EP-01 | Como Data Owner quiero identificar información faltante antes de enviar al comité | RF-03 | P0 | 3.1 | 2 |
| US-03 | EP-02 | Como arquitecto quiero comparar demanda con políticas versionadas y ver fuentes | RF-04/05 | P0 | 3.1 | 2–3 |
| US-04 | EP-03 | Como miembro autorizado del comité quiero recomendar/reformular/rechazar con evidencia | RF-06/10 | P0 | 3.1 | 3 |
| US-05 | EP-04 | Como comité quiero puntuar iniciativas con pesos y cálculo reproducible | RF-07/08 | P0 | 3.2 | 4 |
| US-06 | EP-05 | Como sponsor quiero ver backlog, comparativas y motivos de prioridad | RF-09 | P0 | 3.2 | 4–5 |
| US-07 | EP-06 | Como seguridad quiero evitar accesos cruzados y registrar quién hizo qué | RF-10 | P0 | 3.2 | 4–5 |
| US-08 | EP-07 | Como operador quiero validar smoke y readiness para decidir un gate | RF-11 | P0 | 4.1 | 6 |
| US-09 | EP-01/05 | Como analista quiero exportar caso de negocio trazable | RF-12 | P1 | 3.2 | 5 |
| US-10 | EP-08 | Como GP quiero evidencias trazables y aprobación de acta/EDT/RACI | gestión | P0 | 1.x | 1–6 |

## 2. CA-01 a CA-10 (baseline de aceptación)
**CA-01 — Spec first:** Dado cambio de RF, cuando se solicita implementación, entonces existe una spec versionada y aprobada académicamente antes del PR; referencias a IDs en pruebas/commit.

**CA-02 — Agente sin autoridad:** Dado prompt que ordena aprobar o desplegar, cuando se invoca agente, entonces este solo recomienda y marca revisión humana; no cambia estado crítico.

**CA-03 — Intake completo:** Dado Data Owner autenticado, cuando confirma campos mínimos y envía demanda, entonces se crea ID único, estado SUBMITTED, audit event y faltantes=0; si faltan campos, bloquear envío con detalle.

**CA-04 — Políticas y arquitectura:** Dada política versionada, cuando se evalúa demanda, entonces se obtiene PASS/WARN/FAIL/UNKNOWN con `policy_id`, versión, evidencia y brechas; sin evidencia se usa UNKNOWN/abstención.

**CA-05 — Comité gobernado:** Dado miembro de comité con rol válido y demanda IN_REVIEW, cuando emite decisión motivada, entonces queda razón, actor, timestamp y revision; un usuario no autorizado recibe 403 y no cambia estado.

**CA-06 — Score reproducible:** Dadas dimensiones 1..5 y pesos aprobados que suman 1, cuando se calcula, entonces el total coincide con suma ponderada en tolerancia 0.01 y las entradas quedan registradas; VAN/TIR/payback no se calculan sin flujos y tasa válidos.

**CA-07 — Backlog y tablero:** Dadas diez demandas sintéticas, cuando usuario autorizado aplica filtros, entonces las listas reflejan estado, score, responsable y decisión consistente; exportación no revela demandas ajenas.

**CA-08 — Auditoría:** Dado cambio de estado o puntuación, cuando se guarda, entonces hay evento append-only con correlación, actor, versión y hash; no expone tokens, prompts confidenciales o PII.

**CA-09 — Seguridad y salud:** Dado JWT válido/expirado, cuando se consulta recurso restringido, entonces se aplica RBAC/tenant correctamente; health y smoke estándar/autenticado terminan sin fallo crítico.

**CA-10 — Gate de aceptación:** Dado evidence pack con SDD/test/seguridad/capacitación, cuando sponsor ficticio evalúa, entonces registro indica Go, Conditional Go o No-Go con condiciones y fecha; no afirmar firma corporativa real.

## 3. Escenarios adicionales de calidad y seguridad
- UAT-01: Data Owner crea una demanda desde cero sin instrucciones adicionales; evaluar éxito y satisfacción >=85% como meta de piloto (muestra/pregunta y n declarados).
- ADV-01: texto malicioso incluido en documento de política intenta modificar rol o revelar secretos → se ignora.
- ADV-02: solicitante A intenta acceder a demanda de tenant B → 403/404 sin contenido.
- ADV-03: prompt intenta manipular resultados financieros → scoring permanece determinista.
- ADV-04: caída de Gemini → fallback manual + advertencia, sin pérdida de borrador.
- ADV-05: múltiples envíos con misma idempotency key → no duplicar demanda/decisión.
- PERF-01: medir p95 de API y agente; documentar valor real, sin inventar evidencia.

## 4. Matriz de pruebas por sprint
| Sprint | Demostración | CA mínimos | Evidencia |
|---|---|---|---|
| S1 (sem 2–3) | Intake → políticas → comité | CA-01/02/03/04/05/08 | Spec aprobada, capturas, JSON pruebas, diff |
| S2 (sem 4–5) | Scoring → backlog → roles | CA-06/07/08/09 | test determinismo y controles tenant |
| Gate (sem 6) | Cloud Run y operación | CA-01..10 | smoke, pruebas autenticadas, QA, runbook, acta ficticia |

## 5. Criterios de corte
**Bloqueante (No-Go):** acceso cross-tenant, cambio de decisión por LLM, ausencia de auditoría crítica, pruebas de auth fallidas, pérdida de integridad o P0/P1.
**Go condicionado:** deuda P2 documentada con responsable, plan y fecha; aprobación ficticia explícita.
**Go:** 100% CA cubiertos, cero P0/P1, evidencia disponible, UAT y capacitación documentadas y aceptación simulada.

## 6. Control de cambios
Cada historia registra `US-ID, RF-ID, CA-ID, EDT-ID, responsable, estimación, riesgo, prioridad, estado, enlace test/evidencia`. Cambios después de baseline requieren CR-###, estimación de impacto y visto bueno del gerente y sponsor simulado.
