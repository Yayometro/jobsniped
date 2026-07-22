# JobSniper — Registro de coordinación entre IAs

> **Tipo de archivo:** registro operativo append-only  
> **Versión del protocolo:** 1.0.0  
> **Propietario:** Jair Vázquez  
> **Creado:** 2026-07-21 21:19 America/Mexico_City  
> **Documento canónico relacionado:** `PROJECT_MASTER_PLAN.md`

---

# 1. Propósito

Este archivo permite que ChatGPT/Codex, Claude, Gemini y modelos locales trabajen en el mismo repositorio sin perder contexto, repetir trabajo o cambiar decisiones silenciosamente.

Contiene:

- protocolo obligatorio antes de trabajar;
- identidad declarada de cada IA;
- tarea y alcance;
- archivos modificados;
- comandos y tests;
- hallazgos;
- pendientes;
- propuestas de cambio;
- handoff para la siguiente IA;
- historial de sesiones.

Este archivo no sustituye Git. Git es el registro técnico de cambios. Este archivo explica **por qué** se hicieron y qué debe saber el siguiente agente.

---

# 2. Regla append-only

Las entradas de sesiones anteriores no se reescriben ni se eliminan.

Cuando una entrada esté equivocada:

1. se agrega una corrección nueva;
2. se referencia la entrada original;
3. se explica el error;
4. no se borra el historial.

Las únicas secciones editables en la parte superior son:

- Estado actual resumido.
- Fase activa.
- Tareas en progreso.
- Bloqueos.
- Último commit estable.

Solo el integrador designado puede actualizar esas secciones.

---

# 3. Honestidad sobre el modelo

Cada IA debe registrar su identidad.

Campos obligatorios:

- proveedor;
- producto o cliente;
- nombre exacto del modelo mostrado por el runtime;
- modo de razonamiento mostrado;
- herramientas disponibles;
- fecha y hora;
- zona horaria.

## 3.1 Prohibición de inventar versión

Si el cliente no muestra el modelo exacto, la IA escribirá:

```text
Modelo exacto: no expuesto por el cliente
```

No debe inferir “Claude 4.8”, “Gemini X Pro” o cualquier número por comportamiento, marketing o memoria.

## 3.2 Descripción válida

Ejemplo:

```text
Proveedor: OpenAI
Cliente: ChatGPT web
Modelo expuesto: GPT-5.6 Thinking
Modo: Thinking
Herramientas: web, terminal, archivos
```

Ejemplo cuando no está visible:

```text
Proveedor: Anthropic
Cliente: Claude Code
Modelo expuesto: no expuesto por el cliente
Modo: no expuesto
Herramientas: terminal y edición de repositorio
```

---

# 4. Autoridad

Orden de autoridad:

1. Jair.
2. `PROJECT_MASTER_PLAN.md`.
3. Decisiones aprobadas en este archivo.
4. Código y tests.
5. Entradas de sesiones anteriores.
6. Suposiciones de una IA.

Una IA puede proponer. Jair aprueba. El integrador implementa la modificación canónica.

---

# 5. Protocolo obligatorio antes de trabajar

Cada IA debe ejecutar o comprobar:

1. leer `PROJECT_MASTER_PLAN.md`;
2. leer la sección “Estado actual” de este archivo;
3. leer al menos las últimas cinco entradas;
4. ejecutar `git status`;
5. identificar la rama;
6. identificar el commit base;
7. revisar tareas activas;
8. comprobar que no exista otro agente trabajando en los mismos archivos;
9. declarar tarea y alcance;
10. crear una rama si va a modificar código.

## 5.1 Encabezado de inicio

Antes de editar, la IA agrega o prepara esta información:

```markdown
### Inicio de sesión

- Session ID:
- Fecha y hora:
- Proveedor:
- Cliente:
- Modelo expuesto:
- Modo:
- Rol:
- Tarea:
- Rama:
- Commit base:
- Archivos reclamados:
- Criterio de finalización:
```

## 5.2 Si no puede escribir el log al inicio

Debe incluir la misma declaración en su primera respuesta a Jair y añadirla al log antes de cerrar la sesión.

---

# 6. Git y trabajo concurrente

Un archivo Markdown no es un sistema de locks. Para evitar conflictos se usa Git.

## 6.1 Rama por tarea

Formato:

```text
ai/<provider>/<task-id>-<descripcion>
```

Ejemplos:

```text
ai/openai/JOB-014-greenhouse-adapter
ai/anthropic/JOB-015-lever-tests
ai/google/JOB-016-react-job-list
```

## 6.2 No trabajar directo en `main`

`main` representa el último estado integrado y estable.

Los agentes:

- crean rama;
- hacen cambios;
- ejecutan tests;
- documentan;
- entregan commit;
- esperan merge.

## 6.3 Trabajo simultáneo

Se permite solo si los alcances no se cruzan.

Ejemplo seguro:

- agente A: `apps/local-service/src/adapters/greenhouse.ts`;
- agente B: `apps/dashboard/src/features/jobs/`;
- agente C: revisión de schemas sin editar código.

Ejemplo inseguro:

- dos agentes modificando `database/schema.sql`;
- dos agentes reorganizando carpetas;
- dos agentes cambiando el contrato `NormalizedJob`;
- dos agentes modificando el plan maestro.

## 6.4 Integrador

Un solo agente o Jair actúa como integrador por ciclo.

Responsabilidades:

- revisar diffs;
- comprobar tests;
- resolver conflictos;
- verificar que el plan siga vigente;
- hacer merge;
- actualizar estado;
- no aceptar cambios no documentados.

## 6.5 Worktrees opcionales

Cuando Jair ejecute agentes de terminal al mismo tiempo, puede usar `git worktree` para darles directorios físicos separados.

Esto evita que dos clientes editen el mismo working tree, pero no evita conflictos lógicos. Las áreas deben seguir asignadas.

---

# 7. Roles de IA

Las marcas no tienen un rol permanente. El rol se decide por tarea.

## 7.1 Arquitecto

- analiza decisiones;
- preserva límites;
- propone interfaces;
- no implementa cambios gigantes sin dividirlos.

## 7.2 Implementador

- desarrolla una unidad pequeña;
- toca únicamente archivos reclamados;
- agrega pruebas;
- evita refactors no solicitados.

## 7.3 Revisor

- revisa un commit ajeno;
- busca bugs;
- revisa seguridad;
- no reescribe todo por preferencia personal.

## 7.4 Tester

- crea fixtures;
- casos de borde;
- pruebas de regresión;
- golden datasets.

## 7.5 Investigador

- usa fuentes oficiales;
- registra fecha;
- separa hechos de inferencias;
- propone cambios, no los impone.

## 7.6 Integrador

- es el único que une ramas;
- actualiza estado;
- implementa cambios aprobados del plan.

---

# 8. Formato de tarea

Cada tarea debe tener:

```markdown
## JOB-NNN — Título

- Estado: backlog | ready | in_progress | review | done | blocked
- Prioridad: P0 | P1 | P2 | P3
- Fase:
- Objetivo:
- Fuera de alcance:
- Archivos esperados:
- Dependencias:
- Criterios de aceptación:
- Tests obligatorios:
- Responsable actual:
```

Una tarea que no puede explicarse con este formato es demasiado grande y debe dividirse.

---

# 9. Formato de entrada de sesión

Copiar al final del archivo:

```markdown
---

## SESSION-YYYYMMDD-HHMM-<provider>

### Identidad

- Fecha y hora:
- Zona horaria:
- Proveedor:
- Cliente:
- Modelo expuesto:
- Modo de razonamiento:
- Herramientas:
- Rol:

### Contexto leído

- Commit base:
- Rama:
- Plan maestro leído: sí/no
- Últimas entradas leídas:
- Tarea:

### Alcance

- Objetivo:
- Fuera de alcance:
- Archivos reclamados:

### Trabajo realizado

- Cambios:
- Decisiones:
- Hallazgos:
- Archivos modificados:
- Archivos creados:
- Archivos eliminados:

### Validación

- Comandos ejecutados:
- Tests:
- Resultado:
- Errores conocidos:
- Riesgos:

### Estado de Git

- Commit(s):
- Working tree limpio: sí/no
- Listo para merge: sí/no

### Handoff

- Falta:
- Próxima tarea recomendada:
- Instrucciones para el siguiente agente:
- Bloqueos:
- Propuestas de cambio al plan:
```

---

# 10. Propuestas de cambio

Las propuestas viven en este archivo hasta que Jair decida.

Formato:

```markdown
## PROPOSAL-NNN — Título

- Fecha:
- Propuesto por:
- Modelo/cliente:
- Estado: proposed | approved | rejected | implemented
- Problema:
- Evidencia:
- Cambio propuesto:
- Alternativas:
- Impacto técnico:
- Impacto en datos:
- Impacto en seguridad:
- Migración:
- Riesgos:
- Autorización de Jair:
- Commit de implementación:
```

## 10.1 Regla

Una propuesta aprobada todavía no está implementada.

Estados:

```text
proposed → approved → implemented
                    ↘ rejected
```

---

# 11. Cambios al plan maestro

Una IA no debe editar `PROJECT_MASTER_PLAN.md` durante una implementación normal.

Proceso:

1. agrega propuesta;
2. Jair aprueba;
3. integrador crea rama;
4. modifica plan;
5. agrega changelog;
6. ejecuta revisión;
7. merge.

Cambios pequeños de ortografía pueden agruparse, pero deben registrarse si alteran significado.

---

# 12. Comandos y evidencia

No escribir “los tests pasan” sin indicar qué se ejecutó.

Ejemplo válido:

```text
bun test apps/local-service/tests/adapters/greenhouse.test.ts
Resultado: 8 passed, 0 failed
```

Ejemplo inválido:

```text
Todo se ve bien.
```

Cuando una IA no pueda ejecutar comandos:

- debe decirlo;
- no puede afirmar que el código compila;
- entrega una lista de comandos para que otro agente los ejecute.

---

# 13. Reglas de implementación

1. No ampliar alcance sin registrar.
2. No hacer refactors masivos junto con una feature.
3. No introducir dependencias sin justificar.
4. No cambiar contratos compartidos sin revisar consumidores.
5. No cambiar schema sin migración.
6. No borrar datos.
7. No agregar secretos.
8. No desactivar tests para “hacerlos pasar”.
9. No inventar resultados.
10. No modificar reglas de seguridad por conveniencia.
11. No usar LinkedIn automatizado.
12. No auto-submit.
13. No exponer API en red pública.
14. No colocar PII en logs.

---

# 14. Definition of Done

Una tarea está terminada cuando:

- cumple criterios;
- compila;
- tests pasan;
- no hay errores de lint relevantes;
- contrato está validado;
- documentación mínima está actualizada;
- no hay secretos;
- Git muestra solo cambios esperados;
- se agregó entrada de sesión;
- existe commit;
- está lista para revisión.

“Escribí el código” no significa “terminado”.

---

# 15. Estado actual

> Esta sección puede actualizarla únicamente el integrador.

- **Fecha:** 2026-07-21 21:19 America/Mexico_City
- **Fase activa:** Fase 0 — Gobierno y memoria compartida
- **Estado:** repositorio local inicializado y enlazado con GitHub; documentación inicial preparada para commit
- **Último commit estable local:** `48dab4f62a09b3f7284edce3ec11d7a96674e8b4`
- **Rama estable:** `main`, rastrea `origin/main`
- **Repositorio remoto canónico:** `https://github.com/Yayometro/jobsniped.git`
- **Directorio local canónico:** `/Users/luisjairvazqueznavarrete/Coding Proyects/JobSniper`
- **Tareas en progreso:** ninguna
- **Bloqueos:** falta autenticar GitHub en este equipo para publicar los commits locales
- **Siguiente puerta:** autenticar GitHub, ejecutar `git push origin main` y comenzar JOB-002
- **Arquitectura vigente:** React + Vite, Bun local service, SQLite, Ollama, Telegram
- **Servidor:** documentado; no implementar
- **LinkedIn:** manual y alertas nativas; no scraping
- **Submit:** manual

---

# 16. Backlog inicial

## JOB-001 — Inicializar repositorio

- Estado: ready
- Prioridad: P0
- Fase: 0
- Objetivo: crear repo Git y guardar documentos
- Fuera de alcance: código de producto
- Criterios:
  - `git init`;
  - rama `main`;
  - dos archivos canónicos;
  - `.gitignore`;
  - primer commit.

## JOB-002 — Crear fuente de verdad personal

- Estado: backlog
- Prioridad: P0
- Fase: 1
- Objetivo: crear los cinco archivos privados/profesionales
- Dependencia: JOB-001
- Criterio: llenar formulario ATS en menos de tres minutos.

## JOB-003 — Scaffold Bun + React/Vite

- Estado: backlog
- Prioridad: P1
- Fase: 2
- Objetivo: API local, SQLite y dashboard mínimo
- Dependencia: JOB-001
- Criterio: `/health` responde y dashboard muestra estado.

## JOB-004 — Primer pool de 30 empresas

- Estado: backlog
- Prioridad: P1
- Fase: 3
- Objetivo: CSV verificado
- Dependencia: JOB-001
- Criterio: 30 tokens/URLs válidos.

## JOB-005 — Greenhouse adapter

- Estado: backlog
- Prioridad: P1
- Fase: 4
- Dependencias: JOB-003, JOB-004
- Criterios: parsing, fixtures, dedup y persistencia.

---

# 17. Entrada inicial

---

## SESSION-20260721-2119-openai

### Identidad

- **Fecha y hora:** 2026-07-21 21:19 America/Mexico_City
- **Zona horaria:** America/Mexico_City
- **Proveedor:** OpenAI
- **Cliente:** ChatGPT web
- **Modelo expuesto:** GPT-5.6 Thinking
- **Modo de razonamiento:** Thinking
- **Herramientas:** archivos, web, Python para creación de artefactos
- **Rol:** arquitecto, integrador inicial y documentador

### Contexto leído

- **Commit base:** no existe
- **Rama:** no existe
- **Plan maestro leído:** creado en esta sesión
- **Fuentes leídas:** dos planes adjuntos y notas explícitas de Jair
- **Tarea:** consolidar la arquitectura y crear los dos documentos compartidos

### Alcance

- **Objetivo:** producir el plan canónico y protocolo multi-IA
- **Fuera de alcance:** implementar código, crear repo local o configurar Ollama
- **Archivos reclamados:** `PROJECT_MASTER_PLAN.md`, `AI_COORDINATION_LOG.md`

### Trabajo realizado

- Reemplazó Next.js por React + Vite.
- Mantuvo Bun, TypeScript, SQLite, Zod y Telegram.
- Definió servicio local con API.
- Documentó servidor futuro sin implementarlo.
- Sustituyó límite rígido por advertencias progresivas.
- Definió gobernanza, ramas, roles, sesiones y propuestas.
- Definió honestidad obligatoria sobre el modelo.
- Definió fases, puertas y backlog inicial.

### Validación

- Se compararon los dos planes adjuntos.
- Se verificaron fuentes oficiales de React, Vite, Ollama, Apple, LinkedIn y ATS.
- No se ejecutaron tests de código porque todavía no existe código.

### Estado de Git

- **Commits:** ninguno
- **Working tree limpio:** no aplica
- **Listo para merge:** los archivos están listos para ser copiados al repo

### Handoff

- **Falta:** inicializar el repositorio local.
- **Próxima tarea:** JOB-001.
- **Instrucciones:** leer ambos archivos, crear Git, guardar en raíz, añadir `.gitignore`, hacer commit inicial.
- **Bloqueos:** ninguno técnico.
- **Propuestas al plan:** ninguna pendiente.

---

# 18. Plantilla vacía para la siguiente IA

Copiar esta sección al final, completarla y no modificar entradas anteriores.

```markdown
---

## SESSION-YYYYMMDD-HHMM-provider

### Identidad
- Fecha y hora:
- Zona horaria: America/Mexico_City
- Proveedor:
- Cliente:
- Modelo expuesto:
- Modo de razonamiento:
- Herramientas:
- Rol:

### Contexto leído
- Commit base:
- Rama:
- Plan maestro leído:
- Últimas entradas leídas:
- Tarea:

### Alcance
- Objetivo:
- Fuera de alcance:
- Archivos reclamados:

### Trabajo realizado
- Cambios:
- Decisiones:
- Hallazgos:
- Archivos modificados:
- Archivos creados:
- Archivos eliminados:

### Validación
- Comandos ejecutados:
- Tests:
- Resultado:
- Errores conocidos:
- Riesgos:

### Estado de Git
- Commit(s):
- Working tree limpio:
- Listo para merge:

### Handoff
- Falta:
- Próxima tarea recomendada:
- Instrucciones para el siguiente agente:
- Bloqueos:
- Propuestas de cambio al plan:
```

---

**Fin del protocolo inicial. Las nuevas sesiones se agregan debajo de esta línea.**

---

## SESSION-20260722-1403-openai

### Identidad

- **Fecha y hora:** 2026-07-22 14:03 America/Mexico_City
- **Zona horaria:** America/Mexico_City
- **Proveedor:** OpenAI
- **Cliente:** Codex desktop
- **Modelo expuesto:** GPT-5
- **Modo de razonamiento:** no expuesto por el cliente
- **Herramientas:** terminal, archivos y Git
- **Rol:** integrador

### Contexto leído

- **Commit base:** `b25e6cb2087201f679ac0fdb7748a834f651ee12` (`origin/main`)
- **Rama:** `main`
- **Plan maestro leído:** sí, versión 1.0.0
- **Últimas entradas leídas:** entrada inicial completa
- **Tarea:** enlazar el directorio local existente con el repositorio GitHub indicado por Jair y documentar su ubicación canónica

### Alcance

- **Objetivo:** inicializar Git, configurar `origin`, conservar el historial remoto y registrar el repositorio local y remoto
- **Fuera de alcance:** scaffold de producto, archivos personales, adaptadores y configuración de Ollama
- **Archivos reclamados:** `PROJECT_MASTER_PLAN.md`, `AI_COORDINATION_LOG.md`, `.gitignore`

### Trabajo realizado

- Se comprobó que el remoto ya contenía la rama `main` y un commit inicial con `README.md`.
- Se inicializó Git en el directorio local sin reemplazar la historia remota.
- Se configuró `origin` con `https://github.com/Yayometro/jobsniped.git`.
- Se vinculó la rama local `main` con `origin/main`.
- Se documentaron la URL, la ruta local y la rama canónicas.
- Se añadió el `.gitignore` inicial de privacidad y artefactos locales.

### Validación

- **Comandos ejecutados:** `git ls-remote --symref`, `git init -b main`, `git fetch origin main`, `git checkout -B main --track origin/main`, `git remote -v`, `git status`
- **Tests:** no aplican; todavía no existe código de producto
- **Resultado:** el working tree local conserva los documentos existentes y `main` rastrea `origin/main`
- **Errores conocidos:** `git push origin main` no pudo leer credenciales HTTPS; GitHub CLI no está instalado y no hay llave SSH disponible
- **Riesgos:** la URL conserva el nombre remoto existente `jobsniped`, aunque el producto se llama JobSniper

### Estado de Git

- **Commit(s):** `48dab4f62a09b3f7284edce3ec11d7a96674e8b4` — `docs: establish canonical JobSniper repository`
- **Working tree limpio:** sí al crear el commit de integración; esta actualización final de la bitácora requiere un commit adicional
- **Listo para merge:** trabajo realizado directamente sobre `main` por instrucción explícita de Jair durante la inicialización

### Handoff

- **Falta:** autenticar GitHub y publicar los commits locales; después comenzar JOB-002
- **Próxima tarea recomendada:** JOB-002 — Crear fuente de verdad personal
- **Instrucciones para el siguiente agente:** trabajar únicamente desde la ruta canónica, leer ambos documentos y verificar `origin/main` antes de editar
- **Bloqueos:** credenciales de GitHub no disponibles en el entorno actual
- **Propuestas de cambio al plan:** ninguna
