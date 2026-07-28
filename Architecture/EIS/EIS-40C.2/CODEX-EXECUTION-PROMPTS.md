# EIS-40C.2 — Prompts de ejecución por Stage

## STAGE 01 — Context

```text
EIS-40C.2 — STAGE 01: CONTEXT

Lee únicamente:
Architecture/EIS/EIS-40C.2/01-Context.md

Comprende propósito, alcance, restricciones, dudas y contradicciones.
NO inspecciones el repositorio en profundidad.
NO modifiques archivos.
NO generes código.
NO avances automáticamente.

Entrega contexto, alcance, fuera de alcance, restricciones, riesgos, dudas y recomendación.
No hagas commit, push ni merge.
```

## STAGE 02 — Repository Discovery

```text
EIS-40C.2 — STAGE 02: REPOSITORY DISCOVERY

Lee únicamente:
Architecture/EIS/EIS-40C.2/02-Repository.md

Ejecuta discovery y análisis.
Puedes inspeccionar todo el repositorio.
NO modifiques archivos.
NO escribas código.
NO crees reglas YAML.
NO migres reglas.
NO avances automáticamente.

Entrega estructura, fuentes, inventario, lote recomendado, exclusiones, dependencias, conflictos, riesgos, archivos potenciales y estado.
Detente ante contradicción bloqueante.
No hagas commit, push ni merge.
```

## STAGE 03 — Architecture

```text
EIS-40C.2 — STAGE 03: ARCHITECTURE

Lee únicamente:
Architecture/EIS/EIS-40C.2/03-Architecture.md

Usa Stages 01 y 02 aprobados.
Diseña exclusivamente arquitectura de migración.
NO implementes.
NO modifiques archivos.
NO crees reglas.
NO avances automáticamente.

Entrega lote final, arquitectura, modelos, manifest, mappings, SHADOW, trazabilidad, rollback, seguridad, estructura, riesgos y autorización/bloqueo.
No hagas commit, push ni merge.
```

## STAGE 04 — Implementation

```text
EIS-40C.2 — STAGE 04: IMPLEMENTATION

Lee y ejecuta exclusivamente:
Architecture/EIS/EIS-40C.2/04-Implementation.md

Usa Stages 01–03 y lote aprobado.
Revisa worktree y no reviertas cambios ajenos.
Implementa solo módulo, manifest, mappings, reglas del lote, comparación SHADOW, diagnósticos, pruebas y documentación mínima.

NO modificar scoring, ResultJson, APIs, frontend, narrativa, Vendor Comparison, prompts productivos, motores/reglas legacy ni startup.
NO eliminar legacy.
NO habilitar autoridad.
NO avanzar a EIS-40C.3.

Entrega resumen, lote, arquitectura, archivos, mappings, configuración, seguridad, pruebas, comandos, resultados, riesgos, desviaciones y confirmación de no impacto.
No hagas commit, push ni merge.
```

## STAGE 05 — Validation

```text
EIS-40C.2 — STAGE 05: VALIDATION AND CORRECTION

Lee y ejecuta exclusivamente:
Architecture/EIS/EIS-40C.2/05-Validation.md

Stages 01–04 completados.
Valida inventario, manifest, mappings, reglas, SHADOW, equivalencia, packaging, rollback y no impacto.
Construye matriz, ejecuta pruebas, corrige solo defectos EIS-40C.2 y repite.

No modifiques scoring, ResultJson, APIs, frontend, narrativa, Vendor Comparison, prompts productivos, motores ni reglas legacy.
No declares aprobado ante diferencias no resueltas, cambios productivos, inseguridad, duplicados, fingerprint ignorado, packaging/rollback fallido, legacy eliminado o autoridad.

Entrega estado, matriz, defectos, correcciones, archivos, comandos, resultados, equivalencia, packaging, rollback, riesgos, confirmación y recomendación.
No generes Stage 06 todavía.
No avances a EIS-40C.3.
No hagas commit, push ni merge.
```

## STAGE 06 — Final Certification

```text
EIS-40C.2 — STAGE 06: FINAL CERTIFICATION REPORT

Lee y ejecuta exclusivamente:
Architecture/EIS/EIS-40C.2/06-Report.md

Stages 01–05 completados.
Certifica formalmente EIS-40C.2.
NO crear ni modificar código.
NO agregar pruebas.
NO corregir deuda.
NO eliminar legacy.
NO promover Rule Engine.
NO avanzar a EIS-40C.3.

Verifica inventario, lote, manifest, mappings, reglas, pruebas, equivalencia, packaging, rollback, no impacto y no autoridad.
Entrega exclusivamente el reporte final.
No hagas commit, push ni merge.
```
