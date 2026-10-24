# Semana 01 — Taller 1 | Caso de negocio, enfoque y entorno
**Grupo 5 · Versión normalizada 1.1 · 24/10/2026**
**Control de cambio:** Las menciones a 8 semanas y 3 meses en la versión inicial quedan reemplazadas por la **línea base aprobada de 6 semanas**. El payback de **1.5 años (est.)** permanece como hipótesis de negocio ficticia.

## Ficha 1. Mi proyecto
- **Nombre:** Implantación del Agente ATLAS DataGob: portafolio de demanda de datos, analítica e IA.
- **Equipo:** Julio César Córdova Díaz (gerente); Oliver Jhonatan Cueva López (analista funcional); Robinson Eduardo Guevara Ángeles (líder técnico GCP); Jorge Luis Caballero Andrada (calidad); Nicol Grijaldo Chicana Chicana (negocio/cambio, incorporada formalmente en S02).
- **Problema:** unas 120 solicitudes/año por correo y Excel, 35% incompletas, 45 días promedio a decisión (todos **est.**, datos simulados).
- **Usuarios:** Data Owners, Comité Operativo, arquitectos de datos y Comité Estratégico.
- **Sponsor:** Gerente de Datos e IA de la Organización Piloto Ficticia (cargo simulado).
- **Producto:** web de intake agéntico, validaciones, comité, scoring, backlog y tablero sobre Cloud Run y Firestore.
- **Plazo:** 6 semanas.

## Ficha 2. Caso de negocio v1
**Resumen ejecutivo:** gestionar en un flujo gobernado la demanda de datos e IA, reduciendo de 45 a 20 días el tiempo hasta decisión y de 35% a 10% las solicitudes incompletas durante seis meses de seguimiento posterior. Inversión simulada S/ 110,000; recuperación **estimada en 1.5 años**, bajo supuestos ficticios.

**Oportunidad:** ahorro estimado de 1,150 horas/año (~S/ 130,000) y evitación potencial de inversión de bajo valor (~S/ 50,000). No son resultados validados y podrían solaparse: se requiere estimación incremental.

**Objetivos SMART:**
1. Reducir tiempo de registro a decisión **45 → 20 días**, hasta el mes 6 posterior al despliegue.
2. Reducir solicitudes incompletas **35% → 10%**, hasta el mes 6 posterior al despliegue.
3. Registrar **100% de demandas nuevas con scoring y auditoría**, como meta de adopción al mes 3 de iniciado el piloto; validar viabilidad temporal frente a implantación de seis semanas.

**Costos de negocio:** implantación S/ 90,000; nube/equipamiento S/ 20,000; mantenimiento S/ 30,000/año (est.). La **inversión inicial es S/ 110,000**, no debe sumarse nuevamente al desglose de presupuesto S02.

**Supuestos:** sponsor y PO participan; Seguridad autoriza integración al IdP existente; Data Owners entregan checklists de valor.

**Flujo originalmente reportado (simulado):**

| Concepto | Año 0 | Año 1 | Año 2 | Total |
|---|---:|---:|---:|---:|
| Beneficios | 40,000 | 160,000 | 200,000 | 400,000 |
| Costos | 110,000 | 30,000 | 30,000 | 170,000 |
| Beneficio neto | -70,000 | 130,000 | 170,000 | 230,000 |

**Payback consignado: 1.5 años (est.), hipótesis de la simulación.** La tabla no demuestra ese plazo con precisión: se preserva como supuesto declarado y queda pendiente una desagregación mensual para sustentarlo, sin presentarlo como cálculo validado.

**Recomendación:** aprobar académicamente el proyecto condicionado a evidencias de aceptación, privacidad, operaciones y beneficio esperado.

## Ficha 3. Enfoque y entorno
**Enfoque híbrido:** 3 criterios predictivos (requisitos, baja incertidumbre técnica del piloto, documentación/gates), 2 ágiles (revisión quincenal, incrementos) y una situación mixta. Gestión por EDT y controles; construcción iterativa en dos sprints de dos semanas; spec-first SDD.

**FAE:**
1. Ley 29733 y normas de seguridad corporativa: acceso, privacidad y auditoría.
2. Cultura de correo/Excel: necesidad de capacitación y gestión del cambio.
3. GCP, cuentas de servicio y presupuesto disponibles: arquitectura, despliegue y FinOps.

**APO:**
1. Políticas documentadas de arquitectura, comité, FinOps y ciclo de vida de datos.
2. Checklists de release, smoke tests, evidence pack y runbooks.
3. ADRs, lecciones aprendidas de sprints técnicos previos y datasets sintéticos.

## Fichas 4 y 5
La checklist de la S01 se usa como lista de verificación y requiere revisión cruzada verificable. Evidencia individual del curso autoinstructivo: **pendiente**; no fabricar capturas o certificaciones.

**Documento SDD asociado:** [SDD maestro](../sdd/00-SDD-MAESTRO.md).
