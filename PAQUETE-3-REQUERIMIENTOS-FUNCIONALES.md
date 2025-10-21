# 📋 Paquete 3 — Requerimientos Funcionales (por prioridad)

## 🎯 Por qué este paquete

Saber **qué construir primero** para entregar un MVP funcional de PEER-LEGAL-AI.

---

## ⚙️ Requerimientos Funcionales del MVP

### **RF-01: Formulario de carga de caso**

- **Descripción:** Permitir al usuario ingresar la descripción textual del caso y subir archivo visual (imagen o video).
- **Entrada:** Texto del caso (descripción libre), archivo visual (imagen/video, máx 50 MB).
- **Salida:** Confirmación de carga exitosa, caso almacenado con ID único.
- **Prioridad:** **Must**
- **Dependencia:** Ninguna

---

### **RF-02: Análisis automático del texto del caso**

- **Descripción:** Procesar el texto con NLP para extraer hechos clave, partes involucradas, área legal y conceptos jurídicos.
- **Entrada:** Texto del caso (del RF-01).
- **Salida:** Resumen estructurado con: partes identificadas, hechos cronológicos, área legal, conceptos jurídicos clave.
- **Prioridad:** **Must**
- **Dependencia:** RF-01

---

### **RF-03: Búsqueda de precedentes jurídicos**

- **Descripción:** Buscar y mostrar 2-3 precedentes legales relevantes al caso del usuario.
- **Entrada:** Resumen del caso (del RF-02).
- **Salida:** Lista de 2-3 sentencias/precedentes con: nombre del caso, similitud, explicación de pertinencia.
- **Prioridad:** **Must**
- **Dependencia:** RF-02

---

### **RF-04: Generación de líneas argumentales**

- **Descripción:** Generar 2-3 posibles líneas de defensa basadas en hechos del caso y precedentes encontrados.
- **Entrada:** Resumen del caso (RF-02) + precedentes (RF-03).
- **Salida:** 2-3 líneas argumentales con: argumento principal, fortalezas, debilidades.
- **Prioridad:** **Must**
- **Dependencia:** RF-02, RF-03

---

### **RF-05: Visualización integrada de resultados**

- **Descripción:** Mostrar en un panel unificado: resumen del caso, precedentes y líneas argumentales.
- **Entrada:** Resultados de RF-02, RF-03, RF-04.
- **Salida:** Dashboard/panel con todas las secciones visibles y navegables.
- **Prioridad:** **Must**
- **Dependencia:** RF-02, RF-03, RF-04

---

### **RF-06: Retroalimentación del usuario**

- **Descripción:** Permitir al usuario calificar la utilidad del análisis generado.
- **Entrada:** Calificación de 1-5 estrellas.
- **Salida:** Feedback almacenado en base de datos.
- **Prioridad:** **Should**
- **Dependencia:** RF-05

---

### **RF-07: Análisis de evidencia visual con IA**

- **Descripción:** Analizar archivo visual usando GPT-4 Vision para identificar elementos legalmente relevantes.
- **Entrada:** Archivo visual del RF-01.
- **Salida:** Análisis visual con: elementos detectados, eventos clave, descripción contextualizada para uso legal.
- **Prioridad:** **Must**
- **Dependencia:** RF-01

---

### **RF-08: Descarga de reporte del análisis**

- **Descripción:** Generar y descargar reporte PDF con el análisis completo del caso.
- **Entrada:** Resultados del análisis (RF-02 a RF-04).
- **Salida:** Archivo PDF descargable.
- **Prioridad:** **Could**
- **Dependencia:** RF-05

---

### **RF-09: Autenticación y gestión de usuarios**

- **Descripción:** Sistema de login con roles diferenciados (Estudiante, Profesional).
- **Entrada:** Email, contraseña, tipo de usuario.
- **Salida:** Sesión autenticada con permisos según rol.
- **Prioridad:** **Must**
- **Dependencia:** Ninguna

---

### **RF-10: Control de cuotas por rol**

- **Descripción:** Aplicar límites de casos procesados según tipo de usuario.
- **Entrada:** Usuario autenticado + intento de crear caso.
- **Salida:** Validación de cuota disponible (Estudiante: 10/día | Profesional: 50/día).
- **Prioridad:** **Must**
- **Dependencia:** RF-09

---

### **RF-11: Perfiles diferenciados por rol**

- **Descripción:** Funcionalidades adicionales según tipo de usuario.
- **Entrada:** Rol del usuario.
- **Salida:** 
  - **Estudiante:** Acceso básico, casos simulados
  - **Profesional:** Exportación PDF avanzada, casos anonimizados, mayor cuota
  - *(Futuro: Panel Admin para métricas del sistema)*
- **Prioridad:** **Should**
- **Dependencia:** RF-09

---

## 📊 Resumen de Prioridades

| Prioridad | Requerimientos | Total |
|-----------|----------------|-------|
| **Must** | RF-01, RF-02, RF-03, RF-04, RF-05, RF-07, RF-09, RF-10 | 8 |
| **Should** | RF-06, RF-11 | 2 |
| **Could** | RF-08 | 1 |

---

## ✅ Criterio de Aceptación

Todos los requerimientos **Must** deben estar implementados y funcionales para considerar el MVP completo.

Los requerimientos **Should** se implementan si hay tiempo disponible.

Los requerimientos **Could** son para versiones futuras post-MVP.

