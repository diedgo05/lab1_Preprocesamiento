# Declaración de Uso de IA - Laboratorio 5

De acuerdo con las normativas académicas de la asignatura Minería de Datos (UPCh 2026A), se detalla la intervención de herramientas de IA generativa en el desarrollo de este entregable.

### 1. Herramienta utilizada
- **Modelo:** Gemini (Google)

### 2. Celdas e Implementaciones intervenidas
- **Celda `id: ca8ca1ac` (Carga e inicialización de FastText):** Soporte en la sintaxis de métodos nativos de la librería `fasttext` en Python (`get_word_vector` y `get_nearest_neighbors`).
- **Celda `id: da4af8b3` y `id: 44a39eed` (Representación densa y Buscador Semántico):** Apoyo en la vectorización estructurada de textos mediante la consolidación de promedios multidimensionales a lo largo de ejes de matrices de NumPy (`np.mean(axis=0)`), y su mapeo de ordenamiento inverso en similitudes coseno densas.

### 3. Modificaciones realizadas por el estudiante
- Se encadenaron de manera estricta los diccionarios de títulos y listas de tokens heredados directamente de las colecciones de los laboratorios previos para asegurar la coherencia analítica en las tablas comparativas.
- Se redactaron e interpretaron de forma personalizada las respuestas analíticas basándose en los resultados de recuperación empírica observados en el corpus.