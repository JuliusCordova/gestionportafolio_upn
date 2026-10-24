# Matriz de trazabilidad académico → SDD → evidencia
**Fuente primaria académica: guías resueltas Grupo 5 de semanas 01 y 02 recibidas el 24/10/2026.** Versión normalizada no sustituye archivo DOCX original entregado en Blackboard.

## 1. Elementos que solicita la UPN
| Guía | Ficha o criterio | Artefacto GitHub UPN | Relación con SDD | Estado |
|---|---|---|---|---|
| S01 | F1 Equipo, proyecto, problema, cliente, plazo | entregas/S01_Taller1.md | SDD §1–3 | Documentado |
| S01 | F2 Caso de negocio, objetivos SMART, costos, flujo y payback | entregas/S01_Taller1.md | SDD §1, §10 | Documentado; payback hipotético |
| S01 | F3 Enfoque híbrido y FAE/APO | entregas/S01_Taller1.md | SDD §7, §9 | Documentado |
| S01 | F4 Checklist revisión cruzada | entregas/S01_Taller1.md | SDD 04 | Validación externa pendiente |
| S01 | F5 Avance curso autoinstructivo | evidencia individual | SDD 04 | Pendiente captura |
| S02 | F1 Acuerdo, roles, canal, tablero y carpeta | entregas/S02_Taller2.md | SDD §9 | Links de tablero/carpeta pendientes |
| S02 | F2 Charter, triple restricción, riesgos, hitos, sponsor, presupuesto | entregas/S02_Taller2.md | SDD §1, §10 | Documentado, sponsor ficticio |
| S02 | F3 Registro >=6, estrategia y poder/interés | entregas/S02_Taller2.md | SDD §2 y RF por actor | Documentado |
| S02 | F4 Enfoque, alcance, 5 fases, EDT y 3 paquetes del diccionario | entregas/S02_Taller2.md | SDD §3–8, historias | Documentado |
| S02 | F5 Organigrama, RACI, roles, incorporación/liberación | entregas/S02_Taller2.md | SDD §7, roles AG | Documentado |
| S02 | F6 Checklist calidad y revisor | entregas/S02_Taller2.md | SDD 04 | Revisión cruzada pendiente |
| S02 | F7 Evidencia individual autoinstructivo | evidencia individual | SDD 04 | Pendiente captura |

## 2. Alineación de las cinco funciones del alcance
| Bloque del acta | Épica/RF | Historias | API propuestas | Criterio | EDT |
|---|---|---|---|---|---|
| Intake agéntico | EP-01 RF-01/02/03 | US-01,02 | API-01/02/03 | CA-02/03 | 3.1 |
| Políticas y arquitectura | EP-02 RF-04/05 | US-03 | API-04/05 | CA-04 | 3.1 |
| Comité y scoring | EP-03/04 RF-06/07/08 | US-04/05 | API-06/07 | CA-05/06 | 3.1/3.2 |
| Backlog y tablero | EP-05 RF-09/12 | US-06/09 | API-08/12 | CA-07 | 3.2 |
| Despliegue/identidad/gate | EP-06/07 RF-10/11 | US-07/08 | API-09/10/11 | CA-08/09/10 | 4.1 |
| Gestión académica | EP-08 | US-10 | No aplica | Revisión guías | 1.x |

## 3. Reutilización del producto real
| Materia | Referencia principal en portafoliodatagob | Uso permitido |
|---|---|---|
| Producto y requisitos | docs/specs/01-product-spec.md, 07-functional-requirements.md | contexto y contraste |
| Modelo de agentes | docs/specs/02-agent-map.md, 09-mvp-agentic-architecture.md | patrón, sujetos a revisión |
| GCP | docs/specs/04-gcp-architecture.md, docs/architecture/google-cloud-end-to-end-reference-architecture.md | arquitectura base |
| Políticas | docs/policies/operative-committee-architecture-approval.md, finops-by-design.md | principios y reglas |
| Dominio y auditoría | docs/specs/03-domain-model.md, docs/architecture/demand-lifecycle-contract.md | adaptar esquema piloto |
| Criterios | docs/specs/11-acceptance-criteria.md | comparar CA frente a baseline UPN |
| API | docs/api/openapi.yaml | **fuente de contratos efectivos**, hacer diff con API propuestas |
| Despliegue/gate | docs/deployment/controlled-cloud-run-pilot-execution.md, production-promotion-gate.md | evidencia sin afirmar ejecución |
| UI y servicio | apps/web, apps/api, docs/managed-service/managed-service-plan.md | referente técnico, no duplicar |

**Repositorio de referencia:** https://github.com/JuliusCordova/portafoliodatagob

## 4. Inconsistencias resueltas y no resueltas
- **Resuelto:** duración única 6 semanas. 8 semanas/3 meses de S01 quedan como antecedentes y no vigentes.
- **Resuelto:** sponsor es cargo ficticio conforme a la guía; el estado de firma es simulado/sin firma real, no se inventa identidad.
- **Conservado por instrucción:** payback 1.5 años **(est.)** como supuesto ficticio; flujo original anual no lo demuestra de manera exacta.
- **Conciliado:** los dos desglose de S/ 110,000 son vistas alternativas de la misma inversión, no adiciones.
- **Pendiente:** URL real tablero GitHub Projects, carpeta compartida, evidencias individuales del curso y revisión cruzada.
- **Pendiente:** contraste API-01..12 con OpenAPI real antes de implementar; validar pesos del scoring y mecanismo final de OIDC con el equipo.
- **Pendiente:** aceptación del sponsor se representa solo como ejercicio simulado y no como acuerdo de una entidad externa.

## 5. Checklist de integridad documental
- [x] S01/S02 alineadas a 6 semanas.
- [x] Fichas del docente descritas y vinculadas.
- [x] 5 fases y EDT con paquetes y 3 diccionarios.
- [x] Seis interesados, valoración y matriz RACI con un solo A por entregable.
- [x] SDD con agentes, guardrails, RF, RNF, contratos, datos, backlog y CA.
- [x] Plan semanal/hitos y presupuesto.
- [ ] Revisiones por integrantes y docente, si se solicitan.
- [ ] Capturas auténticas de pruebas, demos y autoinstructivo.
- [ ] Pruebas reales en GCP y conciliación de OpenAPI.

## 6. Arquitectura objetivo incorporada
La Ficha 4 (S02) — EDT 2.2, 3.1, 3.2 y 4.1 — se conecta a [SDD 05 arquitectura Hexagonal Slice](../sdd/05-ARQUITECTURA-HEXAGONAL.md), [SDD 06 componentes GCP](../sdd/06-ARQUITECTURA-COMPONENTES-GCP.md) y [SDD 07 decisiones/estándares](../sdd/07-ADR-ESTANDARES.md). Los criterios ARCH-01..08 requieren pruebas de independencia del dominio, permisos, contratos y evidencias antes del gate de semana 6. Fuente DevPattern; infraestructura base coherente con el producto técnico ATLAS, sin presumir refactor implementado.
