# JobSniper — Plan maestro del proyecto

> **Estado:** Plan canónico aprobado para iniciar la construcción  
> **Versión:** 1.0.0  
> **Propietario y autoridad final:** Jair Vázquez  
> **Última actualización:** 2026-07-22 14:03 America/Mexico_City  
> **Archivo complementario obligatorio:** `AI_COORDINATION_LOG.md`  
> **Regla principal:** cualquier IA o persona debe leer este archivo y las últimas entradas de `AI_COORDINATION_LOG.md` antes de modificar código, datos, arquitectura o documentación.
> **Repositorio Git canónico:** `https://github.com/Yayometro/jobsniped.git`  
> **Directorio local canónico:** `/Users/luisjairvazqueznavarrete/Coding Proyects/JobSniper`  
> **Rama estable:** `main`

---

## 0. Para qué existe este documento

Este archivo es la **fuente única de verdad** de JobSniper.

Sirve para que ChatGPT/Codex, Claude, Gemini, modelos locales y Jair trabajen sobre la misma arquitectura, las mismas reglas y el mismo orden de prioridades. No es un borrador temporal ni una conversación pegada: es el contrato operativo del proyecto.

Este archivo contiene:

- el objetivo real del sistema;
- lo que está permitido y lo que está prohibido;
- la arquitectura local;
- la arquitectura futura con servidor;
- los lenguajes, herramientas y modelos;
- la estructura de carpetas;
- el modelo de datos;
- el flujo de una vacante;
- el scoring;
- la seguridad y privacidad;
- el plan de implementación por fases;
- los criterios que deben cumplirse antes de avanzar;
- el protocolo para modificar este plan.

### 0.1 Autoridad y cambios

Jair es la única autoridad que puede aprobar cambios al plan maestro.

Una IA puede:

1. detectar un problema;
2. proponer un cambio en `AI_COORDINATION_LOG.md`;
3. explicar impacto, riesgos y alternativa;
4. esperar autorización explícita de Jair;
5. actualizar este documento después de la autorización;
6. agregar la modificación al historial al final del archivo.

Una IA **no debe cambiar silenciosamente** una decisión canónica porque “le parece mejor”.

### 0.2 Qué significa “canónico”

Cuando exista una contradicción:

1. manda la instrucción más reciente y explícita de Jair;
2. después manda este archivo;
3. después manda `AI_COORDINATION_LOG.md`;
4. después mandan comentarios de código y documentos secundarios;
5. una conversación anterior nunca reemplaza automáticamente este plan.

### 0.3 Ubicación canónica del proyecto

El repositorio remoto oficial de JobSniper es:

```text
https://github.com/Yayometro/jobsniped.git
```

El working tree local principal de Jair está en:

```text
/Users/luisjairvazqueznavarrete/Coding Proyects/JobSniper
```

La rama estable es `main` y el remoto debe llamarse `origin`. Antes de trabajar, cualquier IA debe confirmar que está dentro de este repositorio y ejecutar `git remote -v`, `git branch --show-current` y `git status`. Una copia en otra ruta no sustituye al working tree canónico salvo instrucción explícita de Jair o uso documentado de `git worktree`.

---

# 1. Resumen ejecutivo

JobSniper será una aplicación local que corre principalmente en la MacBook Pro M4 Max de Jair.

No será un bot que envía solicitudes. Será un **sistema de inteligencia de vacantes** que:

1. consulta job boards y páginas públicas de empresas;
2. detecta vacantes nuevas;
3. normaliza la información;
4. evita duplicados;
5. descarta puestos claramente irrelevantes;
6. analiza los puestos restantes con un modelo local;
7. calcula un score reproducible;
8. manda alertas por Telegram;
9. muestra una cola en un dashboard de React;
10. prepara evidencia, CV y respuestas;
11. deja la revisión y el botón `Submit` en manos de Jair;
12. registra resultados para aprender qué fuentes generan entrevistas.

El objetivo no es enviar cientos de solicitudes genéricas. El objetivo es reducir el tiempo de una aplicación relevante sin degradar su calidad.

```text
Fuentes públicas
      ↓
Colectores
      ↓
Normalización
      ↓
SQLite + deduplicación
      ↓
Reglas duras
      ↓
LLM local extrae hechos
      ↓
Código calcula score
      ↓
Telegram + React dashboard
      ↓
Preparación de aplicación
      ↓
Jair revisa y presiona Submit
      ↓
Tracking y métricas
```

---

# 2. Qué es JobSniper y qué no es

## 2.1 Sí es

- Un radar de vacantes.
- Un agregador local de fuentes públicas.
- Un clasificador de oportunidades.
- Un sistema de deduplicación.
- Un asistente de preparación.
- Un registro de aplicaciones.
- Un proyecto local-first y privacy-first.
- Un proyecto de portafolio técnicamente defendible.
- Una herramienta para aumentar velocidad y consistencia.

## 2.2 No es

- No es un scraper autenticado de LinkedIn.
- No es un bot de Easy Apply.
- No es un sistema de auto-submit.
- No es una herramienta para evadir detección.
- No es una fábrica de CVs con experiencia inventada.
- No es una excusa para dejar de aplicar mientras se construye.
- No es un servicio 24/7 durante el MVP.
- No es una plataforma pública multiusuario.
- No necesita Next.js.
- No necesita una base remota al inicio.

---

# 3. Objetivos y métricas de éxito

## 3.1 Objetivos de producto

1. Detectar vacantes relevantes lo antes posible desde sus fuentes originales.
2. Reducir ruido y duplicados.
3. Interpretar correctamente restricciones de ubicación y autorización laboral.
4. Recomendar el CV correcto.
5. Mostrar evidencia real para cada requisito.
6. Reducir el tiempo por aplicación.
7. Registrar respuestas, entrevistas y resultados.
8. Aprender de los datos reales de Jair.

## 3.2 Métricas principales

Las métricas que importan son:

- aplicaciones relevantes por hora;
- tiempo medio por aplicación;
- respuestas por cada 100 aplicaciones;
- entrevistas por cada 100 aplicaciones;
- entrevistas por fuente;
- entrevistas por empresa;
- entrevistas por CV;
- entrevistas por rango de score;
- tiempo entre `first_seen_at` y `applied_at`;
- falsos positivos del scoring;
- falsos negativos del scoring;
- porcentaje de vacantes con ubicación ambigua;
- errores de autorización laboral detectados antes del envío.

## 3.3 Meta inicial

La primera meta operativa será:

- completar cinco aplicaciones relevantes en menos de quince minutos;
- sin inventar experiencia;
- sin enviar automáticamente;
- con cada aplicación revisada por Jair.

## 3.4 Regla contra la postergación

Desde la Fase 0, Jair seguirá enviando al menos cinco aplicaciones manuales al día cuando existan vacantes razonables.

Si JobSniper empieza a generar entrevistas antes de estar “terminado”, se prioriza la búsqueda de trabajo sobre la construcción de funciones adicionales.

---

# 4. Reglas invariables

Estas reglas no se negocian sin una decisión explícita y registrada de Jair.

## 4.1 LinkedIn

1. El sistema nunca guarda ni usa la cookie `li_at`.
2. Nunca inicia sesión en LinkedIn con Playwright, Selenium, Puppeteer o browser-use.
3. Nunca scrapea LinkedIn autenticado.
4. No se implementa scraping de LinkedIn guest.
5. No se crea una cuenta secundaria para automatización.
6. LinkedIn se usa manualmente y mediante alertas nativas.
7. Una vacante encontrada manualmente puede registrarse en JobSniper.

## 4.2 Envío

1. El sistema nunca presiona `Submit`.
2. El sistema nunca envía Easy Apply.
3. El sistema nunca envía automáticamente formularios de ATS.
4. Jair revisa todas las respuestas.
5. Jair decide si aplicar incluso cuando el score sea alto.

## 4.3 Veracidad

1. El LLM no puede afirmar una habilidad sin evidencia.
2. El LLM no puede aumentar años de experiencia.
3. El LLM no puede cambiar cargos, empresas ni fechas.
4. Un requisito sin evidencia se muestra como faltante o parcial.
5. Los cambios de CV deben indicar la evidencia que los respalda.
6. La validación anti-alucinación se implementa en código, no solo en prompts.

## 4.4 Privacidad

1. No se suben cookies ni contraseñas.
2. Los datos privados se mantienen fuera de Git.
3. Los datos EEO no participan en scoring.
4. Ollama no se expone a internet.
5. El servidor futuro no recibirá datos personales salvo una decisión posterior explícita.
6. Los logs no deben contener teléfono, correo completo, dirección ni respuestas sensibles.

## 4.5 Construcción incremental

1. No se agregan 500 empresas antes de validar 30.
2. No se agrega IA antes de que funcione el detector básico.
3. No se agrega Workday antes de estabilizar los ATS sencillos.
4. No se agregan embeddings antes de que el volumen los justifique.
5. No se paga un servidor antes de demostrar valor local.
6. No se construye autofill propio antes de probar una solución supervisada existente.

---

# 5. Decisiones tecnológicas definitivas

| Capa | Elección inicial | Motivo |
|---|---|---|
| Lenguaje principal | TypeScript | Unifica poller, API, dashboard, validación y tipos |
| Runtime local | Bun | Inicio rápido, `fetch`, TypeScript y SQLite integrado |
| HTTP | `fetch` nativo | Suficiente para APIs públicas |
| Validación | Zod | Protege contra respuestas externas y JSON inválido del LLM |
| Base local | SQLite mediante `bun:sqlite` | Simple, local, transaccional y suficiente |
| ORM | Ninguno en MVP | SQL directo reduce configuración |
| Servicio local | `Bun.serve` | API local ligera sin framework obligatorio |
| Dashboard | React + Vite + TypeScript | SPA local, rápida y sin Next.js |
| Estado de servidor | TanStack Query | Cache y sincronización con la API local |
| Routing | React Router si se requieren múltiples vistas | Separación clara de cola, detalle y métricas |
| CSS | CSS normal o Tailwind, decisión de implementación | No afecta arquitectura |
| Scheduler | `launchd` + `StartCalendarInterval` + `next_check_at` | Integración nativa con macOS |
| IA local | Ollama con backend disponible para Apple Silicon | API local estable |
| Modelo diario | Qwen3.5-9B | Extracción y clasificación a bajo costo |
| Modelo profundo | Qwen3.6-27B | Análisis, código y preparación compleja |
| Embeddings futuros | Qwen3-Embedding-0.6B | Similitud y búsqueda semántica |
| Notificaciones | Telegram Bot | Push rápido y multiplataforma |
| Browser automation | Playwright solo como último recurso | Workday o páginas públicas sin endpoint |
| Tests | Vitest + fixtures + MSW cuando convenga | Pruebas deterministas de adaptadores |
| Control de cambios | Git | Historial, ramas, rollback y colaboración multi-IA |
| Ejecución 24/7 | Documentada, no implementada inicialmente | Evita gasto y complejidad prematuros |

---

# 6. Arquitectura general

## 6.1 Arquitectura activa durante el MVP

```text
┌────────────────────────────────────────────────────┐
│ FUENTES PÚBLICAS                                   │
│ Greenhouse · Lever · Ashby · Workable              │
│ RSS y alertas nativas en fases posteriores         │
│ Workday/custom con Playwright solo al final         │
└───────────────────────┬────────────────────────────┘
                        │
                        ▼
┌────────────────────────────────────────────────────┐
│ JOBSNIPER LOCAL SERVICE — Bun                      │
│                                                    │
│  adapters → normalizer → dedup → SQLite            │
│  hard filters → Ollama → deterministic scoring     │
│  Telegram → local REST API                         │
└───────────────────────┬────────────────────────────┘
                        │ http://127.0.0.1
                        ▼
┌────────────────────────────────────────────────────┐
│ REACT + VITE DASHBOARD                             │
│ cola · detalle · aplicación · historial · métricas │
└───────────────────────┬────────────────────────────┘
                        │
                        ▼
                Jair revisa y envía
```

## 6.2 Separación de responsabilidades

### Servicio local de Bun

Es responsable de:

- consultar fuentes;
- respetar frecuencias;
- validar respuestas;
- normalizar vacantes;
- guardar SQLite;
- detectar duplicados;
- ejecutar filtros;
- llamar Ollama;
- calcular scores;
- mandar Telegram;
- exponer una API local;
- servir el build estático del dashboard en una etapa posterior.

### Dashboard de React

Es responsable de:

- mostrar datos;
- filtrar y ordenar;
- abrir vacantes;
- copiar respuestas;
- mostrar evidencia;
- marcar estados;
- solicitar análisis profundo;
- mostrar métricas;
- nunca acceder directamente a SQLite;
- nunca guardar secretos en el navegador.

### Ollama

Es responsable de:

- extracción estructurada;
- clasificación semántica;
- detección de ambigüedades;
- análisis profundo bajo demanda;
- preparación de aplicación;
- nunca calcular por sí solo el score final;
- nunca decidir el envío.

---

# 7. Fuentes de vacantes

## 7.1 Orden de implementación

1. Greenhouse.
2. Lever.
3. Ashby.
4. Workable.
5. RSS y job boards públicos con feeds.
6. Alertas nativas de LinkedIn por correo.
7. Career pages personalizadas.
8. Workday.
9. Otros ATS: SmartRecruiters, Teamtailor, Recruitee, BambooHR u otros según cobertura real.

## 7.2 Greenhouse, Lever, Ashby y Workable

Para estos vendors se utilizarán endpoints públicos cuando estén disponibles.

No se usará Playwright si un `fetch` devuelve datos estructurados.

Cada vendor tendrá un adaptador independiente que convertirá su respuesta al mismo contrato `NormalizedJob`.

## 7.3 LinkedIn

LinkedIn no forma parte del scraping.

Se contemplan tres usos:

1. búsquedas manuales;
2. alertas nativas diarias o semanales;
3. registro manual o parsing futuro de correos de alertas.

El parser de correo será una fuente secundaria porque las alertas no garantizan baja latencia.

No se accederá al sitio de LinkedIn desde el poller.

## 7.4 Workday

Workday se implementa al final porque:

- cambia entre tenants;
- utiliza frontends complejos;
- puede requerir identificar peticiones internas;
- a veces necesita Playwright;
- es más frágil y costoso de mantener.

Primero se intentará un adaptador HTTP. Playwright será la última opción.

---

# 8. Contratos y tipos principales

## 8.1 Empresa

```ts
export type AtsVendor =
  | "greenhouse"
  | "lever"
  | "ashby"
  | "workable"
  | "workday"
  | "custom"
  | "rss"
  | "linkedin-alert";

export type CompanyTier = "A" | "B" | "C";

export interface Company {
  id: string;
  name: string;
  domain: string;
  careersUrl: string;
  ats: AtsVendor;
  token?: string;
  tier: CompanyTier;
  country?: string;
  latamHiring: "yes" | "no" | "unknown";
  status: "active" | "paused" | "broken";
  lastCheckedAt?: string;
  nextCheckAt?: string;
}
```

## 8.2 Vacante normalizada

```ts
export interface NormalizedJob {
  source: AtsVendor;
  sourceJobId: string;
  companyId: string;

  title: string;
  department?: string;
  descriptionPlain: string;
  descriptionHtml?: string;

  locationRaw?: string;
  workplaceType: "remote" | "hybrid" | "onsite" | "unknown";
  employmentType?: string;

  salaryMin?: number;
  salaryMax?: number;
  salaryCurrency?: string;

  sourcePublishedAt?: string;
  sourceUpdatedAt?: string;

  jobUrl: string;
  applyUrl: string;
}
```

## 8.3 Adaptador

```ts
export interface AtsAdapter {
  source: AtsVendor;
  fetchJobs(company: Company, cache?: HttpCache): Promise<FetchJobsResult>;
}
```

El poller no debe conocer detalles internos del vendor.

---

# 9. Estructura final del repositorio

```text
jobsniper/
├── PROJECT_MASTER_PLAN.md
├── AI_COORDINATION_LOG.md
├── package.json
├── bun.lock
├── tsconfig.base.json
├── .gitignore
├── .env.example
│
├── apps/
│   ├── local-service/
│   │   ├── src/
│   │   │   ├── api/
│   │   │   │   ├── server.ts
│   │   │   │   ├── routes/
│   │   │   │   └── serializers/
│   │   │   ├── poller/
│   │   │   │   ├── poll.ts
│   │   │   │   ├── scheduler.ts
│   │   │   │   └── source-health.ts
│   │   │   ├── adapters/
│   │   │   │   ├── greenhouse.ts
│   │   │   │   ├── lever.ts
│   │   │   │   ├── ashby.ts
│   │   │   │   ├── workable.ts
│   │   │   │   ├── workday.ts
│   │   │   │   └── adapter-registry.ts
│   │   │   ├── normalization/
│   │   │   ├── deduplication/
│   │   │   ├── filters/
│   │   │   ├── scoring/
│   │   │   ├── llm/
│   │   │   ├── notifications/
│   │   │   ├── database/
│   │   │   │   ├── connection.ts
│   │   │   │   ├── migrations/
│   │   │   │   └── repositories/
│   │   │   ├── security/
│   │   │   └── config/
│   │   └── tests/
│   │
│   └── dashboard/
│       ├── src/
│       │   ├── app/
│       │   ├── api/
│       │   ├── components/
│       │   ├── features/
│       │   │   ├── jobs/
│       │   │   ├── applications/
│       │   │   ├── companies/
│       │   │   └── metrics/
│       │   ├── routes/
│       │   ├── hooks/
│       │   └── styles/
│       ├── vite.config.ts
│       └── tests/
│
├── packages/
│   └── shared/
│       ├── src/
│       │   ├── types/
│       │   ├── schemas/
│       │   └── constants/
│       └── tests/
│
├── data/
│   ├── companies.csv
│   ├── jobs.sqlite
│   ├── fixtures/
│   └── private/
│       ├── profile.yaml
│       ├── evidence.json
│       ├── answers.private.json
│       ├── employment-scenarios.json
│       └── eeo.private.json
│
├── resumes/
│   ├── source/
│   └── generated/
│
├── scripts/
│   ├── import-companies.ts
│   ├── backup-db.ts
│   └── install-launch-agent.sh
│
├── config/
│   ├── scoring.yaml
│   ├── filters.yaml
│   └── models.yaml
│
└── logs/
```

## 9.1 Por qué una aplicación React separada

El dashboard es una SPA local.

Durante desarrollo:

```text
React/Vite: http://127.0.0.1:5173
Bun API:    http://127.0.0.1:8787
```

Vite tendrá un proxy para `/api`.

Después:

1. Vite genera `apps/dashboard/dist`;
2. `Bun.serve` puede servir esos archivos;
3. todo puede abrirse desde una sola URL local;
4. no se necesita Next.js ni SSR.

---

# 10. Archivos de perfil y verdad profesional

## 10.1 `profile.yaml`

Contiene únicamente datos útiles para scoring:

- roles objetivo;
- seniority;
- años reales;
- skills fuertes;
- skills de trabajo;
- skills en aprendizaje;
- preferencias de modalidad;
- idiomas;
- zona horaria;
- restricciones.

No contiene teléfono, documentos, EEO ni contraseñas.

## 10.2 `evidence.json`

Respalda cada habilidad y afirmación.

```json
{
  "React": {
    "years": 4,
    "evidence": [
      {
        "context": "Octaura",
        "what": "Widgets financieros con React y TypeScript"
      }
    ]
  },
  "GraphQL": {
    "years": 0,
    "evidence": [],
    "note": "Sin experiencia comprobable en producción"
  }
}
```

## 10.3 `answers.private.json`

Contiene:

- identidad;
- datos de contacto;
- URLs;
- disponibilidad;
- compensación;
- respuestas largas;
- datos de contratación;
- notice period.

Está en `.gitignore`.

## 10.4 `employment-scenarios.json`

Separa escenarios que suelen confundirse:

- empleado en México;
- empleado legal en Estados Unidos;
- contractor en México para empresa estadounidense;
- EOR/PEO;
- contratación en Canadá;
- reubicación.

El sistema nunca reutiliza una respuesta de autorización sin identificar primero el escenario exacto.

## 10.5 `eeo.private.json`

Contiene respuestas EEO opcionales.

No se envía:

- al scoring;
- al modelo;
- al servidor;
- a Telegram;
- a logs.

Se consulta manualmente al llenar el formulario.

---

# 11. Base de datos

## 11.1 Tablas mínimas

### `companies`

Registra cada empresa, vendor, token, tier, estado y salud.

### `jobs`

Registra la identidad canónica, contenido, ubicación, fechas, estado y score.

### `job_sources`

Relaciona una vacante canónica con todas las fuentes donde apareció.

### `applications`

Registra cada aplicación enviada manualmente.

### `application_events`

Registra cambios:

- applied;
- recruiter_response;
- screen;
- technical_interview;
- final_interview;
- rejected;
- offer;
- withdrawn;
- no_response.

### `poll_runs`

Registra ejecuciones, duración, nuevas vacantes y errores.

### `source_health`

Registra ETag, fallos, `disabled_until` y último error.

### `ai_analyses`

Guarda versión del modelo, prompt/schema, salida validada y fecha.

## 11.2 Fechas importantes

- `source_published_at`: dato reportado por la fuente.
- `source_updated_at`: fecha de actualización reportada.
- `first_seen_at`: primera vez que JobSniper vio la vacante.
- `last_seen_at`: última vez que siguió activa.
- `closed_at`: fecha en que dejó de aparecer.
- `applied_at`: momento en que Jair marcó la aplicación.

`first_seen_at` es la fecha operativa más confiable para medir qué tan fresca era una vacante.

---

# 12. Deduplicación

## 12.1 Identificador de fuente

```text
greenhouse:stripe:123456
```

Evita procesar dos veces el mismo ID.

## 12.2 Fingerprint canónico

```text
sha256(
  company_domain
  + normalized_title
  + normalized_location
  + normalized_department
  + stable_description_fragment
)
```

## 12.3 Resultado de deduplicación

Una comparación puede ser:

- `exact`;
- `probable_duplicate`;
- `different`.

Solo los duplicados exactos se fusionan automáticamente.

Los probables se muestran para revisión o se resuelven con reglas conservadoras.

---

# 13. Política de aplicaciones por empresa

No habrá una regla rígida de “una aplicación cada 30 días”.

Tampoco se asumirá que 10–15 solicitudes a la misma empresa siempre son inocuas.

La política será contextual y configurable.

## 13.1 Bloqueos reales

Se bloquea únicamente:

- volver a marcar como aplicada la misma vacante;
- una vacante con el mismo `sourceJobId`;
- un duplicado exacto ya aplicado;
- una operación accidental repetida.

## 13.2 Advertencias progresivas en una ventana de 30 días

| Aplicaciones a la empresa | Comportamiento |
|---:|---|
| 0–2 | Sin advertencia |
| 3–5 | Advertencia amarilla: revisar roles anteriores |
| 6–9 | Advertencia naranja: confirmar equipo y diferencia real |
| 10–14 | Advertencia roja: escribir una razón antes de continuar |
| 15+ | Interlock configurable; no impide aplicar manualmente fuera del sistema |

## 13.3 Similitud del rol

La cantidad no es el único criterio.

El sistema revisará:

- similitud de título;
- equipo;
- ubicación;
- seniority;
- descripción;
- fecha;
- CV usado;
- estado de aplicaciones anteriores.

Aplicar a Frontend Platform y Design Systems puede ser razonable. Aplicar a ocho variantes del mismo Frontend Engineer en una semana probablemente no.

## 13.4 Jair conserva autoridad

La aplicación nunca es automática. Por lo tanto, cualquier advertencia es informativa y Jair puede decidir continuar.

---

# 14. Scheduler y comportamiento de la Mac

## 14.1 Un solo LaunchAgent

No se crearán LaunchAgents separados por tier.

Habrá un solo entrypoint cada quince minutos.

```text
launchd
   ↓
bun run poll
   ↓
SELECT companies WHERE next_check_at <= now
   ↓
consulta las vencidas
   ↓
actualiza next_check_at por tier
```

## 14.2 Frecuencia inicial

### Con 30 empresas

- Tier A: 10 empresas, cada 15 minutos.
- Tier B: 20 empresas, cada 2 horas.

### Con 80 empresas

- Tier A: 20 empresas, cada 15 minutos.
- Tier B: 60 empresas, cada 2 horas.

### Con 500 empresas

- Tier A: 60 empresas, cada 15 minutos.
- Tier B: 200 empresas, cada 2 horas.
- Tier C: 240 empresas, cada 6 horas.

Las frecuencias se ajustarán con métricas y rate limits.

## 14.3 Catch-up

Si la Mac estuvo dormida:

- no reproduce cada intervalo perdido;
- consulta una vez cada empresa vencida;
- actualiza `next_check_at`;
- continúa normalmente.

## 14.4 Tapa cerrada

La expectativa correcta es:

### Modo A — Predeterminado

- La Mac corre JobSniper cuando está despierta.
- Si duerme, el poller se pausa.
- Al despertar, realiza catch-up.
- No tiene costo.
- Es suficiente para validar el MVP.

### Modo B — Escritorio/clamshell

- La Mac está conectada a corriente.
- Utiliza el modo admitido con periféricos/pantalla externa cuando corresponda.
- Puede mantenerse operativa con la tapa cerrada.
- Se debe vigilar ventilación y temperatura.
- No es un requisito del MVP.

### Modo C — Mantener despierta deliberadamente

- Puede utilizarse una configuración o utilidad de “keep awake”.
- Solo mientras esté conectada a corriente y ventilada.
- Debe probarse en la versión real de macOS.
- No se considerará equivalente a un servidor robusto.

### Conclusión

“Tapa cerrada” no significa automáticamente “el proceso continúa”.

El sistema debe funcionar correctamente aunque haya huecos y debe recuperarse al despertar.

---

# 15. Buenos modales de red

Configuración inicial:

- máximo cuatro solicitudes concurrentes;
- timeout de quince segundos;
- un reintento para errores transitorios;
- backoff exponencial ante `429`;
- respetar `Retry-After`;
- jitter pequeño;
- ETag e `If-Modified-Since` cuando existan;
- no reintentar indefinidamente;
- pausar una fuente después de fallos consecutivos;
- Telegram para avisar de adaptadores rotos;
- User-Agent identificable cuando sea apropiado;
- no intentar evadir bloqueos.

---

# 16. Filtros duros

Las reglas rápidas ocurren antes del LLM.

## 16.1 Posibles descalificadores

- internship;
- trainee;
- rol móvil nativo;
- rol de infraestructura sin componente frontend;
- presencial obligatorio fuera de ubicaciones aceptadas;
- ciudadanía estadounidense obligatoria;
- security clearance obligatoria;
- salario explícito por debajo del mínimo;
- idioma obligatorio no disponible;
- stack completamente no deseado.

## 16.2 Ambigüedades que no deben matarse con regex

- “Remote”.
- “Americas”.
- “North America”.
- “US-based”.
- “No sponsorship”.
- “Preferred ET hours”.
- “Global remote”.
- “Contractor”.

Estas expresiones necesitan contexto y pasan al extractor local.

## 16.3 Auditoría

Cada descarte guarda:

- regla;
- fragmento que coincidió;
- versión de configuración;
- fecha.

Así se pueden recuperar falsos negativos.

---

# 17. IA local

## 17.1 Modelo diario

**Modelo:** `qwen3.5:9b` o su variante MLX verificada en el registry.

Usos:

- extraer stack;
- modalidad;
- ubicación;
- seniority;
- años requeridos;
- autorización;
- tipo de contratación;
- salario;
- red flags;
- ambigüedades;
- CV recomendado.

Configuración inicial:

```json
{
  "temperature": 0,
  "num_ctx": 8192,
  "num_predict": 800,
  "think": false
}
```

La salida se fuerza con JSON Schema y se valida con Zod.

## 17.2 Modelo profundo

**Modelo:** `qwen3.6:27b` o variante MLX verificada.

Solo se ejecuta cuando Jair presiona `Preparar aplicación`.

Usos:

- matriz requisito-evidencia;
- resumen de empresa y rol;
- sugerencias verificables de CV;
- respuestas abiertas;
- checklist;
- análisis de código complejo;
- revisión arquitectónica local.

Configuración inicial:

```json
{
  "temperature": 0.1,
  "num_ctx": 16384,
  "think": true
}
```

## 17.3 Modelo opcional

Qwen3.6-35B-A3B puede probarse para:

- agentes de código;
- tool calling;
- muchas iteraciones;
- comparación de velocidad/calidad.

No es requisito. En 36 GB debe utilizarse en sesiones dedicadas y evitando presión de memoria.

## 17.4 Embeddings

Qwen3-Embedding-0.6B se añade únicamente cuando exista suficiente volumen.

Usos:

- búsqueda semántica;
- similitud CV-vacante;
- duplicados aproximados;
- vacantes parecidas a las que generaron entrevista;
- búsqueda de evidencia.

## 17.5 Regla de modelos

Los nombres exactos de tags pueden cambiar. Antes de descargar o actualizar, se verifica el registry oficial de Ollama y se registra el cambio en `AI_COORDINATION_LOG.md`.

---

# 18. Extracción estructurada y scoring

## 18.1 El LLM extrae hechos

Ejemplo:

```json
{
  "roleFamily": "frontend",
  "seniority": "senior",
  "requiredSkills": ["React", "TypeScript"],
  "preferredSkills": ["GraphQL"],
  "minimumYears": 5,
  "workplaceType": "remote",
  "allowedRegions": ["LATAM", "Americas"],
  "mexicoEligibility": "likely",
  "requiresUsAuthorization": false,
  "employmentModels": ["contractor"],
  "salary": null,
  "redFlags": [],
  "uncertainties": ["No especifica entidad legal en México"]
}
```

## 18.2 El código calcula el score

| Categoría | Peso |
|---|---:|
| Stack técnico | 25 |
| Compatibilidad del puesto | 20 |
| Ubicación y contratación | 20 |
| Seniority | 15 |
| Compensación | 10 |
| Prioridad de empresa | 5 |
| Calidad de la publicación | 5 |

Total: 100.

## 18.3 Umbrales iniciales

- 0–54: archivo silencioso.
- 55–74: dashboard.
- 75–84: Telegram normal.
- 85–100: Telegram prioritario.

Se calibran con al menos cincuenta vacantes revisadas por Jair.

## 18.4 Validación

El sistema debe coincidir con el criterio humano en aproximadamente 40–45 de 50 casos antes de tratar el score como confiable.

Cuando falla:

1. se revisa la vacante;
2. se revisa la regla;
3. se revisa el prompt;
4. se revisa el schema;
5. se revisa el perfil;
6. solo al final se cambia de modelo.

---

# 19. Dashboard React + Vite

## 19.1 Principio

El dashboard debe ahorrar minutos. No debe convertirse en otro proyecto de diseño.

## 19.2 Vistas

### Cola

- nuevas;
- prioridad alta;
- revisar;
- descartadas;
- aplicadas;
- entrevistas;
- rechazadas;
- cerradas.

### Detalle de vacante

- score;
- empresa;
- título;
- ubicación;
- modalidad;
- fuente;
- `first_seen_at`;
- stack requerido;
- stack deseable;
- evidencia coincidente;
- faltantes;
- red flags;
- incertidumbres;
- CV recomendado;
- aplicaciones anteriores a la empresa;
- historial de estados.

### Aplicación

- botón abrir ATS;
- respuestas copiables;
- matriz de autorización;
- botón preparar aplicación;
- CV recomendado;
- checklist;
- botón marcar aplicada.

### Empresas

- vendor;
- token;
- tier;
- estado;
- última consulta;
- siguiente consulta;
- errores;
- vacantes;
- aplicaciones;
- respuestas.

### Métricas

- embudo;
- respuesta por fuente;
- respuesta por score;
- tiempo por aplicación;
- tiempo hasta aplicar;
- rendimiento por CV;
- falsos positivos.

## 19.3 Librerías iniciales

- React.
- Vite.
- TypeScript.
- TanStack Query.
- React Router cuando exista más de una vista.
- Zod para contratos compartidos.
- Una solución ligera de UI; Tailwind es opcional.

No se utilizará Next.js.

## 19.4 Estado

- Estado remoto: TanStack Query.
- Estado de filtros temporales: URL o estado local.
- Estado persistente: SQLite mediante API.
- No duplicar la base dentro del navegador.
- Zustand solo si aparece un caso real que no cubran las herramientas anteriores.

---

# 20. API local

Base URL:

```text
http://127.0.0.1:8787/api
```

Endpoints iniciales:

```text
GET    /health
GET    /jobs
GET    /jobs/:id
PATCH  /jobs/:id/status
POST   /jobs/:id/prepare
GET    /companies
PATCH  /companies/:id
GET    /applications
POST   /applications
PATCH  /applications/:id
GET    /metrics/summary
POST   /poll/run
GET    /poll/runs
```

Reglas:

- escucha en `127.0.0.1`, no `0.0.0.0`;
- valida entrada y salida con Zod;
- no expone archivos privados completos;
- no devuelve EEO;
- evita datos sensibles en errores;
- mantiene el dashboard desacoplado de SQLite.

---

# 21. Telegram

Una alerta de alta prioridad debe incluir:

```text
🔥 87/100 — Senior Frontend Engineer
Empresa: Acme
Modalidad: Remote LATAM
Fuente: Greenhouse
Detectada hace: 7 min

Coincide: React, TypeScript, Next.js
Falta: GraphQL
Riesgo: ubicación contractual no explícita

[Abrir vacante]
[Abrir JobSniper]
```

No incluye:

- teléfono;
- correo;
- salario privado;
- información EEO;
- respuestas legales completas.

Telegram se implementa antes que la IA para validar el detector.

---

# 22. Preparación de aplicación

Cuando Jair presiona `Preparar aplicación`, el sistema:

1. carga la vacante;
2. carga perfil;
3. recupera solo evidencia relevante;
4. selecciona un CV base;
5. llama al modelo profundo;
6. valida la salida;
7. muestra resultados;
8. nunca sobrescribe el CV fuente;
9. nunca envía.

Salida esperada:

- resumen;
- match principal;
- matriz requisito-evidencia;
- requisitos faltantes;
- red flags;
- preguntas a revisar;
- sugerencias de CV;
- respuestas abiertas;
- checklist.

Cada sugerencia de CV debe incluir:

- texto anterior;
- texto sugerido;
- evidencia;
- nivel de confianza.

---

# 23. Testing

## 23.1 Adaptadores

Cada adaptador tendrá:

- fixture real anonimizado o público;
- prueba de parsing;
- prueba de campos faltantes;
- prueba de error HTTP;
- prueba de `304`;
- prueba de `429`;
- prueba de normalización.

Nunca depender únicamente de una llamada real durante tests.

## 23.2 Deduplicación

Pruebas:

- mismo ID;
- distinto vendor, misma vacante;
- títulos equivalentes;
- roles similares pero diferentes;
- descripción actualizada;
- ubicación diferente.

## 23.3 Scoring

Golden dataset de cincuenta vacantes:

- decisión humana;
- extracción esperada;
- score esperado aproximado;
- razones;
- errores conocidos.

## 23.4 Privacidad

Pruebas para verificar que:

- EEO no entra al prompt;
- secretos no aparecen en logs;
- API no devuelve datos privados;
- Telegram no contiene información sensible;
- skills inventadas son rechazadas.

---

# 24. Logs, salud y backups

## 24.1 Logs

- estructurados;
- nivel `info`, `warn`, `error`;
- sin PII;
- rotación;
- timestamp ISO 8601;
- correlación por `pollRunId`.

## 24.2 Salud de fuentes

Después de cinco fallos consecutivos:

- marca fuente como degradada;
- aplica `disabled_until`;
- manda alerta;
- no reintenta agresivamente.

## 24.3 Backup

- backup diario de SQLite cuando haya cambios;
- conservar siete diarios y cuatro semanales;
- datos privados cifrados o incluidos en backup local seguro;
- probar restauración antes de depender del sistema.

---

# 25. Flujo diario

1. `launchd` ejecuta el poller.
2. El poller selecciona empresas vencidas.
3. Consulta adaptadores.
4. Normaliza.
5. Deduplica.
6. Guarda.
7. Aplica reglas.
8. Qwen3.5 extrae hechos.
9. El código calcula score.
10. Telegram avisa.
11. Jair abre dashboard.
12. Revisa.
13. Prepara aplicación si conviene.
14. Abre ATS.
15. Autofill supervisado completa campos repetitivos.
16. Jair verifica cada respuesta.
17. Jair presiona `Submit`.
18. Marca aplicada.
19. Actualiza estados cuando hay respuesta.

---

# 26. Colaboración entre ChatGPT, Claude y Gemini

Las IAs se organizan por **rol**, no por marca fija.

Roles:

- **Arquitecto/integrador:** mantiene coherencia y revisa cambios.
- **Implementador:** desarrolla un módulo acotado.
- **Revisor:** busca bugs, riesgos, seguridad y casos faltantes.
- **Tester:** produce fixtures y pruebas.
- **Documentador:** actualiza decisiones autorizadas.
- **Investigador:** valida documentación oficial.

Una misma IA puede asumir distintos roles en distintas sesiones.

Reglas:

1. leer ambos archivos canónicos;
2. revisar Git;
3. declarar identidad y alcance;
4. trabajar en una rama;
5. no editar módulos ajenos al alcance;
6. ejecutar tests;
7. registrar resultados;
8. no modificar este plan sin autorización;
9. no inventar el nombre exacto del modelo;
10. usar “no expuesto por el cliente” cuando no pueda verificarlo.

El protocolo completo está en `AI_COORDINATION_LOG.md`.

---

# 27. Plan de implementación por fases

## Fase 0 — Gobierno y memoria compartida

### Entregables

- `PROJECT_MASTER_PLAN.md`.
- `AI_COORDINATION_LOG.md`.
- repositorio Git.
- reglas de ramas y commits.
- decisión del integrador de la primera iteración.

### Puerta

Todos los agentes pueden explicar:

- qué es JobSniper;
- qué no pueden hacer;
- cuál es la fase activa;
- qué archivo es canónico;
- cómo proponer cambios.

---

## Fase 1 — Fuente de verdad personal

### Entregables

- `profile.yaml`;
- `evidence.json`;
- `answers.private.json`;
- `employment-scenarios.json`;
- `eeo.private.json`;
- dos o tres CVs base;
- `.gitignore`.

### Trabajo humano

Sesión guiada de preguntas.

### Puerta

Jair puede completar un Greenhouse o Lever en menos de tres minutos sin improvisar datos.

---

## Fase 2 — Esqueleto del repositorio

### Entregables

- Bun workspace;
- `apps/local-service`;
- `apps/dashboard`;
- `packages/shared`;
- TypeScript;
- Vitest;
- Zod;
- SQLite;
- endpoint `/health`;
- React/Vite funcionando.

### Puerta

- `bun install`;
- `bun test`;
- API local responde;
- dashboard abre;
- no hay secretos versionados.

---

## Fase 3 — Primer pool de empresas

### Entregables

- 30 empresas verificadas;
- 10 Tier A;
- 20 Tier B;
- URL de careers;
- ATS;
- token;
- país;
- estado de contratación LATAM.

### Puerta

Todas las filas abren y los tokens funcionan.

---

## Fase 4 — Detector sin IA

### Entregables

- Greenhouse;
- Lever;
- normalización;
- SQLite;
- source ID;
- fingerprint;
- cierres;
- salud de fuentes;
- poll runs.

### Puerta

Corre varios días sin duplicar y se recupera después de dormir la Mac.

---

## Fase 5 — Telegram

### Entregables

- bot;
- alertas básicas;
- enlaces;
- avisos de error;
- configuración privada.

### Puerta

Las alertas llegan una sola vez y abren la vacante correcta.

---

## Fase 6 — Reglas y scoring base

### Entregables

- normalización de títulos;
- filtros;
- descalificadores;
- score sin LLM;
- auditoría de reglas.

### Puerta

Los descartes pueden explicarse y revertirse.

---

## Fase 7 — Qwen diario

### Entregables

- Ollama;
- modelo verificado;
- cliente local;
- JSON Schema;
- Zod;
- extracción;
- score combinado;
- dataset de 50 vacantes.

### Puerta

40–45 de 50 decisiones coinciden razonablemente con Jair.

---

## Fase 8 — Dashboard funcional

### Entregables

- React/Vite;
- cola;
- detalle;
- filtros;
- estados;
- copiar respuestas;
- historial por empresa;
- advertencias de volumen;
- marcar aplicada;
- métricas mínimas.

### Puerta

Cinco aplicaciones relevantes en menos de quince minutos.

---

## Fase 9 — Preparación profunda

### Entregables

- Qwen3.6-27B;
- botón `Preparar aplicación`;
- matriz requisito-evidencia;
- sugerencias de CV;
- respuestas;
- checklist;
- validación anti-alucinación.

### Puerta

Ninguna sugerencia inventa experiencia y todas son revisables.

---

## Fase 10 — Ampliar cobertura

Escala:

```text
30 → 80 → 150 → 250 → 500
```

Vendors:

```text
Greenhouse → Lever → Ashby → Workable
→ RSS/alertas → custom → Workday
```

### Puerta

No se amplía si los adaptadores actuales están inestables.

---

## Fase 11 — Embeddings y aprendizaje

### Activadores

- más de 250 empresas;
- miles de vacantes;
- búsqueda semántica necesaria;
- suficientes entrevistas para aprender patrones.

### Entregables

- embeddings;
- similitud;
- vacantes parecidas a éxitos;
- dedup semántico;
- búsqueda de evidencia.

---

## Fase 12 — Servidor futuro, documentado pero no activo

No se implementa inicialmente.

### Activadores

- los huecos de sueño causan pérdidas comprobables;
- el detector local ya es estable;
- más de 100 empresas;
- se necesitan alertas nocturnas;
- el costo mensual está justificado.

### Arquitectura futura

```text
VPS
- poller público
- companies
- public jobs
- dedup
- reglas
- Telegram preliminar

Mac
- datos privados
- Ollama
- CVs
- análisis profundo
- dashboard
- aplicaciones
```

### Datos que jamás se suben por defecto

- EEO;
- identificación;
- dirección;
- teléfono;
- documentos;
- cookies;
- respuestas privadas;
- Ollama;
- CVs completos;
- credenciales.

### Decisión

La arquitectura está lista para migrar porque el servicio local y el dashboard se comunican por API, pero no se paga ni despliega nada hasta que las métricas lo pidan.

---

## Fase 13 — Optimización continua

Cada semana:

- medir fuentes;
- revisar scores;
- revisar falsos positivos;
- revisar errores de ubicación;
- revisar ratios;
- ajustar tiers;
- ajustar frecuencias;
- actualizar empresas;
- eliminar fuentes inútiles.

---

# 28. Orden concreto de la primera semana

## Día 0

- crear Git;
- guardar los dos documentos;
- crear `.gitignore`;
- hacer commit inicial;
- seguir aplicando manualmente.

## Día 1

- completar archivos personales;
- generar CVs base;
- probar una aplicación manual.

## Día 2

- crear workspaces;
- API `/health`;
- React/Vite;
- SQLite.

## Día 3

- diez empresas;
- adaptador Greenhouse;
- tests;
- persistencia.

## Día 4

- adaptador Lever;
- deduplicación;
- Telegram.

## Día 5

- `launchd`;
- `next_check_at`;
- catch-up;
- salud de fuentes.

## Día 6

- ampliar a 30 empresas;
- ejecutar sin IA;
- corregir errores.

## Día 7

- usar el sistema;
- no agregar funciones;
- medir;
- decidir la siguiente fase.

---

# 29. Riesgos y mitigaciones

| Riesgo | Mitigación |
|---|---|
| Convertirlo en postergación | Cinco aplicaciones manuales diarias |
| Scraping prohibido | Solo ATS públicos y alertas nativas |
| Duplicados | Source ID + fingerprint |
| Falso “remote” | Extracción contextual y revisión |
| Respuesta de visa incorrecta | Matriz de escenarios |
| LLM inventa experiencia | Evidence + validación en código |
| Mac duerme | Catch-up; servidor solo si se justifica |
| Rate limit | Concurrencia baja, backoff y ETag |
| Modelo consume demasiada RAM | 9B diario; 27B bajo demanda |
| Tres IAs pisan cambios | Git, ramas, alcance y log append-only |
| Plan se fragmenta | Fuente canónica y changelog |
| Datos privados en Git | `.gitignore`, escaneo y tests |
| Dashboard crece demasiado | MVP orientado a velocidad |
| Workday consume semanas | Implementarlo al final |

---

# 30. Lista definitiva de “no hacer”

- No guardar `li_at`.
- No automatizar LinkedIn.
- No crear cuenta falsa.
- No auto-submit.
- No automatizar Easy Apply.
- No usar Playwright donde existe JSON.
- No comenzar por Workday.
- No descargar todos los modelos desde el primer día.
- No usar 256K de contexto por defecto.
- No enviar EEO al modelo.
- No inventar evidencia.
- No usar el score como orden.
- No bloquear arbitrariamente una empresa durante 30 días.
- No asumir que 15 aplicaciones a una empresa son siempre razonables.
- No pagar servidor durante el MVP.
- No usar Next.js.
- No crear un dashboard visualmente complejo antes del detector.
- No modificar el plan sin registro y aprobación.
- No permitir que un agente diga un modelo exacto si el cliente no lo muestra.
- No dejar que agentes trabajen concurrentemente sobre los mismos archivos sin ramas y asignación.

---

# 31. Protocolo de modificación del plan

Toda modificación se agrega al final con:

```markdown
## Cambio PLAN-YYYYMMDD-NNN

- Fecha y hora:
- Solicitado por:
- Propuesto por:
- Modelo/cliente:
- Estado: propuesto | aprobado | rechazado | implementado
- Autorización de Jair:
- Secciones afectadas:
- Motivo:
- Cambio:
- Impacto:
- Riesgos:
- Commit:
```

Solo un cambio con estado `aprobado` puede modificar las secciones superiores.

---

# 32. Historial de cambios

## Cambio PLAN-20260721-001

- **Fecha y hora:** 2026-07-21 21:19 America/Mexico_City
- **Solicitado por:** Jair Vázquez
- **Propuesto por:** ChatGPT
- **Modelo/cliente:** OpenAI GPT-5.6 Thinking / ChatGPT web
- **Estado:** implementado
- **Autorización de Jair:** consolidar los dos planes y sus notas en el plan final
- **Cambio principal:** React + Vite reemplaza Next.js; servidor queda documentado pero diferido; se adopta coordinación multi-IA con dos archivos canónicos; política por empresa usa advertencias progresivas.
- **Impacto:** define la arquitectura inicial y el orden de construcción.
- **Commit:** pendiente de crear en el repositorio local.

---

# 33. Fuentes oficiales verificadas al crear la versión 1.0.0

Estas referencias son informativas. Las decisiones canónicas siguen estando en este archivo.

- React: crear una app desde cero con Vite  
  https://react.dev/learn/build-a-react-app-from-scratch
- Vite: guía oficial  
  https://vite.dev/guide/
- Ollama en Apple Silicon con MLX  
  https://ollama.com/blog/mlx-performance
- Registry de Qwen3.5  
  https://registry.ollama.com/library/qwen3.5
- Registry de Qwen3.6  
  https://registry.ollama.com/library/qwen3.6
- Apple: trabajos programados y sueño  
  https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/ScheduledJobs.html
- LinkedIn: Crawling Terms  
  https://www.linkedin.com/legal/crawling-terms
- LinkedIn: alertas de empleo  
  https://www.linkedin.com/help/linkedin/answer/a507490/alertas-de-empleo-en-linkedin
- Greenhouse Job Board API  
  https://developers.greenhouse.io/job-board
- Lever Postings API  
  https://github.com/lever/postings-api
- Ashby Public Job Posting API  
  https://developers.ashbyhq.com/docs/public-job-posting-api

---

**Fin del plan canónico v1.0.0**
