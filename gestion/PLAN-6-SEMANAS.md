# Cronograma de seis semanas (línea base única)
| Semana | Gobierno/entregables | Trabajo de producto | Hito / evidencia |
|---|---|---|---|
| S1 | Charter, interesados, acuerdo, EDT, SDD, riesgo, alcance | Validar contratos y arquitectura de referencia, matriz RACI | Specs y arquitectura revisadas; habilitación GCP |
| S2 | Plan de calidad, UAT y comunicaciones | Sprint 1 inicia: intake, estructura, validación de políticas | Demo interna + tests de agentes |
| S3 | Seguimiento, control de alcance | Sprint 1 termina: comité y auditoría | Demo de incremento y aceptación simulada |
| S4 | Riesgos, presupuesto, correcciones | Sprint 2 inicia: scoring, backlog, tablero, roles | Evidencia de API y UX |
| S5 | Monitoreo, interesados, preparación de gate | Sprint 2 termina: seguridad, pruebas de integración | Demo sprint y cero defectos críticos abiertos |
| S6 | Cierre, lecciones aprendidas, sponsor, evidencias | Cloud Run controlado, smoke autenticado, guías, runbooks | Decisión Go/Conditional Go/No-Go y acta simulada |

Dependencias: S1 specs/arquitectura → S2/S3 sprint 1 → S4/S5 sprint 2 → S6 smoke, evidencia y gate. Revisión PO cada 2 semanas. Duración implantación 6 semanas; metas de beneficios a 3/6 meses son indicadores de operación **posterior** y no extienden el plazo contractual del proyecto académico.

**Control de cambios:** registrar CR-ID, motivo, impacto en tiempo/costo/alcance/seguridad, spec afectada, aprobación simulada y versión de documentos. Ninguna nueva función desplaza el gate S6 sin aprobación y rebaselining.
