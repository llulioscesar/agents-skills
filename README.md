# Claude Code Operating System

Un framework práctico para usar Claude Code como un **sistema de ejecución disciplinado**: desde una idea de negocio hasta una entrega técnica validada.

Este repositorio define:

- **subagentes especializados** (producto, shaping técnico, implementación, review, seguridad, QA, riesgo, debugging),
- **modelo operativo** para separar etapas,
- y una guía para escalar de tareas sueltas a workflows repetibles.

---

## Índice

1. [Qué es este repositorio](#qué-es-este-repositorio)
2. [Objetivo principal](#objetivo-principal)
3. [Modelo operativo](#modelo-operativo)
4. [Mapa de subagentes disponibles](#mapa-de-subagentes-disponibles)
5. [Mapa de skills disponibles](#mapa-de-skills-disponibles)
6. [Flujos recomendados](#flujos-recomendados)
7. [Cuándo usar cada subagente](#cuándo-usar-cada-subagente)
8. [Cuándo usar cada skill](#cuándo-usar-cada-skill)
9. [Estado actual y próximos pasos](#estado-actual-y-próximos-pasos)
10. [Principios de uso](#principios-de-uso)

---

## Qué es este repositorio

Este proyecto convierte a Claude Code en un “OS de trabajo” con responsabilidades separadas.

En lugar de pedir “haz todo”, se divide el trabajo en capas:

1. **Definir** (problema, usuario, valor, alcance, criterios de aceptación)
2. **Diseñar técnicamente** (límites, módulos, integraciones, trade-offs, secuencia)
3. **Implementar** (cambio mínimo correcto y mantenible)
4. **Validar** (tests, code review, security review, QA funcional, riesgo de release)
5. **Cerrar** (resumen final y decisión de despliegue)

---

## Objetivo principal

Evitar problemas típicos al usar IA para desarrollo:

- cambios rápidos sin criterio de producto,
- implementación sin diseño técnico previo,
- fixes sin causa raíz,
- revisión insuficiente de seguridad y supply chain,
- falta de criterio de salida para release.

Resultado buscado: **entregas más predecibles, auditables y seguras**.

---

## Modelo operativo

### 1) Subagente = especialista

Cada subagente está optimizado para un tipo de trabajo puntual (por ejemplo: `go-specialist`, `security-reviewer`, `debugger`).

### 2) Skill = workflow

Un skill coordina pasos y subagentes para cerrar una tarea completa (por ejemplo: revisar un cambio, investigar un bug, o entregar de forma segura).

### 3) Contexto limpio

Separar especialistas reduce contaminación del contexto principal y mejora la calidad de decisiones.

---

## Mapa de subagentes disponibles

### Definición y shaping

- `product-owner`
  Convierte una idea ambigua en una definición clara: problema, usuario, valor, alcance, restricciones y criterios de aceptación.

- `technical-shaper`
  Traduce la definición de producto a forma técnica ejecutable: límites, componentes, integraciones, secuencia y trade-offs.

### Implementación

- `go-specialist`
  Implementación idiomática de Go, con foco en simplicidad, cohesión y estructura orientada al dominio real.

### Validación técnica

- `go-reviewer`
  Revisión estricta de código Go para asegurar estilo idiomático y evitar sobreingeniería.

- `code-reviewer`
  Revisión fullstack para TS/JS/React/Node/Flutter (correctness, tipos, estados async, performance, mantenibilidad).

- `test-writer`
  Escritura/actualización de tests para stacks no-Go respetando los patrones del proyecto.

### Calidad, seguridad y entrega

- `security-reviewer`
  Revisión de auth/authz, input validation, secretos, configuración, dependencias y riesgos de supply chain.

- `qa-functional`
  Validación funcional end-to-end: flujos, estados, permisos, errores, casos borde y resultado esperado por usuario.

- `risk-reviewer`
  Evaluación de riesgo de rollout/release: reversibilidad, observabilidad, dependencia crítica, blast radius y mitigaciones.

- `gap-reviewer`
  Detección de huecos entre lo definido y lo realmente cubierto (ambigüedades, supuestos, criterios incompletos).

### Incidentes

- `debugger`
  Investigación disciplinada de bugs con orden estricto: **síntoma → hipótesis → evidencia → causa raíz → fix mínimo**.

---

## Mapa de skills disponibles

Los skills son workflows reutilizables para ejecutar tareas completas y no olvidar pasos críticos.

- `shape-feature`
  Convierte una idea en una definición de producto accionable (problema, usuario, alcance, criterios).

- `build-plan`
  Convierte la definición en plan técnico ejecutable (entregables, secuencia, riesgos, validaciones).

- `review-change`
  Orquesta revisión técnica y de seguridad según los archivos afectados.

- `bug-hunt`
  Investiga bugs con disciplina de causa raíz antes de aplicar el fix.

- `ship-safe`
  Flujo de implementación con cierre disciplinado: código, tests, review y validación.

- `release-check`
  Checklist final de salida antes de desplegar: readiness, riesgos abiertos y condiciones de release.

---

## Flujos recomendados

### Flujo A — Idea a implementación segura

```text
idea/problema
-> product-owner
-> technical-shaper
-> implementación
-> tests
-> code/go review
-> security review (si aplica)
-> qa-functional
-> risk-reviewer
-> despliegue
```

### Flujo B — Bug o regresión

```text
incidente
-> debugger
-> fix mínimo
-> test de regresión
-> review técnico
-> security review (si aplica)
-> despliegue
```

### Flujo C — Cambio pequeño

```text
implementación
-> tests
-> review técnico
-> listo
```

---

## Cuándo usar cada subagente

- Usa `product-owner` cuando la solicitud aún está difusa o mal acotada.
- Usa `technical-shaper` cuando ya hay claridad de producto, pero falta plan técnico.
- Usa `go-specialist` para escribir o modificar Go.
- Usa `debugger` cuando hay comportamiento roto y aún no hay causa raíz validada.
- Usa `test-writer` cuando faltan tests en stacks no-Go.
- Usa `security-reviewer` cuando tocas backend/API/auth/deps/infra/CI/CD.
- Usa `qa-functional` antes de release cuando el flujo de usuario es crítico.
- Usa `risk-reviewer` cuando el cambio tiene impacto operativo o complejidad de rollout.
- Usa `gap-reviewer` cuando sospechas que la definición o plan no cubre todo.

---

## Cuándo usar cada skill

- Usa `shape-feature` cuando arrancas con una idea o necesidad todavía difusa.
- Usa `build-plan` cuando ya tienes claridad de producto y necesitas secuencia técnica.
- Usa `review-change` cuando ya implementaste y quieres revisión consolidada antes de cerrar.
- Usa `bug-hunt` cuando hay bug/regresión y necesitas diagnóstico con evidencia.
- Usa `ship-safe` cuando quieres ejecutar cambios de principio a fin con disciplina.
- Usa `release-check` cuando el cambio está “casi listo” y necesitas decisión de salida.

---

## Estado actual y próximos pasos

### Estado actual

El repositorio ya cubre una cadena completa mucho más allá de “solo código”:

- definición de producto,
- shaping técnico,
- implementación y testing,
- review técnico,
- seguridad,
- QA funcional,
- análisis de riesgo y gaps,
- debugging con enfoque de causa raíz.

### Siguiente evolución recomendada

1. Formalizar skills de orquestación end-to-end (idea→release) con criterios de entrada/salida explícitos.
2. Definir plantillas de handoff entre subagentes (producto → técnico → implementación).
3. Estandarizar checklist de release con umbrales mínimos de seguridad, QA y riesgo.

---

## Principios de uso

1. **Primero claridad, después código.**
2. **Primero causa raíz, después fix.**
3. **No confundir compilar con estar listo para producción.**
4. **Seguridad y riesgo no son “último toque”.**
5. **Usa especialistas para calidad y trazabilidad, no improvisación.**

---

## Resumen en una línea

> Este repo define una forma de trabajar con Claude Code orientada a calidad de entrega: problema claro, diseño técnico explícito, implementación disciplinada y validación real antes de desplegar.
