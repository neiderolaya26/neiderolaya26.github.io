---
title: "Credit Risk Agent Lab"
description: "Un sistema de decisión de crédito de punta a punta con datos públicos de Lending Club: modelos de riesgo, prepago y pérdida, un motor de cupo y precio probado con inferencia causal, y un agente de IA que decide, se audita y redacta el memo de crédito sin intervención humana."
translationKey: "credit-risk-agent-lab"
weight: 4
category: "Proyecto personal · Riesgo de crédito + agentes de IA"
metric: "0,6823"
metric_label: "AUC fuera de tiempo, frente a 0,6784 del score de Lending Club"
role: "Proyecto personal, código abierto"
scale: "Datos públicos de Lending Club · 566 mil préstamos"
period: "2026"
tags: ["LightGBM", "XGBoost", "PyTorch", "Análisis de supervivencia", "Inferencia causal · DML", "LangGraph", "Claude", "MCP", "LangSmith", "Evals"]
---

## La pregunta

¿Se puede, solo con un dataset público, construir de punta a punta un sistema que tome **mejores decisiones de aprobación, cupo y precio**, y que **automatice la decisión** con modelamiento avanzado, LLM y agentes?

## Qué construí

- **Riesgo:** seis modelos (scorecard logístico, Random Forest, XGBoost, LightGBM y dos redes neuronales) ajustados con optimización bayesiana y validados fuera de tiempo. El campeón LightGBM recalibrado le gana al score de Lending Club solo con datos públicos de la solicitud (IC 95% de la diferencia de AUC [+0,0016; +0,0063]).
- **Aprobación:** una política por utilidad esperada que mantiene la utilidad de un corte por PD con mejor ROI (6,30%) y menos mora.
- **Prepago y pérdida:** un modelo de supervivencia en tiempo discreto para el prepago y uno de pérdida económica; la LGD clásica no tuvo señal.
- **Cupo y precio:** un simulador de monto × tasa con cuota máxima del 20% del ingreso. Los préstamos que Lending Club cobró por debajo de la tasa sugerida rindieron 2,8% frente a 6,7% del resto.
- **Inferencia causal:** efectos fijos de dos vías, una prueba placebo y double machine learning sobre el efecto de la tasa en la mora. La relación tasa–mora resultó ser selección y el placebo falló, así que reporté el resultado negativo y agregué un escenario de estrés en vez de un efecto causal que no estaba identificado.
- **Agente de IA:** un grafo en LangGraph con tres especialistas Claude en paralelo, un comité que decide, reglas duras en código y un auditor de IA que devuelve el caso si un número o la política no cuadran. Cada cifra del memo sale de una herramienta que corre los modelos.
- **Operación:** un servidor MCP con las herramientas del motor, un registro en SQLite de cada ejecución con su resultado real, trazas en LangSmith y evals.

## Resultados del agente

Evaluado en 30 préstamos con resultado real conocido, calificado con código y con un modelo juez distinto:

- 100% de cumplimiento de las reglas duras y 94% de las cifras del memo respaldadas por una herramienta.
- 93% de auditorías aprobadas al primer intento y 97% en la rúbrica del juez.
- USD 0,007 por decisión con Claude Haiku. La mayor mejora vino de corregir el diseño (la misma política y los mismos datos para el comité y el auditor), no de cambiar de modelo.

## Enlaces

- [Repositorio en GitHub](https://github.com/neiderolaya26/credit-risk-agent-lab)
- [Notebook con la demo de punta a punta](https://github.com/neiderolaya26/credit-risk-agent-lab/blob/main/notebooks/11_demo_agente.ipynb)
- [Las 20 decisiones de diseño y su porqué](https://github.com/neiderolaya26/credit-risk-agent-lab/blob/main/docs/decisiones.md)

> Proyecto personal construido solo con datos públicos. No contiene datos de ningún empleador ni cliente.
