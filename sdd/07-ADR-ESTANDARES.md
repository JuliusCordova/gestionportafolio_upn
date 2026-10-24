# SDD 07 — ADRs y estándares de arquitectura / desarrollo
**Normativa de diseño adoptada para el caso académico; verificación pendiente contra código real.**

## ADR-002 | Hexagonal Slice
- **Contexto:** integración con LLM, persistencia y decisiones críticas.
- **Decisión:** slices verticales con Domain/Application aislados y puertos/adaptadores donde sean útiles.
- **Consecuencia:** pruebas con fakes y cambio de proveedor sin reescribir reglas; coste moderado de reorganización. Sin imponer refactor global.
- **Fuente:** DevPattern patterns/architecture/hexagonal-slice/README.md.
- **Aceptación:** ARCH-01..08 en SDD 05.

## ADR-003 | Cloud Run-first GCP
- **Contexto:** piloto de seis semanas, demostración ATLAS existente.
- **Decisión:** Cloud Run web/API, Firestore, Gemini/Vertex, ADK compatible, Secret Manager, IAM, observabilidad y CI/CD; servicios extra solo por requisito validado.
- **Consecuencia:** coste/control, autoscaling y despliegue simple; límites para procesos continuos se evalúan después.
- **Fuente:** DevPattern agentic-reference-architectures/gcp.md y proven-practices.md.

## ADR-004 | Decision rights y agentes
- LLM propone; reglas deterministas validan; humano aprueba decisiones y gates. LLM sin permisos de write directo al backend ni IAM elevado.
- Políticas versionadas, evidencia citada, abstención ante insuficiencia y evaluación adversarial.
- Credenciales, autorización e identidad fuera del prompt.

## ADR-005 | Quality-by-contract
- RF/US/CA/EDT como rastreo obligatorio; contratos OpenAPI, schemas JSON, pruebas unit/contract/e2e/evals.
- CI: análisis de dependencias/SAST, validaciones de auth, smoke autenticado, candidate previo a promoción.
- Logs: redactar PII y secretos; trace_id, Git SHA, build ID, image digest, revisión Cloud Run y modelo/prompt.
- Determinismo de scoring, control concurrencia por revision e idempotency keys.

## ADR-006 | Alcance y extensión
- Software de gestión de demanda, NO motor DataOps/ingesta.
- La arquitectura canónica de datos (Medallion y componentes GCP) es **una referencia que el agente valida sobre propuestas**, no la pila obligatoria del backend.
- Seis semanas de trabajo académico; valor/payback observado se evalúa fuera del plazo.
- Cambio de patrón o componente requiere nueva ADR y control de cambios con impacto en EDT y presupuesto.

## Gate de conformidad arquitectónica
- [ ] Reglas del dominio sin importación SDK cloud/LLM.
- [ ] Casos de uso prueban con fake Firestore/Gemini.
- [ ] Contratos alineados con OpenAPI efectivo, tests verdes.
- [ ] Agentes sin bypass de autorización, scoring y decisiones.
- [ ] Seguridad IAM/RBAC/tenant y pruebas negativas.
- [ ] Pipeline con candidate, smoke y evidence pack versionado.
- [ ] Defectos críticos cero para gate; evidencia verificable.
- [ ] Diagramas as-built contra código y recursos realmente desplegados.
