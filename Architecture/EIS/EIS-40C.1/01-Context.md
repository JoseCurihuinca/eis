# EIS-40C.1 — Contexto, alcance y restricciones

## 1. Contexto

La plataforma RFP-AI-AGENT ya dispone de capacidades de extracción, evaluación determinista, recuperación híbrida, grounded assessment y generación de contexto de decisión.

El siguiente paso no consiste en agregar nuevas capacidades narrativas, sino en consolidar el comportamiento determinista en una infraestructura formal de reglas. La finalidad de EIS-40C.1 es crear la base de gobierno que permitirá posteriormente migrar handlers, certificar equivalencia y promover el Rule Engine.

## 2. Arquitectura previa simplificada

```text
Documentos
    ↓
Extracción
    ↓
Motores deterministas
    ↓
Grounded Assessment
    ↓
Decision Context
    ↓
LLM
    ↓
Narrativa
```

Los motores deterministas existentes pueden estar implementados mediante funciones, handlers, servicios o reglas embebidas en código.

## 3. Problema a resolver

La lógica determinista distribuida presenta riesgos:

- reglas difíciles de descubrir;
- duplicación;
- ausencia de ciclo de vida;
- trazabilidad incompleta;
- cambios no gobernados;
- validación desigual;
- dificultad para certificar equivalencia;
- dependencia de conocimiento implícito en código.

## 4. Objetivo de EIS-40C.1

Crear una fundación reusable para administrar reglas como activos gobernados.

El resultado debe permitir:

- identificar cada regla de forma única;
- versionarla;
- clasificarla;
- validar su estructura;
- determinar su estado;
- cargarla desde archivos;
- registrar diagnósticos;
- consultarla desde servicios internos;
- mantenerla desactivada o no autoritativa hasta fases posteriores.

## 5. Fuera de alcance

Queda fuera de alcance:

- reemplazar handlers existentes;
- ejecutar reglas sobre evaluaciones productivas;
- modificar scores;
- modificar penalizaciones;
- modificar recomendaciones;
- modificar hallazgos persistidos;
- modificar esquemas públicos de respuesta;
- modificar `ResultJson`;
- modificar frontend;
- modificar Vendor Comparison;
- modificar generación narrativa;
- agregar nuevos modelos de IA;
- implementar vector retrieval;
- implementar traceability graph completo;
- modificar CI/CD salvo lo mínimo indispensable para empaquetar archivos de reglas, y solo si el repositorio demuestra que es necesario.

## 6. Compatibilidad obligatoria

La implementación debe ser backward compatible.

Después de la fase:

- las mismas entradas deben producir las mismas salidas productivas;
- las APIs existentes deben mantener sus contratos;
- los scores deben permanecer sin cambios;
- no deben aparecer nuevos gaps productivos;
- no debe cambiar la recomendación de estado;
- el Rule Engine no debe intervenir en la ruta de decisión productiva.

## 7. Modo operativo

El Rule Engine debe quedar en uno de estos modos equivalentes:

```text
DISABLED
```

o

```text
SHADOW / NON_AUTHORITATIVE
```

La elección debe adaptarse a la configuración real del repositorio, manteniendo siempre el principio de no impacto.

## 8. Gobierno mínimo de una regla

Toda regla debe tener como mínimo:

- `rule_id`;
- `name`;
- `description`;
- `version`;
- `status`;
- `domain`;
- `severity` o clasificación equivalente;
- `source`;
- `created_at` o metadato equivalente;
- `updated_at` o metadato equivalente;
- criterios o condiciones;
- acción o resultado esperado;
- metadatos de trazabilidad.

## 9. Estados de ciclo de vida

El modelo debe soportar al menos:

- `DRAFT`
- `ACTIVE`
- `DEPRECATED`
- `RETIRED`

Reglas en `DRAFT`, `DEPRECATED` o `RETIRED` no deben quedar disponibles como reglas ejecutables activas.

La fase puede registrar reglas `ACTIVE`, pero no debe conectarlas a decisiones productivas.

## 10. Requisitos de seguridad

- usar `yaml.safe_load` o equivalente seguro;
- prohibir tipos arbitrarios de Python;
- prohibir ejecución de expresiones;
- limitar tamaño y cantidad de archivos cuando sea razonable;
- normalizar rutas;
- impedir path traversal;
- validar extensiones permitidas;
- detectar identificadores duplicados;
- validar checksum cuando se configure;
- registrar errores sin exponer secretos;
- fallar de forma cerrada.

## 11. Requisitos de calidad

- tipado explícito;
- errores de dominio definidos;
- separación de responsabilidades;
- pruebas de casos válidos e inválidos;
- mensajes de diagnóstico accionables;
- sin dependencias innecesarias;
- sin cambios masivos fuera del módulo;
- documentación de configuración.

## 12. Preparación para fases posteriores

### EIS-40C.2 — Rule Migration
Migración gradual de handlers a reglas.

### EIS-40C.3 — Rule Equivalence
Comparación y certificación del comportamiento nuevo versus legacy.

### EIS-40C.4 — Rule Engine Promotion
Promoción controlada del Rule Engine a fuente autoritativa.

No se debe anticipar implementación propia de esas fases.
