# SDD 04 — Validación, evaluación de agentes, operación y salida
**V1 | documento de diseño de pruebas y transición.** No implica que los tests se hayan ejecutado todavía.

## 1. Pirámide de pruebas
1. Unitarias: máquina de estados, puntuación, pesos, cálculo financiero, schema, validación de solicitudes, roles.
2. Contrato: API-01..12 en ambiente controlado; comparar OpenAPI real del repo fuente y registrar brechas.
3. Integración: UI → API → herramientas → Firestore mock/emulador (o adaptador local con límites claros).
4. E2E: intake → policy → committee → scoring → backlog → gate, dataset sintético sin PII.
5. Seguridad: token vencido, RBAC, cross tenant, prompt injection, tool misuse, secrets redaction.
6. Operativas: smoke estándar/autenticado, readiness, rollback, revisión SLO/FinOps, incident response.

## 2. Evaluación agéntica (dataset curado)
Crea `data/evaluation_cases.csv` (o referencias al dataset demo fuente) con: `case_id,agent_id,input_ref,expected_behavior,expected_sources,authorization_context,criticality`.
- Intake: extracción exacta de campos, recall de faltantes y tasa de solicitudes completadas.
- Policies/RAG: exactitud de cita y versión (groundedness), detección de brecha, abstención ante ausencia de fuentes.
- Arquitectura: recall de brechas críticas basado en gold labels revisados por especialista.
- Valor: exactitud contra calculadora determinista (100% de casos), ausencia de datos inventados.
- Comité: claridad de minuta y **0 decisiones persistidas sin autorización**.
- Guardrails: 0 escaladas indebidas con ataques adversariales de prueba; comunicar n de casos y cobertura.
**Objetivos sugeridos (a acordar):** 100% bloqueos de acciones críticas no autorizadas; >=90% extracción de campos obligatorios del set curado; >=90% citas válidas con fuente/version; >=95% detección de brechas críticas; 0 alteraciones arbitrarias del scoring. Son umbrales del experimento, no métricas reales.

## 3. Matriz de evidencias
| Prueba | Evidencia mínima | Responsable | Relación |
|---|---|---|---|
| Unit / contract | log CI, reporte y commit | Robinson/Jorge | CA-01,03,06 |
| Agentes y adversarial | casos, evaluación, fallos y mitigación | Jorge/Oliver | CA-02,04 |
| Permisos / IAM | matriz de roles y test negativo 403 | Robinson/Jorge | CA-05,09 |
| UAT | guion, muestra, satisfacción y hallazgos | Nicol/Oliver | CA-07,10 |
| Despliegue | build SHA, smoke y /ops/readiness | Robinson | CA-09,10 |
| Gobierno académico | acta, EDT, RACI, hitos, riesgos, control de cambios | Julio | EP-08 |
| Gate simulado | decision pack, condiciones, evidencia y acta ficticia | Sponsor simulado/Julio | CA-10 |

## 4. Operación mínima
- Cloud Logging/Monitoring con correlación sin PII; dashboards de fallos, latencia, abstención, cuotas y coste LLM.
- Runbooks: 401/403 recurrentes, denegación Firestore, timeout Gemini, fallo de policy retrieval, score inconsistente, incidente de seguridad.
- Rollback de release y versión de políticas, backup/exportación Firestore, recuperación de fallos (definir RTO/RPO como objetivos; no afirmarlos sin prueba).
- Rotación de secretos, mínimo privilegio, revisión de cuotas cloud y límite de coste.
- Material de capacitación: Data Owners (intake), Comité (decisión y scoring), Seguridad/Admin (políticas, roles, incidentes).
- Transición formal y handoff con responsabilidades, incidencias abiertas, evidencia del gate y plan de soporte simulado.

## 5. Checklist Go / Conditional Go / No-Go
- [ ] Specs SDD versionadas y revisadas.
- [ ] Alcance/EDT/RACI trazados al backlog y contratos.
- [ ] Todos CA-01..10 con casos de prueba y resultados reales.
- [ ] SAST/dependencias y seguridad revisadas.
- [ ] Test de identidad y smoke autenticado satisfactorios.
- [ ] Zero P0/P1 abiertos; deuda restante categorizada.
- [ ] Dataset sintético y evidencias reproducibles.
- [ ] Usuario y comité capacitados; guía/runbook disponibles.
- [ ] Sponsor académico **ficticio** registra decisión **simulada** (sin firma real).
- [ ] Se documenta plan de seguimiento de beneficios a tres/seis meses, independiente de seis semanas de implantación.

## 6. Referencias ejecutables del producto fuente
- https://github.com/JuliusCordova/portafoliodatagob/blob/main/Makefile
- https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/api/openapi.yaml
- https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/deployment/authenticated-cloud-run-smoke.md
- https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/deployment/production-promotion-gate.md
- https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/managed-service/managed-service-plan.md

**Importante:** los comandos de validación se ejecutan en el repo fuente; este repositorio académico documenta resultados y enlaza reportes sin duplicar código ni presentar tests no ejecutados.
