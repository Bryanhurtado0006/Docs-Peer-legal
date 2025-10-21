# 🔒 Paquete 4 — Requerimientos No Funcionales & Restricciones

## 🎯 Por qué este paquete

Garantizar **seguridad, privacidad y performance** adecuados para PEER-LEGAL-AI.

---

## 🛡️ Requerimientos No Funcionales

### **RNF-01: Confidencialidad de Datos**

- **Descripción:** Solo se permiten casos hipotéticos, simulados o completamente anonimizados.
- **Restricción:** El sistema NO debe procesar casos reales en curso ni datos personales identificables.
- **Validación:** Disclaimer visible en el formulario de carga.

---

### **RNF-02: Latencia del Análisis**

- **Descripción:** El análisis completo del caso debe completarse en tiempo razonable.
- **Objetivo:** **< 2 minutos** desde el envío hasta mostrar resultados.
- **Medición:** Tiempo desde submit del formulario hasta visualización de líneas argumentales.

---

### **RNF-03: Formatos de Archivo Soportados**

- **Descripción:** El sistema debe aceptar formatos estándar de evidencia.
- **Formatos permitidos:**
  - **Video:** MP4, AVI
  - **Imagen:** JPG, PNG
  - **Documentos:** PDF (opcional para MVP)
- **Tamaño máximo:** 50 MB por archivo

---

### **RNF-04: Escalabilidad Mínima**

- **Descripción:** El MVP debe soportar uso concurrente básico.
- **Capacidad mínima:** 10 usuarios simultáneos sin degradación de performance.
- **Volumen:** Hasta 100 casos procesados por día.

---

### **RNF-05: Logging y Auditoría**

- **Descripción:** Registrar eventos clave del sistema para debugging y mejora.
- **Eventos a registrar:**
  - Casos cargados (sin contenido sensible)
  - Tiempo de procesamiento por etapa
  - Errores y excepciones
  - Feedback de usuarios
- **Restricción:** NO almacenar texto completo de casos, solo metadatos.

---

### **RNF-06: Anonimización**

- **Descripción:** Proteger identidad de usuarios y datos de casos.
- **Implementación:**
  - IDs de usuario anónimos (UUIDs)
  - Sin vincular casos a identidades reales
  - Datos de entrenamiento completamente anonimizados

---

### **RNF-07: Disponibilidad**

- **Descripción:** El sistema debe estar disponible durante horas de uso esperado.
- **Objetivo:** 95% uptime durante horario laboral (8 AM - 8 PM).
- **Tolerancia:** Máximo 1 hora de downtime no planificado por semana.

---

### **RNF-08: Usabilidad**

- **Descripción:** La interfaz debe ser intuitiva para usuarios no técnicos.
- **Objetivo:** Un nuevo usuario completa el flujo completo sin ayuda en < 5 minutos.
- **Validación:** Test con al menos 3 usuarios reales (estudiantes de derecho).

---

### **RNF-09: Control de Acceso por Roles**

- **Descripción:** Sistema de permisos diferenciados según tipo de usuario.
- **Implementación:**
  - **Estudiante:** Solo casos simulados, 10 casos/día
  - **Profesional:** Casos anonimizados, 50 casos/día, exportación PDF
  - *(Futuro: Admin con acceso a métricas)*
- **Objetivo:** Seguridad y separación de recursos por perfil.

---

### **RNF-10: Límites de Recursos por Rol**

- **Descripción:** Cuotas diferenciadas según tipo de usuario.
- **Especificación:**

| Rol | Casos/día | Almacenamiento | Prioridad Procesamiento |
|-----|-----------|----------------|-------------------------|
| Estudiante | 10 | 500 MB | Normal |
| Profesional | 50 | 5 GB | Alta |

- **Validación:** Sistema bloquea casos adicionales al exceder cuota diaria.

---

## ⚠️ Restricciones del Sistema

| Restricción | Descripción |
|-------------|-------------|
| **Solo casos hipotéticos** | NO procesar casos reales en curso por confidencialidad legal |
| **Sin dictámenes definitivos** | El sistema asiste, NO sustituye criterio legal profesional |
| **Análisis visual con GPT-4 Vision** | Interpretación de imágenes/videos para identificar elementos legalmente relevantes |
| **Jurisdicción única** | MVP enfocado en jurisprudencia colombiana |
| **Sin edición de resultados** | Resultados generados son finales (no editables en MVP) |

---

## ✅ Checklist NFR (Criterio de Aceptación)

- [ ] Disclaimer de confidencialidad visible en UI
- [ ] Análisis completo ejecuta en < 2 minutos
- [ ] Sistema acepta formatos: MP4, JPG, PNG
- [ ] Validación de tamaño de archivo (máx 50 MB)
- [ ] Sistema soporta 10 usuarios concurrentes
- [ ] Logs implementados sin datos sensibles
- [ ] IDs de usuario anónimos (UUIDs)
- [ ] Uptime monitoreado y > 95%
- [ ] Test de usabilidad con 3+ usuarios completado
- [ ] Sistema de roles implementado (Estudiante, Profesional)
- [ ] Cuotas por rol funcionando correctamente
- [ ] Validación de límites diarios por usuario

---

## 📊 Resumen de Prioridades NFR

| Prioridad | Requerimientos |
|-----------|----------------|
| **Crítico** | RNF-01, RNF-02, RNF-03, RNF-09, RNF-10 |
| **Importante** | RNF-05, RNF-06, RNF-08 |
| **Deseable** | RNF-04, RNF-07 |

