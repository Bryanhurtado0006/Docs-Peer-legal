# 🗄️ Diseño de Base de Datos - PEER-LEGAL-AI

## 📊 Diagrama Entidad-Relación (ERD)

Este directorio contiene el diagrama de base de datos del MVP de PEER-LEGAL-AI, mostrando la estructura de tablas y relaciones necesarias para el funcionamiento del sistema.

---

## 📐 Estructura Principal

### **Tablas Core del Sistema:**

**1. users** - Usuarios del sistema
- Gestión de autenticación y roles (Estudiante, Profesional)
- Control de cuotas diarias (10/50 casos según rol)
- Información de perfil y verificación

**2. cases** - Casos legales ingresados
- Almacena descripción textual y metadata del caso
- Vinculado a usuario propietario (user_id)
- Tracking de estado y fechas

**3. evidence** - Evidencia multimedia
- Archivos visuales (imágenes/videos) del caso
- Referencia a almacenamiento externo (Supabase Storage)
- Metadata de archivo (tipo, tamaño, duración)

**4. case_analysis** - Resultados del análisis IA
- Output de los 4 Agentes A2A
- Resumen del caso generado
- Líneas argumentales (2-3)
- Timestamp de generación

**5. jurisprudence** - Precedentes legales
- Base de conocimiento de sentencias
- **Vector embeddings (pgvector)** para búsqueda semántica
- Metadata legal (corte, fecha, área legal)

**6. case_precedents** - Relación muchos-a-muchos
- Conecta casos analizados con precedentes identificados
- Score de similitud/relevancia
- Explicación de pertinencia

---

## 🔗 Relaciones Principales

```
users (1) ──< (N) cases
cases (1) ──< (N) evidence
cases (1) ──< (1) case_analysis
cases (N) >──< (N) jurisprudence (via case_precedents)
```

---

## 🛠️ Tecnología

- **Motor:** Supabase PostgreSQL 15
- **Búsqueda Semántica:** pgvector extension
- **Embeddings:** OpenAI text-embedding-3-large (1536 dimensiones)
- **Índice:** IVFFlat para búsqueda vectorial eficiente

---

## 📋 Alineación con Documentación

Este diseño soporta:
- **RF-01 a RF-11:** Todos los requerimientos funcionales del PAQUETE-3
- **RNF-09, RNF-10:** Control de acceso por roles y cuotas
- **4 Agentes A2A:** Almacenamiento de outputs en `case_analysis`
- **MCP:** Tabla `jurisprudence` accesible vía MCPService

---

## 🎯 Consideraciones de Diseño

✅ **Normalización:** 3NF para evitar redundancia
✅ **Escalabilidad:** Índices optimizados para consultas frecuentes
✅ **Seguridad:** user_id en todas las tablas críticas (RBAC)
✅ **Privacidad:** Eliminación automática de datos después de 30 días (ver PAQUETE-5)

---

## 📝 Notas

- El diagrama muestra la estructura completa; el MVP implementa las tablas core primero
- Algunas tablas auxiliares (logs, analytics) pueden agregarse en fases posteriores
- Los embeddings vectoriales requieren configuración específica de pgvector en Supabase

