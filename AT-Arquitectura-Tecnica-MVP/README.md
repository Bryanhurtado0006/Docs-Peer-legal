# 🏗️ Arquitectura Técnica MVP - PEER-LEGAL-AI

## 📐 Diagrama de Arquitectura

Este directorio contiene la arquitectura técnica completa del MVP de PEER-LEGAL-AI, mostrando el flujo de datos desde el frontend hasta la base de datos, pasando por los agentes A2A.

---

## 🔄 Flujo Técnico del Sistema

### **1️⃣ FRONTEND - React + Vite**

| Componente | Descripción |
|------------|-------------|
| 📤 **Upload Casos** | Formulario para cargar texto + multimedia (imágenes/videos) |
| 📊 **Dashboard Resultados** | Panel de visualización del análisis completo |
| 🔍 **Visualización Análisis** | Interfaz para ver precedentes, argumentos y evidencia visual |

**↓ Comunicación vía API REST**

---

### **2️⃣ BACKEND - Laravel 11**

#### **Controllers (Capa de Control)**

| Controller | Responsabilidad |
|------------|-----------------|
| `CaseController` | Gestión de casos (CRUD, upload) |
| `AnalysisController` | Orquestación del análisis completo |
| `JurisprudenceController` | Consulta de precedentes legales |

#### **Services (Lógica de Negocio)**

| Service | Función |
|---------|---------|
| `MCPService` | Model Context Protocol - Conexión con BD legal |
| `AgentOrchestrator` | Coordinación de agentes A2A |
| `LLMService` | Integración con OpenAI + Anthropic |

**↓ Orquestación A2A**

---

### **3️⃣ AGENTES A2A - Colaboración Inteligente**

```
┌─────────────┐      ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
│   Agente 1  │ ←→   │   Agente 2  │ ←→   │   Agente 3  │ ←→   │   Agente 4  │
│ COORDINADOR │      │JURISPRUDENCIA│      │   VISUAL    │      │ ARGUMENTOS  │
│   Orquesta  │      │   Búsqueda   │      │   Análisis  │      │ Estrategias │
│   análisis  │      │   semántica  │      │  multimedia │      │   legales   │
└─────────────┘      └─────────────┘      └─────────────┘      └─────────────┘
```

#### **Agente 1: COORDINADOR**
- **Rol:** Orquesta el análisis completo
- **Input:** Caso del usuario (texto + multimedia)
- **Output:** Análisis consolidado
- **Tecnología:** Laravel Service + GPT-4

#### **Agente 2: JURISPRUDENCIA**
- **Rol:** Búsqueda semántica de precedentes
- **Input:** Resumen del caso
- **Output:** 2-3 precedentes relevantes con score de similitud
- **Tecnología:** pgvector + OpenAI Embeddings + MCP

#### **Agente 3: VISUAL**
- **Rol:** Análisis de evidencia multimedia
- **Input:** Imágenes/videos + contexto del caso
- **Output:** Elementos legalmente relevantes identificados
- **Tecnología:** GPT-4 Vision

#### **Agente 4: ARGUMENTOS**
- **Rol:** Generación de estrategias legales
- **Input:** Hechos + precedentes + análisis visual
- **Output:** 2-3 líneas argumentales con fortalezas/debilidades
- **Tecnología:** Claude/GPT-4

**↓ MCP Protocol**

---

### **4️⃣ BASE DE DATOS - Supabase PostgreSQL + pgvector**

| Tabla | Descripción |
|-------|-------------|
| 📁 `cases` | Casos ingresados por usuarios |
| 🖼️ `evidence` | Archivos multimedia (videos, imágenes) |
| ⚖️ `jurisprudence` (pgvector) | Precedentes legales con embeddings para búsqueda semántica |
| 📊 `case_analysis` | Resultados del análisis generado |

---

## 🔄 Flujo Completo del Usuario

```
1. Usuario sube caso → Backend procesa → 
2. Agente Coordinador orquesta → Agentes colaboran (A2A) →
3. MCP consulta jurisprudencia → Genera análisis y argumentos →
4. Backend devuelve resultados → Frontend muestra dashboard
```

**⚡ Flujo detallado:**

> Usuario sube caso → Backend procesa → Agentes colaboran (A2A) → MCP consulta jurisprudencia → Genera análisis y argumentos

---

## 🛠️ Stack Tecnológico

### **Frontend**
- React 18
- Vite
- TailwindCSS

### **Backend**
- Laravel 11 (PHP 8.2)
- API REST

### **IA & Machine Learning**
- GPT-4 (texto)
- GPT-4 Vision (imágenes/videos)
- Claude 3.5 Sonnet (Anthropic)
- OpenAI Embeddings (text-embedding-3-large)

### **Base de Datos**
- Supabase PostgreSQL 15
- pgvector (búsqueda semántica)
- Supabase Storage (archivos multimedia)

### **Arquitectura IA**
- **A2A (Agent-to-Agent):** 4 agentes especializados
- **MCP (Model Context Protocol):** Conexión contextualizada con BD legal

---

## 📊 Tecnologías Clave

| Componente | Tecnología |
|------------|------------|
| **React + Vite** | Framework frontend |
| **Laravel 11** | Backend PHP |
| **GPT-4 Vision** | Análisis multimedia |
| **Claude (Anthropic)** | Generación de argumentos |
| **Supabase** | Base de datos + Auth |
| **pgvector** | Búsqueda semántica de jurisprudencia |

---

## 📁 Archivos en este Directorio

- `diagrama-arquitectura-mvp.png` - Diagrama visual completo del flujo técnico
- `README.md` - Este archivo (documentación de la arquitectura)

---

## 🔗 Referencias

Para más detalles técnicos, consulta:
- **PAQUETE-6-ARQUITECTURA-TECNICA.md** - Documentación detallada de agentes y MCP
- **PAQUETE-3-REQUERIMIENTOS-FUNCIONALES.md** - Funcionalidades del MVP

