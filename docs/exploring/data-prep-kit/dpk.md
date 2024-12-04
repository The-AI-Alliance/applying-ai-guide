---
layout: default
title: Data Preparation with Data Prep Kit
nav_order: 20
parent: Exploring AI System Design
---

# Data Preparation with Data Prep Kit

![DPK Architecture](../Data-prep-kit-diagram.png)

Data preparation is a crucial first step in building any Large Language Model (LLM), as the model's quality heavily depends on the data's quality. In this post, we'd like to introduce an open-source toolkit, data-prep-kit, that can aid in data preparation when developing LLM applications. [Data-prep-kit](https://github.com/IBM/data-prep-kit) is an open-source project aimed at democratizing and accelerating unstructured data preparation for LLM app developers. It offers over 20+ modules across code and language domains that can be used immediately for data preparation and quality enhancement. These modules have been developed and tested while building the [Granite models](https://huggingface.co/collections/ibm-granite/granite-code-models-6624c5cec322e4c148c8b330).

The following matrix provides a list of all available modules and their support across various runtimes.

| Modules                                                                              |    Python-only     |        Ray         |       Spark        |     KFP on Ray     |
|:-------------------------------------------------------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|
| **Data Ingestion**                                                                   |                    |                    |                    |                    |
| [Code (from zip) to Parquet](transforms/code/code2parquet/python/README.md)          | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [PDF to Parquet](transforms/language/pdf2parquet/python/README.md)                   | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [HTML to Parquet](transforms/language/html2parquet/python/README.md)                 | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Web to Parquet](transforms/universal/web2parquet/README.md)                         | :white_check_mark: |                    |                    |                    |
| **Universal (Code & Language)**                                                      |                    |                    |                    |                    |
| [Exact dedup filter](transforms/universal/ededup/ray/README.md)                      | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Fuzzy dedup filter](transforms/universal/fdedup/ray/README.md)                      |                    | :white_check_mark: |                    | :white_check_mark: |
| [Unique ID annotation](transforms/universal/doc_id/ray/README.md)                    | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| [Filter on annotations](transforms/universal/filter/python/README.md)                | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| [Profiler](transforms/universal/profiler/ray/README.md)                              | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| [Resize](transforms/universal/resize/python/README.md)                               | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| [HAP](transforms/universal/hap/python/README.md)                                     | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Tokenizer](transforms/universal/tokenization/python/README.md)                      | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| **Language-only**                                                                    |                    |                    |                    |                    |
| [Language identification](transforms/language/lang_id/python/README.md)              | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Document quality](transforms/language/doc_quality/python/README.md)                 | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Document chunking for RAG](transforms/language/doc_chunk/python/README.md)          | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Text encoder](transforms/language/text_encoder/python/README.md)                    | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [PII Annotator/Redactor](transforms/language/pii_redactor/python/README.md)          | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| **Code-only**                                                                        |                    |                    |                    |                    |
| [Programming language annotation](transforms/code/proglang_select/python/README.md)  | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Code quality annotation](transforms/code/code_quality/python/README.md)             | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Malware annotation](transforms/code/malware/python/README.md)                       | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Header cleanser](transforms/code/header_cleanser/python/README.md)                  | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |
| [Semantic file ordering](transforms/code/repo_level_ordering/ray/README.md)          |                    | :white_check_mark: |                    |                    |
| [License Select Annotation](transforms/code/license_select/python/README.md)         | :white_check_mark: | :white_check_mark: |                    | :white_check_mark: |

A user can combine these modules in various ways to create data processing pipelines tailored to their needs. We will discuss an example pipeline and the modules used to build the pipeline below. The goal of this pipeline is to fine tune a Code LLM model to improve its performance on a programming language of choice.

The pipeline starts by allowing users to select raw data from either crawled GitHub repositories or other dataset storage services like Hugging-Face. The data is then converted to parquet format, which serves as a universal file format for all data processing modules. Afterward, the data undergoes filtering procedures to remove duplicates to prevent biasing the model towards any specific data.

Next, the programming language selection module enables users to prioritize the programming languages relevant to their use case and retain the corresponding data. In the subsequent module, users can assess the quality of their data and add annotations to identify which files have been flagged as high or low quality based on data quality rules. The filtering module works in conjunction with the annotation modules (programming language selection and code quality) to empower end users to perform analysis and make informed decisions on which files to filter.

Furthermore, the data is organized in a way that considers the semantic dependencies between the code files and the repository structure, enabling related files to be grouped together and enhancing model learning. For more information on the semantic ordering of files, please refer to our [paper](https://arxiv.org/abs/2407.13739). Finally, the data can be tokenized before being fed into a LLM for fine-tuning. We invite readers to explore the example notebooks [here](https://github.com/IBM/data-prep-kit/tree/dev/examples/notebooks/fine%20tuning) to experiment with the code and construct end-to-end pipelines for their datasets. Additionally, users can check out our [RAG pipelines](https://github.com/IBM/data-prep-kit/tree/dev/examples/notebooks/rag) for data preparation in RAG applications.

So far, we have discussed the capabilities of data-prep-kit in terms of data preparation transforms and the ability to combine them into a pipeline. Next, we discuss the scale of data that can be processed with it. Data-prep-kit supports flexible computing from laptop to data center scale, powered by Ray and Spark runtimes for scalable processing. It also supports [Kubeflow pipelines on Ray](https://github.com/IBM/data-prep-kit/tree/dev/kfp) for automating pipeline usage and maintenance in production scenarios. This is a significant advantage of this toolkit, as it enables users to quickly build proofs-of-concept (PoCs) and scale them to production data. All of this is made possible by a [core data processing library](https://github.com/IBM/data-prep-kit/tree/dev/data-processing-lib) that abstracts out the complexities of Ray and Spark frameworks, allowing data scientists and engineers to add their own modules that run at scale without having to learn about Ray and Spark implementations.

We welcome readers to try out data-prep-kit and look forward to seeing how the community uses this toolkit for building LLM applications. We would like to express our gratitude to the researchers and engineers from IBM and the open source community who have contributed data prep recipes and SDKs, enabling flexible computing that allows the toolkit to run across different data scales.