# EIS-40C.1 — Rule Governance Foundation

## 1. Propósito

Este Enterprise Implementation Specification define la implementación de la base de gobierno de reglas para el backend de **RFP-AI-AGENT**.

La fase EIS-40C.1 debe crear una infraestructura formal, segura, versionable y auditable para registrar, cargar, validar y consultar reglas técnicas. En esta fase el Rule Engine **no será autoritativo** y no podrá modificar el puntaje, el `ResultJson`, la narrativa, la comparación de proveedores ni las decisiones productivas existentes.

## 2. Resultado esperado

Al finalizar esta fase, el repositorio debe contar con:

- un módulo `rule_engine` dentro del backend;
- un registro formal de reglas;
- contratos y modelos tipados;
- un cargador seguro para reglas YAML;
- validación estructural y semántica;
- control de ciclo de vida;
- detección de duplicados;
- diagnósticos de carga;
- pruebas unitarias e integración;
- documentación operativa mínima;
- compatibilidad con el empaquetado y despliegue actual.

## 3. Punto de entrada y orden obligatorio

Codex debe leer los documentos en este orden:

1. `README.md`
2. `01-Context.md`
3. `02-Repository.md`
4. `03-Architecture.md`
5. `04-Implementation.md`
6. `05-Validation.md`
7. `06-Report.md`

No debe comenzar a modificar código antes de completar las actividades de descubrimiento y análisis descritas en `02-Repository.md`.

## 4. Alcance de la fase

Esta fase incluye exclusivamente:

- gobierno de reglas;
- contratos;
- modelos;
- carga;
- validación;
- registro;
- diagnóstico;
- seguridad del loader;
- pruebas;
- documentación técnica mínima.

Esta fase no incluye:

- migración completa de handlers existentes;
- equivalencia entre reglas nuevas y comportamiento legacy;
- promoción del Rule Engine como fuente autoritativa;
- cambios en scoring;
- cambios en `ResultJson`;
- cambios en frontend;
- cambios en decisiones o narrativa productiva;
- implementación de EIS posteriores.

## 5. Dependencias

La fase asume la existencia de:

- `Backend/`;
- `Backend/app/`;
- configuración del backend;
- framework de pruebas;
- implementación actual de evaluación y grounded assessment;
- pipeline o mecanismo de build existente.

Si alguna dependencia crítica no existe, Codex debe aplicar las reglas de detención definidas en `02-Repository.md`.

## 6. Principios de implementación

1. **Fail closed:** una regla inválida, duplicada o insegura no puede quedar activa.
2. **No ejecución dinámica:** queda prohibido usar `eval`, `exec` o equivalentes.
3. **Carga segura:** YAML debe procesarse mediante carga segura.
4. **Determinismo:** la carga y validación deben producir resultados reproducibles.
5. **Trazabilidad:** toda regla debe tener identificador, versión, estado y origen.
6. **Compatibilidad:** ningún comportamiento productivo actual debe cambiar.
7. **Separación:** modelos, validación, carga, registro y diagnóstico deben permanecer desacoplados.
8. **Testabilidad:** cada componente debe poder validarse en forma aislada.

## 7. Secuencia obligatoria de ejecución

1. Ejecutar Repository Discovery.
2. Ejecutar Repository Analysis.
3. Confirmar precondiciones.
4. Identificar contradicciones bloqueantes.
5. Crear la estructura aprobada.
6. Implementar contratos y modelos.
7. Implementar loader y validadores.
8. Implementar registry y diagnósticos.
9. Incorporar archivos de reglas de ejemplo mínimos.
10. Crear pruebas.
11. Ejecutar validaciones.
12. Revisar empaquetado.
13. Entregar el reporte definido en `06-Report.md`.

## 8. Criterio de detención

Codex debe detenerse antes de implementar cuando ocurra cualquiera de estas condiciones:

- no existe el backend esperado;
- la estructura real contradice materialmente el EIS;
- ya existe un Rule Engine autoritativo incompatible;
- no puede identificar el mecanismo de configuración;
- no puede determinar cómo se ejecutan las pruebas;
- la implementación exigiría modificar scoring o `ResultJson`;
- la implementación exigiría cambios fuera del alcance permitido.

La detención debe incluir diagnóstico, evidencia, impacto y recomendación.

## 9. Documentos de ejecución

- Contexto y límites: `01-Context.md`
- Descubrimiento del repositorio: `02-Repository.md`
- Arquitectura objetivo: `03-Architecture.md`
- Tareas de implementación: `04-Implementation.md`
- Validación y aceptación: `05-Validation.md`
- Formato de reporte: `06-Report.md`

## 10. Estado de la especificación

- Identificador: `EIS-40C.1`
- Nombre: `Rule Governance Foundation`
- Tipo: implementación no autoritativa
- Estado: aprobado para ejecución
- Fase siguiente prevista: `EIS-40C.2 — Rule Migration`
