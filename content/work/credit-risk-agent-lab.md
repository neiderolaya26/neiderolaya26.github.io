---
title: "Credit Risk Agent Lab"
description: "An end-to-end credit decision system built on public Lending Club data: risk, prepayment and loss models, a limit and pricing engine, a credit policy in code, and an AI agent that analyzes, gives its opinion and writes the memo, while an analyst signs off everything that is not a clear approval."
translationKey: "credit-risk-agent-lab"
weight: 4
category: "Personal project · Credit risk + AI agents"
metric: "0.6823"
metric_label: "out-of-time AUC, against 0.6784 for Lending Club's own score"
role: "Personal project, open source"
scale: "Lending Club public data · 566k loans"
period: "2026"
tags: ["LightGBM", "XGBoost", "PyTorch", "Survival analysis", "Causal inference · DML", "LangGraph", "Claude", "MCP", "Evals", "Streamlit", "Hugging Face"]
---

## The question

Can a public dataset alone support an end-to-end system that makes **better approval, limit, and pricing decisions**, and **automates the analysis and the decision** with advanced modeling, LLMs, and agents?

## What I built

- **Risk:** six models (logistic scorecard, Random Forest, XGBoost, LightGBM, and two neural networks) tuned with Bayesian optimization and validated out of time. The recalibrated LightGBM champion beats Lending Club's score using only public application data (AUC difference 95% CI [+0.0016; +0.0063]). At the same 20% rejection rate, it adds **USD 337k of realized profit per 10,000 applications** (95% CI [272k; 408k]) by not rejecting expensive but good loans.
- **Prepayment and loss:** a discrete-time survival model for prepayment and an economic-loss model; the classic LGD model had no signal.
- **Limit, pricing and policy:** a simulator over amount × rate with a 20% payment-to-income cap. The policy approves when the expected ROI is at least 4%; otherwise it makes a counteroffer, first lowering the amount and then raising the rate by at most 2 points; if nothing works, it rejects.
- **Causal inference:** two-way fixed effects, a placebo test, wild cluster bootstrap, and double machine learning on the effect of the rate on default. The placebo failed, so the effect is not identified: I reported it as a negative result and kept stress as an assumption with a sensitivity analysis, not as an estimate.
- **AI agent:** the credit policy in code makes the decision. A LangGraph graph with three Claude specialists analyzes the case, an AI committee gives its opinion and writes the memo, and an auditor reviews it. Only approvals where the policy and the committee agree go through on their own; everything else, including disagreements, is signed by an analyst, and the sign-off survives a process restart.
- **Reasons for the applicant:** a fixed catalog of at most 4 reasons computed with TreeSHAP, separate from the internal memo and without variables that act as age proxies.
- **Operations:** an MCP server that exposes the engine's tools, a SQLite log of every decision with its real outcome, mandatory evals in CI, and tests on GitHub Actions. The engine runs in a public demo and the models are published on Hugging Face with a model card.

## Value and fairness, measured honestly

- **Backtest on 10,000 applications from 2015:** the policy approves 84.9%, counteroffers 13.8%, and rejects 1.3%. The 15.1% that goes to sign-off means 126 to 202 analyst hours.
- **Net P&L with assumed costs:** the policy loses USD 0.8M less than approving everything, but **it does not beat a simple subgrade cut** at the same rejection rate. I documented it as a limitation instead of hiding it.
- **Fairness:** the public data has no sex, age or race, so I checked proxies. Calibration stays within ±0.7 points for every housing and region group, and removing the age proxies costs only 0.0032 of AUC.

## Results of the agent

Evaluated on 35 loans with known real outcomes, rejections included, and graded by code:

- The committee's opinion matches the policy in 34 of 35 cases (97%) and in 6 of 6 rejections.
- No committee draft breaks a hard rule (0 of 37), 94% of memo figures are backed by a tool, and 94% of audits pass on the first attempt.
- The full 35-case eval cost USD 0.29 with Claude Haiku. The biggest gain came from fixing the design, not from switching models.

## Links

- [Try it: live demo of the decision engine](https://credit-risk-agent-lab.streamlit.app/)
- [Models and model card on Hugging Face](https://huggingface.co/neiderolaya26/credit-risk-lending-club)
- [Repository on GitHub](https://github.com/neiderolaya26/credit-risk-agent-lab)
- [End-to-end demo notebook](https://github.com/neiderolaya26/credit-risk-agent-lab/blob/main/notebooks/11_demo_agente.ipynb)
- [The 23 design decisions and their rationale](https://github.com/neiderolaya26/credit-risk-agent-lab/blob/main/docs/decisiones.md)

> Personal project built only with public data. It contains no data from any employer or customer.
