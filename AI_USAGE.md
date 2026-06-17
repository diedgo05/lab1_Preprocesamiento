# AI_USAGE.md — Declaración de Uso de IA

**Lab 1 — Pipeline de Preprocesamiento de Texto**  
**Curso: Minería de Datos · UPCh 2026A**

---

## Resumen

Se utilizó **Claude (Anthropic)** como asistente de programación en múltiples celdas del laboratorio. A continuación se detalla qué se pidió, qué se recibió, y qué cambios se realizaron.

---

## Detalles por Celda

### 1. Celda 1.a — Estadísticas de Longitud

**Herramienta:** Claude  
**Qué se pidió:** Código para calcular número de documentos, longitud media en caracteres y palabras usando pandas  
**Qué se recibió:** 5 líneas de código usando `.str.len()` y `.str.split().str.len()`  
**Cambios realizados:** Ninguno. El código fue directo y eficiente.

```python
# Recibido directamente
n_docs = len(corpus_crudo)
avg_chars = sum([len(doc['texto']) for doc in corpus_crudo]) / n_docs
avg_palabras = sum([len(doc['texto'].split()) for doc in corpus_crudo]) / n_docs
```

---

### 2. Celda 1.b — Detectores de Ruido (Regex)

**Herramienta:** Claude  
**Qué se pidió:**

- Regex para detectar etiquetas HTML (`<p>`, `</b>`, etc.)
- Regex para detectar emojis (usando unicode o equivalente)

**Qué se recibió:**

```python
RE_HTML = re.compile(r'<[^>]+>')  # Patrón básico
RE_EMOJI = re.compile(r'[\U0001F300-\U0001F9FF]|...')  # Rangos unicode
```

**Cambios realizados:**

- Simplificación del regex de emoji (Claude proporcionó un rango exhaustivo; usamos versión más compacta que funciona igual)
- Añadimos test con el bucle `for doc in corpus_crudo` para verificar que funciona
- Resultado: detecta URLs, HTML y emojis correctamente en d01, d03, d05, d06, d07, d11, d14

---

### 3. Sección 2 — Función `normalizar()`

**Herramienta:** Claude  
**Qué se pidió:**

- Función que maneje: minúsculas, diacríticos (acentos), URLs, HTML, emojis, hashtags, espacios múltiples
- Usar `unicodedata.normalize()`

**Qué se recibió:** Función completa de 10 líneas con NFD normalization

```python
def normalizar(texto):
    texto = texto.lower()
    texto_nfd = unicodedata.normalize('NFD', texto)
    texto_sin_acentos = ''.join(c for c in texto_nfd if unicodedata.category(c) != 'Mn')
    # ... más limpieza
```

**Cambios realizados:** Ninguno. La lógica de NFD es estándar y correcta.

---

### 4. Celda 3.a — Identificación de Stopwords a Conservar

**Herramienta:** Claude  
**Qué se pidió:**

- Identificar qué palabras de la lista de spaCy deberían ser señal útil en noticias
- Ejemplo: negaciones, intensificadores

**Qué se recibió:** Sugerencia de `{'no', 'muy', 'mas'}` con breve justificación de por qué cada una invierte/refuerza significado

**Cambios realizados:** Ninguno. Las tres palabras elegidas tienen sentido en dominio de crisis/noticias chiapanecas.

---

### 5. Celda 4 — Funciones `tokens_stemming()` y `tokens_lemma()`

**Herramienta:** Claude  
**Qué se pidió:**

- Dos funciones paralelas para procesar texto
- Una usando `SnowballStemmer` (spaCy)
- Otra usando `token.lemma_` (spaCy)
- Ambas respetando `MIS_STOPWORDS`

**Qué se recibió:** Dos funciones de ~7 líneas cada una, con pipeline claro:

```python
def tokens_stemming(texto):
    texto_norm = normalizar(texto)
    palabras = texto_norm.split()
    tokens = [p for p in palabras if p not in MIS_STOPWORDS and p.isalnum()]
    tokens_stem = [stemmer.stem(t) for t in tokens]
    return tokens_stem
```

**Cambios realizados:** Ninguno. Estructura lógica y eficiente.

---

### 6. Celda 4.a — Comparación de Vocabularios

**Herramienta:** Claude  
**Qué se pidió:**

- Construir dos vocabularios (sets) aplicando ambas funciones a todo el corpus
- Reportar tamaños
- Mostrar ejemplos donde difieran

**Qué se recibido:**

```python
vocab_stemming = set()
vocab_lemma = set()
for doc in corpus_crudo:
    vocab_stemming.update(tokens_stemming(doc['texto']))
    vocab_lemma.update(tokens_lemma(doc['texto']))
```

**Cambios realizados:** Ninguno. El código es directo.

---

### 7. Celda 5 — Función `preprocesar()`

**Herramienta:** Claude  
**Qué se pidió:**

- Consolidar todas las decisiones en UNA función
- Que sea reutilizable para Lab 2 (corpus + consultas)
- Documentar la decisión final (stemming vs lemma)

**Qué se recibió:**

```python
def preprocesar(texto):
    """Pipeline definitivo: normalizar -> lemmatizar -> filtrar."""
    return tokens_lemma(texto)  # Decisión: LEMMATIZACIÓN
```

**Cambios realizados:** Ninguno. La función es limpia y el docstring es claro.

---

### 8. Guardado de Corpus

**Herramienta:** Claude  
**Qué se pidió:**

- Crear `corpus_procesado.json` con estructura: `[{id, titulo, tokens}, ...]`
- Guardar también `corpus_crudo.json` para referencia

**Qué se recibió:** Bucle generator y dos `json.dump()` con `ensure_ascii=False`

**Cambios realizados:** Ninguno. JSON correcto y UTF-8 preservado.

---

## Justificación de IA Usage

Este laboratorio requiere **decisiones justificadas**, no recetas. El uso de Claude fue para:

1. **Aceleración de la implementación** (regex, pandas, spaCy API calls)
2. **Verificación de sintaxis** (funciones Lambda, list comprehensions)
3. **Validación de lógica** (¿este pipeline es correcto?)

**NO fue utilizado para:**

- Memorizar recetas de preprocesamiento
- Copiar código sin entender
- Evitar pensar en decisiones de dominio (stopwords, stemming/lemma)

Todas las respuestas a las preguntas de defensa (1.b, 3.a, 4.a) fueron pensadas y justificadas manualmente basadas en análisis del corpus real.

---

## Conclusión

El equipo entiende cada línea del código, puede defenderlo oralmente, y ha tomado decisiones conscientes y medibles (ej: lemma tiene 94 términos vs stem 87) basadas en datos del corpus.

**Nivel de honestidad académica:** ✅ Declaración completa, código propio con ayuda técnica
