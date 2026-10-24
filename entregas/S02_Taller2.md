# Semana 02 — Taller 2 | Equipo, acta, interesados, alcance y recursos
**Grupo 5 · Versión normalizada 1.1 · 24/10/2026**. Organización, patrocinio y presupuesto de **simulación académica**.

## Ficha 1. Acuerdo de equipo
| Integrante | Responsabilidad |
|---|---|
| Julio César Córdova Díaz | Gerencia de proyecto |
| Oliver Jhonatan Cueva López | Análisis funcional |
| Robinson Eduardo Guevara Ángeles | Arquitectura GCP y liderazgo técnico |
| Jorge Luis Caballero Andrada | Aseguramiento de calidad |
| Nicol Grijaldo Chicana Chicana | Negocio, stakeholders y cambio |

**Comunicación:** martes/jueves 21:00, 30 min Meet; revisión quincenal de sprint los sábados 60 min; WhatsApp diario (respuesta hasta 12 h; urgencias 2 h). **Decisiones:** primero evidencia/spec/tablero, luego mayoría; decisiones de diseño dentro de especificación corresponden al líder técnico, cambios de alcance/plazo/costo al gerente con autorización del sponsor cuando corresponda. **Conflictos:** conversación privada → conversación moderada → acuerdo documentado → escalamiento al docente ante doble incumplimiento. Diversidad de perfiles y horarios se aborda mediante trabajo asincrónico y actas escritas.

**Herramientas:** repositorio académico https://github.com/JuliusCordova/gestionportafolio_upn ; repositorio técnico https://github.com/JuliusCordova/portafoliodatagob ; GitHub Projects Kanban (**enlace pendiente**); carpeta APS_Grupo5/01_Gestion,02_Specs_SDD,03_Entregables,04_Evidencias (**enlace pendiente**); WhatsApp APS Grupo 5. Las aceptaciones de equipo declaradas en la ficha del taller son académicas; no sustituyen firmas digitales reales.

## Ficha 2. Acta de constitución (Project Charter)
- **Nombre/sigla:** Implantación del Agente ATLAS DataGob (ATLAS).
- **Justificación:** 120 solicitudes/año, 35% incompletas, decisión en 45 días (est.).
- **Cliente:** Gerencia de Datos e IA, **Organización Piloto Ficticia**.
- **Sponsor simulado:** **Gerente de Datos e IA de la Organización Piloto Ficticia**. Nombre personal: **no asignado por ser ejercicio ficticio**. Autorización: **aprobación académica simulada; firma real pendiente/no aplicable**.
- **Product Owner simulado:** Líder de Gobierno de Datos de la misma organización.
- **Director del proyecto:** Julio César Córdova Díaz (Grupo 5).
- **Plazo único:** 6 semanas, equivalente aproximado a 1.5 meses.
- **Presupuesto total:** S/ 110,000 (est.).
- **Alcance:** (1) intake agéntico, (2) validación de política/arquitectura, (3) comité y scoring VAN/TIR/payback, (4) backlog y tablero, (5) Cloud Run, identidad OIDC y gate.
- **Fuera del alcance:** arreglar calidad de sistemas fuente; modificar legados o integrar fuentes no aprobadas; implementar un proveedor corporativo de identidad.
- **Objetivo de alcance:** entregar los cinco bloques con criterios CA-01..CA-10 y aceptación simulada al cierre de semana 6.
- **Objetivo de tiempo:** gate Go/Conditional Go/No-Go en semana 6.
- **Objetivo de costo:** no superar S/ 110,000 del escenario.
- **Factores críticos:** specs aprobadas semana 1; PO disponible para revisión en 2 días; cuentas GCP y Seguridad autorizadas.
- **Restricciones:** 6 semanas, S/ 110,000, cinco integrantes parciales (lunes-viernes 19–22 y sábados 9–13, est.).
- **Supuestos:** sponsor/PO validan, Seguridad permite IdP existente antes de S2, Data Owners prueban, GCP disponible S1.
- **Organizaciones:** Grupo 5 (equipo de ejecución), organización piloto ficticia (cliente), Seguridad TI simulada (aprobador) y Google Cloud (plataforma tecnológica).
- **Autoridad del GP:** asignación de trabajo y gestión de entregables bajo alcance aprobado; cambios de línea base se elevan al sponsor.
- **Riesgos:** demora de identidad; baja adopción por continuar con Excel; oportunidad de reutilización en otras áreas.
- **Entregables:** acta, specs SDD, arquitectura, incrementos, pruebas, evidence pack, guías y runbooks, aceptación académica.

### Hitos
| Hito | Semana |
|---|---:|
| Charter, EDT y specs basales | 1 |
| Arquitectura e identidad diseñadas | 1 |
| Incremento 1 aceptado (intake y comité) | 3 |
| Incremento 2 aceptado (scoring, cockpit y seguridad) | 5 |
| Gate y cierre del piloto | 6 |

### Presupuesto preliminar
| Concepto | S/ |
|---|---:|
| Personal | 76,000 |
| Otros costos | 18,000 |
| Línea base de costos | 94,000 |
| Contingencia | 8,000 |
| Reserva de gestión | 8,000 |
| **Total autorizado** | **110,000** |

Contingencia forma parte de la referencia de costos; gestión es reserva administrada aparte. Los mismos S/ 110,000 aparecen en el caso de negocio S01, con distinta clasificación, y **no son una inversión adicional**.

### Sponsor del acta
| Nombre/rol | Organización | Autoridad | Estado |
|---|---|---|---|
| Gerente de Datos e IA (personaje sin nombre) | Organización Piloto Ficticia | Patrocinar, aprobar presupuesto, aceptar entrega y decidir gate | **Simulado, sin firma real** |

## Ficha 3. Registro y gestión de interesados
| Actor | Poder | Influencia | Impacto | Total | Interés/estrategia |
|---|---:|---:|---:|---:|---|
| Gerente de Datos e IA (sponsor) | 10 | 10 | 10 | 30 | Gestionar de cerca: reportes semanales |
| Líder de Gobierno de Datos (PO) | 7 | 9 | 9 | 25 | Gestionar de cerca: revisión quincenal y specs |
| Julio Córdova (GP) | 8 | 9 | 8 | 25 | Gestionar de cerca: gestión de desvíos |
| Comité Operativo | 6 | 7 | 8 | 21 | Gestionar de cerca: validación de criterios |
| Oficial de Seguridad | 7 | 6 | 4 | 17 | Mantener satisfecho: aprobación IdP |
| Data Owners | 3 | 4 | 7 | 14 | Mantener informado: capacitación y UAT |

**Regla de la guía:** Poder >= 6 alto, impacto >= 5 se usa como proxy de interés alto. Los perfiles externos al Grupo 5 se modelan como interesados ficticios. Actitud: sponsor, PO, GP, comité = apoyo; Seguridad y Data Owners = neutral. Seguridad y Data Owners pueden representar obstáculos de aprobación/adopción sin clasificarlos como opositores personales.

## Ficha 4. Metodología, alcance y EDT
**Enfoque:** híbrido con planificación predictiva para gestión, aprobaciones y despliegue; Scrum ligero de 2 sprints de 2 semanas; Kanban, SDD spec-first y review quincenal.

**Necesidades y funciones:** intake conversacional estructurado; detección de brechas contra políticas y arquitectura; scoring ponderado 1..5 con indicadores VAN/TIR/payback; auditoría de decisiones y timeline. **Aceptación:** cumplir CA-01..CA-10, smoke estándar y autenticado, cero defectos críticos, satisfacción de usuarios clave >=85% (meta), gate semana 6 y aceptación simulada del patrocinador.

**Fases:** 1. Gestión; 2. Especificación/diseño; 3. Construcción/pruebas; 4. Despliegue y gate; 5. Capacitación/transición.

**Exclusiones/restricciones/supuestos:** iguales al charter. Cambios por control de cambios de SDD.

### EDT v1 — regla del 100%
| Código | Entregable o paquete |
|---|---|
| 1.0 | Gestión del proyecto |
| 1.1 / 1.1.1 | Iniciación / Acta |
| 1.2 / 1.2.1 | Planificación / Alcance, EDT, cronograma |
| 1.3 | Seguimiento, interesados, riesgos, comunicaciones y cierre |
| 2.0 | Especificación y diseño |
| 2.1 | Specs versionadas y CA-01..CA-10 |
| 2.2 | Diseño GCP, identidad y arquitectura agéntica |
| 3.0 | Construcción y pruebas |
| 3.1 | Sprint 1: intake, políticas y comité |
| 3.2 | Sprint 2: scoring, tablero y roles |
| 3.3 | Pruebas integrales y UAT |
| 4.0 | Despliegue y puesta en operación |
| 4.1 | Cloud Run, smoke, evidence pack y gate |
| 5.0 | Capacitación y transición |
| 5.1 | Plan de capacitación |
| 5.2 | Capacitación por rol |
| 5.3 / 5.3.1 | Guías y runbooks / catálogo de políticas |
| 5.3.2 | Guía de usuario |
| 5.3.3 | Runbooks operativos y soporte |
| 5.4 | Acta de entrega y cierre |

### Diccionario de tres paquetes
| Código | Objetivo | Requisitos | Recursos | Dependencias | Aceptación | Supuestos/restricciones |
|---|---|---|---|---|---|---|
| 2.1 | Spec como fuente de verdad | Charter, acceso repo, talleres PO | Oliver, Robinson, Nicol | 1.2.1 | CA-01..CA-10 versionados; validación simulada del PO | 2 días de revisión; cierre S1 |
| 3.1 | Incremento usable de intake/comité | 2.1,2.2 y entorno | Robinson, Jorge | 2.1 y 2.2 | CA-03, CA-05, CA-09 y demo aceptada | Data Owners disponibles; S2–S3 |
| 4.1 | Desplegar y decidir avance | Sprint 2 y seguridad | Robinson, Jorge; sponsor decide | 3.2, 3.3 | Smoke estándar/autenticado, readiness y gate | Seguridad habilitada; cierre S6 |

## Ficha 5. Recursos y responsabilidades
**Organigrama:** Sponsor → GP (Julio) → líder técnico (Robinson), analista funcional (Oliver), calidad (Jorge) y negocio/cambio (Nicol). Sponsor reporta al comité estratégico ficticio.

**RACI (R ejecuta, A único por fila, C consulta, I informado):**

| Entregable | Sponsor | Julio | Robinson | Oliver | Jorge | Nicol |
|---|---|---|---|---|---|---|
| Acta 1.1.1 | A | R | I | I | I | I |
| Alcance/EDT 1.2.1 | A | R | C | C | I | C |
| Specs 2.1 | I | A | C | R | C | C |
| Arquitectura 2.2 | I | A | R | C | I | I |
| Sprint 1 3.1 | I | A | R | C | C | I |
| Sprint 2 3.2 | I | A | R | C | C | I |
| Pruebas UAT 3.3 | I | A | C | C | R | C |
| Gate 4.1 | A | C | R | I | R | I |
| Capacitación y guías 5.1–5.3 | I | A | C | C | I | R |
| Cierre 5.4 | A | R | C | C | C | C |

**Rol GP:** planifica alcance, plazo y costo; lidera acuerdos y revisiones; gestiona interesados, bloqueos, presupuesto, riesgos y acta final; reporta al sponsor. Requiere conocimientos PMBOK, ágil y liderazgo.

**Rol líder técnico:** construye solución de acuerdo con la spec, coordina identidad/Cloud Run, revisa código, dirige pruebas técnicas y despliegue, reporta al GP. Requiere GCP, IAM, Cloud Run, FastAPI, Next.js y agentes/SDD.

**Incorporación/liberación:** GP desde S1 hasta acta S6; funcional y negocio desde S1 hasta documentación/capacitación S6 (formación SDD/DAMA); líder técnico S1 a gate S6 (Gemini/ADK/GCP); calidad S2 a evidence pack S6 (pruebas de aceptación y smoke).

## Fichas 6 y 7
Checklist cruzada requiere revisión del otro equipo y sus observaciones; no inventar revisión. Captura individual del curso autoinstructivo permanece **pendiente**. Conservarlas como evidencias separadas.

**Complementos ejecutables:** [SDD maestro](../sdd/00-SDD-MAESTRO.md) y [trazabilidad](../gestion/TRAZABILIDAD-UPN.md).
