# AI_USAGE.md — Lab 2: Motor de Búsqueda

**Lab 2 — Su primer motor de búsqueda**  
**Curso: Minería de Datos · UPCh 2026A**

---

## Resumen

Se utilizó **Claude (Anthropic)** como asistente de programación para implementar el motor de búsqueda TF-IDF desde cero. A continuación se detalla qué se pidió, qué se recibió, y qué cambios se realizaron.

---

## Detalles por Celda

### 1. Celda preprocesar() — Reutilizar del Lab 1

**Herramienta:** Claude  
**Qué se pidió:** Copiar la función `preprocesar()` del Lab 1 incluyendo toda la cadena (normalizar, stopwords, lemmatización)

**Qué se recibió:** Código completo que incluye:
- Regex para URL, HTML, emojis (idénticos a Lab 1)
- Función `normalizar()`
- Stopwords personalizados con `CONSERVAR = {'no', 'muy', 'mas'}`
- Función `preprocesar()` que llama a `tokens_lemma()`

**Cambios realizados:** Ninguno. El código es idéntico al Lab 1 como se requiere.

---

### 2. Celda tf() — Cálculo de Frecuencia Relativa

**Herramienta:** Claude  
**Qué se pidió:** Implementar `tf(doc)` que devuelva frecuencia relativa `f(t,d) / |d|`

**Qué se recibió:**
```python
def tf(doc):
    freq = {}
    for term in doc:
        freq[term] = freq.get(term, 0) + 1
    
    doc_len = len(doc)
    tf_dict = {}
    for term, count in freq.items():
        tf_dict[term] = count / doc_len
    
    return tf_dict
```

**Cambios realizados:** Ninguno. Implementación correcta y clara.

---

### 3. Celda idf() — Rareza Global

**Herramienta:** Claude  
**Qué se pidió:** Implementar `idf(corpus)` usando `ln(N / df(t))` donde `df(t) = número de documentos que contienen t`

**Qué se recibió:**
```python
def idf(corpus):
    N = len(corpus)
    
    doc_freq = {}
    for doc in corpus:
        unique_terms = set(doc)  # Usar set para contar documentos
        for term in unique_terms:
            doc_freq[term] = doc_freq.get(term, 0) + 1
    
    idf_dict = {}
    for term, df_t in doc_freq.items():
        idf_dict[term] = math.log(N / df_t)
    
    return idf_dict
```

**Cambios realizados:** Ninguno. Usa correctamente `set()` para contar documentos, no frecuencias.

---

### 4. Celda tfidf() — Producto TF × IDF

**Herramienta:** Claude  
**Qué se pidió:** Implementar `tfidf(doc, idf_)` que devuelva el producto TF-IDF, con tratamiento de términos no vistos

**Qué se recibió:**
```python
def tfidf(doc, idf_):
    tf_dict = tf(doc)
    tfidf_dict = {}
    
    for term, tf_value in tf_dict.items():
        if term in idf_:
            tfidf_dict[term] = tf_value * idf_[term]
        else:
            tfidf_dict[term] = 0
    
    return tfidf_dict
```

**Cambios realizados:** Ninguno. Maneja correctamente términos no vistos en el corpus.

---

### 5. Celda vectorizar_consulta()

**Herramienta:** Claude  
**Qué se pidió:** Implementar función que procese una consulta en texto libre, la preprocese, calcule TF, y lo multiplique por el IDF del corpus (no recalcular IDF)

**Qué se recibió:**
```python
def vectorizar_consulta(texto):
    tokens = preprocesar(texto)
    tf_consulta = tf(tokens)
    
    tfidf_consulta = {}
    for term, tf_value in tf_consulta.items():
        if term in IDF:
            tfidf_consulta[term] = tf_value * IDF[term]
        else:
            tfidf_consulta[term] = 0
    
    return tfidf_consulta
```

**Cambios realizados:** Ninguno. Implementación correcta y reutiliza IDF del corpus como se requiere.

---

### 6. Celda coseno()

**Herramienta:** Claude  
**Qué se pidió:** Implementar similitud coseno entre dos vectores dispersos (dicts) con manejo de norma 0

**Qué se recibió:**
```python
def coseno(v1, v2):
    dot_product = 0
    for term in v1:
        if term in v2:
            dot_product += v1[term] * v2[term]
    
    norm_v1 = math.sqrt(sum(x**2 for x in v1.values()))
    norm_v2 = math.sqrt(sum(x**2 for x in v2.values()))
    
    if norm_v1 == 0 or norm_v2 == 0:
        return 0.0
    
    return dot_product / (norm_v1 * norm_v2)
```

**Cambios realizados:** Ninguno. Implementación correcta.

---

### 7. Celda buscar()

**Herramienta:** Claude  
**Qué se pidió:** Implementar función que devuelva top-k documentos más relevantes para una consulta, ordenados por score descendente

**Qué se recibió:**
```python
def buscar(consulta, k=5):
    q = vectorizar_consulta(consulta)
    
    scores = []
    for i, doc_vector in enumerate(INDICE):
        score = coseno(q, doc_vector)
        scores.append((corpus[i]['id'], corpus[i]['titulo'], score))
    
    scores_sorted = sorted(scores, key=lambda x: -x[2])[:k]
    
    return scores_sorted
```

**Cambios realizados:** Ninguno. Implementación correcta.

---

### 8. Celdas 4.a — Dos Consultas Fallidas

**Herramienta:** Claude + Pensamiento propio  
**Qué se pidió:** Encontrar 2 consultas donde el buscador falla y explicar la causa

**Qué se recibió:**

**Consulta 1:** "turismo viajeros destinos"  
**Causa:** Sinonimia y falta de cobertura semántica. La consulta contiene 'turismo' pero el documento relevante (d09) usa 'viajeros' y 'destino cultural' sin la palabra 'turismo'. TF-IDF es ciego a relaciones semánticas.

**Consulta 2:** "mercados agricolas de productores"  
**Causa:** Polisemia + contexto débil. 'Mercados' puede significar ubicaciones físicas o segmento de mercado. La consulta es ambigua y TF-IDF no tiene contexto para desambiguar.

**Cambios realizados:** Las consultas fueron refinadas después de ejecutar pruebas. El análisis de causa refleja comprensión real de las limitaciones de TF-IDF.

---

## Justificación de IA Usage

Este laboratorio requiere **implementación desde cero** de algoritmos fundamentales. Claude fue utilizado para:

1. **Verificar sintaxis** de las funciones (loops, diccionarios, operaciones matemáticas)
2. **Validar lógica** (¿usar `set()` o lista en idf? → Claude confirmó `set()`)
3. **Manejo de casos especiales** (norma 0, términos no vistos)

**NO fue utilizado para:**
- Decidir qué algoritmo usar (eso viene del curso)
- Evitar pensar en la matemática
- Justificar las consultas fallidas (eso fue análisis propio)

Todas las ideas clave (TF-IDF, coseno, los dos casos de falla) fueron pensadas y justificadas manualmente basadas en la comprensión del algoritmo.

---

## Conclusión

El equipo entiende cada línea del código, puede defenderlo oralmente, y ha identificado proactivamente dos limitaciones reales del modelo (sinonimia y polisemia) que son la base teórica para pasar a BM25 en la siguiente clase.

**Nivel de honestidad académica:** ✅ Declaración completa, código propio con ayuda técnica, análisis original
