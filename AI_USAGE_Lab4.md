# Declaración de Uso de IA - Laboratorio 4

De acuerdo con las normativas académicas de la asignatura Minería de Datos (UPCh 2026A), se detalla la intervención de herramientas de IA generativa en el desarrollo de este entregable.

### 1. Herramienta utilizada
- **Modelo:** Gemini (Google)

### 2. Celdas e Implementaciones intervenidas
- **Celda `id: cacd9163` (Matriz TF-IDF L2):** Soporte en la lógica de vectorización simultánea sobre matrices densas de `numpy` utilizando mapeos de diccionarios léxicos (`col[term]`), garantizando la correcta división de componentes vectoriales sobre sus magnitudes en bloques euclidianos sin lanzar excepciones de división entre cero.
- **Celda `id: 542f10c3` (K-Means y validaciones):** Soporte en el traslado funcional del K-Means matricial nativo desarrollado en la Unidad 1 para operar eficientemente sobre la estructura densa del corpus de texto normalizado, coordinándolo con la graficación paralela de ejes compartidos para Inercia y Silueta.

### 3. Modificaciones realizadas por el estudiante
- Se ajustaron los parámetros de la semilla aleatoria (`seed=42`) para garantizar la reproducibilidad exacta de los clusters identificados en la defensa oral.
- Se examinaron individualmente las distribuciones resultantes para justificar teóricamente el error conceptual que induce la indexación por frecuencias (TF-IDF) en escenarios de sinonimia profunda.