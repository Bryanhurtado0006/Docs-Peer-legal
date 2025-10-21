# 🧾 PEER-LEGAL-AI — One Pager (Paquete 1)

## 💡 Nombre del Proyecto

**PEER-LEGAL-AI** — Asistente Inteligente para el Análisis y Preparación de Casos Jurídicos

---

## ⚖️ Problema

Los estudiantes de derecho y abogados junior enfrentan uno de los desafíos más complejos de su formación: **la preparación de casos legales**. Este proceso, tradicionalmente arduo y fragmentado, consume tiempo valioso y puede resultar en análisis incompletos que comprometen la calidad de la defensa.

**Puntos de dolor identificados:**

1. **Proceso Manual y Fragmentado:** Revisión manual de cada pieza de evidencia dispersa en múltiples formatos (documentos, imágenes, videos, testimonios).

2. **Búsqueda Ineficiente de Jurisprudencia:** Horas de investigación para encontrar precedentes relevantes en bases de datos legales.

3. **Análisis de Evidencia Visual Limitado:** Videos y fotografías requieren interpretación manual con riesgo de pasar por alto detalles cruciales.

4. **Generación de Argumentos Compleja:** Conectar hechos con precedentes legales es subjetivo y difícil para estudiantes sin experiencia.

5. **Impacto en el Aprendizaje:** Proceso desmotivante, aprendizaje por prueba y error sin retroalimentación inmediata.

---

## 👥 Usuarios Objetivo

### **👨‍🎓 Usuario Primario: Estudiante**

**María**, estudiante de 5.º año de Derecho, debe preparar una defensa inicial en **48 horas**.

**Caso asignado:**
- 15 páginas de expediente
- 3 testimonios contradictorios
- 1 video de cámara de seguridad (3 minutos)
- Base de datos con 10,000+ sentencias

**Proceso actual (14 horas total):**
- 3 horas leyendo expediente
- 4 horas buscando jurisprudencia
- 2 horas analizando video manualmente
- 5 horas intentando conectar todo
- **Resultado a las 2 AM:** Sin línea argumentativa clara

**Necesidades:**
- Herramienta que integre texto, video e información legal en **minutos, no horas**
- Identificar líneas de defensa sin investigación manual exhaustiva
- Precedentes jurídicos relevantes con explicación de pertinencia
- **Cuota:** 10 casos/día (suficiente para práctica académica)

---

### **⚖️ Usuario Secundario: Profesional**

**Carlos**, abogado junior en bufete pequeño, necesita preparar casos rápidamente.

**Diferencias con Estudiante:**
- Trabaja con **casos reales anonimizados** (no solo simulados)
- Necesita **exportación de informes PDF** profesionales
- Mayor volumen de casos: **50 casos/día**
- Requiere mayor nivel de precisión y detalle

**Uso típico:**
- Pre-análisis rápido antes de reunión con cliente
- Búsqueda de precedentes para fundamentar estrategia
- Generación de reportes para revisión del socio senior

---

### **🔧 Posible Usuario Futuro: Administrador**
*(Fase 2-3 del roadmap)*
- Panel de métricas del sistema
- Gestión de usuarios y cuotas
- Analytics de uso y performance

---

## 🎯 Objetivo del MVP

Desarrollar un **prototipo funcional** que integre el análisis de texto y evidencia visual para:

1. **Identificar** los elementos legales clave del caso.
2. **Relacionarlos** con jurisprudencia relevante.
3. **Generar** 3 posibles líneas argumentales como base para la defensa.

**El propósito del MVP es acelerar la preparación inicial de un caso**, brindando un punto de partida confiable y claro para el análisis jurídico.

---

## ⚙️ Alcance del MVP

### **Entradas:**
- Descripción textual del caso (input manual).
- Archivo visual (imagen o video del incidente).

### **Procesamiento:**
- Análisis de texto con **NLP avanzado** (GPT-4 / Claude).
- Análisis de evidencia visual con **GPT-4 Vision** (interpretación de imágenes/videos).
- **Búsqueda semántica de jurisprudencia** usando embeddings y pgvector.
- Generación de **líneas argumentales** mediante agentes IA especializados (A2A).

### **Salidas:**
- **Resumen del caso** con hechos clave, partes involucradas y conceptos jurídicos.
- **Análisis de evidencia visual** con elementos legalmente relevantes identificados.
- **2-3 precedentes relevantes** con score de similitud y explicación de pertinencia.
- **2-3 líneas argumentales** con fortalezas, debilidades y precedentes de apoyo.

### **Restricciones:**
- Solo se trabajará con **casos hipotéticos o completamente anonimizados**.
- El sistema **NO emite dictámenes legales definitivos** (solo asiste en preparación).
- Análisis visual con GPT-4 Vision para identificar elementos relevantes en imágenes/videos.

---

## 📊 Criterios de Éxito del MVP

✅ El sistema recibe texto y evidencia visual **sin errores**.

✅ Los resultados son **coherentes** entre el texto y la jurisprudencia sugerida.

✅ El usuario (María) logra identificar una línea de defensa clara en **menos de 10 minutos**.

✅ Al menos el **80% de los usuarios de prueba** perciben que PEER-LEGAL-AI acelera su análisis inicial.

---

## 🧩 Momentos del Reto (Flujo General)

| # | Momento | Descripción |
|---|---------|-------------|
| **1** | **Comprensión del caso** | El usuario carga la descripción textual y archivos visuales |
| **2** | **Procesamiento del texto** | PEER-LEGAL-AI extrae hechos, partes involucradas y conceptos jurídicos |
| **3** | **Conexión con jurisprudencia** | Busca precedentes relevantes en bases de datos públicas |
| **4** | **Generación de líneas argumentales** | Propone tres posibles enfoques de defensa |
| **5** | **Visualización integrada** | Muestra texto, precedentes y argumentos en un panel unificado |
| **6** | **Retroalimentación** | El usuario califica la utilidad del análisis |

---

## 🧱 KPIs Iniciales

| Métrica | Objetivo |
|---------|----------|
| Tiempo de análisis | **< 10 minutos** por caso |
| Precisión de coincidencia con jurisprudencia | **≥ 70%** de relevancia percibida |
| Nivel de satisfacción del usuario | **≥ 4/5** estrellas |
| Casos analizados sin error | **95%** |

---

## 🔧 Recursos Técnicos Iniciales

- **Backend:** Laravel 11 (PHP)
- **Frontend:** React + Vite
- **Base de datos:** Supabase PostgreSQL + pgvector (búsqueda semántica)
- **LLM:** GPT-4 + GPT-4 Vision (OpenAI) + Claude (Anthropic)
- **Arquitectura IA:** 
  - **Agentes A2A:** Coordinador, Visual, Jurisprudencia, Argumentos
  - **MCP (Model Context Protocol):** MCPService para conexión con BD legal
- **Storage:** Supabase Storage para archivos visuales

---

## 🚀 Visión a Futuro

En etapas posteriores se incluirá:

- ✨ **Análisis de video avanzado** con detección de eventos temporales específicos.
- ✨ **Generación automática de informes legales** estructurados (PDF/Word).
- ✨ **Entrenamiento continuo** con corpus de argumentación jurídica colombiana.
- ✨ **Colaboración entre usuarios** para compartir análisis de casos similares.
- ✨ **Integración con bases de datos oficiales** (Relatoría, Corte Constitucional).

---

## 📅 Próximo Paso

**Paquete 2:** Documentación detallada de los 6 momentos del reto con flujos de usuario específicos para el caso de María.

