# EIS-40C.1 — Repository Discovery y Repository Analysis

## 1. Regla general

Codex debe inspeccionar el repositorio antes de modificarlo.

No debe asumir rutas, frameworks, nombres de módulos, configuración, comandos de prueba ni mecanismos de empaquetado.

## 2. Repository Discovery

Inspeccionar como mínimo:

### 2.1 Estructura general

- raíz del repositorio;
- `Backend/`;
- `Frontend/`;
- `Architecture/`;
- archivos de pipeline;
- Dockerfiles;
- `.dockerignore`;
- `.gitignore`;
- scripts de build;
- scripts de startup;
- archivos de dependencias.

### 2.2 Backend

Verificar:

- existencia de `Backend/app/`;
- estructura de módulos;
- patrón de configuración;
- framework web;
- inicialización de aplicación;
- rutas;
- servicios;
- modelos;
- validadores;
- excepciones;
- logging;
- pruebas;
- fixtures;
- manejo de settings;
- convenciones de nombres.

### 2.3 Capacidades existentes

Buscar referencias a:

- `rule_engine`;
- `rule registry`;
- `rules`;
- `grounded_assessment`;
- `decision_context`;
- `decision_engine`;
- `evaluation`;
- `scoring`;
- `ResultJson`;
- `prompt_rules`;
- validación YAML;
- registries;
- feature flags;
- shadow mode.

### 2.4 Empaquetado

Determinar:

- qué directorios se copian al artefacto;
- si `Backend/rules/` quedaría incluido;
- si Docker ignora YAML;
- si el pipeline empaqueta solo código;
- cómo se resuelven rutas en runtime;
- cuál es el working directory real;
- si existe variable de entorno para configuración externa.

### 2.5 Pruebas

Determinar:

- framework de pruebas;
- comando oficial;
- ubicación;
- configuración;
- cobertura existente;
- convenciones de fixtures;
- pruebas de integración disponibles.

## 3. Repository Analysis

Antes de implementar, producir internamente una matriz como la siguiente:

| Elemento | Existe | Ruta real | Compatible | Acción |
|---|---:|---|---:|---|
| Backend | Sí/No | ruta | Sí/No | conservar/adaptar/detener |
| Configuración | Sí/No | ruta | Sí/No | integrar |
| Tests | Sí/No | ruta | Sí/No | ampliar |
| Rule Engine previo | Sí/No | ruta | Sí/No | reutilizar/detener |
| Registry previo | Sí/No | ruta | Sí/No | reutilizar/adaptar |
| YAML loader | Sí/No | ruta | Sí/No | reutilizar/reemplazar |
| Feature flags | Sí/No | ruta | Sí/No | integrar |
| Packaging rules | Sí/No | ruta | Sí/No | ajustar |

Esta matriz no necesita persistirse en el repositorio, salvo que el proyecto ya tenga un patrón para documentación de descubrimiento.

## 4. Precondiciones obligatorias

La implementación puede comenzar solamente cuando:

- existe un backend identificable;
- existe una ubicación válida para módulos internos;
- existe una estrategia de configuración;
- existe un mecanismo para ejecutar pruebas;
- puede agregarse `Backend/rules/` o una ruta equivalente sin alterar contratos públicos;
- el Rule Engine puede permanecer no autoritativo.

## 5. Condiciones bloqueantes

Detener implementación cuando:

- el repositorio real no contiene el backend esperado;
- la rama o copia de trabajo está incompleta;
- existe otro Rule Engine autoritativo incompatible;
- la única forma de integrar exige modificar scoring;
- la única forma de integrar exige modificar `ResultJson`;
- no se puede determinar cómo empaquetar reglas;
- hay conflictos estructurales que implican una decisión arquitectónica no definida;
- tests están rotos antes de comenzar y no puede distinguirse deuda previa de regresión;
- el cambio exige modificar Frontend o fases posteriores.

## 6. Cambios permitidos

Se permite crear o modificar, ajustando las rutas a la estructura real:

```text
Backend/app/rule_engine/
Backend/rules/
Backend/tests/
Backend/app/config/
Backend/README*
Architecture/EIS/EIS-40C.1/
```

También se permiten cambios mínimos en:

- inicialización de configuración;
- registro de dependencias internas;
- empaquetado;
- Dockerfile;
- `.dockerignore`;
- pipeline.

Estos cambios mínimos solo están permitidos cuando sean indispensables para incluir y localizar reglas en runtime.

## 7. Cambios prohibidos

No modificar:

- algoritmos de scoring;
- penalizaciones;
- reglas de recomendación existentes;
- `ResultJson`;
- contratos API públicos;
- esquemas de respuestas existentes;
- frontend;
- Vendor Comparison;
- narrativa;
- prompts productivos;
- lógica de IA;
- persistencia histórica;
- migraciones de base de datos, salvo necesidad demostrada y explícitamente aprobada;
- autenticación o autorización;
- EIS posteriores.

## 8. Estrategia ante componentes existentes

### 8.1 Si ya existe un módulo de reglas compatible

Reutilizarlo y extenderlo sin duplicar responsabilidades.

### 8.2 Si existe parcialmente

Adaptar nombres y rutas a las convenciones reales, preservando los requisitos funcionales del EIS.

### 8.3 Si existe pero es autoritativo

Detenerse y reportar contradicción.

### 8.4 Si no existe

Crear la estructura descrita en `03-Architecture.md`.

## 9. Expected Repository

La estructura objetivo conceptual es:

```text
RFP-AI-AGENT/
├── Architecture/
│   └── EIS/
│       └── EIS-40C.1/
├── Backend/
│   ├── app/
│   │   └── rule_engine/
│   │       ├── __init__.py
│   │       ├── contracts.py
│   │       ├── models.py
│   │       ├── enums.py
│   │       ├── exceptions.py
│   │       ├── loader.py
│   │       ├── validator.py
│   │       ├── registry.py
│   │       ├── diagnostics.py
│   │       ├── checksum.py
│   │       └── settings.py
│   ├── rules/
│   │   ├── README.md
│   │   ├── schema/
│   │   └── examples/
│   └── tests/
│       └── rule_engine/
└── Frontend/
```

La estructura física puede variar si el repositorio utiliza otras convenciones. Debe conservarse la separación de responsabilidades.

## 10. Salida requerida si se detiene

El reporte de detención debe incluir:

- hallazgo;
- evidencia;
- ruta;
- contradicción;
- impacto;
- razón por la que no se implementó;
- alternativas;
- decisión requerida.
