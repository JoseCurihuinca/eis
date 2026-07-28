# EIS-40C.2 — Rule Migration

## 1. Propósito

Migrar reglas seleccionadas desde los motores y configuraciones actuales hacia el catálogo gobernado creado en EIS-40C.1, manteniendo equivalencia funcional, trazabilidad, reversibilidad y operación no autoritativa.

EIS-40C.2 no promueve el Rule Engine a modo autoritativo. La fase prepara un catálogo gobernado equivalente y validado para que fases posteriores puedan certificar equivalencia y, eventualmente, promover el motor.

## 2. Estado inicial requerido

Antes de ejecutar este EIS debe estar cerrado y aprobado EIS-40C.1 — Rule Governance Foundation.

Deben existir, al menos:

- `Backend/app/rule_engine/`;
- `Backend/rules/eis/`;
- loader YAML seguro;
- validadores estructural y semántico;
- registry;
- diagnósticos;
- checksum SHA-256;
- modos `DISABLED` y `SHADOW`;
- pruebas específicas de la fundación.

## 3. Alcance

Este EIS cubre:

- descubrimiento de reglas actuales;
- clasificación por fuente, dominio y criticidad;
- selección explícita del lote de migración;
- definición de formato canónico;
- transformación hacia YAML gobernado;
- trazabilidad source-to-target;
- coexistencia con reglas legacy;
- ejecución comparativa en `SHADOW`;
- validación de no impacto;
- rollback y retiro controlado de artefactos defectuosos.

## 4. Fuera de alcance

No corresponde en esta fase:

- reemplazar los motores actuales;
- eliminar reglas legacy;
- modificar scoring;
- alterar penalizaciones;
- cambiar `ResultJson`;
- cambiar contratos públicos;
- modificar frontend;
- cambiar narrativa;
- ejecutar reglas EIS como fuente autoritativa;
- promover el Rule Engine;
- migrar todas las reglas sin lote aprobado;
- avanzar a EIS-40C.3 o EIS-40C.4.

## 5. Principios obligatorios

1. No impacto productivo.
2. Migración por lote.
3. Trazabilidad completa.
4. Equivalencia observable.
5. Reversibilidad.
6. Fail closed.
7. Operación no autoritativa.
8. Sin duplicados silenciosos.
9. Sin interpretación inventada.
10. Evidencia verificable.

## 6. Orden de ejecución

1. `01-Context.md`
2. `02-Repository.md`
3. `03-Architecture.md`
4. `04-Implementation.md`
5. `05-Validation.md`
6. `06-Report.md`

Cada etapa requiere revisión manual antes de continuar.

## 7. Gates

### Gate 01 — Contexto aprobado

Se entiende qué reglas se migrarán, qué fuentes son autoritativas actualmente, qué motores no deben tocarse y qué significa equivalencia.

### Gate 02 — Repository Discovery aprobado

Existe inventario real de motores, archivos, clases, constantes, prompts, configuraciones, reglas efectivas, dependencias y pruebas.

### Gate 03 — Arquitectura aprobada

Existe diseño explícito de catálogo destino, mapeo legacy → canónico, manifest, comparación SHADOW y rollback.

### Gate 04 — Implementación aprobada

Solo se implementó el lote permitido.

### Gate 05 — Validación aprobada

Se demostró validez, equivalencia, no impacto, packaging y reversibilidad.

### Gate 06 — Certificación

Se genera el informe final y se cierra EIS-40C.2.

## 8. Criterios de detención

Detener si:

- EIS-40C.1 no está completo;
- no puede identificarse la fuente real de una regla;
- una regla legacy tiene semántica ambigua;
- una migración requiere modificar scoring o contratos públicos;
- existen conflictos de `rule_id` o versión;
- el lote no tiene aprobación;
- la comparación SHADOW produce diferencias no explicadas;
- la suite existente falla por esta fase;
- el catálogo destino no queda empaquetado;
- se requiere habilitar modo autoritativo.

## 9. Resultado esperado

Al finalizar debe existir:

- inventario de reglas legacy;
- lote aprobado;
- manifest source-to-target;
- reglas YAML gobernadas;
- adaptadores mínimos de transformación/comparación;
- diagnósticos de equivalencia;
- pruebas de migración;
- evidencia de no impacto;
- reporte final.

## 10. Estado objetivo

```text
Legacy Rules
    ↓ inventory
Migration Manifest
    ↓ transformation
Governed YAML Rules
    ↓ secure loading
Shadow Comparison
    ↓ diagnostics
Certified Migration Batch

Authoritative execution: NO
Legacy removal: NO
Production behavior change: NO
```
