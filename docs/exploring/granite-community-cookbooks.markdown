---
layout: default
title: Granite Community and Cookbooks - IBM
nav_order: 60
parent: Exploring AI System Design
---

# Granite Community and Cookbooks

IBM introduces the Granite Community for building with IBM's
[Granite Model Family](https://www.ibm.com/granite){:target="granite-fam"}.

The Granite family of foundation models span an increasing variety of modalities, including language, code, time series, and science (e.g., materials) - with much more to come. We're building them with transparency and with focus on fulfilling rigorous enterprise requirements that are emerging for AI. If you'd like to learn more about the models themselves and how we build them, check out
[Granite Models](https://github.com/ibm-granite){:target="granite"}.
For a overview of Granite in PDF form, please also see the [Granite Technical Guide](https://github.com/ibm-granite-community/documentation/blob/main/IBM%20Granite%20Technical%20Guide.pdf){:target="granite-guide"}.

The mission of the Granite Community organization is to work collaboratively across industries and geographies to leverage Granite to solve problems and bring value across use cases, from code generation and modernization, to forecasting and predictive maintenance, to materials discovery.

Our first goal is to build out a community kitchen for Granite, starting with cookbooks that have proven recipes to help enrich, adapt, and apply Granite models to real world applications.

First and foremost, the _Granite Cookbooks_ provide open-source recipes for using these models in various practical scenarios.  Successful recipes should leave the reader ready to begin planning similar work in their environment.

The first four Cookbooks in development are:

* [Granite Code Cookbook](https://github.com/ibm-granite-community/granite-code-cookbook){:target="code"}
* [Granite Legal Cookbook](https://github.com/ibm-granite-community/granite-legal-cookbook){:target="legal"}
* [Granite Finance Cookbook](https://github.com/ibm-granite-community/granite-finance-cookbook){:target="finance"}
* [Granite Time Series Cookbook](https://github.com/ibm-granite-community/granite-timeseries-cookbook){:target="timeseries"}

Some of the first recipes included simple use cases that demonstrate zero-shot inference with the code and time series models.
The recipes are growing more complex to cover data preparation (including [Data Prep Kit](https://github.com/IBM/data-prep-kit){:target="dpk"}), fine-tuning, RAG, tool use, and agentic workflows.

The Granite Code Cookbook includes examples of code-specific patterns, including several that utilize analysis available from [CodeLLM Devkit](https://github.com/IBM/codellm-devkit/){:target="codellm"}
(also see a [related blog entry](https://research.ibm.com/blog/cldk-codellm-devkit){:target="codellm-blog"}).

The Cookbook recipes – Jupyter notebooks – are licensed under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.en){:target="cc-by"}.
Code is licensed under
[Apache 2.0](https://opensource.org/license/apache-2-0){:target="apache"}
and example datasets licensed under
[CDLA Permissive 2.0](https://cdla.dev/permissive-2-0/){:target="cdla"}.

Granite Models have been published to
[Replicate](https://replicate.com/ibm-granite){:target="granite"} for remote use,
and [Ollama](https://ollama.com/library/granite-code){:target="code"} for local use by the recipes.
To help assure the models are up-to-date, the Granite Cookbook recipes are automatically tested in every pull request, like you'd expect from any other software artifact.

The [project board](https://github.com/orgs/ibm-granite-community/projects/1){:target="project"} has more information
on planned new recipes and improvements to the Cookbooks.

Please create issues or submit pull requests to the appropriate repos if you have suggestions
for improving or expanding the recipes.

To find the community of Cookbook readers, please join the conversation
on the [“Granite Community” Discord server](https://discord.gg/GgDyu9jBKw){:target="discord"}.
