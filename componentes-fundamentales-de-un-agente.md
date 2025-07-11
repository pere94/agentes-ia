# Componentes Fundamentales de un Agente

Independientemente de la arquitectura específica, la mayoría de los agentes comparten componentes conceptuales:

* **Percepción (Sensors):** Cómo el agente recibe información del entorno (datos de sensores, APIs, bases de datos, interacción humana).
* **Base de Conocimiento (Knowledge Base / Memoria):** Dónde el agente almacena información sobre el mundo, su estado interno, sus metas, y experiencias pasadas. Puede ser desde una simple base de datos hasta complejos grafos de conocimiento o embeddings vectoriales.
* **Motor de Razonamiento/Decisión (Reasoning/Decision Engine):** El "cerebro" del agente. Aquí es donde se procesa la información percibida y el conocimiento para decidir qué acción tomar. Puede usar lógica, reglas, aprendizaje automático, planificación, etc.
* **Mecanismo de Acción (Actuators/Effectors):** Cómo el agente interactúa y modifica su entorno (enviar comandos, llamar a APIs, generar texto, mover un robot).
* **(Opcional) Módulo de Aprendizaje:** Permite al agente mejorar su rendimiento con el tiempo basándose en la experiencia.
