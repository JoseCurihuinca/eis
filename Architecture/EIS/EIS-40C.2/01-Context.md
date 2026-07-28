# EIS-40C.2 — Contexto, alcance y objetivos

## 1. Contexto

EIS-40C.1 creó una fundación de gobierno de reglas independiente, segura y no autoritativa. Sin embargo, la lógica efectiva continúa distribuida entre motores, configuraciones, prompts, clases y archivos legacy.

EIS-40C.2 convierte una parte controlada de esas reglas en artefactos gobernados, sin alterar la ejecución productiva.

## 2. Problema

Actualmente pueden coexistir reglas en ubicaciones como:

- `Backend/app/assessment/rule_engine`;
- `Backend/app/rfp/rules_engine`;
- `Backend/app/rfp/config_engine`;
- `Backend/app/rfp/prompt_rules`;
- `Backend/app/evidence_engine/contradiction/rule_engine`;
- configuraciones YAML/JSON;
- constantes embebidas;
- postprocesadores;
- reglas expresadas dentro de prompts.

Esto genera duplicidad, inconsistencia, baja trazabilidad, dificultad de versionado y riesgo de migración incorrecta.

## 3. Objetivo general

Migrar un lote explícito de reglas legacy hacia el catálogo gobernado de EIS, conservando intención funcional, dominio, severidad, condiciones, acciones, metadata, fuente, versión, lifecycle, evidencia y comportamiento no autoritativo.

## 4. Objetivos específicos

1. Identificar las fuentes de reglas candidatas.
2. Clasificarlas por dominio y criticidad.
3. Seleccionar un lote limitado.
4. Definir un formato canónico.
5. Generar reglas YAML válidas.
6. Registrar trazabilidad origen-destino.
7. Comparar resultados legacy y EIS en SHADOW.
8. Detectar diferencias semánticas.
9. Mantener reversibilidad.
10. Probar no impacto productivo.

## 5. Unidad de migración

Cada regla debe contar con:

- fuente;
- identificador legado o fingerprint;
- dominio;
- condición;
- resultado esperado;
- severidad;
- evidencia;
- versión destino;
- estado de migración.

No se migran bloques ambiguos sin descomposición verificable.

## 6. Clasificación de reglas

- seguridad;
- continuidad;
- observabilidad;
- integración;
- gobierno;
- costos/TCO;
- SLA/soporte;
- exit strategy/vendor lock-in;
- arquitectura;
- datos;
- IA;
- calidad;
- madurez;
- evidencia;
- consistencia;
- scoring;
- presentación/narrativa.

Las categorías `scoring` y `presentación/narrativa` se inventarían, pero no se migran en esta fase.

## 7. Estados de migración

- `DISCOVERED`
- `ANALYZED`
- `APPROVED_FOR_MIGRATION`
- `MIGRATED`
- `VALIDATED`
- `DEFERRED`
- `REJECTED`
- `BLOCKED`

## 8. Criterios para aprobar una regla

Puede migrarse si:

- tiene intención clara;
- tiene fuente identificable;
- puede representarse con el esquema EIS;
- no exige modificar scoring;
- no cambia contratos públicos;
- no depende de interpretación no determinista;
- existe prueba o caso de referencia;
- no entra en conflicto con otra regla gobernada.

## 9. Criterios para diferir una regla

Diferir si:

- depende de LLM o prompt semántico no estructurado;
- combina múltiples reglas;
- usa contexto no disponible;
- produce side effects;
- cambia scoring;
- carece de fuente inequívoca;
- requiere diseño adicional;
- pertenece a una fase posterior.

## 10. Compatibilidad obligatoria

```text
Scoring changed: NO
ResultJson changed: NO
Public API contracts changed: NO
Frontend changed: NO
Production findings changed: NO
Narrative changed: NO
Rule Engine authoritative: NO
Legacy rules removed: NO
```

## 11. Riesgos principales

- semántica incompleta;
- duplicidad;
- severidad distinta;
- alteración de orden;
- pérdida de excepciones legacy;
- confundir prompt con regla determinista;
- diferencias de normalización;
- equivalencia sin evidencia;
- artefactos inseguros en runtime.

## 12. Resultado de Stage 01

El reporte debe confirmar alcance, fuentes conocidas, exclusiones, compatibilidad, dudas y autorización o bloqueo para Stage 02.
