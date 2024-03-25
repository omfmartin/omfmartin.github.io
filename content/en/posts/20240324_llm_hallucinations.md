---
title: "Beyond the Illusion: Understanding and Mitigating LLM Hallucinations"
url: "/posts/2024-03-24-llm-hallucination"
summary: "How can we detect and mitigate hallucinations for safer LLM deployment?"
date: 2024-03-24
---

**Large Language Models (LLMs) make stuff up**. Occasionally, these models will generate content that, while seemingly entirely plausible, is fundamentally incorrect. This phenomenon is known as "hallucinations".

**Hallucinations can have disastrous consequences**. They can spread misinformation, potentially offend users, and even compromise sensitive information. This issue is a major drawback of LLMs and is the reason why many are cautious about employing them in real-world settings.

In this article, we will discuss what hallucinations are, explain why they happen, and explore strategies to detect and mitigate them.

![A surreal and psychedelic painting that portrays a linguistic model embarking on a hallucinatory and mind-altering journey.](hallucinating_llm.png)

## What are Hallucinations?

Hallucinations can be loosely defined as output that is **factually incorrect, unfaithful to the provided information, or nonsensical**. Most often, the model will appear quite confident and its response may even look entirely plausible. This makes detecting hallucinations a difficult task. Generally, hallucinations fall into two categories: intrinsic and extrinsic.

**Intrinsic hallucinations** happen when the model's output directly contradicts the information it was given. For example:

>>> **Prompt**: The capital of France is Paris. What is the capital of France?
**Response**: The capital of France is Lyon.

**Extrinsic hallucinations** occur when the model makes statements that cannot be verified or contradicted based on the provided information. For example:

>>> **Prompt**: Describe the economic impact of renewable energy sources.
**Response**: According to a 2045 report, renewable energy sources have led to the creation of 5 million new jobs worldwide.

## Why do LLMs Hallucinate?

Hallucinations in language models are thought to be unavoidable. They can occur factors due to a variety of factors, which can be broadly classified into issues related to data, training process, and inference.

### Data-Related Causes

- **Inaccuracies in Training Data**: Training data is the foundation of LLMs. However, data is not always of the best quality and may be inaccurate or biased. Given the vast amount of data processed, ensuring its accuracy and relevance is challenging. This implies that some of the information learned by the model will lead it to generate misleading or incorrect responses.
- **Out-of-Distribution Queries**: LLMs are trained on datasets that cover certain topics, time periods, and types of information. When faced with questions or tasks that lie outside of these boundaries, a language model may generate incorrect answers. For instance, asking about the latest news events will result in answers based on outdated or irrelevant information.
- **Data Utilization Issues**: How language models utilize their knowledge base can also contribute to hallucinations. Language models tend to rely on word co-occurrence patterns to generate responses. This reliance might lead to incorrect conclusions, such as identifying Sydney as the capital of Australia due to their more frequent association compared to the correct answer, Canberra.

### Training-Related Causes

- **Exposure Bias**:  This issue arises from a discrepancy between how the model is trained and the model's actual use case. During training, the model learns to predict the next token relying on the previous correct sequence from the training data, a method known as "teacher forcing." However, during inference, the model relies on its outputs. This can lead to an accumulation of errors or snowball effect.
- **Architectural Limitations**: Causal language models generate text by predicting the next token based solely on preceding tokens. This design limits the model's ability to fully grasp contextual nuances, which is believed to contribute to the generation of hallucinated content.
- **Belief Misalignment**: LLMs typically undergo a two-stage training process. Initially, they are pretrained to acquire broad knowledge. Subsequently, they are fine-tuned to better align with specific user instructions or prompts, often using techniques like [reinforcement learning](https://en.wikipedia.org/wiki/Reinforcement_learning). This fine-tuning might inadvertently lead the model to prioritize user preferences over accuracy.

### Inference-Related Causes

- **Randomness in Sampling**: The decoding strategies of language models often incorporate some randomness to enhance creativity and output quality, for example by adjusting the temperature parameter to favor less likely choices. This approach can inadvertently increase the likelihood of generating nonsensical content.
- **Softmax Bottleneck**: The last layer of an LLM estimates the probability of the next token. The majority of LLMs use a softmax layer, which is known to have a limitation called the softmax bottleneck. This limitation impedes the model's capacity to distinguish among highly probable outcomes effectively.
- **Diluted Attention**: With longer input sequences, the efficiency of the model’s attention mechanism may decline, leading to a weakened focus on essential details and context. This dilution of attention compromises the model's ability to remain coherent and accurate over extended texts.

## How can we detect hallucinations?

Detecting hallucinations presents significant challenges that standard metrics used in Natural Language Processing such as [BLEU](https://en.wikipedia.org/wiki/BLEU), [ROUGE](https://en.wikipedia.org/wiki/ROUGE_(metric)), or [BERTScore](https://en.wikipedia.org/wiki/Sentence_embedding) fail to address. Not only do these metrics fail to accurately predict hallucinations, but also rely on reference response, which is impractical for widespread applications.

One approach for hallucination detection is to employ **probabilistic metrics** that calculate the likelihood of hallucinations based on token probabilities or entropy measures, for example by estimating [perplexity](https://en.wikipedia.org/wiki/Perplexity). However, these metrics require access to token probabilities, which may not be available, which limits their adoption.

A more effective strategy has been the development of **model-based metrics**. This approach involves utilizing another language model, for example, a state-of-the-art LLM, to assess the likelihood of hallucinatory content. These metrics can be further classified into token-level and sentence-level metrics.

**Token-level metrics** attempt to predict for each token if it was hallucinated or not. To implement this strategy, researchers typically generate a synthetic hallucinated dataset by applying perturbations to reference datasets free of hallucinations. A binary language classifier, such as one based on [BERT](https://en.wikipedia.org/wiki/BERT_(language_model)) or [XLNet](https://doi.org/10.48550/arXiv.1906.08237), is then trained to discern hallucinated tokens from non-hallucinated ones. Notable projects utilizing this approach are [HADES](https://github.com/microsoft/HaDes) and [fairseq-detect-hallucination](https://github.com/violet-zct/fairseq-detect-hallucination) hallucination detection pipeline.

On the other hand, **sentence-level metrics** evaluate the entire sentence to determine the presence of hallucinations. A prominent method is [SelfCheckGPT](https://github.com/potsawee/selfcheckgpt), a sampling-based technique grounded in the assumption that hallucinations are more likely to occur when the model is uncertain. This method assesses self-consistency by comparing various sampled responses, using standard metrics such as N-gram matching or BERTScore, to estimate the probability of hallucinatory content. One strength of this method is that it does not require any external data, making it easily scalable. 

## Strategies for Mitigating Hallucinations

Hallucination mitigation strategies aim at reducing the likelihood of an LLM generating hallucinatory content. Below, we explore several techniques, ordered from straightforward adjustments to more complex interventions.

![Visualize a moment capturing the aftermath of a hallucinatory journey for a linguistic model.](llm_waking_up.png)

### 1. Adjusting Inference Parameters

One of the simplest methods to reduce hallucinations involves tweaking the LLM's operational settings. For instance, fine-tuning parameters such as the generation temperature and top-k sampling can regulate the randomness of the LLM's output. Lower temperature or top-k values may yield more cautious and conservative responses, thereby potentially minimizing hallucinatory outputs.

### 2. Prompt Engineering

Effective prompt engineering is crucial for steering LLMs toward accurate responses. The following general tips are essential for optimizing prompts:

- **Clarity and Precision**: Ensure the prompt is clear about the exact type of information required. 

 - **Uncertainty Acknowledgement**: Explicitly request that the model should avoid making up information and express uncertainty when applicable, for example by saying "I don't know".
    
- **Emphasis on Key Information**: Repeating important details within the prompt can help underline the importance of these elements. This helps ensure that they receive due attention in the model's response.
    
- **Strategic Information Placement**: Positioning the most crucial information towards the end of the prompt can enhance the model's focus on these key details.
    
- **Output Format Specification**: Directing the model to structure its response in a specific format can help in obtaining information that is organized in a manner that best suits the user's needs.
    
Furthermore, more advanced [prompt engineering](https://www.promptingguide.ai) techniques offer additional layers of sophistication:

- **Multi-shot Prompting** involves providing the model with multiple examples of the desired output. This approach helps the model grasp the context better and align its responses with the expected answer format.
    
- **Chain of Thought** encourages the model to exhibit its reasoning process step-by-step, offering users insights into the logical progression leading to a given answer.
    
- **Self-consistency** involves generating several responses and selecting the one that demonstrates the highest degree of internal consistency, thereby improving reliability.
    
- **Chain of Verification** is a technique where the model is used to verify its outputs or make annotations, further enhancing the accuracy and trustworthiness of the information provided.


### 3. Retrieval-based Approaches

These approaches enhance the model's responses by incorporating external knowledge sources and context to the prompt. Probably the most known technique is **Retrieval Augmented Generation (RAG)**:. This approach encodes external documents as [sentence embedding](https://en.wikipedia.org/wiki/Sentence_embedding), usually using some transformer-based neural network such as [BERT](https://en.wikipedia.org/wiki/BERT_(language_model)). The resulting embeddings are then stored in a [vector database](https://en.wikipedia.org/wiki/Vector_database), which can then be efficiently queried using the original prompt (or some variation). The retrieved sentences are then used to enrich the prompt. A popular framework is [FAISS](https://github.com/facebookresearch/faiss). The main drawback of these retrieval-based approaches is that they require maintaining a large and curated database.

### 4. LLM Fine-tuning

The quality of a model's output directly depends on its training data. Fine-tuning an LLM with updated or more relevant data can address specific weaknesses or gaps in its knowledge base. While retraining from scratch is often impractical, fine-tuning offers a cost-effective alternative for enhancing model accuracy in targeted applications. Techniques such as [LoRA](https://doi.org/10.48550/arXiv.2106.09685) help reduce the number of trainable parameters which accelerates the fine-tuning making it a viable option.

## Closing Thought

There is a fine line between creativity and hallucinations. On one hand, a model that leans too much towards caution may avoid hallucinations but at the cost of producing responses that are uninteresting or too simplistic. On the other hand, an overly creative model will produce outputs that resemble [an interview with Salvador Dali](https://www.youtube.com/watch?v=A3FAy0teMNo).

Finding the right balance is key to harnessing the full potential of LLM while minimizing the risks associated with hallucinations. Ultimately, understanding and mitigating hallucinations is not only about preventing errors; it's about building trust between humans and artificial intelligence, ensuring that these tools enhance our decision-making processes, creativity, and access to information.

## Bibliography

- [Huang et al. A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions. (2023)](https://doi.org/10.48550/arXiv.2311.05232)
- [Ji et al. Survey of Hallucination in Natural Language Generation. (2022)](https://doi.org/10.1145/3571730)
- [Luo et al. Hallucination Detection and Hallucination Mitigation: An Investigation. (2024)](https://doi.org/10.48550/arXiv.2401.08358)
- [Manakul et al. SelfCheckGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models. (2023)](https://doi.org/10.48550/arXiv.2303.08896)
- [Xu et al. Hallucination is Inevitable: An Innate Limitation of Large Language Models. (2024)](https://doi.org/10.48550/arXiv.2401.11817)
- [Zhou et al. Detecting Hallucinated Content in Conditional Neural Sequence Generation. (2020)](https://doi.org/10.48550/arXiv.2011.02593)
