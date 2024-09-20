---
layout: default
title: Glossary of Terms
nav_order: 30
has_children: false
---

# Glossary of Terms

Let's define the common terms we use. Some of the terms defined here are industry standards, while others are not standard, but they are useful for our purposes.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## AI System

Umbrella term for an application or system with AI components, including datasets, models, safety detection and mitigation components, external services, databases for runtime queries, and other application logic that together provide functionality.

## Alignment

A general term for how well an [AI System's](#ai-system) outputs (e.g., replies to queries) and behaviors correspond to end-user and service provider objectives, including the quality and utility of results, as well as safety requirements. Quality implies factual correctness and utility implies the results are fit for purpose, e.g., a Q&A system should answer user questions concisely and directly, a Python code-generation system should output valid, bug-free, and secure Python code. [Eleuther AI defines alignment this way](https://www.eleuther.ai/alignment){:target="eleuther"}, &ldquo;Ensuring that an artificial intelligence system behaves in a manner that is consistent with human values and goals.&rdquo; See also the [Alignment Forum](https://www.alignmentforum.org/){:target="alignment-forum"}.

## Annotation

[\[1\]](#mlc) External data that complements a [Dataset](#dataset), such as labels that classify individual items.

## Benchmark

[\[1\]](#mlc) A methodology or function used for offline evaluation of a model or system for a particular purpose and to interpret the results. It consists of:
* A set of tests with metrics
* A summarization of the results

## Dataset

(See also [\[1\]](#mlc)) A collection of data items used for training, evaluation, etc. Usually, a given dataset has a schema (which may be “this is unstructured text”) and some metadata about provenance, licenses for use, transformations and filters applied, etc. 

## Explainability

Can humans understand why the system behaves the way that it does in a particular scenario?

## Evaluation Framework

An umbrella term for the software tools, runtime services, benchmark systems, etc. used to run different [Evaluators](#evaluator) to measure [AI Systems](#ai-system).

## Evaluator

A classifier model or similar tool that can quantify an [AI System's](#ai-system) inputs and outputs to detect the presence of risky content, such as hate speech, hallucinations, etc. For our purposes, an evaluator is API compatible for execution within an [Evaluation Framework](#evaluation-framework). In general, an evaluator could be targeted towards non-safety needs, such as measuring other aspects of [Alignment](#alignment), model latency and throughput, carbon footprint, etc. Also, a given evaluator could be used at many points in the total AI life cycle, e.g., for a benchmark and an inference-time test.

## Hallucination

When a model generates text that seems plausible, but is not factually accurate. Lying is not the right term, because there is no malice intended by the model, which only knows how to generate [Tokens](#token); sequences that are plausible, i.e., probabilistically likely.

## Token

For language models, the training texts and query prompts are split into tokens, usually whole words or fractions according to a vocabulary of tens of thousands of tokens that can include common single characters, several characters, and &ldquo;control&rdquo; tokens (like &ldquo;end of input&rdquo;). The rule of thumb is a corpus will have roughly 1.5 times the number of tokens as it will have words.

Next, we [explore ai system design concepts]({{site.baseurl}}/exploring/exploring) as expressed by various expert organizations.
