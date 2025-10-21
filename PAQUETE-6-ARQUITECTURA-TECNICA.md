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

## 🗄️ Base de Datos (Supabase PostgreSQL)

### **Tabla: cases**
```sql
CREATE TABLE cases (
  id UUID PRIMARY KEY,
  user_id UUID NOT NULL,
  title VARCHAR(500),
  description TEXT,
  legal_area VARCHAR(100),
  created_at TIMESTAMP
);
```

### **Tabla: evidence**
```sql
CREATE TABLE evidence (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  file_type VARCHAR(20), -- 'image', 'video'
  file_url TEXT,
  visual_analysis JSONB, -- Resultado del Agente Visual
  created_at TIMESTAMP
);
```

### **Tabla: jurisprudence (+ pgvector)**
```sql
CREATE TABLE jurisprudence (
  id UUID PRIMARY KEY,
  case_name VARCHAR(500),
  court VARCHAR(200),
  decision_date DATE,
  legal_area VARCHAR(100),
  full_text TEXT,
  embedding VECTOR(1536), -- OpenAI embeddings
  created_at TIMESTAMP
);

-- Índice para búsqueda semántica
CREATE INDEX ON jurisprudence USING ivfflat (embedding vector_cosine_ops);
```

### **Tabla: case_analysis**
```sql
CREATE TABLE case_analysis (
  id UUID PRIMARY KEY,
  case_id UUID REFERENCES cases(id),
  summary JSONB,
  precedents JSONB,
  argument_lines JSONB,
  created_at TIMESTAMP
);
```

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

