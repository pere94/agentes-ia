# DOC

en google ads, si tengo una campaña de shopping, si detecto algun producto que no esta funcionando bien, como quitarlo de esa campaña sin inhabilitarlo como producto o eliminarlo del feed ?=

***

¡Excelente pregunta! Es una situación muy común. Quieres optimizar tu campaña de Shopping sin afectar la disponibilidad general de tus productos.

La forma correcta de hacerlo es utilizando las **Exclusiones de Grupos de Productos** (o "Listing Groups" en campañas Performance Max) dentro de la propia campaña de Google Ads.

Aquí te explico cómo:

**Opción 1: Excluir productos individualmente (por ID de artículo)**

Esta es la forma más precisa si tienes unos pocos productos específicos.

1. **Ve a tu campaña de Shopping en Google Ads.**
2. **Selecciona el Grupo de Anuncios** que contiene los productos (en campañas de Shopping Estándar. En Performance Max, irás directamente a los Grupos de Recursos o "Listing groups").
3. **Haz clic en "Grupos de Productos"** (o "Listing groups" en PMax) en el menú de la izquierda.
4. Verás un grupo llamado "Todos los productos" (o el nombre de tu subdivisión principal).
5. **Haz clic en el icono "+" o el lápiz (editar) al lado de "Todos los productos"** (o la subdivisión donde se encuentre el producto). Se abrirá una ventana para subdividir este grupo.
6. En la ventana de subdivisión, **selecciona "ID de artículo"** en el menú desplegable "Subdividir \[nombre del grupo] por:".
7. **Busca y selecciona los IDs de los productos** que quieres excluir. Puedes pegar una lista de IDs.
8. **Haz clic en "Guardar sin editar pujas"** o "Continuar para editar pujas".
9. Ahora verás que se han creado grupos de productos individuales para cada uno de esos IDs, y también un grupo de "Todo lo demás en '\[nombre del grupo original]'".
10. **Encuentra los grupos de productos recién creados para los IDs específicos.** En la columna "CPC máx." (o la estrategia de puja que estés usando), **haz clic en el valor de la puja (o el guion) y selecciona "Excluido"** en el menú desplegable o la opción que aparezca.
11. **Asegúrate de que el grupo "Todo lo demás..." SÍ tenga una puja activa** para que el resto de tus productos sigan publicitándose.

**Opción 2: Usar Etiquetas Personalizadas (Custom Labels) para grupos de productos**

Si son muchos productos o quieres una forma más dinámica de gestionarlos, puedes usar etiquetas personalizadas en tu feed de Merchant Center y luego excluir basándote en esa etiqueta.

1. **En tu feed de productos (Google Merchant Center):**
   * Añade o modifica una columna de etiqueta personalizada (ej. custom\_label\_0).
   * Asigna un valor específico (ej. "bajo\_rendimiento" o "excluir\_shopping\_X") a los productos que quieres excluir de esa campaña.
   * Vuelve a procesar tu feed en Merchant Center. Espera a que se actualice.
2. **En tu campaña de Shopping en Google Ads:**
   * Ve a "Grupos de Productos" (o "Listing groups").
   * Haz clic en el "+" o el lápiz al lado de "Todos los productos" (o la subdivisión relevante).
   * Subdivide por la etiqueta personalizada que usaste (ej. "Etiqueta personalizada 0").
   * Se crearán grupos para cada valor de esa etiqueta (ej. uno para "bajo\_rendimiento" y otro para "Todo lo demás en 'Etiqueta personalizada 0'").
   * Encuentra el grupo de productos con el valor de la etiqueta que quieres excluir (ej. "bajo\_rendimiento") y en la columna de puja, selecciona "Excluido".
   * Asegúrate de que los otros grupos (incluyendo "Todo lo demás...") tengan pujas activas.

**Puntos Clave:**

* **No afecta al Merchant Center:** Al hacer esto, el producto sigue activo en tu Merchant Center, disponible para otras campañas (si las tienes) y no se elimina del feed. Simplemente no se mostrará en esta campaña específica.
* **Control granular:** Puedes ser muy específico con qué productos se muestran.
* **Revisión:** Después de hacer los cambios, espera un poco y revisa la pestaña "Productos" dentro de tu campaña para asegurarte de que los productos ya no aparecen como "Aptos" o "Activos" para esa campaña, sino que podrían mostrarse como "Excluidos" o simplemente no listar.

De esta manera, mantienes tu feed intacto y tus productos disponibles, pero controlas finamente qué se anuncia en cada campaña de Shopping.

***

si lo hago con el api de google ads con los label ?



Sí, puedes lograr esto usando la API de Google Ads y las **Etiquetas Personalizadas** (custom\_label\_0 a custom\_label\_4) que configuras en tu feed de Google Merchant Center.

La idea general es:

1. **En Google Merchant Center (fuera de la API):**
   * Asegúrate de que los productos que quieres excluir tengan un valor específico en una de tus columnas de etiqueta personalizada (ej. custom\_label\_0 = "excluir\_campaña\_X").
   * Este paso es crucial y se hace directamente en tu feed o a través de las reglas de feed en Merchant Center. La API de Google Ads lee esta información, no la establece en el feed.
2. **En la API de Google Ads:**
   * Identificarás el grupo de anuncios de tu campaña de Shopping.
   * Modificarás la estructura de los **Grupos de Productos** (AdGroupCriterion con ListingGroupInfo) de ese grupo de anuncios.
   * Subdividirás el grupo de productos "Todos los productos" (o un subgrupo existente) utilizando la dimensión de la etiqueta personalizada que has configurado (ej. ProductCustomAttribute.index = PRODUCT\_CUSTOM\_ATTRIBUTE\_INDEX\_0).
   * Esto creará nodos hijos: uno para cada valor único de esa etiqueta personalizada (incluyendo tu valor "excluir\_campaña\_X") y un nodo para "Todo lo demás".
   * Al nodo hijo que representa el valor de la etiqueta "excluir\_campaña\_X" **no le asignarás una puja** (o si tenía una, la eliminarás/pondrás a cero). En la interfaz de Google Ads, esto se mostrará como "Excluido".
   * Te asegurarás de que el nodo "Todo lo demás" (y otros nodos que quieras mantener activos) tengan una puja o hereden la estrategia de puja.

**Pasos conceptuales con la API (usando la biblioteca cliente de Python como ejemplo):**

Supongamos que quieres excluir productos con custom\_label\_0 = "no\_mostrar\_api" del grupo de anuncios con ad\_group\_id en la campaña con campaign\_id.

1.  **Obtener el cliente de la API y los servicios necesarios:**

    ```
    from google.ads.googleads.client import GoogleAdsClient
    from google.ads.googleads.errors import GoogleAdsException

    client = GoogleAdsClient.load_from_storage("path/to/your/google-ads.yaml")
    ad_group_criterion_service = client.get_service("AdGroupCriterionService")
    # Necesitarás el customer_id y el ad_group_id
    customer_id = "YOUR_CUSTOMER_ID"
    ad_group_id = "YOUR_AD_GROUP_ID"
    ad_group_resource_name = client.get_service("AdGroupService").ad_group_path(customer_id, ad_group_id)
    ```

    content\_copydownloadUse code [with caution](https://support.google.com/legal/answer/13505487).Python
2.  **Identificar el nodo raíz del grupo de productos (normalmente "Todos los productos"):**

    * Necesitarás encontrar el criterion\_id del nodo raíz del grupo de productos para poder subdividirlo. Si no ha sido subdividido antes, suele haber solo uno y su listing\_group.parent\_criterion\_id es nulo.
    * Si ya está subdividido, tendrás que navegar la jerarquía o encontrar el nodo específico que quieres subdividir.

    ```
    # Esta es una forma simplificada de encontrar el nodo raíz.
    # En un escenario real, podrías necesitar una consulta más específica.
    query = f"""
        SELECT ad_group_criterion.criterion_id
        FROM ad_group_criterion
        WHERE ad_group_criterion.type = LISTING_GROUP
        AND ad_group_criterion.listing_group.parent_ad_group_criterion IS NULL
        AND ad_group_criterion.ad_group = '{ad_group_resource_name}'
        LIMIT 1
    """
    ga_service = client.get_service("GoogleAdsService")
    stream = ga_service.search_stream(customer_id=customer_id, query=query)
    root_criterion_id = None
    for batch in stream:
        for row in batch.results:
            root_criterion_id = row.ad_group_criterion.criterion_id
            break
        if root_criterion_id:
            break

    if not root_criterion_id:
        raise Exception("No se pudo encontrar el nodo raíz del grupo de productos.")

    root_criterion_resource_name = ad_group_criterion_service.ad_group_criterion_path(
        customer_id, ad_group_id, root_criterion_id
    )
    ```

    content\_copydownloadUse code [with caution](https://support.google.com/legal/answer/13505487).Python
3.  **Crear las operaciones de mutación:**

    * **Operación 1: Modificar el nodo raíz (o el nodo padre elegido) para que sea una SUBDIVISIÓN** basada en custom\_label\_0.
    * **Operación 2: Crear un nuevo nodo hijo (UNIT) para el valor de la etiqueta que quieres excluir** (ej. "no\_mostrar\_api"). Este nodo no tendrá una puja (o cpc\_bid\_micros = 0).
    * **Operación 3: Crear un nuevo nodo hijo (UNIT) para "Todo lo demás"** en custom\_label\_0. Este nodo sí tendrá una puja o usará la puja del grupo de anuncios/campaña.

    ```
    operations = []

    # --- Operación 1: Modificar el nodo padre para ser una subdivisión ---
    # (Asumimos que root_criterion_resource_name es el que queremos subdividir)
    # Y que antes era una UNIT (unidad)
    parent_update_operation = client.get_type("AdGroupCriterionOperation")
    parent_criterion_to_update = parent_update_operation.update
    parent_criterion_to_update.resource_name = root_criterion_resource_name
    parent_criterion_to_update.listing_group.type_ = client.enums.ListingGroupTypeEnum.SUBDIVISION
    # Especifica la dimensión de la subdivisión: custom_label_0
    parent_criterion_to_update.listing_group.case_value.product_custom_attribute.index = client.enums.ProductCustomAttributeIndexEnum.PRODUCT_CUSTOM_ATTRIBUTE_INDEX_0
    # Quitar la puja si la tenía (las subdivisiones no pujan)
    # parent_criterion_to_update.cpc_bid_micros = 0 # Si estaba con CPC manual
    update_mask = client.field_mask(None, parent_criterion_to_update._pb)
    parent_update_operation.update_mask.CopyFrom(update_mask)
    operations.append(parent_update_operation)


    # --- Operación 2: Crear el nodo para el valor a excluir (SIN PUJA) ---
    # Usaremos un ID temporal negativo para referenciarlo si es necesario
    excluded_criterion_id = -1
    excluded_value_operation = client.get_type("AdGroupCriterionOperation")
    excluded_criterion = excluded_value_operation.create
    excluded_criterion.ad_group = ad_group_resource_name
    excluded_criterion.status = client.enums.AdGroupCriterionStatusEnum.ENABLED # Importante
    # excluded_criterion.criterion_id = excluded_criterion_id # ID temporal
    excluded_criterion.listing_group.type_ = client.enums.ListingGroupTypeEnum.UNIT
    excluded_criterion.listing_group.parent_ad_group_criterion = root_criterion_resource_name # Padre
    excluded_criterion.listing_group.case_value.product_custom_attribute.index = client.enums.ProductCustomAttributeIndexEnum.PRODUCT_CUSTOM_ATTRIBUTE_INDEX_0
    excluded_criterion.listing_group.case_value.product_custom_attribute.value = "no_mostrar_api" # El valor de tu etiqueta
    # NO se establece cpc_bid_micros, lo que lo marca como excluido.
    operations.append(excluded_value_operation)

    # --- Operación 3: Crear el nodo para "Todo lo demás" (CON PUJA o heredada) ---
    other_value_criterion_id = -2
    other_value_operation = client.get_type("AdGroupCriterionOperation")
    other_criterion = other_value_operation.create
    other_criterion.ad_group = ad_group_resource_name
    other_criterion.status = client.enums.AdGroupCriterionStatusEnum.ENABLED
    # other_criterion.criterion_id = other_value_criterion_id # ID temporal
    other_criterion.listing_group.type_ = client.enums.ListingGroupTypeEnum.UNIT
    other_criterion.listing_group.parent_ad_group_criterion = root_criterion_resource_name # Padre
    # Para "Todo lo demás", NO se establece case_value.index ni case_value.value
    # Si usas CPC manual, establece una puja. Si no, heredará.
    # other_criterion.cpc_bid_micros = 500000  # Ejemplo: 0.50 de tu moneda
    operations.append(other_value_operation)
    ```

    content\_copydownloadUse code [with caution](https://support.google.com/legal/answer/13505487).Python
4.  **Ejecutar la mutación:**

    ```
    try:
        response = ad_group_criterion_service.mutate_ad_group_criteria(
            customer_id=customer_id, operations=operations
        )
        print("Grupos de productos modificados exitosamente:")
        for result in response.results:
            print(f"\tResource name: {result.resource_name}")
    except GoogleAdsException as ex:
        print(
            f"La solicitud falló con estado '{ex.error.code().name}' y errores:"
        )
        for error in ex.failure.errors:
            print(f"\tError: {error.message}")
            if error.location:
                for field_path_element in error.location.field_path_elements:
                    print(f"\t\tEn campo: {field_path_element.field_name}")
    ```

    content\_copydownloadUse code [with caution](https://support.google.com/legal/answer/13505487).Python

**Consideraciones Importantes:**

* **IDs Temporales:** Cuando creas una jerarquía (un padre subdividido y sus hijos) en una sola llamada mutate, necesitas usar IDs de criterio temporales (negativos) para que los hijos puedan referenciar al padre que se está creando/modificando en la misma solicitud. El ejemplo anterior es simplificado y asume que el padre ya existe y solo se actualiza. Si creas la subdivisión y sus hijos de golpe, el padre también debe tener un ID temporal y los hijos referenciarlo.
* **Jerarquía Existente:** Si ya tienes una estructura de grupos de productos compleja, necesitarás más lógica para encontrar el parent\_ad\_group\_criterion correcto y asegurarte de no romper otras subdivisiones. A menudo, es mejor obtener la jerarquía existente primero.
* **Estrategias de Puja:** Si usas estrategias de puja inteligente (ej. Maximizar Clics, ROAS Objetivo), no establecerás cpc\_bid\_micros directamente en los nodos UNIT. El hecho de no tener una puja o ser "Excluido" seguirá funcionando para que no se muestren esos productos.
* **custom\_label\_0 a custom\_label\_4:** Puedes usar cualquiera de las 5 etiquetas personalizadas. Asegúrate de que el ProductCustomAttributeIndexEnum coincida (PRODUCT\_CUSTOM\_ATTRIBUTE\_INDEX\_0 para custom\_label\_0, \_INDEX\_1 para custom\_label\_1, etc.).
* **Validación:** Siempre prueba esto en una campaña de prueba o con un pequeño conjunto de productos primero.
* **Eliminación de la exclusión:** Para volver a incluir los productos, podrías eliminar el nodo específico de la etiqueta excluida y el nodo "Todo lo demás", y volver a convertir el nodo padre en una UNIT con una puja (o simplemente darle una puja al nodo que estaba excluido si quieres mantener la subdivisión por otros motivos).

Este enfoque te da un control programático preciso sobre qué productos se anuncian en tus campañas de Shopping sin afectar su estado en Merchant Center.

***

como se puede hacer mas sencillo, es que veo muchas operaciones y en mi cabeza pienso que modificando solo el label 0 ejemplo y teniendo una regla que las campañas eviten ese valor del label pudiera funcionar no ?



Tienes toda la razón en buscar una forma más conceptual y sencilla de entenderlo. ¡Vamos a simplificarlo!

La idea que tienes es la correcta en esencia: **usas una etiqueta (label) en tu feed para "marcar" los productos, y luego en Google Ads le dices a la campaña que "ignore" o "no puje por" los productos marcados con esa etiqueta específica.**

La "complicación" con la API surge porque la API necesita que le describas _exactamente_ cómo quieres que se vea la estructura de los grupos de productos para lograr esa "ignorancia". No hay un único comando de "excluir por valor de etiqueta X en toda la campaña".

**Piénsalo así, de forma simplificada (conceptual y manualmente):**

1. **En tu feed de productos (Merchant Center):**
   * Identificas el producto "ABC" que no funciona bien.
   * Vas a la columna `custom_label_0` (o la que elijas) para ese producto "ABC".
   * Le pones el valor: `excluir_de_campaña_principal`.
   * Guardas y reenvías tu feed. _Este es el "marcado"_.
2. **En Google Ads (la "regla" se aplica aquí, dentro de la campaña):**
   * Vas a tu campaña de Shopping -> Grupo de Anuncios -> Grupos de Productos.
   * Inicialmente, podrías tener solo "Todos los productos".
   * **Acción Clave:** Subdivides "Todos los productos" **por `custom_label_0`**.
     * Esto crea automáticamente "ramas" o "cubos" para cada valor único que tengas en `custom_label_0` en tu feed.
     * Por ejemplo, se creará:
       * Un grupo para `custom_label_0 = excluir_de_campaña_principal`
       * Un grupo para `custom_label_0 = super_ventas` (si tienes esa etiqueta)
       * Un grupo para `custom_label_0 =` (productos sin valor en esa etiqueta)
       * Un grupo para "Todo lo demás en `custom_label_0`" (para valores no explícitamente separados o productos nuevos).
   * **Acción de Exclusión:** Ahora, encuentras la rama/cubo que dice `custom_label_0 = excluir_de_campaña_principal` y, en lugar de ponerle una puja, la marcas como **"Excluido"**.
   * El resto de las ramas/cubos SÍ tendrán pujas.

**¿Por qué la API parece tener "muchas operaciones"?**

Porque la API necesita que le digas explícitamente:

1. "Toma el grupo de productos 'Todos los productos' (o el que sea)".
2. "Cambia su tipo de 'Unidad' (puja por todo) a 'Subdivisión'".
3. "Dile que la subdivisión se basará en la dimensión `custom_label_0`".
4. "Ahora, crea un nuevo grupo de productos hijo para el valor `excluir_de_campaña_principal` de `custom_label_0`. Este hijo es una 'Unidad'".
5. "A este hijo `excluir_de_campaña_principal`, NO le pongas puja (o ponle puja cero, que es como se excluye)".
6. "Crea otro grupo de productos hijo para 'Todo lo demás' bajo `custom_label_0`. Este también es una 'Unidad'".
7. "A este hijo 'Todo lo demás', SÍ ponle una puja (o deja que herede la del grupo de anuncios)".

**La simplificación si la estructura ya existe:**

* **Si ya tienes tus productos subdivididos por `custom_label_0` en Google Ads:**
  * Entonces, cuando añades `excluir_de_campaña_principal` a un nuevo producto en tu feed, Google Ads automáticamente lo meterá en el "cubo" o grupo de productos que corresponde a ese valor de etiqueta.
  * Si ese cubo ya está configurado como "Excluido" en Google Ads, ¡listo! No necesitas hacer nada más en la API de Google Ads para _ese producto nuevo_. La "regla" ya está vigente.
  * Solo necesitarías usar la API (o la interfaz) la _primera vez_ para crear esa estructura de subdivisión y marcar el cubo de la etiqueta como excluido.

**En resumen:**

* **Sí, tu idea es correcta:** Marcas con una etiqueta en el feed.
* **La "regla" es la estructura de grupos de productos en Google Ads:** Debes tener una subdivisión por esa etiqueta, y el grupo específico para el valor de "excluir" debe estar configurado como "Excluido" (sin puja).
* **La API detalla los pasos para construir o modificar esa estructura.** Si la estructura ya existe y es correcta, solo necesitas modificar la etiqueta en el feed.

**¿Cómo hacerlo más "sencillo" con la API si la estructura no existe?**

Desafortunadamente, la creación inicial de esa estructura de subdivisión y la asignación de la exclusión al grupo de etiquetas deseado requiere esas operaciones que parecen múltiples. No hay un atajo en la API para decir "si un producto tiene `custom_label_0 = 'X'`, exclúyelo de esta campaña" sin antes definir esa estructura de grupos de productos que usa `custom_label_0` como criterio de subdivisión.

Lo que puedes hacer es tener una **función reutilizable en tu código API** que:

1. Compruebe si ya existe la subdivisión por `custom_label_0`.
2. Si no existe, la cree (los pasos de subdividir el padre, crear el hijo para el valor a excluir, crear el hijo para "todo lo demás").
3. Asegure que el grupo de productos para el valor de la etiqueta a excluir esté "Excluido".

Así, solo llamas a esa función cuando sea necesario, y ella se encarga de la "complejidad".

***

como lo haria en el interfaz para ver ?



¡Claro! Hacerlo en la interfaz de Google Ads es bastante intuitivo. Así es como aplicarías la "regla" de exclusión basada en una etiqueta personalizada (o por ID de artículo).

**Asumamos que ya has hecho esto en tu feed de Google Merchant Center:**

* Has identificado los productos que no rinden bien.
* A estos productos, en tu feed, les has asignado un valor específico en una etiqueta personalizada. Por ejemplo, en `custom_label_0` les has puesto el valor `bajo_rendimiento_shopp`.
* Has vuelto a procesar tu feed en Merchant Center y está actualizado.

**Ahora, en la interfaz de Google Ads:**

**Método 1: Usar Etiquetas Personalizadas (Recomendado para grupos de productos)**

1. **Ve a tu cuenta de Google Ads.**
2. En el menú de navegación de la izquierda, haz clic en **"Campañas"**.
3. **Selecciona la campaña de Shopping** específica donde quieres excluir los productos.
4. Dentro de la campaña, en el menú de la izquierda, selecciona el **"Grupo de anuncios"** relevante. (Si es una campaña Performance Max, irás a "Grupos de recursos" y luego a "Listing groups").
5.  En el menú de la izquierda del grupo de anuncios, haz clic en **"Grupos de productos"**. (En Performance Max, esto se llama "Listing groups" y puede estar un poco más anidado, a menudo dentro de un grupo de recursos).

    ![Product Groups Menu](https://i.imgur.com/1fV8YyM.png) _(Imagen ilustrativa de dónde encontrar "Grupos de productos")_
6. Verás un grupo de productos llamado **"Todos los productos"** (o el nombre de tu subdivisión principal si ya tienes alguna).
7.  **Pasa el ratón por encima de "Todos los productos"** (o el grupo que quieras subdividir) y aparecerá un **icono de lápiz (Editar)** o un **signo más (+)**. Haz clic en él.

    ![Subdivide Product Group](https://i.imgur.com/oX23PjY.png) _(Imagen ilustrativa del icono para subdividir)_
8. Se abrirá una ventana para subdividir este grupo. En el menú desplegable **"Subdividir \[nombre del grupo] por:"**, elige la etiqueta personalizada que configuraste en tu feed. Por ejemplo, **"Etiqueta personalizada 0"**.
9. Google Ads te mostrará los valores que ha detectado para "Etiqueta personalizada 0" en tu feed.
   * **Marca la casilla** junto al valor que quieres excluir (ej. `bajo_rendimiento_shopp`).
   * **¡Muy importante!** Asegúrate de que también esté marcada la casilla para **"Todo lo demás en 'Etiqueta personalizada 0'"** (o el nombre de tu etiqueta). Esto asegura que los productos que _no_ tienen esa etiqueta sigan activos.
10. Haz clic en **"Guardar"** o "Guardar sin editar pujas".
11. Ahora, en tu lista de grupos de productos, verás nuevas filas:
    * Una fila para `Etiqueta personalizada 0 = bajo_rendimiento_shopp`
    * Una fila para `Todo lo demás en 'Etiqueta personalizada 0'`
12. **Encuentra la fila** correspondiente a `Etiqueta personalizada 0 = bajo_rendimiento_shopp`.
13. En la columna **"CPC máx."** (o la columna de puja, dependiendo de tu estrategia de puja):

    * Haz clic en el valor de la puja actual (o el guion "-").
    * En el menú desplegable o la opción que aparezca, selecciona **"Excluido"**.

    ![Exclude Product Group](https://i.imgur.com/42iB1A3.png) _(Imagen ilustrativa de cómo seleccionar "Excluido")_
14. Haz clic en **"Guardar"**.

¡Listo! Los productos que tengan `custom_label_0 = bajo_rendimiento_shopp` en tu feed ya no se publicarán en _esta_ campaña específica. Los demás productos (en "Todo lo demás...") seguirán funcionando con su puja normal.

**Método 2: Excluir Productos Individualmente por ID de Artículo (para pocos productos)**

Si solo son uno o dos productos, puedes hacerlo por ID directamente:

1. Sigue los pasos 1-7 del método anterior.
2. En el menú desplegable **"Subdividir \[nombre del grupo] por:"**, elige **"ID de artículo"**.
3. Busca y selecciona los IDs de los productos que quieres excluir. Puedes pegar una lista de IDs si los tienes. Marca las casillas correspondientes.
4. **¡Muy importante!** Asegúrate de que también esté marcada la casilla para **"Todo lo demás en '\[nombre del grupo original]'"**.
5. Haz clic en **"Guardar"**.
6. Ahora verás filas para cada ID de artículo que seleccionaste y una para "Todo lo demás".
7. En la fila de cada ID de artículo que quieres excluir, ve a la columna "CPC máx." y selecciona **"Excluido"**.
8. Haz clic en **"Guardar"**.

**Verificación:**

Después de unos minutos, puedes ir a la pestaña **"Productos"** (dentro de tu campaña o grupo de anuncios) y filtrar por los IDs de los productos que excluiste. Deberías ver que su estado para esa campaña ha cambiado (podría no aparecer, o indicar que no es apto debido a la exclusión del grupo de productos).

Usar las etiquetas personalizadas es más escalable si sueles hacer esto con frecuencia o con muchos productos. El método por ID es más rápido para exclusiones puntuales.

***

y como se onfiguran las campañas para que cumplan con esto ? par que quede simplificada la ejecucion ?



¡Entendido! Quieres una configuración de campaña que, una vez hecha, te permita gestionar las exclusiones de productos de forma muy sencilla, idealmente solo modificando la etiqueta en tu feed.

Aquí está la estrategia para configurar tus campañas de Shopping (Estándar o Performance Max con feeds) para que esto funcione así:

**La Clave: Estructura de Grupos de Productos Predefinida**

La idea es configurar tu campaña de Google Ads **una vez** para que "escuche" un valor específico de una etiqueta personalizada. Cuando un producto en tu feed reciba esa etiqueta, automáticamente caerá en un grupo de productos "excluido" dentro de la campaña.

***

**Paso 1: Decide tu Estrategia de Etiquetado (En tu Feed de Merchant Center)**

1. **Elige una Etiqueta Personalizada:** Decide cuál de tus `custom_label_0` a `custom_label_4` usarás para este propósito. Por ejemplo, usemos `custom_label_0`.
2.  **Define un Valor de Exclusión:** Elige un valor claro y consistente que significará "excluir de esta/s campaña/s".

    * Ejemplo: `excluir_shopping_principal`
    * Ejemplo: `bajo_rendimiento`
    * Ejemplo: `no_anunciar_aqui`

    _Este valor lo añadirás a los productos en tu feed cuando quieras excluirlos._

***

**Paso 2: Configura tu Campaña de Google Ads (Una Sola Vez por Campaña)**

Esto lo harás en la interfaz de Google Ads.

1. **Ve a tu Campaña de Shopping:**
   * Selecciona la campaña específica.
   * Ve al **Grupo de Anuncios** correspondiente.
   * Navega a **"Grupos de productos"**. (En PMax: "Grupos de recursos" > selecciona el grupo de recursos > "Listing groups").
2. **Subdivide por tu Etiqueta Personalizada Elegida:**
   * Si solo tienes "Todos los productos", haz clic en el icono `+` o el lápiz para subdividirlo.
   * **"Subdividir por":** Elige la etiqueta personalizada que decidiste en el Paso 1 (ej. `Etiqueta personalizada 0`).
   * Google Ads te mostrará los valores que encuentra actualmente en tu feed para esa etiqueta. **No te preocupes si tu "valor de exclusión" (ej. `excluir_shopping_principal`) aún no aparece porque ningún producto lo tiene todavía.**
   * **Importante:** Asegúrate de que la opción para crear un grupo para **"Todo lo demás en 'Etiqueta personalizada 0'"** esté seleccionada (o añádela manualmente si es necesario). Este es el grupo que contendrá todos los productos que _no_ tienen tu valor de exclusión y que _sí_ quieres anunciar.
   * Haz clic en **"Guardar"**.
3. **Crea y Excluye el Grupo para tu Valor de Exclusión (Incluso si está vacío inicialmente):**
   * Después de la subdivisión anterior, puede que no veas un grupo para `excluir_shopping_principal` si ningún producto tiene esa etiqueta _ahora mismo_.
   * Vuelve a hacer clic en el `+` junto al grupo padre (el que subdividiste, que ahora es `Etiqueta personalizada 0`).
   * Se te ofrecerá añadir más subdivisiones para `Etiqueta personalizada 0`.
   * Busca una opción para **"Añadir un grupo de productos para todo lo que no está subdividido" O una opción para "Añadir subdivisión para un valor específico"**.
   * Si puedes añadir un valor específico, escribe tu valor de exclusión: `excluir_shopping_principal`.
   * Selecciónalo y haz clic en **"Guardar"** o "Continuar para editar pujas".
   * Ahora tendrás un grupo de productos específico para `Etiqueta personalizada 0 = excluir_shopping_principal`.
   * En la columna de puja ("CPC máx." o similar) para este grupo `excluir_shopping_principal`, haz clic y selecciona **"Excluido"**.
   * Haz clic en **"Guardar"**.
4. **Asegura la Puja para "Todo lo demás":**
   * Verifica que el grupo **"Todo lo demás en 'Etiqueta personalizada 0'"** (o como se llame tu grupo catch-all para esa etiqueta) tenga una **puja activa**. Si no, tus otros productos no se mostrarán.

**Resumen de la Estructura de Grupos de Productos Resultante (Ejemplo):**

* Etiqueta personalizada 0 (Subdivisión principal)
  * `excluir_shopping_principal` -> **Excluido**
  * `valor_etiqueta_A` (si existe) -> Con puja
  * `valor_etiqueta_B` (si existe) -> Con puja
  * `Todo lo demás en 'Etiqueta personalizada 0'` -> **Con puja** (¡Muy importante!)

***

**Paso 3: Ejecución Simplificada (Día a Día)**

¡Aquí es donde se vuelve fácil!

1. **Identificas un producto que no funciona bien.**
2. **Vas a tu feed de productos (en Merchant Center o donde lo gestiones):**
   * Encuentras el producto.
   * En la columna `custom_label_0` (o la que elegiste), le pones el valor: `excluir_shopping_principal`.
3. **Guardas y reenvías/actualizas tu feed en Merchant Center.**
4. **Espera a que Google procese el feed y sincronice con Google Ads** (puede tardar desde minutos hasta unas horas).

**¿Qué sucede automáticamente?**

* Google Ads verá que el producto ahora tiene `custom_label_0 = excluir_shopping_principal`.
* Automáticamente moverá ese producto de "Todo lo demás en 'Etiqueta personalizada 0'" (donde tenía puja) al grupo de productos `Etiqueta personalizada 0 = excluir_shopping_principal`.
* Como ese grupo está configurado como "Excluido", el producto dejará de mostrarse en los anuncios de esa campaña.

**Para volver a incluir un producto:**

1. En tu feed, cambia el valor de `custom_label_0` del producto (ej. quítale `excluir_shopping_principal` o ponle otro valor).
2. Actualiza tu feed.
3. El producto volverá al grupo "Todo lo demás..." (o al grupo correspondiente a su nueva etiqueta) y se volverá a anunciar si ese grupo tiene una puja.

***

**Ventajas de esta Configuración:**

* **Manejo Centralizado:** Controlas las exclusiones desde tu feed.
* **Simplicidad Operativa:** Una vez configurada la campaña, solo necesitas editar etiquetas en el feed.
* **Escalabilidad:** Fácil de aplicar a muchos productos.
* **Consistencia:** Puedes usar la misma lógica de etiquetas en múltiples campañas (solo necesitas configurar cada campaña una vez).

Esta es la forma más simplificada y eficiente de gestionar exclusiones de productos a largo plazo sin tocar constantemente la configuración de la campaña en Google Ads después de la configuración inicial.
