# 🏗️ Paquete 6 — Arquitectura Técnica

## 🎯 Por qué este paquete

Documentar la **arquitectura técnica del sistema**, especialmente la implementación de agentes A2A y Model Context Protocol (MCP).

---

## 📐 Arquitectura General

```
┌─────────────────────────────────────────────────┐
│         FRONTEND (React + Vite)                 │
│  - Upload casos (texto + multimedia)            │
│  - Visualización de análisis                    │
│  - Dashboard de resultados                      │
└─────────────────────────────────────────────────┘
                    ↓ (API REST)
┌─────────────────────────────────────────────────┐
│         BACKEND (Laravel 11)                    │
│  ┌─────────────────────────────────────────┐   │
│  │  Controllers:                           │   │
│  │  - AuthController (roles/permisos)     │   │
│  │  - CaseController                       │   │
│  │  - AnalysisController                   │   │
│  │  - JurisprudenceController              │   │
│  └─────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────┐   │
│  │  Services:                              │   │
│  │  - MCPService (Model Context Protocol)  │   │
│  │  - AgentOrchestrator (A2A Coordinator)  │   │
│  │  - LLMService (OpenAI + Anthropic)      │   │
│  └─────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────┐
│         AGENTES A2A (Laravel Services)          │
│  ┌──────────────┐  ┌──────────────┐           │
│  │  Agente 1    │  │  Agente 2    │           │
│  │  Coordinador │←→│ Jurisprudencia│           │
│  └──────────────┘  └──────────────┘           │
│  ┌──────────────┐  ┌──────────────┐           │
│  │  Agente 3    │  │  Agente 4    │           │
│  │  Visual      │  │  Argumentos  │           │
│  └──────────────┘  └──────────────┘           │
└─────────────────────────────────────────────────┘
                    ↓
┌─────────────────────────────────────────────────┐
│  BASE DE DATOS (Supabase PostgreSQL + pgvector)│
│  - cases                                        │
│  - evidence                                     │
│  - jurisprudence (+ pgvector embeddings)        │
│  - case_analysis                                │
└─────────────────────────────────────────────────┘
```

---

## 🤖 Agentes A2A (Agent-to-Agent)

### **Agente 1: Coordinador**
- **Responsabilidad:** Orquestar el flujo completo de análisis
- **Funciones:**
  - Recibir caso del usuario
  - Distribuir tareas a agentes especializados
  - Consolidar resultados
  - Generar respuesta final
- **Tecnología:** Laravel Service + GPT-4
- **Inputs:** Caso completo (texto + archivo visual)
- **Outputs:** Análisis consolidado para UI

---

### **Agente 2: Jurisprudencia**
- **Responsabilidad:** Buscar y rankear precedentes legales relevantes
- **Funciones:**
  - Crear embedding del caso
  - Búsqueda semántica en pgvector
  - Filtrar por área legal y jurisdicción
  - Explicar pertinencia de cada precedente
- **Tecnología:** Laravel Service + OpenAI Embeddings + pgvector
- **Inputs:** Resumen del caso (del Agente Coordinador)
- **Outputs:** Lista de 2-3 precedentes con score de similitud

---

### **Agente 3: Visual**
- **Responsabilidad:** Analizar evidencia visual (imágenes/videos)
- **Funciones:**
  - Procesar imagen/video con GPT-4 Vision
  - Identificar elementos legalmente relevantes (vehículos, señales, personas)
  - Describir eventos cronológicos
  - Generar insights para argumentación
- **Tecnología:** Laravel Service + GPT-4 Vision API
- **Inputs:** Archivo visual + contexto del caso
- **Outputs:** Análisis visual estructurado

---

### **Agente 4: Argumentos**
- **Responsabilidad:** Generar líneas argumentales basadas en hechos y precedentes
- **Funciones:**
  - Sintetizar hechos + precedentes + análisis visual
  - Generar 2-3 líneas argumentales alternativas
  - Identificar fortalezas y debilidades
  - Sugerir estrategia recomendada
- **Tecnología:** Laravel Service + GPT-4 / Claude
- **Inputs:** Resumen del caso + precedentes + análisis visual
- **Outputs:** 2-3 líneas argumentales completas

---

## 🔗 Model Context Protocol (MCP)

### **¿Qué es MCP?**
Protocolo que permite a los agentes acceder a bases de datos y herramientas especializadas de forma contextualizada.

### **MCPService (Laravel)**
**Funciones:**
- Conectar agentes con base de datos de jurisprudencia
- Proveer acceso a embeddings y búsqueda semántica
- Exponer herramientas especializadas (ej. búsqueda por jurisdicción, filtrado por fecha)
- Gestionar contexto compartido entre agentes

**Ejemplo de uso:**
```php
// Agente Jurisprudencia solicita precedentes vía MCP
$precedents = MCPService::searchJurisprudence([
    'case_embedding' => $embedding,
    'legal_area' => 'Derecho de Tránsito',
    'jurisdiction' => 'Colombia',
    'top_k' => 3
]);
```

---

## 🗄️ Base de Datos (Supabase PostgreSQL + pgvector)

**Estructura simplificada:**
- `cases` - Casos ingresados por usuarios
- `evidence` - Archivos multimedia asociados
- `jurisprudence` - Precedentes legales con embeddings vectoriales (pgvector)
- `case_analysis` - Resultados del análisis generado por IA

**Tecnología clave:** pgvector para búsqueda semántica de jurisprudencia mediante embeddings.

---

## 📊 Schema SQL Temporal (Laravel + Supabase)

> ⚠️ **ADVERTENCIA:** Este schema es solo para contexto y no está diseñado para ejecutarse directamente. El orden de las tablas y las restricciones pueden no ser válidas para ejecución.

Este SQL representa el trabajo implementado con Laravel 11 y Supabase PostgreSQL:

```sql
-- WARNING: This schema is for context only and is not meant to be run.
-- Table order and constraints may not be valid for execution.

CREATE TABLE public.cache (
  key character varying NOT NULL,
  value text NOT NULL,
  expiration integer NOT NULL,
  CONSTRAINT cache_pkey PRIMARY KEY (key)
);

CREATE TABLE public.cache_locks (
  key character varying NOT NULL,
  owner character varying NOT NULL,
  expiration integer NOT NULL,
  CONSTRAINT cache_locks_pkey PRIMARY KEY (key)
);

CREATE TABLE public.cases_analysis (
  id bigint NOT NULL DEFAULT nextval('cases_analysis_id_seq'::regclass),
  uuid uuid NOT NULL UNIQUE,
  legal_case_id bigint NOT NULL,
  status character varying NOT NULL DEFAULT 'pending'::character varying CHECK (status::text = ANY (ARRAY['pending'::character varying, 'processing'::character varying, 'completed'::character varying, 'failed'::character varying]::text[])),
  coordinator_result json,
  jurisprudence_result json,
  visual_analysis_result json,
  arguments_result json,
  legal_elements json,
  relevant_precedents json,
  defense_lines json,
  alternative_scenarios json,
  confidence_scores json,
  processing_time numeric,
  agent_execution_log json,
  executive_summary text,
  version integer NOT NULL DEFAULT 1,
  previous_analysis_id bigint,
  started_at timestamp without time zone,
  completed_at timestamp without time zone,
  created_at timestamp without time zone,
  updated_at timestamp without time zone,
  deleted_at timestamp without time zone,
  CONSTRAINT cases_analysis_pkey PRIMARY KEY (id),
  CONSTRAINT cases_analysis_legal_case_id_foreign FOREIGN KEY (legal_case_id) REFERENCES public.legal_cases(id),
  CONSTRAINT cases_analysis_previous_analysis_id_foreign FOREIGN KEY (previous_analysis_id) REFERENCES public.cases_analysis(id)
);

CREATE TABLE public.evidence (
  id bigint NOT NULL DEFAULT nextval('evidence_id_seq'::regclass),
  uuid uuid NOT NULL UNIQUE,
  legal_case_id bigint NOT NULL,
  title character varying,
  description text,
  type character varying NOT NULL DEFAULT 'document'::character varying CHECK (type::text = ANY (ARRAY['document'::character varying, 'image'::character varying, 'video'::character varying, 'audio'::character varying, 'testimony'::character varying, 'other'::character varying]::text[])),
  file_path character varying,
  file_url character varying,
  mime_type character varying,
  file_size bigint,
  analysis_result json,
  is_analyzed boolean NOT NULL DEFAULT false,
  analyzed_at timestamp without time zone,
  metadata json,
  created_at timestamp without time zone,
  updated_at timestamp without time zone,
  deleted_at timestamp without time zone,
  CONSTRAINT evidence_pkey PRIMARY KEY (id),
  CONSTRAINT evidence_legal_case_id_foreign FOREIGN KEY (legal_case_id) REFERENCES public.legal_cases(id)
);

CREATE TABLE public.failed_jobs (
  id bigint NOT NULL DEFAULT nextval('failed_jobs_id_seq'::regclass),
  uuid character varying NOT NULL UNIQUE,
  connection text NOT NULL,
  queue text NOT NULL,
  payload text NOT NULL,
  exception text NOT NULL,
  failed_at timestamp without time zone NOT NULL DEFAULT CURRENT_TIMESTAMP,
  CONSTRAINT failed_jobs_pkey PRIMARY KEY (id)
);

CREATE TABLE public.job_batches (
  id character varying NOT NULL,
  name character varying NOT NULL,
  total_jobs integer NOT NULL,
  pending_jobs integer NOT NULL,
  failed_jobs integer NOT NULL,
  failed_job_ids text NOT NULL,
  options text,
  cancelled_at integer,
  created_at integer NOT NULL,
  finished_at integer,
  CONSTRAINT job_batches_pkey PRIMARY KEY (id)
);

CREATE TABLE public.jobs (
  id bigint NOT NULL DEFAULT nextval('jobs_id_seq'::regclass),
  queue character varying NOT NULL,
  payload text NOT NULL,
  attempts smallint NOT NULL,
  reserved_at integer,
  available_at integer NOT NULL,
  created_at integer NOT NULL,
  CONSTRAINT jobs_pkey PRIMARY KEY (id)
);

CREATE TABLE public.jurisprudence (
  id bigint NOT NULL DEFAULT nextval('jurisprudence_id_seq'::regclass),
  uuid uuid NOT NULL UNIQUE,
  case_number character varying NOT NULL UNIQUE,
  court character varying NOT NULL,
  jurisdiction character varying,
  decision_date date,
  case_title character varying NOT NULL,
  summary text NOT NULL,
  ruling text NOT NULL,
  legal_reasoning text,
  keywords json,
  articles_cited json,
  url character varying,
  full_text text,
  embedding json,
  relevance_level character varying NOT NULL DEFAULT 'medium'::character varying CHECK (relevance_level::text = ANY (ARRAY['high'::character varying, 'medium'::character varying, 'low'::character varying]::text[])),
  metadata json,
  created_at timestamp without time zone,
  updated_at timestamp without time zone,
  deleted_at timestamp without time zone,
  CONSTRAINT jurisprudence_pkey PRIMARY KEY (id)
);

CREATE TABLE public.legal_cases (
  id bigint NOT NULL DEFAULT nextval('legal_cases_id_seq'::regclass),
  uuid uuid NOT NULL UNIQUE,
  title character varying NOT NULL,
  description text NOT NULL,
  case_type character varying NOT NULL DEFAULT 'otro'::character varying CHECK (case_type::text = ANY (ARRAY['civil'::character varying, 'penal'::character varying, 'laboral'::character varying, 'administrativo'::character varying, 'constitucional'::character varying, 'otro'::character varying]::text[])),
  status character varying NOT NULL DEFAULT 'draft'::character varying CHECK (status::text = ANY (ARRAY['draft'::character varying, 'analyzing'::character varying, 'analyzed'::character varying, 'archived'::character varying]::text[])),
  parties json,
  incident_date date,
  facts text,
  metadata json,
  user_id bigint,
  created_at timestamp without time zone,
  updated_at timestamp without time zone,
  deleted_at timestamp without time zone,
  CONSTRAINT legal_cases_pkey PRIMARY KEY (id),
  CONSTRAINT legal_cases_user_id_foreign FOREIGN KEY (user_id) REFERENCES public.users(id)
);

CREATE TABLE public.migrations (
  id integer NOT NULL DEFAULT nextval('migrations_id_seq'::regclass),
  migration character varying NOT NULL,
  batch integer NOT NULL,
  CONSTRAINT migrations_pkey PRIMARY KEY (id)
);

CREATE TABLE public.password_reset_tokens (
  email character varying NOT NULL,
  token character varying NOT NULL,
  created_at timestamp without time zone,
  CONSTRAINT password_reset_tokens_pkey PRIMARY KEY (email)
);

CREATE TABLE public.personal_access_tokens (
  id bigint NOT NULL DEFAULT nextval('personal_access_tokens_id_seq'::regclass),
  tokenable_type character varying NOT NULL,
  tokenable_id bigint NOT NULL,
  name text NOT NULL,
  token character varying NOT NULL UNIQUE,
  abilities text,
  last_used_at timestamp without time zone,
  expires_at timestamp without time zone,
  created_at timestamp without time zone,
  updated_at timestamp without time zone,
  CONSTRAINT personal_access_tokens_pkey PRIMARY KEY (id)
);

CREATE TABLE public.sessions (
  id character varying NOT NULL,
  user_id bigint,
  ip_address character varying,
  user_agent text,
  payload text NOT NULL,
  last_activity integer NOT NULL,
  CONSTRAINT sessions_pkey PRIMARY KEY (id)
);

CREATE TABLE public.users (
  id bigint NOT NULL DEFAULT nextval('users_id_seq'::regclass),
  name character varying NOT NULL,
  email character varying NOT NULL UNIQUE,
  email_verified_at timestamp without time zone,
  password character varying NOT NULL,
  remember_token character varying,
  created_at timestamp without time zone,
  updated_at timestamp without time zone,
  CONSTRAINT users_pkey PRIMARY KEY (id)
);
```

### **Tablas Clave del MVP:**

| Tabla | Propósito |
|-------|-----------|
| `users` | Gestión de usuarios (Estudiante/Profesional) |
| `legal_cases` | Casos legales cargados por usuarios |
| `evidence` | Archivos multimedia asociados a casos |
| `jurisprudence` | Precedentes legales con embeddings vectoriales |
| `cases_analysis` | Resultados del análisis A2A (coordinador, jurisprudencia, visual, argumentos) |
| `jobs` / `job_batches` | Cola de trabajos asíncronos (Laravel Queues) |
| `cache` / `cache_locks` | Sistema de caché para optimización |

---

## 🔄 Flujo de Datos entre Agentes

```
[Usuario carga caso]
        ↓
[Agente Coordinador recibe caso]
        ↓
    ┌───┴────┬─────────┬─────────┐
    ↓        ↓         ↓         ↓
[NLP]  [Agente  [Agente    [MCP]
       Visual]  Jurisp.]
    │        │         │         │
    └───┬────┴─────────┴─────────┘
        ↓
[Agente Coordinador consolida]
        ↓
[Agente Argumentos genera líneas]
        ↓
[Respuesta a usuario]
```

---

## 🛠️ Stack Tecnológico Detallado

| Capa | Tecnología | Propósito |
|------|------------|-----------|
| **Frontend** | React 18 + Vite | UI reactiva y rápida |
| **Backend** | Laravel 11 (PHP 8.2) | API REST + lógica de negocio |
| **Base de Datos** | Supabase PostgreSQL 15 | Persistencia + Auth |
| **Vector Search** | pgvector extension | Búsqueda semántica de jurisprudencia |
| **LLM (Texto)** | GPT-4 Turbo / Claude 3.5 Sonnet | NLP y generación de argumentos |
| **LLM (Visión)** | GPT-4 Vision | Análisis de imágenes/videos |
| **Embeddings** | text-embedding-3-large | Vectorización de casos y precedentes |
| **Storage** | Supabase Storage | Archivos multimedia |
| **Orchestration** | Laravel Services (A2A) | Coordinación entre agentes |
| **Context Protocol** | MCPService (Laravel) | Acceso a BD legal contextualizado |

---

## ⚡ Optimizaciones de Performance

1. **Caché de Embeddings:** Precedentes pre-vectorizados
2. **Procesamiento Paralelo:** Agentes Visual y Jurisprudencia ejecutan simultáneamente
3. **Lazy Loading:** Solo cargar análisis visual si usuario lo solicita
4. **Queue Jobs:** Análisis pesados en background (Laravel Queues)

---

## 🔒 Seguridad

- **Autenticación:** Supabase Auth (email + password)
- **Autorización:** Usuarios solo acceden a sus propios casos
- **Encriptación:** Datos sensibles encriptados en BD
- **Rate Limiting:** Máximo 10 casos por usuario/día
- **Validación:** Sanitización de inputs en Laravel

---

## ✅ Criterios de Aceptación Técnica

- [ ] 4 agentes A2A implementados como Laravel Services
- [ ] MCPService conecta agentes con BD de jurisprudencia
- [ ] Integración GPT-4 Vision funcional
- [ ] pgvector configurado con índice IVFFlat
- [ ] API REST documentada (Swagger/OpenAPI)
- [ ] Frontend React consume API correctamente
- [ ] Tiempo de respuesta total < 2 minutos
- [ ] Tests unitarios de cada agente

