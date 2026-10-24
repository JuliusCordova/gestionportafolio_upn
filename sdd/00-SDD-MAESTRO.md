# SDD maestro v1.0 — ATLAS DataGob | UPN
**Estado: baseline académica propuesta** · 24/10/2026 · **Plazo: seis semanas** · presupuesto simulado S/ 110,000 · sponsor ficticio.
**Norma de especificación:** una implementación, cambio o prueba debe referenciar IDs de requisitos, historias y criterios en este repositorio antes de modificar código fuente. No sustituye las specs reales del producto.

## 1. Visión, problema y North Star
ATLAS DataGob transforma solicitudes informales de datos/IA (correo y Excel) en demandas estructuradas, evaluadas, priorizadas y auditables para el comité de una organización ficticia. Problema base simulado: 120 demandas/año, 35% incompletas y 45 días de registro a decisión.
**North Star de negocio:** mediana de días calendario entre registro validado y decisión de comité para demandas del piloto. Meta a 6 meses tras puesta en operación: de 45 a 20 días (est.).
Métricas secundarias: incompletas en comité 35%→10% a 6 meses, trazabilidad de demanda y scoring en 100% de nuevas demandas a 3 meses, satisfacción UAT >=85%, 0 defectos críticos al gate. No prometer estas metas como resultados en seis semanas.

## 2. Personas, necesidades y límites
- Data Owner/solicitante: iniciar intake, adjuntar supuestos y consultar estado.
- Analista/Arquitecto: evaluar calidad del pedido, políticas y brechas arquitectónicas.
- Comité Operativo: deliberar, solicitar reformulación, recomendar y registrar decisión.
- Comité Estratégico/Sponsor: revisar recomendaciones, priorizar, aprobar gates.
- Seguridad y admin: gestionar RBAC, auditar y preparar operación.
- GP/QA: seguir EDT, criterios, sprints, evidencias y riesgos.
No hay integraciones productivas con ERP/legacy, ni corrección de datos fuente, ni construcción de IdP, ni autorizaciones de pago.

## 3. Alcance funcional (épicas EP)
EP-01 Intake conversacional gobernado y catalogación.
EP-02 Evaluación de políticas, riesgos, arquitectura y brechas.
EP-03 Flujo de revisión y decisión humana del comité.
EP-04 Modelo de priorización con scoring y argumentos financieros.
EP-05 Backlog de iniciativas y tablero ejecutivo con filtros.
EP-06 Identidad, roles, consentimiento, privacidad y auditoría.
EP-07 Despliegue controlado, pruebas, observabilidad y readiness.
EP-08 Gestión académica: charter, interesados, EDT, RACI, cronograma, cambios, evidencias y cierre.

## 4. Requerimientos funcionales (RF)
- **RF-01** Alta y edición de demanda por solicitante; estado DRAFT.
- **RF-02** Asistente conversa, propone título, objetivo, área, beneficio, costos, dependencias y métricas; exige confirmación humana.
- **RF-03** Clasificación de dominio/riesgo de IA y detección de campos incompletos.
- **RF-04** Recuperar política versionada y devolver estado compliance, brechas, fuentes y confianza informativa.
- **RF-05** Contrastar arquitectura propuesta con patrón GCP y solicitar revisión humana.
- **RF-06** Enviar solicitud completa a comité, generar minuta/recomendación; persistir decisión con actor y motivo.
- **RF-07** Registrar criterios ponderados de valor, factibilidad, riesgo y costo; calcular score determinista.
- **RF-08** Calcular VAN, TIR y payback con entradas explícitas y flujos definidos; no inventar valores.
- **RF-09** Visualizar backlog y cockpit por permisos, score, riesgo, estado y fecha.
- **RF-10** Auditar creación, cambios, puntuación, decisión, rechazo y excepciones.
- **RF-11** Emitir y consultar estados Go / Conditional Go / No-Go con evidencia.
- **RF-12** Exportar ficha de caso de negocio/decision pack con fuentes, supuestos y versión.

## 5. No funcionales (RNF, objetivos de diseño no resultados)
- **RNF-01 Seguridad:** OIDC/IdP existente, RBAC server-side, mínimos privilegios, tokens breves, no claves en Git, TLS, cifrado en reposo.
- **RNF-02 Privacidad:** datos sintéticos por defecto, minimización y control de acceso; retención configurable; tratamiento Ley 29733 sujeto a revisión humana.
- **RNF-03 Auditoría:** correlación request_id/trace_id, actor, timestamp, versión spec/policy/model, hash evidencia y antes/después sin datos personales sensibles.
- **RNF-04 Disponibilidad:** health/readiness y procedimientos de recuperación; objetivos SLO a acordar con sponsor académico.
- **RNF-05 Rendimiento (metas de piloto):** p95 API no-LLM <2 s, interacciones agente p95 <20 s con timeout/reintento máximo configurable; medir, no presumir.
- **RNF-06 Calidad:** pruebas automáticas, cero P0/P1 en gate, >=85% satisfacción UAT simulada.
- **RNF-07 Reproducibilidad:** seed/reset sintético, manifests, contratos versionados y scripts de smoke.
- **RNF-08 IA responsable:** respuesta fundada, abstención si evidencia insuficiente, no decisiones automáticas irreversibles.
- **RNF-09 FinOps:** límite presupuestario de llamadas a modelo, telemetría tokens/costo por demanda, alertas y fallback no-LLM.
- **RNF-10 Accesibilidad:** navegación por teclado, estados entendibles, mensajes explicativos y contraste verificable.

## 6. Arquitectura y decisiones
UI Next.js → API FastAPI en Cloud Run → orquestación de agentes (Gemini/ADK cuando se valide) → servicios deterministas de políticas, scoring y comité → Firestore (piloto), objetos/evidencias en almacenamiento aprobado, Logging/Monitoring.
- IAM/OIDC, Secret Manager, service accounts separados; solo privilegios necesarios.
- Agentes recomiendan; los servicios autorizados y el humano ejecutan transiciones críticas.
- JSON local y seed sintético sirven solo para demo, Firestore en piloto controlado.
- Arquitectura detallada [01](01-ARQUITECTURA-AGENTICA.md); APIs y esquema [02](02-CONTRATOS-Y-DATOS.md).

## 7. Ciclo de desarrollo agéntico asistido (SDD)
1. **Entender**: problema/usuario, RF, fuentes y riesgos; enlazar Ficha S01/S02 y stakeholder.
2. **Especificar**: PR a documentos, IDs RF/CA, contrato API, estados y amenaza; validación GP/PO simulada.
3. **Planificar**: desglose EDT→épica→historia→tarea→test; responsable y Definition of Ready.
4. **Implementar con copiloto/agente de código**: prompt con spec exacta, alcance mínimo, dependencias permitidas, pruebas exigidas. No entregar secretos; no conceder despliegue automático.
5. **Verificar**: revisar diff humano, SAST/dependency scan, lint, unit/contract/e2e, pruebas agénticas de groundedness y autorización.
6. **Evidenciar**: PR, resultado test, capturas redactadas, trazas sin PII, aprobación y matriz RF→CA.
7. **Entregar**: revisión sprint, gate de seguridad, decisión simulada; cambios posteriores modifican primero SDD.

### Plantilla de prompt de desarrollo
> Actúa como asistente de ingeniería, no como aprobador de negocio. Contexto: [RF-ID, US-ID, CA-ID, API-ID, ruta fuente]. Implementa únicamente [alcance]. Respeta roles/estados, no inventes reglas. Produce plan, cambios propuestos y tests; enumera supuestos y riesgos. Si faltan datos, genera TODO explícito y no acciones ficticias. Requiere revisión de humano para aprobar o desplegar.

## 8. Definition of Ready / Done
**Ready:** historia con actor, valor, entradas, autorización, criterios Gherkin, datos de prueba y responsable, dependencias identificadas.
**Done:** spec versionada; tests unit/integration/contract y tests agénticos aprobados; revisión code/seguridad; auditoría; documentación UX; demo; evidencia enlazada; rollback definido; aprobación simulada del PO.

## 9. Gestión PMBOK exigida por docente
El SDD conecta caso de negocio (S01), enfoque y FAE/APO, acuerdo de equipo, charter, seis interesados con mapa poder-interés, alcance/exclusiones, EDT y diccionario, RACI, roles, hitos y presupuesto. [S01](../entregas/S01_Taller1.md) · [S02](../entregas/S02_Taller2.md) · [Matriz](../gestion/TRAZABILIDAD-UPN.md).

## 10. Restricciones y gobernanza del proyecto
Seis semanas para implantación; metas de impacto a 3/6 meses son hipótesis a evaluar fuera del ciclo. S/ 110,000 total simulado. El payback de 1.5 años es **supuesto ficticio no demostrado por la tabla anual S01**. Sponsor = cargo de Gerente de Datos e IA de Organización Piloto Ficticia; sin firma real. Todo cambio requiere CR, actualización spec, reestimación e identificación de aprobador.

## 11. Enlaces a repositorio técnico (no duplicación)
- Producto: https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/specs/01-product-spec.md
- Requisitos: https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/specs/07-functional-requirements.md
- Criterios: https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/specs/11-acceptance-criteria.md
- Arquitectura: https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/architecture/google-cloud-end-to-end-reference-architecture.md
- Despliegue: https://github.com/JuliusCordova/portafoliodatagob/blob/main/docs/deployment/controlled-cloud-run-pilot-execution.md

## 12. Índice
[Agentes y arquitectura](01-ARQUITECTURA-AGENTICA.md) · [Contratos y datos](02-CONTRATOS-Y-DATOS.md) · [Historias y CA](03-BACKLOG-Y-ACEPTACION.md) · [Validación y operación](04-VALIDACION-Y-OPERACION.md).
