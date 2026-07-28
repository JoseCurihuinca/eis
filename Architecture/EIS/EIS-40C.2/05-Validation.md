# EIS-40C.2 — Validación, equivalencia y criterios de aceptación

## 1. Objetivo

Demostrar que el lote migrado es trazable, válido, equivalente, reversible, no autoritativo y compatible.

## 2. Pruebas unitarias obligatorias

### 2.1 Inventory

- fuente válida/inexistente;
- símbolo inexistente;
- fingerprint estable/distinto;
- clasificación;
- regla ambigua;
- regla fuera de alcance.

### 2.2 Manifest

- válido;
- falta `batch_id`;
- estado/versión inválidos;
- mapping incompleto;
- duplicado;
- target fuera de root;
- checksum inválido;
- exclusión inválida;
- lote vacío;
- YAML inseguro.

### 2.3 Mapping

- 1:1;
- adaptable;
- composite diferido;
- source/target duplicado;
- versión distinta;
- fingerprint mismatch;
- destino inexistente/extra;
- metadata incompleta.

### 2.4 Mapper

- transformación determinista;
- campos obligatorios;
- no inventa severidad ni metadata;
- preserva dominio, condición, resultado y orden;
- lifecycle no autoritativo;
- normalización.

### 2.5 Comparator

- equivalencia exacta/funcional;
- diferencias de condición, severidad, resultado y orden;
- serialización;
- errores por caso;
- repetibilidad.

### 2.6 Diagnostics

Conteos, lote, migradas, validadas, diferidas, bloqueadas, equivalentes, no equivalentes, checksums, duración, archivos y ausencia de secretos.

## 3. Pruebas de integración

### 3.1 Lote válido

Manifest, mappings, reglas, registry, diagnósticos y orden determinista.

### 3.2 Lote mixto

Regla válida, mapping inválido, fingerprint mismatch, target extra y duplicado. Verificar fail closed y no impacto.

### 3.3 Comparación SHADOW

Legacy como fuente; EIS en paralelo; normalización; diferencias registradas; score, respuesta y findings sin cambios.

### 3.4 DISABLED

Backend inicia; migración no se ejecuta; catálogo no afecta APIs; ausencia de reglas no rompe producción.

### 3.5 Packaging

Reglas, manifest, mappings y checksums presentes; paths y permisos; pruebas inseguras excluidas.

### 3.6 Rollback

Retiro del lote, fuentes legacy continúan, startup no se rompe, trazabilidad preservada.

## 4. Regresión

Suite completa, sin reducir pruebas, sin skips ocultos, sin cambios de snapshots, scores, respuestas, findings ni APIs.

## 5. Validación estática

Compile/import, formatter, linter, type checker, scanner, coverage si existe, búsqueda de acoplamientos y `git diff --check`.

## 6. Seguridad

YAML seguro, sin eval/exec/imports dinámicos, path traversal protegido, checksums, límites, diagnósticos sin secretos, manifest/mappings no ejecutables, fail closed y duplicados bloqueados.

## 7. Equivalencia

Estados:

- `EQUIVALENT`;
- `EQUIVALENT_WITH_NORMALIZATION`;
- `ACCEPTED_DIFFERENCE`;
- `NOT_EQUIVALENT`;
- `BLOCKED`.

No puede haber `NOT_EQUIVALENT` sin resolución. `ACCEPTED_DIFFERENCE` requiere justificación. Informar porcentaje.

## 8. Aceptación funcional

Inventario, lote, manifest, mappings, reglas, fingerprints, duplicados, SHADOW, diagnósticos, rollback, legacy operativo y producción sin cambios.

## 9. Aceptación arquitectónica

Módulo separado, reutiliza EIS-40C.1, sin acoplar scoring/frontend/LLM, sin modificar legacy, rutas configurables, orden, packaging y preparación para EIS-40C.3.

## 10. Compatibilidad

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

## 11. Definition of Done

Inventario, lote, manifest, mappings, reglas, comparación, diferencias resueltas, suites, lint focalizado, packaging, rollback, documentación y reporte.

## 12. Fallos aceptables

Reglas diferidas fuera del lote, sin endpoint/UI, registry no persistente, comparación offline y deuda previa no relacionada.

## 13. Fallos bloqueantes

Regla no equivalente presentada como equivalente, cambio productivo, YAML/manifest inseguro, path traversal, duplicado, fingerprint ignorado, legacy eliminado, modo autoritativo, suite rota, artefactos fuera del package o loader sin fail closed.

## 14. Evidencia mínima

Matriz por regla, source/target, fingerprints, checksums, resultados legacy/EIS, porcentaje, comandos, pruebas, packaging, no impacto, rollback y riesgos.

## 15. Resultado esperado

Estado: `APROBADO`, `APROBADO CON OBSERVACIONES` o `BLOQUEADO`; recomendación para Stage 06 o repetición.
