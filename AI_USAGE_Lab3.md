# Declaración de Uso de IA - Laboratorio 3

De acuerdo con las normativas académicas de la asignatura Minería de Datos (UPCh 2026A), se detalla la intervención de herramientas de IA generativa en el desarrollo de este entregable.

### 1. Herramienta utilizada
- **Modelo:** Gemini (Google)

### 2. Celdas e Implementaciones intervenidas
- **Celda `id: 85371372` (BM25 desde cero):** Se solicitó soporte matemático para modelar correctamente el suavizado de la variante IDF de BM25 (`math.log(1 + (N - df + 0.5) / (df + 0.5))`) evitando salidas negativas, junto a la manipulación del denominador con la longitud relativa del documento (`doc_len / avgdl`).
- **Celda `id: 25975a15` (Métricas de Recuperación de Información):** Generación de lógica nativa para el cálculo acumulativo del descuento logarítmico en `ndcg_at_k` empleando ordenamiento inverso ideal (`IDCG`).

### 3. Modificaciones realizadas por el estudiante
- Se adaptó la entrada de las funciones de evaluación para acoplarse directamente a las estructuras nativas de datos en formato de diccionario de Python (`dict`) construidas a lo largo de los Laboratorios 1 y 2.
- Se verificaron manualmente los scores resultantes del barrido paramétrico para validar que el escalamiento de los coeficientes respondiera a la teoría explicada en clase.