# 🧭 Paquete 2 — Momentos y Flujo de Usuario

## 🎯 Objetivo del Paquete

Documentar los **momentos clave del reto** (proceso de uso) y el **flujo de interacción del usuario** dentro del MVP PEER-LEGAL-AI, para orientar el diseño de pantallas, componentes y funcionalidades.

---

## ⚖️ Personaje Principal

**María**, estudiante de 5.º año de Derecho, debe preparar una defensa inicial en **48 horas**.

Usa **PEER-LEGAL-AI** para analizar un caso práctico que incluye texto y video de un accidente de tránsito con testimonios contradictorios.

---

## ⏱️ Momentos del Reto (Flujo de Valor del Usuario)

| Momento | Descripción del objetivo | Acción del usuario | Resultado esperado / salida del sistema |
|---------|--------------------------|--------------------|-----------------------------------------|
| **Momento 1 — Ingreso y comprensión del caso** | María inicia sesión o accede al prototipo e ingresa los datos del caso. | Escribe la descripción del caso y sube los archivos (texto, imagen o video). | El sistema valida la información y prepara los datos para análisis. Muestra confirmación de carga exitosa. |
| **Momento 2 — Procesamiento del texto** | PEER-LEGAL-AI analiza el texto usando NLP para identificar hechos, partes involucradas y términos jurídicos relevantes. | (Automático) | Se genera un **resumen del caso** con lista de elementos clave: hechos cronológicos, partes, área legal, conceptos jurídicos identificados. |
| **Momento 3 — Procesamiento del material visual** | PEER-LEGAL-AI analiza la evidencia visual usando GPT-4 Vision para identificar elementos legalmente relevantes. | (Automático) | Se genera análisis visual con: elementos detectados, eventos clave, detalles jurídicamente significativos. |
| **Momento 4 — Conexión con jurisprudencia** | El sistema busca en bases de datos abiertas los precedentes legales más similares al caso de María. | (Automático) | Se muestran **2–3 sentencias o precedentes** con breve explicación de su relevancia y similitud con el caso actual. |
| **Momento 5 — Generación de líneas argumentales** | PEER-LEGAL-AI propone 3 posibles líneas de defensa basadas en los hechos extraídos y la jurisprudencia encontrada. | El usuario revisa las opciones generadas. | Se presentan **3 líneas de defensa** con fortalezas, debilidades y nivel de viabilidad estimado. |
| **Momento 6 — Revisión y feedback del usuario** | María revisa el resultado completo, selecciona la línea más prometedora y da retroalimentación sobre la utilidad. | Escoge una línea preferida y califica la utilidad (1-5 estrellas). | El sistema registra la elección y feedback para mejorar futuras recomendaciones. |

---

## 📲 Flujo de Usuario (versión MVP)

```
[Inicio de sesión / Pantalla principal]
        │
        ▼
[Momento 1: Ingreso del caso]
→ Usuario escribe descripción del caso
→ Sube archivo visual (imagen/video del incidente)
→ Confirma envío
        │
        ▼
[Momento 2: Análisis de texto]
→ NLP identifica: partes, hechos, área legal, conceptos jurídicos
→ Sistema muestra resumen del caso
        │
        ▼
[Momento 3: Procesamiento visual (GPT-4 Vision)]
→ Agente Visual analiza imagen/video
→ Identifica elementos legalmente relevantes (ej. señales, personas, vehículos)
→ Genera descripción contextualizada del incidente
        │
        ▼
[Momento 4: Conexión con jurisprudencia]
→ Búsqueda semántica en base de precedentes
→ Muestra 2-3 sentencias relevantes con explicación
        │
        ▼
[Momento 5: Líneas argumentales sugeridas]
→ Generación de 3 posibles líneas de defensa
→ Cada línea incluye: argumento principal, fortalezas, debilidades
        │
        ▼
[Momento 6: Revisión y retroalimentación]
→ Usuario revisa las 3 líneas argumentales
→ Selecciona su preferida
→ Califica la utilidad del análisis (1-5 estrellas)
        │
        ▼
[FIN — Resultados guardados / Panel de visualización integrado]
```

---

## 🧱 Componentes Clave del MVP (por momento)

| Momento | Componentes del sistema | Tipo |
|---------|-------------------------|------|
| **1** | Formulario de carga de caso (texto + archivo visual) | React + Vite / Frontend |
| **2** | Agente Coordinador + NLP (GPT-4/Claude) | Laravel Services / A2A |
| **3** | Agente Visual (GPT-4 Vision) | Laravel Services / A2A |
| **4** | Agente Jurisprudencia (pgvector + embeddings) | Laravel Services / A2A + MCPService |
| **5** | Agente Argumentos (LLM + precedentes) | Laravel Services / A2A |
| **6** | Dashboard de resultados + feedback | React + Vite / Frontend |

---

## 💡 Valor generado por cada momento

| Momento | Valor para el usuario | Valor para el sistema |
|---------|----------------------|----------------------|
| **1** | Centraliza toda la información del caso en un solo flujo. | Obtiene datos de entrada estructurados y validados. |
| **2** | Ahorra horas de lectura manual y extracción de hechos relevantes. | Identifica patrones textuales y conceptos legales clave. |
| **3** | Identifica elementos visuales clave que podrían pasar desapercibidos. | Entrena y valida capacidad del Agente Visual en contexto legal. |
| **4** | Reduce el tiempo de búsqueda en jurisprudencia de horas a segundos. | Mejora su base de conocimiento legal y precisión de coincidencias. |
| **5** | Da claridad y dirección para argumentar, elimina el "¿por dónde empiezo?". | Demuestra el potencial de la IA jurídica generativa. |
| **6** | Refuerza el aprendizaje práctico y permite iterar sobre el análisis. | Permite mejorar el modelo con retroalimentación real de usuarios. |

---

## 🧩 Conclusión del Paquete 2

El flujo del usuario en **PEER-LEGAL-AI** se divide en **6 momentos clave** que van desde la carga del caso hasta la obtención de líneas argumentales y la retroalimentación.

Cada momento agrega **valor progresivo**, y el MVP se puede construir inicialmente con:
- ✅ **Momentos 1-2** (ingreso y análisis básico de texto)
- ✅ **Momento 4** (conexión con jurisprudencia)
- ✅ **Momento 5 básico** (al menos 2 líneas argumentales)

Esto permite validar la **utilidad core** del sistema: **acelerar la preparación inicial de casos** con análisis coherente y precedentes relevantes.

---

## 📅 Próximo Paso

**Paquete 3:** Requerimientos funcionales priorizados del MVP.

