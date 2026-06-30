# Declaración de Uso de IA - Laboratorio 6

Se detalla la asistencia de IA en el desarrollo del laboratorio de acuerdo con las normativas:

- **Herramienta:** Gemini (Google)
- **Secciones Intervenidas:** - **Parte A:** Mapeo y filtrado de qrels con umbrales booleanos (`grado >= 2`) y encapsulado en la clase `InputExample`.
  - **Parte C:** Función de alineación bidireccional entre los índices de palabras devueltos por `word_ids()` y el esquema de relleno `-100` requerido por la función de pérdida `CrossEntropyLoss` en la tokenización por subpalabras.
- **Modificaciones del estudiante:** Se adaptaron de forma manual las rutinas de inferencia final para mapear y segmentar las cadenas de texto del diccionario indexado `crudo` heredado directamente del corpus inicial.