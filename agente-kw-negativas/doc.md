# DOC



## Estrategia de Negativización de Palabras Clave para Campañas de Google Shopping

### 🚀 Objetivo

Optimizar las campañas de Google Shopping mediante la exclusión de términos de búsqueda que:

* No generan conversiones.
* Representan intenciones no transaccionales.
* Atraen tráfico de baja calidad o de bajo margen (afiliación).

***

### ✅ Criterios de Negativización

#### 1. Términos Informacionales

* Intención: informarse, no comprar.
* Ejemplos: "qué es", "cómo funciona", "opiniones", "review", "mejores marcas de".
* **Acción:** negativizar estos términos en concordancia amplia o de frase.

#### 2. Marcas No Propias o No Vendidas

* Intención: buscar productos de marcas específicas que no ofrecemos.
* Ejemplo: "aspiradora Dyson" (si no vendemos Dyson).
* **Acción:** negativizar nombre de marcas no disponibles en nuestro catálogo.

#### 3. Términos con Bajo Rendimiento

* Reglas sugeridas:
  * **Clics ≥ 20**, **conversiones = 0**
  * **ROAS < 1** y gasto > X €
  * **ConversionRate < 0.5%** + CPC alto
* **Acción:** negativizar tras revisión periódica.

#### 4. Términos Genéricos

* Ejemplos: "aspiradora", "portátil", "ratón".
* **Problema:** volumen alto pero baja conversión.
* **Acción:** limitar aparición o usar solo como parte de frases más largas (long tail).

#### 5. Ubicaciones No Relevantes

* Ejemplos: "comprar en Argentina", "envío a México" (si solo operamos en España).
* **Acción:** negativizar geolocalizaciones fuera de nuestra zona de actuación.

#### 6. Problemas Técnicos o Postventa

* Ejemplos: "manual cafetera X", "reparar freidora", "reset cafetera".
* **Acción:** negativizar ya que indican que el usuario ya posee el producto.

#### 7. Palabras Clave de Segunda Mano o Regalo

* Ejemplos: "segunda mano", "gratis", "ocasión", "regalo cafetera".
* **Acción:** negativizar para evitar clics no rentables.

***

### 📊 Consideraciones Específicas para Afiliación

* **Margen bajo:** cada clic debe justificarse con alto potencial de conversión.
* **Tolerancia cero** con palabras de bajo rendimiento.
* ROAS mínimo recomendado: **3x o 4x**
* Revisión de términos: **semanal o quincenal**

***

### ⚖️ Herramientas Recomendadas

| Herramienta        | Uso                                                |
| ------------------ | -------------------------------------------------- |
| Google Ads Scripts | Automatizar exclusión de términos negativos        |
| Google Sheets      | Visualización y gestión manual de exclusiones      |
| BigQuery           | Análisis a gran escala de palabras clave           |
| Python + pandas    | Reglas personalizadas de evaluación de rendimiento |
| IA (GPT / ML)      | Clasificación de intención de búsqueda             |

***

### ⚙️ Automatización Recomendable

1. **Script que recoja datos** de rendimiento por término.
2. **Criterios lógicos automáticos:** por clics, conversiones, ROAS.
3. **Generación de lista negativa automática** por semana.
4. **Aprobación manual opcional** de lista final.

***

### 🌐 Ampliaciones Futuras

* Entrenar modelo binario (clasificador ML) con:
  * Entradas: CTR, CPC, CR, coste, ROAS
  * Salida: bueno (mantener) / malo (negativizar)
  * Algoritmos sugeridos: RandomForest, XGBoost, Logistic Regression

***

### 📅 Revisión Continua

* Establecer rutina de **auditoría semanal**.
* Cruzar datos de rendimiento con feed de productos.
* Alinear estrategia con el equipo de producto y marketing.

***

### 👥 Equipo Involucrado

* **Performance Marketing**: definición de criterios
* **Data Analyst**: generación de reportes y dashboard
* **Desarrollador / Script Manager**: automatizaciones

***

### 🔐 Conclusión

Negativizar correctamente es clave en campañas de Shopping, especialmente con bajo margen. Aplicar estas reglas y procesos te permitirá reducir coste, mejorar ROAS y escalar de forma sostenible.
