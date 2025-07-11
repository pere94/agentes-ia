# Caso detallado

Objetivo principal:

* Necesidad de hacer un sistema de agentes IA que se encarguen de optimizar campañas de Shopping en Google Ads.

Agentes modulares que tendrá el sistema:

* Agente optimizador de presupuestos de campañas.
* Agente minero de palabras claves negativas
* Agentes encargados de estrategias (Campaña de producto ganador, ...)
* Agente optimizador de feed de producto...

Todos estos agentes deben estar conectando a un cerebro (agente orquestador padre, LLM openai, claude, gemini, o cualquier otro) y el mismo estará conectado a una fuente de datos donde se gestionaran logs de acciones, configuraciones, y todo dato que necesitemos para configurar, registrar, analizar y sacar conclusiones...

Este super agente debe encargarse de orquestar las tareas en le dia a dia y para ello tendrá a mano una serie de agentes ejecutores de tareas.
