# EIS-40C.2 — Arquitectura objetivo de migración

## 1. Objetivo

Definir cómo migrar reglas legacy al catálogo gobernado sin alterar la ejecución productiva.

## 2. Arquitectura objetivo

```text
Legacy Rule Sources
        ↓ discovery
Rule Inventory
        ↓ approval
Migration Manifest
        ↓ deterministic transformation
Governed YAML Rules
        ↓ EIS-40C.1 Loader
Structural + Semantic Validation
        ↓
In-Memory Registry
        ↓
Shadow Comparison Adapter
        ↓
Migration Diagnostics
```

La ejecución legacy continúa siendo la fuente productiva.

## 3. Componentes

### 3.1 Rule Inventory

Incluye source ID, tipo, ruta, símbolo, fingerprint, dominio, estado y notas.

### 3.2 Migration Manifest

Debe declarar lote, versión, fecha, responsables, reglas incluidas, mapeo source-target, estado, checksum, rollback y exclusiones.

Ubicación sugerida:

```text
Backend/rules/migrations/eis-40c2/
    manifest.yaml
    mappings.yaml
    checksums/
```

### 3.3 Canonical Rule Mapper

Transforma una regla legacy analizada a estructura canónica. No infiere campos faltantes, no inventa severidad, no usa LLM y no muta scoring.

### 3.4 Migration Validator

Valida fuente, fingerprint, identidad, versión, metadata, referencias, checksum y estado del lote.

### 3.5 Shadow Comparison Adapter

Compara decisión legacy y EIS para casos controlados, incluyendo severidad, categoría, condición y resultado normalizado. No modifica respuestas productivas.

### 3.6 Migration Diagnostics

Informa descubiertas, aprobadas, migradas, validadas, diferidas, bloqueadas, conflictos, diferencias, duración, checksums y archivos.

## 4. Modelos mínimos

### MigrationSource

- `source_id`
- `source_type`
- `source_path`
- `symbol`
- `fingerprint`
- `domain`
- `description`

### MigrationTarget

- `rule_id`
- `version`
- `target_path`
- `checksum`
- `status`

### MigrationMapping

- `source`
- `target`
- `equivalence_type`
- `migration_status`
- `approved_by`
- `approved_at`
- `notes`

### ComparisonResult

- `case_id`
- `legacy_result`
- `eis_result`
- `equivalent`
- `difference_type`
- `details`

## 5. Identidad y versionado

`rule_id` estable; versiones explícitas; cambios semánticos requieren nueva versión; no reemplazo silencioso; un fingerprint no puede mapearse a múltiples reglas activas.

## 6. Trazabilidad

Cada YAML migrado debe incluir source type, path, symbol, fingerprint, EIS, batch, fecha, owner, rationale y referencias de pruebas.

## 7. Estrategia de equivalencia

- exacta;
- funcional;
- diferencia aceptada;
- no equivalente.

## 8. Orden determinista

Orden por dominio, prioridad, `rule_id` y versión. Si el legacy depende del orden, se preserva y documenta.

## 9. Lifecycle

Las reglas comienzan en estado no autoritativo, como `DRAFT` o `SHADOW`, según enums reales.

## 10. Rollback

Marcar mapping como `REJECTED` o `ROLLED_BACK`, retirar la regla del lote, mantener fuente legacy y preservar evidencia.

## 11. Seguridad

YAML seguro, paths validados, checksums, manifest validado, sin imports dinámicos, sin eval/exec, sin secretos, límites, fail closed y diagnósticos sanitizados.

## 12. Integración

Sin endpoints públicos. La comparación puede ser servicio interno, utilidad offline o fixture de integración. Evitar startup productivo.

## 13. Expected Repository

```text
Backend/
  app/
    rule_engine/
    rule_migration/
      __init__.py
      enums.py
      models.py
      contracts.py
      inventory.py
      manifest.py
      mapper.py
      validator.py
      comparator.py
      diagnostics.py
      exceptions.py
  rules/
    eis/
    migrations/
      eis-40c2/
        manifest.yaml
        mappings.yaml
  tests/
    rule_migration/
      test_inventory.py
      test_manifest.py
      test_mapper.py
      test_comparator.py
      test_integration.py
```

Adaptar al repositorio real sin duplicaciones.

## 14. Criterio de aprobación

Lote definido, trazabilidad, manifest, mapper, comparación SHADOW, rollback, estructura, restricciones, riesgos y cero autoridad.
