# Estudio de costes



### Medios

* Prisa
  * El País (26)
  * As (41)
* Godo
  * La Vanguardia (28)
  * Mundo Deportivo (43)
* Vocento
  * ABC (62)
* Hola (9)

***

## Campañas que tenemos para estos medios

Total de campañas: 21736\
Total de campañas con nombre de url: 19911\
Total de campañas con enlaces aparentemente validos: 16075

```
SELECT
    *
FROM
    campaigns
WHERE
    campaigns.client_id IN (26, 41, 28, 43, 62, 9)
    AND campaigns.name LIKE 'https://%'
    AND campaigns.name not LIKE 'https://editor2-fashion.hola%'
    AND campaigns.name not LIKE 'https://editor2.hola.com%'
    AND campaigns.name not LIKE 'https://mina2.hola%'
    AND campaigns.name not LIKE 'https://admin.lavanguardia.com%'
    AND campaigns.name not LIKE 'https://qa.hola%'
    AND campaigns.name not LIKE 'https://staging.hola%'
    AND campaigns.name not LIKE 'https://dev.hola%'
    AND campaigns.name not LIKE 'https://admin.mundodeportivo%'
    AND campaigns.name not LIKE 'https://0-www-lavanguardia-com.aladi.diba.cat'
    AND campaigns.name not LIKE 'https://badge.spotlesscloth.com'
    AND campaigns.name not LIKE 'https://www-lavanguardia-com.sire.ub.edu'
;
```

***

## Costes de embedding

```
# Llevar a embedings todas las category_path (5000)

* Aproximadamente 0.01€
```

```
# Categorizar 1 sola campaña

* Modelo para llevar a embedding de OpenAI: text-embedding-3-small
    - dimensiones: 1536
    - coste: 0.02€/1M tokens
    - coste por token: 0,00000002€/1 token
* Analisis de coste para 1 contenido
    - cantidad de tokes de un contenido aproximadamente: 1024
    - coste de convertir a embedding: 0,00002048€
    - Estimación: 1€/48828 contenidos
```

***

## Analizando coste de Lambda para proyecto de búsqueda(FALTA agregar costes de Elastic Seacrch (Opensearch) en AWS)

Claro, aquí tienes el análisis de coste correcto y conciso, aplicando el Nivel Gratuito de AWS.

#### **Análisis de Coste Correcto para AWS Lambda (incluyendo Nivel Gratuito)**

**1. Parámetros de Carga**

* **Memoria:** 1 GB
* **Duración por invocación:** 3 segundos
* **Invocaciones mensuales:** 3000/semana × 4 semanas = **12000**
* **GB-segundos mensuales:** **12000** invocaciones × 1 GB × 20 s = **240000 GB-s**

**2. Cálculo de Coste Mensual (Región us-east-1)**

Se aplica el **Nivel Gratuito mensual** de AWS Lambda:

* `1,000,000` de invocaciones gratuitas.
* `400,000` GB-segundos de cómputo gratuitos.

**A. Coste por Invocaciones:**

* Invocaciones totales: 12000
* Invocaciones gratuitas: 0
* **Invocaciones facturables: 12000**
* Cálculo: (120000 / 1,000,000) × $0.20 = **$0.024**

**B. Coste por Tiempo de Cómputo (x86):**

* GB-segundos totales: 240000
* GB-segundos gratuitos: 0
* **GB-segundos facturables:** 240000
* Cálculo: 240000 × $0.0000166667 = **$4**

**3. Total Mensual Estimado**

#### **$4** (Cómputo) + **$0.024** (Invocaciones) = **✅ $5 / mes**



***

## Estimación de Costes - Elasticsearch en AWS (OpenSearch Service)

Este estudio estima el coste mensual de desplegar un clúster básico de **Elasticsearch** mediante **Amazon OpenSearch Service**, con la configuración mínima disponible (2 GB RAM).



#### 🔧 Configuración Seleccionada

| Parámetro              | Valor                       |
| ---------------------- | --------------------------- |
| Tipo de instancia      | `t3.small.search`           |
| Memoria RAM            | 2 GB                        |
| vCPU                   | 2                           |
| Almacenamiento         | 20 GB EBS gp3               |
| Zona de disponibilidad | 1 (sin alta disponibilidad) |
| Región estimada        | `us-east-1` (referencia)    |
|                        |                             |
|                        |                             |



#### 💰 Desglose de Costes

| Concepto                   | Unidad              | Tarifa estimada | Costo mensual aprox. |
| -------------------------- | ------------------- | --------------- | -------------------- |
| Instancia OpenSearch       | 1 × t3.small.search | $0.034/hora     | **$24.50 USD**       |
| Disco EBS (gp3)            | 20 GB               | $0.08/GB/mes    | **$1.60 USD**        |
| **Total estimado mensual** |                     |                 | **≈ $26.10 USD**     |





***

## 🧾 Estimación de Costes - Solución en EC2 (VPS) usando `t4g.large`

#### ✅ Descripción General

Esta solución despliega toda la arquitectura (API + scraping + búsqueda vectorial en Elasticsearch) en una única instancia EC2 tipo **`t4g.large`** en AWS, aprovechando sus 2 vCPU y 8 GB de RAM. Es una alternativa autogestionada, más económica y flexible frente a soluciones gestionadas como OpenSearch Service.

#### 🔧 Configuración de la Instancia

| Recurso              | Valor                                     |
| -------------------- | ----------------------------------------- |
| Tipo de instancia    | `t4g.large`                               |
| vCPU                 | 2 (Graviton2 – ARM64)                     |
| RAM                  | 8 GB                                      |
| Almacenamiento       | 20 GB EBS SSD (gp3)                       |
| Sistema operativo    | Ubuntu Server 22.04 LTS (ARM64)           |
| Servicios instalados | Elasticsearch 8.x, Node.js API, Puppeteer |

#### 💰 Coste Mensual Aproximado

> Precios estimados en región **us-east-1**, modo **on-demand**.

| Concepto                 | Coste mensual aprox. (USD)        |
| ------------------------ | --------------------------------- |
| EC2 `t4g.large`          | \~$8.27 (con CPU credit incluido) |
| 20 GB EBS SSD (gp3)      | \~$1.80                           |
| Tráfico de salida (5 GB) | \~$0.45                           |
| **Total estimado**       | ✅ **$10.52 / mes**                |

#### 🧠 Consideraciones Técnicas

* **Rendimiento**: suficiente para entornos de producción ligeros y medios con tráfico moderado.
* **Optimización**: puedes hacer tuning de Elasticsearch, cachear resultados y usar cron jobs para scraping.
* **Seguridad y mantenimiento**: requiere gestionar actualizaciones, firewall, backups, etc.
* **Requisitos**: arquitectura ARM64 (algunas librerías de scraping como Puppeteer deben compilarse correctamente).

#### 🟢 Ventajas

* Coste muy bajo (más del 60% más barato que OpenSearch Service + Lambda).
* Control total sobre la configuración y optimización de todos los servicios.
* Menor latencia al tener todo integrado en el mismo servidor.

#### 🔴 Desventajas

* Necesita administración técnica (monitorización, seguridad, actualizaciones).
* No cuenta con autoescalado automático como los servicios gestionados.

***

## Estudio de concurrencia en VPS

La respuesta corta es: para tu caso de uso específico (I/O-bound), una configuración optimizada en una `t4g.large` probablemente podría manejar entre **50 y 200 solicitudes concurrentes** de manera estable.

Ahora, vamos al desglose detallado para que tu equipo entienda el porqué.

#### **Factores Clave que Definen la Concurrencia**

FastAPI, al ser asíncrono (`asyncio`), es excepcionalmente bueno manejando tareas "I/O-bound". Esto significa que brilla cuando la mayor parte del tiempo de una solicitud se pasa **esperando** por algo, como:

1. **Esperando la respuesta de la web que estás scrapeando (el factor más importante).**
2. Esperando la respuesta de la base de datos de Elasticsearch.

No está limitado por el número de hilos, sino por la eficiencia con la que el bucle de eventos puede cambiar de tarea mientras espera.

Sin embargo, tu VPS tiene límites físicos duros:

* **CPU (2 vCPUs):** El recurso para el trabajo "real" (procesar el HTML, ejecutar la lógica de Python).
* **RAM (8 GB):** El recurso más crítico en tu caso, debido a Elasticsearch.
* **Red:** El ancho de banda de la instancia EC2.

***

#### **Análisis de los Cuellos de Botella**

**1. La Configuración del Servidor de Aplicaciones (Gunicorn + Uvicorn)**

No ejecutas FastAPI directamente. Usas un servidor ASGI como `Uvicorn`, gestionado por un supervisor de procesos como `Gunicorn`.

* **Workers de Gunicorn:** La regla general para empezar es `(2 * número_de_núcleos) + 1`.
  * En tu `t4g.large` con 2 vCPUs, esto sería: `(2 * 2) + 1 = 5` workers.
  * Esto significa que puedes tener **5 procesos de Python ejecutando tu código en paralelo**, aprovechando ambos núcleos de CPU.
* **Concurrencia por Worker:** Cada uno de estos 5 workers, gracias a `Uvicorn` y `asyncio`, puede manejar **muchas** solicitudes concurrentes que están esperando por I/O.

**Conclusión:** Tienes 5 "carriles" para el trabajo de CPU, y cada carril puede tener una larga "fila de coches" esperando (peticiones I/O).

**2. El Límite de la Memoria RAM (¡El Principal Sospechoso!)**

Este será tu principal cuello de botella.

* **Sistema Operativo:** \~0.5 - 1 GB
* **Elasticsearch (JVM):** ¡Es muy hambriento! Sin configuración, intentará usar hasta el 50% de la RAM del sistema. Es **crítico** limitar su consumo. Pongamos un límite de **3-4 GB** para la JVM.
* **Workers de FastAPI:** Cada proceso de Python consumirá RAM. Digamos 150 MB por worker × 5 workers = \~750 MB.
* **Cachés del Sistema, etc.:** \~0.5 GB

**Suma Rápida:** 1 (SO) + 4 (ES) + 0.75 (API) + 0.5 (Otros) = **6.25 GB**.

Esto te deja con un búfer de \~1.75 GB. Si Elasticsearch tiene un pico de uso o tu API carga datos grandes en memoria, podrías agotar la RAM, lo que haría que el sistema se vuelva extremadamente lento (swapping) y empiece a fallar.

**3. La Latencia Externa (El Gran Desconocido)**

* **Si las webs que scrapeas responden en 500 ms:** Tu API puede procesar muchas peticiones rápidamente.
* **Si las webs que scrapeas tardan 5 segundos en responder:** Tu servidor tendrá muchas conexiones abiertas esperando. Esto consume descriptores de archivo y un poco de memoria por conexión, pero la CPU estará mayormente libre. FastAPI maneja esto de maravilla.

#### **Estimación de Concurrencia (Back-of-the-Envelope)**

Basado en lo anterior, podemos hacer una estimación:

* **Escenario Pesimista (Webs lentas, consultas ES complejas):**
  * Cada uno de los 5 workers podría manejar unas 10-20 solicitudes concurrentes antes de que los recursos (especialmente la RAM para los buffers de conexión) comiencen a ser un problema.
  * **Total: 50 - 100 solicitudes concurrentes.**
* **Escenario Optimista (Webs rápidas, consultas ES simples):**
  * Cada worker podría manejar 40-50 solicitudes concurrentes.
  * **Total: 200 - 250 solicitudes concurrentes.**

Un punto medio realista y seguro para empezar a probar es **\~150 solicitudes concurrentes**.

#### **¿Cómo Encontrar el Número Real? ¡Test de Carga!**

La teoría es útil, pero solo los datos reales te darán la respuesta.

1.  **Configurar Gunicorn:** Inicia tu aplicación con el número correcto de workers.

    ```bash
    gunicorn -w 5 -k uvicorn.workers.UvicornWorker tu_app:app
    ```
2.  **Limitar la RAM de Elasticsearch:** ¡Esto es crucial! Al ejecutar tu contenedor de Docker, establece la variable de entorno para limitar la JVM.

    ```bash
    docker run -e ES_JAVA_OPTS="-Xms3g -Xmx3g" ... elasticsearch:version
    ```

    Esto fija la memoria de Elasticsearch a 3 GB, protegiendo al resto del sistema.
3. **Realizar un Test de Carga:** Usa herramientas como `locust`, `k6` o `JMeter` para simular usuarios concurrentes.
   * Empieza con 20 usuarios concurrentes y aumenta gradualmente.
   * **Monitoriza en tiempo real** en el servidor usando `htop` (para CPU y RAM) y `docker stats` (para ver el consumo de Elasticsearch).
4. **Busca el "Punto de Quiebre":** Verás uno de estos tres comportamientos cuando superes la capacidad:
   * **La latencia de respuesta se dispara:** Las peticiones tardan mucho más en completarse.
   * **La tasa de errores aumenta:** Comienzas a ver errores 5xx (Server Error).
   * **El uso de CPU se mantiene al 100%** o **la memoria RAM se agota**.

El número de usuarios concurrentes justo antes de que esto ocurra es tu capacidad máxima sostenible.
