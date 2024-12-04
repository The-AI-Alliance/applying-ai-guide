---
layout: default
title: Retrieval Augmented Fine-Tuning for Domain-Specific Chatbots
nav_order: 30
parent: Exploring AI System Design
---

# Retrieval Augmented Fine-Tuning for Domain-Specific Chatbots

As the popularity of Meta Llama models grows, we've seen a surge in demand to adapt them to specific domains, enabling businesses to better serve their customers. For example, a company might have a vast collection of plain text documents related to their custom domain and want to create a chatbot that can answer client questions.

Supervised fine-tuning (SFT) a Meta Llama based chatbot involves adapting a pre-trained model to a new domain by updating its weights based on domain-specific data. However relying on SFT might not be enough especially  when we are dealing with frequent updates to the domain documents. Additionally, Large Language Models (LLMs) suffer from hallucinations, where occasionally their outputs might not be factual. On the other hand, Retrieval-Augmented Generation (RAG)-based chatbot combines information retrieval systems with Llama models, retrieving relevant information from a database to enhance the model’s responses. However, sometimes the chatbot response is not ideal as the retrieved documents may be noisy, this would lead to inaccurate outputs where models may hallucinate and return responses that are not factual.

Here, we make use of  Retrieval Augmented Fine Tuning (RAFT), a straightforward and powerful fine-tuning Chatbot recipe to enhance the Llama model's performance in answering questions within specific domains when combined with Retrieval-Augmented Generation (RAG) system. The end to end recipe can be found [here](https://github.com/meta-llama/llama-recipes/tree/main/recipes/use_cases/end2end-recipes/RAFT-Chatbot).

## Architecture

![RAFT Architecture](../architecture.png)

RAFT is a novel technique designed to enhance the fine-tuning process by incorporating retrieval mechanisms directly into the training loop. This method allows the model to not only generate responses based on its internal knowledge but also to retrieve and consider relevant external documents which have been seen during the fine-tuning phase.

The key innovation here is training the model to prioritize pertinent information from retrieved documents while ignoring irrelevant data, thus improving its accuracy and relevance during inference in Retrieval-Augmented Generation (RAG)-based applications.

The following graph illustrates the RAFT main concepts:

![RAFT Concepts](../raft_concepts.png)

## Process

Let’s break down the steps involved in a simple way:

### 1. Preparing the RAFT dataset

Each sample in the RAFT dataset contains the following:

- Question (Q): Start with a question (Q) that the model needs to answer. This question is generated from the Oracle Documents (D*).
- Documents (Dk): Along with the question, there’s a set of documents. These documents are divided into two types:
  - Oracle Documents (D*): This is the document that actually contains the information needed to answer the question.
  - Distractor Documents (Di): These documents do not have the answer and are included to make the task a bit more challenging for the model.
- Answer (A*): a corresponding Chain-of-though style answer (A*) generated from Oracle Documents (D*).

Then RAFT dataset is designed to include different combinations of these documents:

- For a certain percentage (P%) of the questions: The dataset includes the question, the oracle document (the one with the answer), and a few distractor documents. The goal is to train the model to find the correct answer by focusing on the right document(s).

- For the rest of the questions (1-P%): The dataset includes the question but only the distractor documents, with no oracle document. Here, the model is trained to give an answer even when the exact document with the answer isn’t provided, encouraging it to use what it has learned from previous data.

### 2. Fine-tuning the Model

- The model is then fine-tuned using this RAFT dataset. The fine-tuning process teaches the model to generate the correct answer (A*) by either finding the right information in the oracle document(s) or using its learned knowledge when only distractors are available.

### 3. Deploying with RAG

- Lastly, the RAFT model is deployed with the RAG system. It takes a user’s query, retrieves top-K related documents based on similarity through RAG, then generates the correct responses by finding the useful information while ignoring the unrelated text in the provided documents.

## Case Study: Implementing FAQ Chatbot for Llama using RAFT

In this case study, we have built a Llama chatbot capable of answering Llama-related questions using our Meta Llama 3 models. While the Meta Llama 3 70B Instruct model is a strong candidate due to its great reasoning capabilities, the costs associated with running it in production are significant. To reduce these costs, we aim to create a chatbot based on the Meta Llama 8B Instruct model, with the goal of maintaining similar accuracy while lowering inference costs.

### Data Preparation

Ideally, we would use all Llama-related web pages to train the model. However, for this case study, we focused on official documents, including all Llama website documents and Pytorch official documents. We included Pytorch documents because many Llama-related questions often involve topics like fine-tuning and quantization, which are closely related to Pytorch.

To create the RAFT dataset, inspired by [self-instruct paper](https://arxiv.org/abs/2212.10560) we used LLama 3 70B  to assist building the dataset from our data. Please find the code [here](https://github.com/meta-llama/llama-recipes/blob/main/recipes/use_cases/end2end-recipes/RAFT-Chatbot/raft.py).

1. **Document Splitting**: We split the official documents into chunks of 1,000 characters each.
2. **Question and Answer Generation**: For each chunk, we used the 70B Instruct model to generate a question and corresponding chain-of-thought (COT) style answers.
3. **Dataset Compilation**: These domain-specific questions, along with their associated documents and COT answers generated by the 70B Instruct model, formed the basis of our RAFT dataset.

We made an important modification by including additional refusal examples. Specifically, when the related documents were not present, we labeled the COT answer as, "Sorry, I don't know the answer to this question because related documents are not found. Please try again."

Our hypothesis was that this approach would increase answer precision and reduce the risk of the chatbot producing hallucinations. In real-world production scenarios, we prefer that the chatbot refuses to answer when there is insufficient context. This allows us to detect the refusal signal and mitigate the risk of generating incorrect or misleading answers, such as by having a human agent take over the conversation to better assist customers.

### Fine-tuning

We fine-tuned the Meta Llama 3 8B Instruct model using the prepared RAFT dataset. The fine-tuning process involved using the generated questions and documents as inputs and the COT answers as the labels.

### Evaluations using LLM as Judge

To evaluate the chatbot, we created [a test set of 71 Q&A pairs](https://github.com/meta-llama/llama-recipes/blob/main/recipes/use_cases/end2end-recipes/RAFT-Chatbot/eval_llama.json),  based on our Llama FAQ page, supplemented with some human-annotated Q&A pairs. We used "LLM-as-judge" where we asked the 70B Instruct model to compare the chatbot’s answers to the ground truth, allowing us to assess its performance, please find the code [here](https://github.com/meta-llama/llama-recipes/blob/main/recipes/use_cases/end2end-recipes/RAFT-Chatbot/raft_eval.py). In this case study, we evaluate three different RAFT models:

1. **llama_only model**: This model was trained exclusively with documents related to Llama, consisting of over 1,980 RAFT generated examples.
2. **pytorch_only model**: This model was trained using only Pytorch-related documents, with over 20,000 RAFT generated examples.
3. **all_data model**: This model was trained on a combination of both the Llama and Pytorch datasets mentioned above.

We tested these three RAFT models using Retrieval-Augmented Generation (RAG) and compared their performance with two baseline models: the Meta Llama 3 8B Instruct and the Meta Llama 3 70B Instruct. The comparison was based on different RAG document top-k retrieval parameters, which determine how many documents the model retrieves to generate an answer.

For evaluation, we used a Meta Llama 70B Instruct model as the judge. It scored the answers generated by our RAFT models against the correct answers in our evaluation set. The LLM scores are shown below:

![LLM as Judge Comparison](../llm_as_judge_comparison.png)

Our findings revealed that the RAFT models performed similarly to the 8B RAG baseline but significantly underperformed compared to the 70B RAG baseline when the number of retrieved documents was limited (top_k ≤ 5). However, when top_k was increased to 7, the performance of the RAFT models improved dramatically. Specifically, in this case, the all_data 8B model achieved a score of 76.06%, surpassing the 70B baseline’s score of 74.65%.

### Refusal Examples

![Refusal Examples](../refusal.png)

For all 71 Q&A pairs from the eval set, we also analyzed instances where the models refused to answer, responding with "Sorry, I do not know." The all_data model was more cautious and frequently refused to answer, whereas the llama_only RAFT model rarely refused, likely due to the smaller size of its training dataset.

### Precision Analysis

We calculated the precision of our model’s answers, which measures how often the model’s answers were correct when it chose to respond.

![Precision Comparison](../precision_comparison.png)

It’s important to note that the 8B and 70B RAG baseline models never refused to answer, so their precision was the same as their overall score. On the other hand, our all_data and pytorch_only models tended to refuse to answer when fewer documents were available (top_k < 5). However, when they did choose to answer, their precision was higher. Notably, when top_k was set to 7, the all_data RAFT model had an 82.97% chance of producing a correct answer when it decided to respond, outperforming the 70B baseline.

## Key Takeaways

Reduced Hallucinations: RAFT models integrate relevant documents from the RAG system into their input, which significantly reduces the likelihood of generating incorrect or "hallucinated" responses, leading to more accurate and reliable answers.
Cost Efficiency: RAFT reduces the need for frequent and expensive fine-tuning updates compared to standard supervised fine-tuning (SFT) methods. We demonstrated that an 8B RAFT model could outperform a 70B instruct base model under the same RAG system, making RAFT a cost-effective solution for building domain-specific chatbots.
Enhanced Precision: Including some refusal examples in the training data allows the model to recognize when it lacks enough context to provide a correct answer, further improving the precision of its responses.

## Conclusion

Retrieval-Augmented Fine-Tuning (RAFT) represents a significant advancement in developing domain-specific chatbots. By combining the strengths of fine-tuning and RAG, RAFT provides a robust solution for creating accurate, reliable, and cost-effective Llama models tailored to specific domains. As we continue to explore and refine this technique, RAFT holds immense potential to transform customer service and other applications.

## Acknowledgement

We would like to extend special thanks to Tianjun Zhang & Kai Wu, the authors of the [RAFT](https://gorilla.cs.berkeley.edu/blogs/9_raft.html), for collaborating with us on this blog and providing valuable guidance throughout our experiments. Our case study code can be found in [this RAFT chatbot tutorial](https://github.com/meta-llama/llama-recipes/tree/b5f64c0b69d7ff85ec186d964c6c557d55025969/recipes/use_cases/end2end-recipes/RAFT-Chatbot) from [llama-recipe](https://github.com/meta-llama/llama-recipes), and our code is also inspired by the [RAFT github](https://github.com/tianjunz/raft_llm/).
