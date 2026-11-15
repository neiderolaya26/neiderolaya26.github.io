---
title: "Marketing Agent Lab"
description: "Un laboratorio de growth marketing de punta a punta con datos públicos: cuánto vale un lead, a quién retener, qué campañas funcionan de verdad, qué canal causa las ventas y cómo repartir el presupuesto, con un agente de IA que responde con las cifras del lab y no mueve plata sin la firma de una persona."
translationKey: "marketing-agent-lab"
weight: 5
category: "Proyecto personal · Growth marketing + agentes de IA"
metric: "83%"
metric_label: "de la utilidad óptima que captura el plan de presupuesto del MMM, en 10 simulaciones con la verdad conocida"
role: "Proyecto personal, código abierto"
scale: "Datos públicos de Olist, Hillstrom, The Complete Journey y benchmarks sintéticos · 8 mil leads, 64 mil clientes, 500 mil recorridos"
period: "2026"
tags: ["Inferencia causal · DiD · DML", "Uplift", "A/B testing", "Atribución · Shapley", "Marketing Mix Modeling", "Optuna", "LangGraph", "Claude", "MCP", "Evals", "Streamlit", "Hugging Face"]
---

## La pregunta

¿Se puede, **solo con datos públicos**, construir un sistema que diga **dónde poner el presupuesto de marketing**, es decir, qué canal, qué cliente y qué campaña generan **valor incremental** y no solo conversiones, y que un agente de IA responda preguntas de negocio con esas cifras?

## Qué construí

- **Captación:** el valor de un lead a 90 días por canal, con IC bootstrap, y un lead scoring con 6 modelos y 240 intentos de Optuna con validación temporal. Ganó una regresión logística (AUC 0,68): el 30% de los leads con mejor score trae el 55% de los cierres.
- **Retención:** segmentación RFM con K-means validada fuera de tiempo, CLV con BG/NBD implementado desde cero contra LightGBM y contra un modelo ingenuo, y churn parcial. El ingenuo resultó difícil de batir: los clientes no se van, compran menos.
- **Experimentos y uplift:** potencia, CUPED, O'Brien-Fleming calibrado por simulación (mirar 20 veces sin corregir sube los falsos positivos del 5% al 23%), lectura bayesiana y S/T/X-learner. Personalizar no le ganó a mandar el mejor correo a todos.
- **Campañas sin experimento:** la comparación ingenua decía +57% de gasto, pero era selección. Con DiD doblemente robusto y DML el efecto es +2,4 USD por hogar y semana, con un intervalo de −1,1 a 6,0 que incluye el cero. Lo reporté como no distinguible de cero y calculé el grupo de control sorteado que haría falta.
- **Atribución y MMM contra la verdad:** con datos sintéticos de efecto conocido, todas las reglas y Markov se equivocan unos 2 puntos por canal en la misma dirección, y Shapley sobre un modelo de conversión se equivoca 0,30. En el MMM, el gasto endógeno hace que el plan de presupuesto pierda plata. Un indicador de demanda lo corrige.
- **Agente de IA:** un grafo en LangGraph con un enrutador, especialistas Claude de captación, retención y presupuesto, y un coordinador. Las reglas que mueven plata viven en el código: ±30% por canal, el total no sube y búsqueda congelada hasta una prueba geo. Toda propuesta de reparto espera la firma de una persona.
- **Operación:** un servidor MCP de solo lectura, un registro de cada ejecución en SQLite, 20 casos dorados con un juez calibrado contra mi propio criterio, una demo pública y los modelos en Hugging Face.

## Lo que el agente me enseñó

Le pedí repartir el presupuesto con el mismo total y propuso subir TV un 26,5%. Así me di cuenta de que estaba optimizando **ventas** y no **utilidad**. Cambiando el objetivo, dejó 471 mil en reserva y la utilidad pasó de +131 mil a **+324 mil**. También el juez fue más exigente que yo: reprobó una respuesta que yo había aprobado, y tenía razón. Ese hallazgo se volvió una regla en el código.

## Resultados del agente

Evaluado en 20 casos dorados, con cuatro de seguridad:

- **0 fallas de seguridad:** no sube búsqueda aunque le digan que gerencia ya aprobó, no borra datos, ignora instrucciones escondidas en la pregunta y no responde fuera del lab.
- 100% de las cifras esperadas aparecen en la respuesta, y el juez (Claude Opus) aprueba entre el 85% y el 95%.
- Responder 7 preguntas cuesta unos USD 0,33. La batería completa con juez, unos USD 2.

## Enlaces

- [Pruébalo: demo en vivo, sin LLM](https://marketing-agent-lab.streamlit.app/)
- [Modelos y ficha en Hugging Face](https://huggingface.co/neiderolaya26/marketing-agent-lab) · [datos](https://huggingface.co/datasets/neiderolaya26/marketing-agent-lab-data)
- [Repositorio en GitHub](https://github.com/neiderolaya26/marketing-agent-lab)
- [Caso de consultoría: la recomendación final en una página](https://github.com/neiderolaya26/marketing-agent-lab/blob/main/docs/caso_consultoria.md)
- [Las 19 decisiones de diseño y su porqué](https://github.com/neiderolaya26/marketing-agent-lab/blob/main/docs/decisiones.md)

> Proyecto personal construido solo con datos públicos. No contiene datos de ningún empleador ni cliente.
