# Claude Code Operating System

Guía completa para usar Claude Code como un sistema de trabajo desde una idea o problema de negocio hasta implementación, validación, seguridad, debugging y despliegue.

Este README documenta el modelo mental, la arquitectura de trabajo, los subagentes, los skills, los workflows, el estado actual del sistema y la evolución esperada.

---

# Índice

1. [Propósito](#1-propósito)
2. [Modelo mental general](#2-modelo-mental-general)
3. [Objetivo del sistema](#3-objetivo-del-sistema)
4. [Capas del sistema](#4-capas-del-sistema)
5. [Subagentes actuales](#5-subagentes-actuales)
6. [Skills actuales](#6-skills-actuales)
7. [Subagentes built-in importantes](#7-subagentes-built-in-importantes)
8. [Cómo pensar el flujo completo](#8-cómo-pensar-el-flujo-completo)
9. [Cuándo usar cada subagente](#9-cuándo-usar-cada-subagente)
10. [Cuándo usar cada skill](#10-cuándo-usar-cada-skill)
11. [Cómo se combinan subagentes y skills](#11-cómo-se-combinan-subagentes-y-skills)
12. [Estado actual del sistema](#12-estado-actual-del-sistema)
13. [Roles que faltan](#13-roles-que-faltan)
14. [Skills que faltan](#14-skills-que-faltan)
15. [Workflows recomendados](#15-workflows-recomendados)
16. [Filosofía de diseño](#16-filosofía-de-diseño)
17. [Reglas prácticas](#17-reglas-prácticas)
18. [Roadmap sugerido](#18-roadmap-sugerido)
19. [Resumen corto](#19-resumen-corto)

---

# 1. Propósito

El objetivo de este setup es evitar usar Claude Code como un simple generador de código y convertirlo en un sistema operativo de ejecución con roles, responsabilidades, workflows y validaciones claras.

La idea no es solo pedir código.

La idea es poder recorrer de forma disciplinada este camino:

```text
idea / problema / oportunidad
-> definición de producto
-> definición técnica
-> implementación
-> tests
-> review
-> seguridad
-> validación final
-> despliegue
```

Este sistema busca resolver varios problemas comunes del trabajo con IA:

- contaminación de contexto por exploración excesiva
- generación de código sin validación suficiente
- falta de separación entre construir, revisar y diagnosticar
- bugs corregidos sin entender causa raíz
- huecos de seguridad o supply chain en cambios aparentemente correctos
- ausencia de workflows repetibles para revisar, corregir o entregar cambios
- dificultad para pasar de una idea de negocio a una ejecución técnica consistente

---

# 2. Modelo mental general

Para que este sistema tenga sentido, hay que separar claramente las piezas que Claude Code maneja.

## 2.1 Subagentes

Los subagentes son especialistas.

Cada subagente tiene:

- su propio prompt del sistema
- su propia ventana de contexto
- herramientas específicas
- modelo configurable
- memoria opcional
- comportamiento diferenciado

Su propósito es ejecutar tareas especializadas sin ensuciar el contexto principal de la conversación.

Ejemplos:

- un especialista en Go
- un reviewer de seguridad
- un debugger
- un reviewer fullstack

Regla corta:

```text
Subagente = especialista
```

## 2.2 Skills

Los skills son workflows reutilizables.

No son especialistas como tal. Son recetas, flujos u orquestadores que indican cómo ejecutar una tarea completa.

Sirven para:

- coordinar varios subagentes
- imponer disciplina de ejecución
- encapsular flujos repetitivos
- estandarizar cómo se cierra un cambio
- convertir procesos complejos en comandos reutilizables

Ejemplos:

- revisar un cambio completo
- investigar un bug
- hacer una entrega segura

Regla corta:

```text
Skill = workflow
```

## 2.3 CLAUDE.md

`CLAUDE.md` es el contexto persistente del proyecto.

Ahí deberían vivir:

- reglas del proyecto
- definiciones importantes
- convenciones
- decisiones de alto nivel
- restricciones de negocio
- filosofía técnica del repositorio
- definición de done del equipo

No es el lugar ideal para meter lógica operativa detallada de subagentes o skills.  
Es más bien el marco general de trabajo.

Regla corta:

```text
CLAUDE.md = reglas y contexto del proyecto
```

## 2.4 Built-in subagents

Claude Code ya trae subagentes integrados.

Los más importantes en este setup son:

- `Explore`
- `Plan`
- `general-purpose`

Estos no sustituyen a tus subagentes personalizados.  
Los complementan.

- `Explore` sirve para explorar repositorios y ubicar código sin contaminar mucho contexto
- `Plan` sirve para investigar antes de plantear un plan
- `general-purpose` sirve como subagente flexible para tareas complejas

Regla corta:

```text
Explore = investigación
Plan = planificación
general-purpose = ejecución general
```

---

# 3. Objetivo del sistema

El objetivo final de este sistema es permitir este recorrido:

```text
idea / negocio / problema
-> clarificación del problema
-> definición de producto
-> diseño técnico
-> implementación
-> testing
-> review
-> security review
-> validación de release
-> despliegue
```

Hoy el sistema ya cubre bien la parte técnica:

- implementación
- review
- debugging
- tests
- seguridad

Todavía falta formalizar mejor la parte de producto y arquitectura.

---

# 4. Capas del sistema

## 4.1 Capa de definición

Convierte una idea de negocio o problema en algo implementable.

Aquí viven roles como:

- Product / PO
- Architect
- Risk / Gap / QA conceptual

Todavía no todos estos roles están implementados como subagentes, pero forman parte del modelo deseado.

## 4.2 Capa de implementación

Convierte una decisión en código.

Actualmente:

- `go-specialist` implementa Go
- para otros stacks, la implementación la hace el agente principal

## 4.3 Capa de revisión

Valida que el cambio no solo funcione, sino que esté bien hecho.

Actualmente:

- `go-reviewer`
- `code-reviewer`
- `security-reviewer`

## 4.4 Capa de incidentes

Cuando algo falla, se investiga primero y se corrige después.

Actualmente:

- `debugger`

## 4.5 Capa de tests

Asegura cobertura útil y evita regresiones.

Actualmente:

- `go-specialist` escribe tests de Go cuando corresponde
- `test-writer` escribe tests para TS/JS/React/Node/Flutter

## 4.6 Capa de orquestación

Encadena especialistas y fuerza workflows repetibles.

Actualmente:

- `review-change`
- `bug-hunt`
- `ship-safe`

---

# 5. Subagentes actuales

## 5.1 `go-specialist`

Especialista en implementación Go.

### Responsabilidades

- escribir Go idiomático
- respetar estructura basada en dominio real
- evitar arquitectura importada de Java/C#
- usar tipos concretos antes que interfaces anticipadas
- manejar errores, `context`, HTTP, gRPC y SQL de forma idiomática
- escribir tests de Go cuando haga falta

### Filosofía

No debe generar Go “enterprise” por plantilla.  
Debe producir Go natural, directo y con cohesión real.

---

## 5.2 `go-reviewer`

Revisor estricto de Go.

### Responsabilidades

- revisar si el Go realmente se siente como Go
- detectar sobreingeniería
- detectar capas artificiales
- revisar nombres, paquetes, errores, `context` y estructura
- señalar olores de Java/C# disfrazados en Go

### Filosofía

No revisa solo si compila.  
Revisa si el código es idiomático, limpio y coherente con la forma natural de Go.

---

## 5.3 `code-reviewer`

Revisor fullstack para no-Go.

### Stacks cubiertos

- TypeScript
- JavaScript
- React
- Node.js
- Flutter

### Responsabilidades

- correctness
- type safety
- async/state bugs
- framework misuse
- performance
- maintainability
- test quality
- seguridad básica

### Filosofía

No debe revisar toda la codebase completa.  
Debe enfocarse en el cambio reciente y en el contexto inmediato que lo afecta.

---

## 5.4 `debugger`

Especialista en bugs y regresiones.

### Responsabilidades

- entender síntoma
- formular hipótesis
- investigar con evidencia
- validar causa raíz
- aplicar el fix mínimo solo después de explicar la causa

### Filosofía

No debe parchear por intuición.  
No debe saltar al fix sin diagnóstico.  
Su orden correcto es:

```text
síntoma -> hipótesis -> evidencia -> causa raíz -> fix
```

---

## 5.5 `security-reviewer`

Revisor de seguridad y supply chain.

### Responsabilidades

- auth y authz
- validación de entrada
- inyecciones
- exposición de datos
- secretos
- configuración insegura
- uploads
- dependencias vulnerables, obsoletas o sospechosas
- supply chain
- CI/CD
- Docker
- lockfiles y manifests
- riesgos de dependencias recién publicadas o sospechosas

### Filosofía

No debe limitarse a “OWASP genérico”.  
Debe revisar seguridad real, supply chain, configuración y riesgo de dependencia.

---

## 5.6 `test-writer`

Escritor de tests para stacks no-Go.

### Stacks cubiertos

- TypeScript
- JavaScript
- React
- Node.js
- Flutter

### Responsabilidades

- seguir el framework y patrones existentes del proyecto
- escribir tests al nivel correcto
- testear comportamiento, no implementación
- añadir tests de regresión cuando haga falta
- evitar over-mocking y arquitecturas paralelas de tests

### Filosofía

Debe respetar el estilo de tests que ya usa el proyecto.  
No debe imponer otra filosofía porque sí.

---

# 6. Skills actuales

## 6.1 `/review-change`

Workflow para revisar un cambio antes de darlo por terminado.

### Hace

- detecta qué tipo de archivos cambiaron
- llama `go-reviewer` si hay Go
- llama `code-reviewer` si hay TS/JS/React/Node/Flutter
- llama `security-reviewer` si el cambio toca backend, auth, deps, config, infra, CI/CD, uploads o integraciones
- consolida findings en un solo resultado

### Cuándo usarlo

Cuando ya hiciste un cambio y quieres una revisión consolidada antes de considerarlo listo.

### Uso

```text
/review-change
```

---

## 6.2 `/bug-hunt`

Workflow para investigar y corregir bugs con disciplina de causa raíz.

### Hace

- usa `Explore` para ubicar el área
- usa `debugger` para diagnosticar y arreglar
- si aplica, usa `go-specialist` o `test-writer` para añadir tests
- corre validaciones relevantes
- resume causa raíz, fix y prevención

### Cuándo usarlo

Cuando algo dejó de funcionar, hay un bug, una regresión o un error que no debe corregirse a ciegas.

### Uso

```text
/bug-hunt
```

---

## 6.3 `/ship-safe`

Workflow para implementar un cambio completo con validación.

### Hace

- implementa
- añade o actualiza tests
- corre validaciones
- pasa review
- pasa security review si aplica
- devuelve estado final honesto

### Cuándo usarlo

Cuando quieres ejecutar un cambio con disciplina completa de implementación y cierre.

### Uso

```text
/ship-safe
```

---

# 7. Subagentes built-in importantes

## 7.1 `Explore`

Subagente de exploración de solo lectura.

### Útil para

- buscar dónde vive algo
- entender estructuras
- explorar codebase sin contaminar el contexto principal

### Cuándo usarlo

Cuando necesitas investigar una base de código o ubicar piezas antes de decidir cómo intervenir.

---

## 7.2 `Plan`

Subagente de investigación para planear antes de tocar código.

### Útil para

- recopilar contexto
- entender alcance técnico
- preparar un plan antes de implementar

### Cuándo usarlo

Cuando la tarea es grande, ambigua o peligrosa y necesitas contexto antes de ejecutar.

---

## 7.3 `general-purpose`

Subagente general para tareas complejas de varios pasos.

### Útil para

- investigación con acción
- tareas complejas
- operaciones que no encajan perfectamente en un especialista

---

# 8. Cómo pensar el flujo completo

La visión completa del sistema es esta:

```text
idea / problema / negocio
-> shape-feature
-> build-plan
-> implementación
-> tests
-> review-change
-> security-reviewer
-> release-check
-> despliegue
```

Hoy la parte más madura es desde implementación en adelante.

La parte que todavía falta formalizar mejor es:

- definición de producto
- definición técnica previa
- análisis de riesgos
- análisis de gaps
- readiness de release

---

# 9. Cuándo usar cada subagente

## Usa `go-specialist` cuando:

- estés implementando o modificando Go
- necesites escribir Go idiomático
- necesites añadir tests Go junto con la implementación

## Usa `go-reviewer` cuando:

- ya cambiaste Go y quieres validar calidad idiomática
- sospechas sobreingeniería o mala forma Go
- quieres un filtro de calidad fuerte para ese stack

## Usa `code-reviewer` cuando:

- cambiaste TS/JS/React/Node/Flutter
- quieres revisar correctness, tipos, framework y calidad
- el cambio ya está implementado y buscas validación técnica

## Usa `debugger` cuando:

- algo falla
- hay regresión
- aparece un error
- el comportamiento no coincide con lo esperado
- necesitas causa raíz antes de tocar código

## Usa `security-reviewer` cuando:

- tocaste backend
- auth
- dependencias
- CI/CD
- configuración
- uploads
- integraciones externas
- secretos
- infraestructura

## Usa `test-writer` cuando:

- hiciste cambios no-Go y faltan tests o regresiones
- necesitas respetar el framework de tests del proyecto
- quieres añadir cobertura sin inventar otro estilo

---

# 10. Cuándo usar cada skill

## Usa `/review-change` cuando:

- ya hiciste el cambio
- quieres una revisión consolidada
- quieres que entren reviewer técnico y security reviewer según corresponda

## Usa `/bug-hunt` cuando:

- estás ante un bug real
- no quieres corregir a ciegas
- quieres causa raíz + fix mínimo + test de regresión

## Usa `/ship-safe` cuando:

- quieres hacer el ciclo completo de implementación con disciplina
- el cambio debe salir con tests, review y seguridad si aplica

---

# 11. Cómo se combinan subagentes y skills

La combinación correcta es esta:

## Subagentes

Especializan el trabajo.

Ejemplos:

- implementar Go
- revisar Go
- revisar no-Go
- depurar
- revisar seguridad
- escribir tests

## Skills

Orquestan el trabajo.

Ejemplos:

- revisar un cambio
- corregir un bug
- entregar un cambio completo

Regla práctica:

```text
Subagentes hacen partes del trabajo
Skills coordinan el flujo completo
```

---

# 12. Estado actual del sistema

Actualmente el sistema está más fuerte en la parte técnica que en la parte de producto.

## Ya cubierto

- implementación Go
- review Go
- review fullstack
- debugging
- seguridad
- tests no-Go
- workflows de review, bugfix y entrega segura

## Todavía débil o pendiente

- definición de producto
- arquitectura previa al código
- análisis explícito de riesgos
- análisis explícito de gaps
- QA funcional más allá de tests
- validación de release como workflow formal

---

# 13. Roles que faltan

## 13.1 `product-owner`

Debe encargarse de:

- definir problema
- identificar usuario y valor
- delimitar alcance
- escribir criterios de aceptación
- priorizar

## 13.2 `architect`

Debe encargarse de:

- traducir requisitos a estructura técnica
- definir límites del sistema
- identificar tradeoffs
- decidir shape general de la solución

## 13.3 `risk-reviewer`

Debe encargarse de:

- detectar riesgos operativos
- detectar riesgo de rollout
- detectar dependencias frágiles
- detectar puntos únicos de falla
- detectar complejidad peligrosa

## 13.4 `gap-reviewer`

Debe encargarse de:

- detectar ambigüedades
- detectar cobertura incompleta
- detectar criterios faltantes
- detectar supuestos no explicitados
- detectar huecos entre idea y entrega real

## 13.5 QA conceptual

Debe encargarse de:

- validar cobertura funcional
- revisar definición de done
- detectar flujos no cubiertos
- revisar escenarios borde desde comportamiento, no solo desde test framework

---

# 14. Skills que faltan

## 14.1 `shape-feature`

Debería convertir una idea en:

- problema
- usuario
- objetivo
- restricciones
- alcance
- criterios de aceptación

## 14.2 `build-plan`

Debería convertir una definición en:

- entregables
- piezas técnicas
- secuencia de implementación
- riesgos
- validaciones

## 14.3 `release-check`

Debería validar antes de desplegar:

- tests
- review
- seguridad
- riesgos pendientes
- readiness real

---

# 15. Workflows recomendados

## 15.1 Idea a implementación

```text
idea / problema
-> shape-feature
-> build-plan
-> implementación
-> tests
-> review-change
-> security-reviewer
-> release-check
-> despliegue
```

## 15.2 Bug a fix seguro

```text
incidente / bug
-> bug-hunt
-> fix mínimo
-> test de regresión
-> review-change
-> despliegue
```

## 15.3 Cambio técnico simple

```text
implementación
-> tests
-> review-change
-> listo
```

## 15.4 Cambio sensible

```text
implementación
-> tests
-> review-change
-> security-reviewer
-> release-check
-> despliegue
```

---

# 16. Filosofía de diseño

Este sistema sigue estas ideas:

- la implementación no se considera terminada solo porque compila
- primero se entiende el problema, luego se corrige o construye
- los cambios deben pasar por validación, review y seguridad cuando aplica
- el contexto principal no debe contaminarse con exploración innecesaria
- cada subagente debe ser enfocado
- cada skill debe ser un workflow claro y reusable
- la arquitectura debe emerger del problema, no de plantillas
- debugging sin causa raíz no es debugging
- tests sin criterio de comportamiento no son suficiente validación
- seguridad no es un “último toque”, sino una capa real de revisión

---

# 17. Reglas prácticas

## Regla 1
Si el problema es exploración, usa `Explore`.

## Regla 2
Si el problema es construir Go, usa `go-specialist`.

## Regla 3
Si el problema es validar Go, usa `go-reviewer`.

## Regla 4
Si el problema es revisar stacks no-Go, usa `code-reviewer`.

## Regla 5
Si el problema es encontrar la causa de un bug, usa `debugger`.

## Regla 6
Si el cambio toca auth, backend, deps, config o infra, mete `security-reviewer`.

## Regla 7
Si falta cobertura en stacks no-Go, usa `test-writer`.

## Regla 8
Si quieres cerrar un cambio completo, usa `/ship-safe`.

## Regla 9
Si quieres revisar un cambio ya hecho, usa `/review-change`.

## Regla 10
Si algo está roto, no lo parches primero: usa `/bug-hunt`.

---

# 18. Roadmap sugerido

## Fase 1 — Base técnica
Ya lograda en buena parte.

- `go-specialist`
- `go-reviewer`
- `code-reviewer`
- `debugger`
- `security-reviewer`
- `test-writer`
- `review-change`
- `bug-hunt`
- `ship-safe`

## Fase 2 — Producto y arquitectura
Pendiente.

- `product-owner`
- `architect`
- `shape-feature`
- `build-plan`

## Fase 3 — Riesgos y release
Pendiente.

- `risk-reviewer`
- `gap-reviewer`
- `release-check`

## Fase 4 — Coordinación más avanzada
Opcional después.

- teams reales
- hooks más inteligentes
- MCP para sistemas externos
- release automation
- integración con issue tracker y deploys

---

# 19. Resumen corto

```text
Subagente = especialista
Skill = workflow
Explore = investigación
Review = validación
Security = riesgo real
Debugger = causa raíz antes del fix
```

Este sistema hoy ya cubre bien la mitad técnica del ciclo.

La siguiente evolución natural es completar la mitad de producto y arquitectura para poder llevar mejor una idea desde negocio/problema hasta una entrega segura y validada.
