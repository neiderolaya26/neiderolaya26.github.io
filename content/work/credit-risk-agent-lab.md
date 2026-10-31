---
title: "Credit Risk Agent Lab"
description: "An end-to-end credit decision system built on public Lending Club data: risk, prepayment and loss models, a limit and pricing engine tested with causal inference, and an AI agent that decides, audits itself, and writes the credit memo with no human in the loop."
translationKey: "credit-risk-agent-lab"
weight: 4
category: "Personal project · Credit risk + AI agents"
metric: "0.6823"
metric_label: "out-of-time AUC, against 0.6784 for Lending Club's own score"
role: "Personal project, open source"
scale: "Lending Club public data · 566k loans"
period: "2026"
tags: ["LightGBM", "XGBoost", "PyTorch", "Survival analysis", "Causal inference · DML", "LangGraph", "Claude", "MCP", "LangSmith", "Evals"]
---

## The question

Can a public dataset alone support an end-to-end system that makes **better approval, limit, and pricing decisions**, and **automates the decision** with advanced modeling, LLMs, and agents?

## What I built

- **Risk:** six models (logistic scorecard, Random Forest, XGBoost, LightGBM, and two neural networks) tuned with Bayesian optimization and validated out of time. The recalibrated LightGBM champion beats Lending Club's score using only public application data (AUC difference 95% CI [+0.0016; +0.0063]).
- **Approval:** an expected-utility policy that keeps the same profit as a PD cutoff with a better ROI (6.30%) and lower default.
- **Prepayment and loss:** a discrete-time survival model for prepayment and an economic-loss model; the classic LGD model had no signal.
- **Limit and pricing:** a simulator over amount × rate with a 20% payment-to-income cap. Loans Lending Club priced below the suggested rate returned 2.8% against 6.7% for the rest.
- **Causal inference:** two-way fixed effects, a placebo test, and double machine learning on the effect of the rate on default. The rate–default link turned out to be selection; the placebo failed, so I reported the negative result and added a stress scenario instead of a fake causal effect.
- **AI agent:** a LangGraph graph with three Claude specialists in parallel, a committee that decides, hard rules in code, and an AI auditor that sends the case back when a number or the policy does not check out. Every figure in the memo comes from a tool that runs the models.
- **Operations:** an MCP server that exposes the engine's tools, a SQLite log of every run with its real outcome, LangSmith traces, and evals.

## Results of the agent

Evaluated on 30 loans with known real outcomes, graded by code and by a separate judge model:

- 100% compliance with the hard rules and 94% of memo figures backed by a tool.
- 93% of audits passed on the first attempt and 97% on the judge's rubric.
- USD 0.007 per decision with Claude Haiku. The biggest gain came from fixing the design (shared policy and data for the committee and the auditor), not from switching models.

## Links

- [Repository on GitHub](https://github.com/neiderolaya26/credit-risk-agent-lab)
- [End-to-end demo notebook](https://github.com/neiderolaya26/credit-risk-agent-lab/blob/main/notebooks/11_demo_agente.ipynb)
- [The 20 design decisions and their rationale](https://github.com/neiderolaya26/credit-risk-agent-lab/blob/main/docs/decisiones.md)

> Personal project built only with public data. It contains no data from any employer or customer.
