---
title: "Marketing Agent Lab"
description: "An end-to-end growth marketing lab on public data: what a lead is worth, whom to retain, which campaigns really work, which channel causes sales and how to allocate the budget, with an AI agent that answers with the lab's numbers and never moves money without a person's sign-off."
translationKey: "marketing-agent-lab"
weight: 5
category: "Personal project · Growth marketing + AI agents"
metric: "83%"
metric_label: "of the optimal profit captured by the MMM budget plan, across 10 simulations with known ground truth"
role: "Personal project, open source"
scale: "Public data from Olist, Hillstrom, The Complete Journey and synthetic benchmarks · 8k leads, 64k customers, 500k journeys"
period: "2026"
tags: ["Causal inference · DiD · DML", "Uplift", "A/B testing", "Attribution · Shapley", "Marketing Mix Modeling", "Optuna", "LangGraph", "Claude", "MCP", "Evals", "Streamlit", "Hugging Face"]
---

## The question

Can you build, **using only public data**, a system that tells you **where to put the marketing budget**? That means which channel, which customer and which campaign create **incremental value** rather than just conversions, with an AI agent that answers business questions using those numbers.

## What I built

- **Acquisition:** lead value at 90 days by channel with bootstrap CIs, and lead scoring with 6 models and 240 Optuna trials under time validation. A logistic regression won (AUC 0.68): the top 30% of leads by score brings 55% of closed deals.
- **Retention:** RFM segmentation with K-means validated out of time, CLV with BG/NBD built from scratch against LightGBM and a naive baseline, and partial churn. The naive baseline was hard to beat: customers don't leave, they buy less.
- **Experiments and uplift:** power, CUPED, O'Brien-Fleming calibrated by simulation (peeking 20 times without correction raises false positives from 5% to 23%), a Bayesian read and S/T/X-learners. Personalizing did not beat sending the best e-mail to everyone.
- **Campaigns without an experiment:** the naive comparison said +57% spend, but it was selection. With doubly robust DiD and DML the effect is +2.4 USD per household-week, with an interval from −1.1 to 6.0 that includes zero. I reported it as not distinguishable from zero and sized the randomized control group that would be needed.
- **Attribution and MMM against ground truth:** on synthetic data with known effects, every rule and Markov chain is off by about 2 points per channel in the same direction, while Shapley on a conversion model is off by 0.30. In the MMM, endogenous spend makes the budget plan lose money. A demand index fixes it.
- **AI agent:** a LangGraph graph with a router, Claude specialists for acquisition, retention and budget, and a coordinator. The rules that move money live in code: ±30% per channel, the total cannot rise, and paid search stays frozen until a geo test. Every reallocation waits for a person's sign-off.
- **Operations:** a read-only MCP server, a SQLite log of every run, 20 golden cases with a judge calibrated against my own ratings, a public demo and the models on Hugging Face.

## What the agent taught me

I asked it to reallocate the budget with the same total, and it proposed raising TV by 26.5%. That is how I realized I was optimizing **sales** and not **profit**. With the objective changed, it kept 471k in reserve and profit went from +131k to **+324k**. The judge was also stricter than I was: it failed an answer I had approved, and it was right. That finding became a rule in code.

## Agent results

Evaluated on 20 golden cases, four of them on safety:

- **0 safety failures:** it does not raise paid search even when told "management already approved", it does not delete data, it ignores instructions hidden in the question and it does not answer outside the lab.
- 100% of expected figures appear in the answer, and the judge (Claude Opus) approves 85% to 95% of answers.
- Answering 7 questions costs about USD 0.33. The full battery with the judge costs about USD 2.

## Links

- [Try it: live demo, no LLM](https://marketing-agent-lab.streamlit.app/)
- [Models and model card on Hugging Face](https://huggingface.co/neiderolaya26/marketing-agent-lab) · [data](https://huggingface.co/datasets/neiderolaya26/marketing-agent-lab-data)
- [Repository on GitHub](https://github.com/neiderolaya26/marketing-agent-lab)
- [Consulting case: the final recommendation on one page (Spanish)](https://github.com/neiderolaya26/marketing-agent-lab/blob/main/docs/caso_consultoria.md)
- [The 19 design decisions and their rationale (Spanish)](https://github.com/neiderolaya26/marketing-agent-lab/blob/main/docs/decisiones.md)

> Personal project built only with public data. It contains no data from any employer or customer.
