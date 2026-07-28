# EIS-40C.1 — Reporte final esperado de Codex

Al finalizar, Codex debe entregar un informe técnico con esta estructura exacta.

## 1. Estado

Uno de:

```text
COMPLETADO
COMPLETADO CON OBSERVACIONES
BLOQUEADO
```

## 2. Resumen ejecutivo

Descripción breve de:

- qué se implementó;
- resultado;
- compatibilidad;
- limitaciones.

## 3. Repository Discovery

Informar:

- estructura detectada;
- framework;
- configuración;
- pruebas;
- empaquetado;
- componentes previos reutilizados.

## 4. Arquitectura implementada

Describir:

- contracts;
- models;
- loader;
- validator;
- registry;
- diagnostics;
- settings;
- checksum;
- integración no autoritativa.

Incluir un diagrama textual simple.

## 5. Archivos creados

Tabla:

| Archivo | Propósito |
|---|---|

## 6. Archivos modificados

Tabla:

| Archivo | Cambio | Justificación |
|---|---|---|

## 7. Configuración

Documentar variables nuevas:

| Variable | Default | Obligatoria | Propósito |
|---|---|---:|---|

## 8. Seguridad

Confirmar:

- YAML seguro;
- sin eval/exec;
- protección de rutas;
- límites;
- checksum;
- fail closed;
- logging sin secretos.

## 9. Pruebas

Tabla:

| Comando | Resultado | Total |
|---|---|---:|

Separar:

- unitarias nuevas;
- integración;
- regresión;
- linter;
- type checker;
- packaging.

## 10. Evidencia de compatibilidad

Declarar expresamente:

```text
Scoring changed: NO
ResultJson changed: NO
Public API contracts changed: NO
Frontend changed: NO
Production findings changed: NO
Narrative changed: NO
Rule Engine authoritative: NO
```

Si alguna respuesta no es `NO`, la fase no puede reportarse como completada.

## 11. Empaquetado y runtime

Indicar:

- ubicación de `RULES_ROOT`;
- inclusión en artefacto;
- inclusión en Docker;
- working directory;
- comportamiento cuando faltan reglas;
- comportamiento en `DISABLED`;
- comportamiento en `SHADOW`.

## 12. Riesgos residuales

Tabla:

| Riesgo | Impacto | Mitigación | Fase prevista |
|---|---|---|---|

## 13. Deuda técnica

Incluir solamente deuda real y no funcionalidades de fases posteriores.

## 14. Desviaciones del EIS

Listar:

- desviación;
- motivo;
- impacto;
- decisión tomada.

Si no existen:

```text
No se detectaron desviaciones.
```

## 15. Próximo paso permitido

Únicamente:

```text
EIS-40C.2 — Rule Migration
```

No implementar ni detallar trabajo propio de esa fase.

## 16. Control de cambios

Confirmar:

```text
Commit realizado: NO
Push realizado: NO
Merge realizado: NO
```
