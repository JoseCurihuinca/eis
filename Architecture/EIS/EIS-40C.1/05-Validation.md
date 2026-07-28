# EIS-40C.1 — Validación, pruebas y criterios de aceptación

## 1. Objetivo

Demostrar que la fundación de gobierno de reglas es segura, determinista, no autoritativa y compatible con el backend existente.

## 2. Pruebas unitarias obligatorias

### 2.1 Modelos

- regla válida;
- falta de `rule_id`;
- versión inválida;
- estado inválido;
- dominio inválido;
- severidad inválida;
- fechas inválidas;
- metadata incompleta.

### 2.2 Loader

- carga de `.yaml`;
- carga de `.yml`;
- rechazo de extensión no permitida;
- YAML inválido;
- YAML con tipo raíz incorrecto;
- archivo inexistente;
- directorio inexistente;
- path traversal;
- límite de tamaño;
- límite de cantidad;
- carga segura sin constructores arbitrarios.

### 2.3 Validator

- campos obligatorios;
- tipos incorrectos;
- operadores no permitidos;
- reglas activas incompletas;
- lifecycle inválido;
- identificador inválido;
- múltiples errores en un mismo archivo;
- warning no bloqueante.

### 2.4 Registry

- registro exitoso;
- búsqueda por id;
- búsqueda por versión;
- listado por dominio;
- listado por estado;
- duplicado exacto;
- mismo id con versión distinta;
- intento de reemplazo silencioso;
- snapshot inmutable;
- clear controlado.

### 2.5 Checksum

- SHA-256 estable;
- contenido distinto;
- mismatch;
- política obligatoria;
- política opcional.

### 2.6 Diagnostics

- conteos correctos;
- errores asociados al archivo;
- duración;
- checksums;
- duplicados;
- serialización;
- ausencia de contenido sensible.

## 3. Pruebas de integración obligatorias

### 3.1 Catálogo mixto

Directorio con:

- regla válida;
- regla inválida;
- archivo ignorado;
- duplicado.

Verificar:

- solo reglas válidas registradas;
- diagnósticos completos;
- comportamiento fail closed configurado;
- orden determinista.

### 3.2 Inicio con engine deshabilitado

Verificar:

- backend inicia;
- APIs actuales funcionan;
- ausencia de reglas no rompe producción;
- no se invoca lógica de evaluación.

### 3.3 Inicio en shadow

Verificar:

- reglas se cargan;
- diagnósticos disponibles internamente;
- scores no cambian;
- respuestas no cambian;
- no aparecen hallazgos adicionales.

### 3.4 Empaquetado

Verificar en artefacto o imagen:

- carpeta de reglas presente;
- ruta resoluble;
- permisos de lectura;
- ausencia de archivos de prueba inseguros en runtime.

## 4. Pruebas de regresión

Ejecutar la suite existente completa.

Criterio:

- no reducir el número de pruebas;
- no marcar pruebas como skip para ocultar fallos;
- no actualizar snapshots productivos salvo que el cambio sea puramente técnico y no altere contenido;
- no aceptar cambios en scores o respuestas.

## 5. Validación estática

Ejecutar herramientas existentes:

- formatter;
- linter;
- type checker;
- security scanner, si existe;
- test coverage, si existe.

No introducir nuevas herramientas salvo necesidad justificada.

## 6. Criterios de aceptación funcional

La fase se acepta cuando:

1. existe un catálogo de reglas gobernado;
2. las reglas se cargan con YAML seguro;
3. los modelos son tipados;
4. los errores son accionables;
5. se detectan duplicados;
6. el lifecycle es validado;
7. el registry permite consultas;
8. los diagnósticos son serializables;
9. se soporta checksum SHA-256;
10. el engine permanece no autoritativo;
11. el comportamiento productivo no cambia;
12. las pruebas pasan.

## 7. Criterios de aceptación de seguridad

- no hay `eval`;
- no hay `exec`;
- no hay imports dinámicos desde reglas;
- no se permite path traversal;
- no se usa YAML inseguro;
- no se registran secretos;
- reglas inválidas no quedan activas;
- duplicados no se resuelven silenciosamente;
- checksums funcionan;
- límites configurables existen.

## 8. Criterios de aceptación de arquitectura

- separación loader/validator/registry;
- contratos definidos;
- sin dependencia directa desde scoring;
- sin dependencia directa desde frontend;
- sin acoplamiento a LLM;
- rutas configurables;
- empaquetado validado;
- preparada para migración en EIS-40C.2.

## 9. Criterios de aceptación de compatibilidad

Debe confirmarse explícitamente:

```text
Scoring changed: NO
ResultJson changed: NO
Public API contracts changed: NO
Frontend changed: NO
Production findings changed: NO
Narrative changed: NO
```

## 10. Definition of Done

La fase está terminada cuando:

- todos los archivos previstos existen;
- el código compila o importa correctamente;
- la suite nueva pasa;
- la suite existente pasa;
- el linter pasa;
- el empaquetado incluye reglas;
- la documentación existe;
- los diagnósticos funcionan;
- no hay cambios fuera de alcance;
- se entrega el reporte de `06-Report.md`.

## 11. Evidencia mínima

El reporte debe incluir:

- comandos ejecutados;
- resultados;
- total de pruebas;
- archivos creados;
- archivos modificados;
- evidencia de packaging;
- evidencia de no impacto;
- riesgos residuales.

## 12. Fallos aceptables versus bloqueantes

### Aceptables como deuda documentada

- cobertura inferior al objetivo global, si no disminuye;
- ausencia de endpoint diagnóstico público;
- ausencia de persistencia de registry;
- ausencia de UI;
- ausencia de workflow de aprobación.

### Bloqueantes

- cambios de score;
- cambios en `ResultJson`;
- reglas ejecutándose en producción;
- YAML inseguro;
- duplicados aceptados;
- path traversal;
- suite existente rota;
- reglas fuera del artefacto;
- loader sin fail closed.
