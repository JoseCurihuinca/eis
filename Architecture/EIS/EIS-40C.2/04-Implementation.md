# EIS-40C.2 — Implementación de Rule Migration

## 1. Objetivo

Implementar exclusivamente la migración del lote aprobado, con trazabilidad y comparación SHADOW.

## 2. Precondiciones

Stages 01–03 aprobados, lote explícito, worktree inspeccionado, EIS-40C.1 operativo, conflictos resueltos y rutas permitidas identificadas.

## 3. Implementación obligatoria

### 3.1 Módulo de migración

Crear o adaptar módulo aislado para inventario, manifest, mapping, validación, transformación, comparación, diagnósticos y excepciones.

### 3.2 Manifest

Debe incluir:

- `eis_id`;
- `batch_id`;
- `version`;
- `status`;
- `created_at`;
- `owner`;
- `source_scope`;
- `target_root`;
- `mappings`;
- `exclusions`;
- `rollback`;
- `checksum_policy`.

### 3.3 Mappings

Cada mapping incluye source ID, tipo, path, símbolo, fingerprint, target rule ID, versión, target path, equivalence type, estado, pruebas y notas.

### 3.4 Migrar solo el lote

Cada regla debe cumplir schema EIS, tener identidad, versión, metadata, lifecycle no autoritativo, checksum si aplica y no cambiar scoring.

### 3.5 Validación del manifest

Validar campos, enums, paths, fingerprints, targets, duplicados, versiones, estado, checksums, referencias y reglas extra/faltantes.

### 3.6 Comparación SHADOW

Ejecutar casos controlados, obtener resultado legacy/EIS, normalizar, comparar y diagnosticar sin modificar el flujo productivo.

### 3.7 Diagnósticos

Incluir lote, fuentes, migradas, válidas, bloqueadas, diferencias, equivalencia, checksums, duración, errores y estado final.

### 3.8 Documentación

Documentar lote, manifest, mapping, validación, rollback, limitaciones y relación con EIS-40C.3.

## 4. Allowed Changes

- módulo de migración;
- pruebas de migración;
- reglas del lote bajo `Backend/rules/eis/`;
- manifests/mappings;
- documentación;
- requirements solo con necesidad real;
- packaging solo si es necesario.

## 5. Forbidden Changes

No modificar scoring, penalizaciones, `ResultJson`, APIs, frontend, narrativa, Vendor Comparison, prompts productivos, motores legacy, reglas legacy, startup, base de datos ni fases posteriores.

## 6. Reglas de implementación

Reutilizar EIS-40C.1, type hints, modelos serializables, sin eval/exec/imports dinámicos, sin LLM, sin inventar campos, sin reemplazos silenciosos, orden determinista, copias defensivas y diagnósticos sanitizados.

## 7. Ambigüedad

Marcar `BLOCKED` o `DEFERRED`, no crear regla activa, registrar motivo y respetar fail closed.

## 8. Pruebas mínimas

- manifest válido/inválido;
- source inexistente;
- fingerprint mismatch;
- target inexistente/extra;
- duplicado;
- versión conflictiva;
- mapping válido;
- transformación determinista;
- metadata;
- lifecycle no autoritativo;
- comparación equivalente/no equivalente;
- normalización;
- orden;
- rollback;
- diagnóstico;
- no impacto.

## 9. Definition of implementation complete

Módulo, manifest, lote, mappings, reglas, comparación, pruebas, lint focalizado, no impacto y reporte técnico.

## 10. Entrega de Stage 04

1. resumen;
2. lote;
3. arquitectura;
4. creados;
5. modificados;
6. mappings;
7. configuración;
8. seguridad;
9. pruebas;
10. comandos;
11. resultados;
12. riesgos;
13. desviaciones;
14. confirmación:

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

No hacer commit, push ni merge.
