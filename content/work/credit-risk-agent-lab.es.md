---
title: "Credit Risk Agent Lab"
description: "Un sistema de decisión de crédito de punta a punta con datos públicos de Lending Club: modelos de riesgo, prepago y pérdida, un motor de cupo y precio, una política de crédito en código y un agente de IA que analiza, opina y redacta el memo, mientras un analista firma todo lo que no es una aprobación clara."
translationKey: "credit-risk-agent-lab"
weight: 4
category: "Proyecto personal · Riesgo de crédito + agentes de IA"
metric: "0,6823"
metric_label: "AUC fuera de tiempo, frente a 0,6784 del score de Lending Club"
role: "Proyecto personal, código abierto"
scale: "Datos públicos de Lending Club · 566 mil préstamos"
period: "2026"
tags: ["LightGBM", "XGBoost", "PyTorch", "Análisis de supervivencia", "Inferencia causal · DML", "LangGraph", "Claude", "MCP", "Evals", "Streamlit", "Hugging Face"]
---

## La pregunta

¿Se puede, solo con un dataset público, construir de punta a punta un sistema que tome **mejores decisiones de aprobación, cupo y precio**, y que **automatice el análisis y la decisión** con modelamiento avanzado, LLM y agentes?

## Qué construí

- **Riesgo:** seis modelos (scorecard logístico, Random Forest, XGBoost, LightGBM y dos redes neuronales) ajustados con optimización bayesiana y validados fuera de tiempo. El campeón LightGBM recalibrado le gana al score de Lending Club solo con datos públicos de la solicitud (IC 95% de la diferencia de AUC [+0,0016; +0,0063]). Con la misma tasa de rechazo del 20%, suma **USD 337 mil de utilidad real por cada 10.000 solicitudes** (IC 95% [272; 408] mil) al no rechazar préstamos caros pero buenos.
- **Prepago y pérdida:** un modelo de supervivencia en tiempo discreto para el prepago y uno de pérdida económica; la LGD clásica no tuvo señal.
- **Cupo, precio y política:** un simulador de monto × tasa con cuota máxima del 20% del ingreso. La política aprueba con ROI esperado ≥ 4%; si no, hace una contraoferta, primero bajando el monto y después subiendo la tasa como máximo 2 puntos; si nada cumple, rechaza.
- **Inferencia causal:** efectos fijos de dos vías, una prueba placebo, wild cluster bootstrap y double machine learning sobre el efecto de la tasa en la mora. El placebo falló, así que el efecto no está identificado: lo reporté como resultado negativo y el estrés quedó como un supuesto con análisis de sensibilidad, no como una estimación.
- **Agente de IA:** la decisión la toma la política en código. Un grafo en LangGraph con tres especialistas Claude analiza el caso, un comité de IA da su opinión y redacta el memo, y un auditor lo revisa. Solo sale automática una aprobación en la que coinciden la política y el comité; lo demás, incluidos los desacuerdos, lo firma un analista, y la firma sobrevive aunque el proceso se reinicie.
- **Razones para el cliente:** un catálogo fijo de máximo 4 razones calculadas con TreeSHAP, separado del memo interno y sin variables que funcionan como proxy de edad.
- **Operación:** un servidor MCP con las herramientas del motor, un registro en SQLite de cada decisión con su resultado real, evals obligatorias en CI y tests en GitHub Actions. El motor corre en una demo pública y los modelos están publicados en Hugging Face con su ficha.

## Valor y equidad, medidos sin maquillaje

- **Backtest de 10.000 solicitudes de 2015:** la política aprueba 84,9%, contraoferta 13,8% y rechaza 1,3%. El 15,1% va a firma, entre 126 y 202 horas de analista.
- **P&L neto con costos dummy (ilustrativos, no reales):** la política pierde USD 0,8 M menos que aprobar todo, pero **no le gana a un corte simple por subgrado** con el mismo rechazo. Lo dejé documentado como límite en vez de esconderlo.
- **Equidad:** los datos públicos no traen sexo, edad ni raza, así que revisé proxies. La calibración queda dentro de ±0,7 puntos por grupo de vivienda y región, y quitar los proxies de edad cuesta solo 0,0032 de AUC.

## Resultados del agente

Evaluado en 35 préstamos con resultado real conocido, incluidos rechazos, y calificado con código:

- La opinión del comité coincide con la política en 34 de 35 casos (97%) y en 6 de 6 rechazos.
- Ningún borrador del comité viola una regla dura (0 de 37), el 94% de las cifras del memo están respaldadas por una herramienta y el 94% de las auditorías pasa al primer intento.
- La eval completa de 35 casos costó USD 0,29 con Claude Haiku. La mayor mejora vino de corregir el diseño, no de cambiar de modelo.

## Enlaces

- [Pruébalo: demo en vivo del motor de decisión](https://credit-risk-agent-lab.streamlit.app/)
- [Modelos y ficha en Hugging Face](https://huggingface.co/neiderolaya26/credit-risk-lending-club)
- [Repositorio en GitHub](https://github.com/neiderolaya26/credit-risk-agent-lab)
- [Notebook con la demo de punta a punta](https://github.com/neiderolaya26/credit-risk-agent-lab/blob/main/notebooks/11_demo_agente.ipynb)
- [Las 23 decisiones de diseño y su porqué](https://github.com/neiderolaya26/credit-risk-agent-lab/blob/main/docs/decisiones.md)

> Proyecto personal construido solo con datos públicos. No contiene datos de ningún empleador ni cliente.
