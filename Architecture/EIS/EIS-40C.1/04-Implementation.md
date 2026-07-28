# EIS-40C.1 — Tareas de implementación

## 1. Regla de ejecución

Completar las tareas en orden. No avanzar a una tarea dependiente si la anterior no cumple sus pruebas mínimas.

## 2. Task 1 — Preparar estructura

Crear, según convenciones reales:

```text
Backend/app/rule_engine/
Backend/rules/
Backend/tests/rule_engine/
```

Agregar `__init__.py` donde corresponda.

No crear una carpeta top-level `Rules/`.

## 3. Task 2 — Definir enums

Definir enums para:

- `RuleStatus`;
- `RuleDomain`;
- `RuleSeverity`;
- `RuleSourceType`;
- `RuleEngineMode`.

Valores mínimos:

```text
RuleStatus:
DRAFT
ACTIVE
DEPRECATED
RETIRED
```

Modo mínimo:

```text
DISABLED
SHADOW
```

No agregar `AUTHORITATIVE` como modo operativo activo en esta fase, salvo como valor futuro no seleccionable y claramente documentado.

## 4. Task 3 — Definir modelos

Crear modelos para:

- `RuleSource`;
- `RuleMetadata`;
- `RuleCondition`;
- `RuleAction`;
- `RuleDefinition`;
- `RuleFileRecord`;
- `RuleValidationIssue`;
- `RuleLoadDiagnostic`;
- `RuleRegistrySnapshot`.

Requisitos:

- tipado estricto;
- defaults seguros;
- validación de campos obligatorios;
- fechas normalizadas;
- versiones validadas;
- objetos serializables;
- sin lógica productiva de scoring.

## 5. Task 4 — Definir contratos

Crear contratos para:

```python
RuleLoader
RuleValidator
RuleRegistry
RuleDiagnosticsProvider
```

Cada contrato debe documentar:

- entradas;
- salidas;
- errores;
- comportamiento fail closed;
- garantía de no ejecución.

## 6. Task 5 — Implementar settings

Integrar configuración con el mecanismo real del backend.

Parámetros:

- `RULE_ENGINE_MODE`;
- `RULES_ROOT`;
- `RULE_ALLOWED_EXTENSIONS`;
- `RULE_MAX_FILE_SIZE_BYTES`;
- `RULE_MAX_FILES`;
- `RULE_REQUIRE_CHECKSUM`;
- `RULE_FAIL_CLOSED`.

Defaults recomendados:

```text
RULE_ENGINE_MODE=DISABLED
RULES_ROOT=<Backend>/rules
RULE_ALLOWED_EXTENSIONS=.yaml,.yml
RULE_REQUIRE_CHECKSUM=false
RULE_FAIL_CLOSED=true
```

No usar rutas absolutas hardcodeadas.

## 7. Task 6 — Implementar checksum

Crear utilidad SHA-256 para:

- bytes;
- archivos;
- comparación segura;
- reporte hexadecimal normalizado.

Agregar pruebas de contenido igual, contenido distinto y archivo inexistente.

## 8. Task 7 — Implementar secure loader

El loader debe:

1. resolver raíz;
2. validar existencia;
3. enumerar archivos permitidos;
4. aplicar límites;
5. impedir path traversal;
6. leer bytes;
7. calcular checksum;
8. cargar YAML de forma segura;
9. normalizar documento;
10. enviar resultado al validator;
11. acumular diagnósticos;
12. omitir registro de inválidos.

El loader no debe:

- usar `yaml.load` inseguro;
- usar `eval`;
- ejecutar operadores;
- importar módulos indicados por YAML;
- aceptar clases arbitrarias;
- seguir rutas fuera de la raíz.

## 9. Task 8 — Implementar structural validator

Validar:

- tipo raíz;
- `rule_id`;
- nombre;
- descripción;
- versión;
- estado;
- dominio;
- severidad;
- source;
- conditions;
- actions;
- metadata.

Cada error debe incluir:

- código;
- mensaje;
- ruta del campo;
- severidad del diagnóstico;
- archivo de origen.

## 10. Task 9 — Implementar semantic validator

Validar:

- patrón de `rule_id`;
- versión;
- fechas;
- coherencia de lifecycle;
- campos requeridos para reglas `ACTIVE`;
- dominio y severidad soportados;
- ausencia de operadores no permitidos;
- duplicados dentro del mismo archivo;
- combinaciones inválidas.

No ejecutar las condiciones.

## 11. Task 10 — Implementar registry

Funciones mínimas:

```python
register(rule)
register_many(rules)
get(rule_id, version=None)
list_all()
list_by_domain(domain)
list_by_status(status)
contains(rule_id, version=None)
snapshot()
clear()
```

Condiciones:

- prevenir duplicados;
- proteger estado interno;
- no reemplazar silenciosamente;
- registrar solo reglas válidas;
- mantener orden determinista cuando se serializa.

## 12. Task 11 — Implementar diagnostics

Crear un resultado de carga con:

- timestamp;
- duración;
- root;
- archivos descubiertos;
- archivos procesados;
- archivos rechazados;
- reglas válidas;
- reglas rechazadas;
- warnings;
- errores;
- duplicados;
- checksums;
- estado final.

Debe ser apto para logs y pruebas.

## 13. Task 12 — Crear schema documentado

Agregar un esquema o documento en:

```text
Backend/rules/schema/
```

Puede ser:

- JSON Schema;
- YAML schema documentado;
- Markdown normativo.

Debe cubrir todos los campos obligatorios y enums.

## 14. Task 13 — Crear reglas de ejemplo

Agregar ejemplos no productivos:

```text
Backend/rules/examples/
```

Mínimo:

- una regla válida `DRAFT`;
- una regla inválida para pruebas, preferiblemente bajo fixtures y no en runtime;
- una regla válida `DEPRECATED` opcional.

No crear reglas `ACTIVE` conectadas a evaluación.

## 15. Task 14 — Integración no autoritativa

Integrar el módulo solamente para:

- inicialización opcional;
- validación;
- diagnósticos;
- disponibilidad interna.

No invocar el registry desde:

- scoring;
- postprocessor;
- ai evaluator;
- vendor comparison;
- narrativa;
- cálculo de madurez;
- generación de gaps productivos.

## 16. Task 15 — Packaging

Verificar que `Backend/rules/` se incluya en:

- artefacto;
- imagen Docker;
- despliegue;
- ejecución local.

Modificar Dockerfile, scripts o pipeline solo si es indispensable.

Documentar cualquier cambio.

## 17. Task 16 — Logging

Agregar logs estructurados usando el logger existente.

Eventos mínimos:

- rules_load_started;
- rules_load_completed;
- rules_load_failed;
- rule_rejected;
- duplicate_rule_detected;
- checksum_mismatch.

No exponer contenido completo.

## 18. Task 17 — Documentación

Crear `Backend/rules/README.md` con:

- propósito;
- ubicación;
- formato;
- lifecycle;
- seguridad;
- cómo validar;
- cómo agregar una regla;
- prohibición de activar reglas sin proceso de gobierno;
- relación con EIS posteriores.

## 19. Task 18 — Pruebas

Crear pruebas unitarias y de integración descritas en `05-Validation.md`.

## 20. Task 19 — Limpieza

Antes de finalizar:

- eliminar código muerto creado durante la implementación;
- verificar imports;
- ejecutar formatter/linter;
- ejecutar pruebas;
- revisar diff;
- confirmar que no hay cambios fuera de alcance;
- confirmar que no cambió comportamiento productivo.
