# EIS-40C.1 — Arquitectura objetivo

## 1. Visión

La fase debe introducir una capa de gobierno de reglas independiente de los motores productivos.

```text
Rule Files
    ↓
Secure Rule Loader
    ↓
Structural Validator
    ↓
Semantic Validator
    ↓
Rule Registry
    ↓
Diagnostics / Queries

        no authoritative execution
```

## 2. Componentes

### 2.1 Rule Contracts

Define interfaces o protocolos estables para:

- regla;
- loader;
- validator;
- registry;
- diagnostics provider.

Los contratos no deben acoplarse a FastAPI ni a persistencia.

### 2.2 Rule Models

Representan:

- metadatos;
- condiciones;
- acciones;
- versión;
- estado;
- origen;
- clasificación;
- trazabilidad;
- checksum;
- diagnósticos.

Usar el mecanismo de modelos predominante en el backend, por ejemplo dataclasses o Pydantic.

### 2.3 Rule Loader

Responsable de:

- localizar archivos;
- validar rutas;
- aceptar extensiones permitidas;
- leer contenido;
- usar carga YAML segura;
- capturar errores;
- devolver objetos intermedios;
- no registrar reglas inválidas.

No debe ejecutar la regla.

### 2.4 Rule Validator

Debe separar:

#### Validación estructural

- campos obligatorios;
- tipos;
- valores enum;
- formato de versión;
- identificador;
- estructura de condiciones y acciones.

#### Validación semántica

- consistencia de estado;
- compatibilidad de severidad;
- coherencia de dominio;
- fechas;
- duplicidad;
- transición de lifecycle;
- restricciones de reglas activas.

### 2.5 Rule Registry

Responsable de:

- registrar reglas válidas;
- indexar por `rule_id`;
- consultar por dominio;
- consultar por estado;
- consultar por versión;
- detectar duplicados;
- impedir reemplazo silencioso;
- exponer snapshot inmutable o copia defensiva.

### 2.6 Diagnostics

Debe proporcionar:

- total de archivos;
- total de reglas válidas;
- total de reglas rechazadas;
- errores por archivo;
- warnings;
- duplicados;
- checksums;
- tiempo de carga;
- origen;
- estado del registry.

Los diagnósticos no deben exponer secretos ni contenido sensible completo.

### 2.7 Settings

Configuración mínima:

- `RULES_ROOT`;
- modo del engine;
- extensiones permitidas;
- tamaño máximo opcional;
- cantidad máxima opcional;
- política de checksum;
- comportamiento fail closed;
- logging.

Debe integrarse al mecanismo de settings existente.

## 3. Modelo conceptual de regla

Ejemplo orientativo:

```yaml
rule_id: SEC-SECRET-001
name: Secret management required
description: Verifica que el RFP exija gestión empresarial de secretos.
version: 1.0.0
status: DRAFT
domain: SECURITY
severity: MAJOR
source:
  type: CORPORATE_STANDARD
  reference: Global Security Policy
conditions:
  any:
    - field: content
      operator: contains_any
      values:
        - key vault
        - secret manager
actions:
  finding_type: GAP_RFP
  message: El RFP no exige una capacidad empresarial de gestión de secretos.
metadata:
  owner: architecture
  created_at: 2026-07-01
  updated_at: 2026-07-01
```

Este ejemplo no debe conectarse a scoring ni evaluación productiva.

## 4. Identificadores

`rule_id` debe:

- ser obligatorio;
- ser estable;
- ser único;
- usar un patrón documentado;
- no depender del nombre de archivo.

Patrón recomendado:

```text
<DOMINIO>-<CAPACIDAD>-<SECUENCIA>
```

Ejemplo:

```text
SEC-SECRET-001
```

## 5. Versionado

Usar SemVer o un formato equivalente ya utilizado en el repositorio.

Una combinación de `rule_id` y `version` debe ser única.

El registry debe impedir:

- dos reglas activas con el mismo `rule_id` y la misma versión;
- reemplazos silenciosos;
- versiones inválidas.

## 6. Lifecycle

Transiciones mínimas válidas:

```text
DRAFT → ACTIVE
ACTIVE → DEPRECATED
DEPRECATED → RETIRED
```

No se requiere implementar workflow administrativo completo.

Sí se requiere validar estados y evitar que reglas no activas se presenten como activas.

## 7. Inmutabilidad

Después de registrar una regla:

- no debe modificarse por referencia externa;
- las consultas deben devolver objetos inmutables o copias;
- el registry debe proteger sus índices internos.

## 8. Errores de dominio

Crear excepciones específicas, por ejemplo:

- `RuleEngineError`
- `RuleLoadError`
- `RuleValidationError`
- `DuplicateRuleError`
- `UnsafeRulePathError`
- `RuleChecksumError`
- `RuleRegistryStateError`

Adaptar nombres a las convenciones existentes.

## 9. Observabilidad

Registrar como mínimo:

- inicio de carga;
- ruta lógica;
- cantidad de archivos;
- reglas válidas;
- reglas rechazadas;
- duración;
- error resumido.

No registrar:

- secretos;
- contenido completo de archivos;
- información personal;
- stack traces en respuestas públicas.

## 10. Integración

El módulo puede inicializarse durante startup o mediante lazy loading.

Condiciones:

- no debe bloquear producción por ausencia de reglas cuando el modo sea `DISABLED`;
- debe fallar cerrado si se configura explícitamente carga obligatoria;
- no debe invocarse desde scoring;
- no debe modificar respuestas públicas;
- no debe agregar endpoints públicos salvo que el repositorio ya tenga un patrón interno y el cambio sea estrictamente diagnóstico.

## 11. Rendimiento

Objetivos razonables para un catálogo inicial:

- carga lineal respecto del número de archivos;
- índices O(1) por `rule_id`;
- consultas por dominio mediante índice;
- no releer archivos en cada evaluación;
- evitar hashing repetido innecesario;
- conservar diagnósticos de la última carga.

## 12. Seguridad de rutas

El loader debe:

- resolver `RULES_ROOT`;
- normalizar rutas;
- verificar que cada archivo permanezca bajo la raíz;
- rechazar enlaces o rutas que escapen de la raíz cuando aplique;
- aceptar únicamente `.yaml` y `.yml`;
- ignorar archivos ocultos o temporales según política documentada.

## 13. Checksums

Debe existir capacidad para calcular checksum SHA-256.

Uso mínimo:

- registrar checksum por archivo;
- permitir validación contra checksum esperado si la configuración lo exige;
- reportar discrepancias;
- no considerar válido un archivo que falle una política obligatoria.
