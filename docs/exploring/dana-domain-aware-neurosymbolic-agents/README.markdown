---
layout: default
title: Domain-Aware Neurosymbolic Agent (DANA) Architecture for Industrial AI
nav_order: 80
parent: Exploring AI System Design
---

<!-- markdownlint-disable MD013 MD025 MD043 MD050 -->

# Domain-Aware Neurosymbolic Agent (DANA) Architecture for Industrial AI

> _arXiv Paper:_ [__DANA: Domain-Aware Neurosymbolic Agents for Consistency and Accuracy__](https://arxiv.org/abs/2410.02823)
>
> _Open-Source Implementation:_ [__OpenSSA__](https://github.com/aitomatic/openssa) framework for Small Specialist Agents

DANA is [Aitomatic](https://www.aitomatic.com)'s agentic-AI system architecture for solving complex, high-stakes problems in industries like semiconductors, energy, and finance. By integrating domain-specific knowledge with neurosymbolic techniques, DANA significantly outperforms current LLM-based systems in both consistency and accuracy. Through its implementation in the open-source OpenSSA framework, DANA offers developers and researchers a powerful tool for building reliable AI solutions that combine the flexibility of neural networks with the precision of symbolic AI.

## Key Benefits

- __Consistent and Accurate Results__ for complex industrial problems
- __Scalable Expertise__ through AI agents incorporating deep domain knowledge from human experts
- __Economical and Efficient Computation__ thanks to usage of small models

![DANA Architecture]({{site.baseurl}}/exploring/dana-domain-aware-neurosymbolic-agents/DANA-Architecture.png)

## Architecture Components

DANA's architecture consists of the key components:

- __Knowledge Store:__ A repository of domain-specific knowledge, including facts, rules, and expert heuristics.
- __Program Store:__ A collection of pre-existing programs applicable to well-characterized problems in the domain.
- __Program Search Process:__ Mechanisms for finding suitable pre-existing programs or creating new ones when needed.
- __Knowledge Capture Process:__ Methods for populating the Knowledge and Program Stores, including both manual and automated approaches.

## Performance Benchmarking

DANA achieved over 90% accuracy on the [FinanceBench](https://arxiv.org/abs/2311.11944) dataset, significantly outperforming tools like LangChain ReAct and OpenAI Assistant in both accuracy and consistency across 150 financial analysis questions of varying difficulty.

![DANA Outperforms Current Agent Tools]({{site.baseurl}}/exploring/dana-domain-aware-neurosymbolic-agents/DANA-Outperforms-Current-Agent-Tools.png)

- `0-RETRIEVE`: retrieve a single data point
- `1-COMPARE`: compare a small number of retrievable data points of the same type
- `2-CALC-CHANGE`: calculate relative change in same retrievable data point over time
- `3-CALC-COMPLEX`: calculate complex financial metrics involving multiple data points of different types
- `4-CALC-AND-JUDGE`: calculate complex financial metrics and judge their goodness/healthiness
- `5-EXPLAIN-FACTORS`: explain major driving factors behind a change
- `6-OTHER-ADVANCED`: answer an unusually tricky financial question

## Industrial Use: Semiconductor Etching Advisor Example

DANA's consistency and accuracy make it ideal to physical-industry workflows requiring precise AI analyses and recommendations.

In semiconductor manufacturing, plasma etching recipe formulation is a critical process involving dozens of parameters that affect etch rate, selectivity, and uniformity. Suboptimal recipes can lead to inefficient cycle times and expensive quality problems like mask erosion, pattern distortion, and plasma instability, significantly impacting wafer yield. Traditionally, optimizing these recipes requires deep expertise and time-consuming analyses by scarce experts.

In this example, the Etching Advisor AI agent was constructed by integrating DANA with [SemiKong](https://SemiKong.ai), the world's first open-source semiconductor LLM pioneered by the AI Alliance's Foundation Models workgroup. This agent demonstrates precise etching recipe analyses and recommendations, including pros-and-cons comparisons of feasible alternatives. It illustrates how such an AI solution could help process engineers save time and quickly arrive at optimal recipes.

![Typical Recipe Analysis & Recommendation from DANA-and-SemiKong-based Semiconductor Etching Advisor]({{site.baseurl}}/exploring/dana-domain-aware-neurosymbolic-agents/DANA-and-SemiKong-based-Semiconductor-Etching-Advisor-Recommendation.png)

Watch the demo on [YouTube](https://www.youtube.com/watch?v=1pUaIwyky9Y).

## Open-Source Implementation

Aitomatic's [OpenSSA](https://github.com/aitomatic/openssa) framework for small specialist agents
implements a variant of the DANA architecture,
employing Hierarchical Task Planning (HTP) for structuring programs
and Observe-Orient-Decide-Act Reasoning (OODAR) for executing such programs.

OpenSSA supports using [Llama](https://llama.com) as its primary open-source LM backend.
