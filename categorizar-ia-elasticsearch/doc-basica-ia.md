# Doc basica IA



### Sistema de Categorización Semántica usando Elasticsearch + Vector DB (Embeddings)

#### Objetivo

Crear un sistema de categorización automática de títulos de productos o contenidos (por ejemplo, etiquetas H1), utilizando ingeniería inversa y búsqueda semántica en lugar de LLMs. El objetivo es reducir costes y escalar fácilmente.

***

#### Enfoques comparados

| Enfoque                           | Costo inicial | Costo por escala | Precisión inicial | Mantenimiento                     | Escalabilidad      | Latencia            |
| --------------------------------- | ------------- | ---------------- | ----------------- | --------------------------------- | ------------------ | ------------------- |
| **LLMs (GPT, Claude, etc.)**      | Alto          | Alto (tokens/$)  | Alta (85–95%)     | Bajo (poco código)                | Media (por precio) | Alta (puede tardar) |
| **Elasticsearch (texto clásico)** | Bajo          | Muy bajo         | Media (70–80%)    | Medio (hay que mejorar búsquedas) | Alta               | Muy baja (rápido)   |
| **Elasticsearch + Vector DB**     | Medio         | Bajo             | Alta (80–90%)     | Medio (gestión de vectores)       | Muy alta           | Baja (rápida)       |

***

#### Ventajas del enfoque con Elasticsearch + Vector DB

* Permite usar embeddings semánticos en búsquedas kNN.
* Mucho más barato que usar LLMs para cada clasificación.
* Precisión cercana al uso de modelos grandes si se ajustan bien los datos.
* Escalable a millones de productos sin aumentar costes significativamente.

***

#### ¿Cómo se generan los vectores (embeddings) localmente y gratis?

Usando modelos open source como `all-MiniLM-L6-v2` de `sentence-transformers`, puedes generar vectores desde texto sin pagar APIs externas.

**Requisitos**

* Python
* `pip install sentence-transformers`

```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer('all-MiniLM-L6-v2')
titulo = "iPhone 14 Pro Max 128GB - Apple"
vector = model.encode(titulo)
print(vector.shape)  # (384,)
```

***

#### Integración con Elasticsearch

**Estructura de índice:**

```json
PUT productos
{
  "mappings": {
    "properties": {
      "titulo": { "type": "text" },
      "categoria": { "type": "keyword" },
      "embedding": {
        "type": "dense_vector", "dims": 384,
        "index": true, "similarity": "cosine"
      }
    }
  }
}
```

**Flujo del sistema:**

1. Se genera el embedding del título usando el modelo local.
2. Se hace búsqueda `kNN` en Elasticsearch con ese vector.
3. Se recuperan las categorías más similares.
4. Se asigna la categoría más frecuente o con mejor score.

***

#### Mejora progresiva

* Si la precisión es >80%, es una alternativa rentable a los LLMs.
* Se puede aplicar lógica de fallback: usar LLM **solo si** Elastic devuelve score bajo.

***

#### Próximos pasos sugeridos

* Preparar un script que genere embeddings desde CSV/JSON de títulos.
* Cargar esos datos en Elasticsearch con sus vectores.
* Hacer pruebas de precisión categorizando textos nuevos.

***

Este enfoque permite montar un sistema de categorización semántica **100% escalable, de bajo coste y con resultados muy sólidos** sin depender de APIs de pago. Ideal para proyectos con grandes volúmenes de datos como ecommerce, agregadores de contenido, etc.
