# EIS-40C.2 — Repository Discovery y análisis de reglas legacy

## 1. Objetivo

Descubrir, inventariar y clasificar las reglas existentes antes de migrar. En esta etapa no se modifica código.

## 2. Repository Discovery obligatorio

Inspeccionar:

- `Backend/app`;
- `Backend/rules`;
- pruebas relacionadas;
- configuraciones;
- loaders;
- motores legacy;
- prompts con reglas explícitas;
- postprocesadores;
- validadores;
- modelos;
- scoring;
- generación de findings;
- enums y constantes;
- imports;
- packaging;
- startup;
- variables de entorno.

## 3. Fuentes mínimas a revisar

```text
Backend/app/assessment/rule_engine*
Backend/app/rfp/rules_engine*
Backend/app/rfp/config_engine*
Backend/app/rfp/prompt_rules*
Backend/app/evidence_engine/contradiction/rule_engine*
Backend/app/**/postprocessor*
Backend/app/**/validator*
Backend/app/**/scoring*
Backend/rules/**
Backend/tests/**
```

La estructura real prevalece.

## 4. Inventario

Registrar:

- `source_id`;
- archivo;
- símbolo/clase/función;
- línea o rango;
- dominio;
- descripción;
- condición;
- resultado;
- severidad;
- dependencia;
- side effects;
- pruebas;
- criticidad;
- candidato;
- motivo;
- estado.

## 5. Fingerprint

Cada regla debe tener fingerprint estable basado en ruta, símbolo, condición normalizada, resultado normalizado y hash del fragmento relevante. No almacenar secretos.

## 6. Clasificación de fuente

- `CODE_CONDITION`
- `CONFIG_RULE`
- `PROMPT_RULE`
- `POSTPROCESSOR_RULE`
- `VALIDATION_RULE`
- `SCORING_RULE`
- `MAPPING_RULE`
- `CONTRADICTION_RULE`
- `OTHER`

## 7. Dependencias

Identificar entradas, auxiliares, orden, estado global, configuración, LLM, scoring, modelos, APIs, persistencia, logging y excepciones.

## 8. Equivalencia potencial

- `DIRECT`
- `ADAPTABLE`
- `COMPOSITE`
- `NON_DETERMINISTIC`
- `OUT_OF_SCOPE`
- `AMBIGUOUS`

## 9. Selección del lote

El lote inicial debe ser pequeño, representativo y reversible. Priorizar reglas deterministas, sin side effects, con pruebas, dominio acotado, severidad clara y sin dependencia de scoring o narrativa.

Nada fuera del lote puede migrarse.

## 10. Preconditions

Antes de Stage 03 debe existir inventario, fuentes, lote, exclusiones, conflictos, dependencias, riesgos, evidencia de EIS-40C.1 y revisión del worktree.

## 11. Allowed Changes

Ninguno.

## 12. Forbidden Changes

No crear reglas YAML, modificar motores, mover lógica, cambiar pruebas, scoring, configuración productiva, dependencias, frontend ni startup.

## 13. Blocking Conditions

Detener si no hay fuente de verdad, la regla pertenece a scoring, cambia respuestas públicas, falta referencia, EIS-40C.1 no está disponible, existen conflictos o el lote es demasiado amplio.

## 14. Entrega de Stage 02

1. estructura;
2. motores y fuentes;
3. inventario resumido;
4. lote recomendado;
5. exclusiones;
6. dependencias;
7. conflictos;
8. riesgos;
9. archivos potenciales;
10. autorización o bloqueo.
