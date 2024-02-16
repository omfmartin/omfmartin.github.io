---
title: "A Quick Guide to Explainable AI"
url: "/2024-02-explainable-ai"
summary: ""
---

## What is Explainable AI?

Artificial intelligence and machine learning models grow in complexity each year, a trend driven by the increase in computational power, the development of faster algorithms, and the abundance of data. In particular, certain models, such as Large Language Models, are now composed of hundreds of billions of parameters. 

This increasing complexity makes it increasingly challenging to understand their decision-making processes, even for their developers. This opacity brings significant challenges, particularly when the decisions made by these systems have far-reaching implications.

 <figure> <img style="margin: auto;" src="increasing_number_of_parameters.png" alt="The increasing number of AI model parameters between 1954 and 2024."> <figcaption>The increasing in the number of model parameters between 1954 and 2024. Data from <a href="https://towardsdatascience.com/parameter-counts-in-machine-learning-a312dc4753d0">Jaime Sevilla</a>.</figcaption> </figure>

In response, eXplainable AI (XAI) approaches emerge as a solution to open these black boxes and understand of how predictions and decisions are made. 

This blog post is a succinct introduction to XAI, highlighting its importance and the methodologies involved. For those seeking a deeper dive into the subject, the resources section at the end of this post offers a selection of insightful books and software tools dedicated to XAI.

## When Should We Open the Black Box?

Understanding the algorithmic decision-making process does more than satisfy academic curiosity. In many practical applications, the explanation can be just as valuable as the prediction. Let's see some interesting use cases of XAI techniques. 

![A person stands in a dimly lit room, their silhouette outlined by the soft glow emanating from an open, mysterious black box they hold in their hands.](opening_the_black_box.webp)
### High-Stake Scenarios

In critical sectors like healthcare, finance, and criminal justice, transparent and accountable decision-making is imperative due to the profound impact that algorithms' decisions and predictions can have. Particularly in high-risk scenarios such as social scoring systems, individual categorization, facial recognition, law enforcement tools, employment screening, and essential services, the potential consequences on individuals' lives and well-being are significant.

AI models are fallible; they can make errors and may harbor inherent biases, resulting in unjust decisions or discriminatory outcomes. These concerns are not merely theoretical. The utilization of the COMPAS tool in the US criminal justice system has faced criticism for potential bias, as it was discovered to label Black individuals as twice as likely to re-offend compared to White individuals. Similar stories exist for healthcare and job applications. Such instances underscore the necessity for accountability in AI systems deployed in sensitive and impactful domains.

### Debugging Models

Explanations of predictive models can be a valuable tool for debugging and improving their performance. These insights derived from these explanations can help identify situations where the model is making mistakes. Additionally, it may help reveal situations where the model is directing attention to irrelevant features. For example, if a classifier trained to classify animals or plants begins relying on sky color instead of animal features, it indicates overfitting and underscores the necessity for refinement.

### Detecting Bias

Algorithms are susceptible to biases that exacerbate social inequalities and unfairly impact minority groups. As mentioned above, instances of such biased decisions have already been documented.

Bias in AI typically arises due to training on skewed or incomplete datasets, or due to the implicit assumptions embedded within the models themselves. By helping understand how and why certain predictions are made, XAI methods help identify and mitigate these biases. This transparency is the first step toward rectifying biased outcomes and working toward more equitable AI systems.

### Knowledge Discovery

Using XAI for generating new knowledge is probably the most overlooked application. This capability is particularly interesting given that machine learning models are more apt at identifying complex patterns and trends in data compared to traditional statistical tests such as t-tests or chi-squared tests. This approach can be used for biomarker discovery. The explanation can allow to order of the most interesting candidate biomarker that predicts specific biological or health conditions.

Despite its potential, employing XAI for knowledge generation is not without challenges. Some of the most significant issues include:

- **Significance Threshold Ambiguity**: Unlike traditional statistical methods, which have well-defined criteria for statistical significance, XAI lacks a standardized framework to determine the significance level of variables.
- **Observational Data Limitations**: The majority of data used in machine learning is observational, which is inherently less effective at establishing causal relationships.
- **Overfitting**: There exists a risk of models mistaking random noise for significant patterns, potentially leading to incorrect interpretations and conclusions regarding the data.

### Regulatory Compliance

The complexity of predictive algorithms makes it difficult to verify their compliance with legal standards. As the deployment of artificial intelligence becomes more widespread, laws and regulations are starting to require that AI decisions should be transparent and explainable. This shift aims to guarantee that these algorithms operate within ethical boundaries.

For example, the European Union's General Data Protection Regulation (GDPR) grants people a "right to explanation" for AI decisions that affect them. More recently, the AI Act targeted "high-risk" algorithms with requirements related to "transparency and provision of information to users". While these laws face challenges in enforcement and interpretation, they underscore a growing concern related to transparency.

## When We Can Away Without an Explanation

Not all predictions made by AI should be explained. Generating explanations introduces additional software complexity and computational needs, which translate to financial expenses, and environmental impact. Explainable AI may be unjustified in certain scenarios, such as:

- **Low-Stakes Scenarios**: In situations where the consequences of errors are minimal, such as recommendations on a streaming platform, users typically have little interest in understanding the inner workings of how these suggestions are generated.
- **Well-Understood Problems**: For well-established problems like spam filtering in email systems, the explanations for each decision may not be essential because the overall reliability and accuracy of the system are already well understood.
- **Risks of Manipulation**: In some cases, providing detailed explanations can unintentionally aid those seeking to exploit the system. For instance, SEO experts might misuse in-depth information about search algorithms to manipulate rankings and undermine the system's integrity.

## Approaches to Explainaing AI

Interpretability and explainability are distinguished in the context of AI. Interpretability refers to the degree to which one can understand the underlying algorithm. Explainability, on the other hand, pertains to the ability to understand how the algorithm arrived at a particular result.

The intricate mathematical details and computations of an algorithm often provide very little insight into its decision-making process. This is because the representations learned by the predictive model are typically beyond the human brain's capacity to understand. As such, direction approaches, such as disclosing source code or mathematical computations, fall short of providing understanding.

To surmount these challenges, XAI techniques either favor inherently interpretable models or approximate complex, "black-box" models with simpler, more understandable ones. XAI is an active field of research. There is currently (and there may never be) a universal solution for explaining complex predictive algorithms. This stems from the vast diversity of algorithms and the various contexts in which they are applied.

Our focus will be on categorizing the techniques used to explain predictive models, highlighting some of the most commonly employed methods. For those interested in the subject, further reading is recommended.

### Stage: Ante-hoc vs Post-hoc

- **Ante-hoc (Intrinsic Interpretability)**: This approach focuses on using models that are inherently interpretable due to their simple structure and transparent decision-making processes. Examples include linear models, decision trees, and rule-based systems. While these models offer clarity from the outset, their interpretability can diminish as complexity or the number of parameters increases. There is currently research to design high-performing yet inherently interpretable models, however, it has thus far not been very successful.
- **Post-hoc (Extrinsic Interpretability)**: Post-hoc methods aim to explain decisions made by complex, black-box models after they have been trained. These explanations are not built into the model but are derived from analyzing the model's behavior. This approach is particularly useful for "black-box" models, such as deep neural networks. Post-hoc explanations strive to be locally faithful, meaning they accurately reflect the model's reasoning for specific instances or decisions.

 <figure> <img src="complicated_decision_tree.png" alt="A complicated decision tree."> <figcaption>Given enough parameters, even a decision tree becomes uninterpretable.</figcaption> </figure>

### Model: Model-specific vs Model-agnostic

- **Model-Specific**: These techniques are only applicable to specific types of models, exploiting their unique architectures to provide explanations. For deep learning models, techniques such as Saliency Maps, DeepLIFT, and Class Activation Maps (CAM) illustrate how different features influence the model's predictions.
    
- **Model-Agnostic**: Offering a flexible alternative, model-agnostic methods do not require knowledge of the model's internal workings, making them applicable across a wide range of AI systems. Techniques like SHAP (SHapley Additive exPlanations) and LIME (Local Interpretable Model-agnostic Explanations) can be used to explain any model by approximating its decision boundary or output in an interpretable way.

### Scope: Global vs Local

- **Local Explanations**: These are focused on individual predictions or decisions, providing insights into why the model made a particular choice for a specific instance. Tools like SHAP and LIME offer local explanations and show the influence of various features on a single outcome.
    
- **Global Explanations**: In contrast, global explanations seek to illuminate the model's behavior across the entire dataset or in a more general sense. Techniques for achieving global explanations include feature importance rankings and partial dependence plots, which aggregate the effects of features across multiple instances to offer a broader understanding of the model's logic.

## The Limitations of Explainable AI

As AI continues to permeate various aspects of our lives,the imperative for transparency and understandability in AI systems has never been more pronounced. Yet, as with any rapidly evolving  domain, XAI is not without its challenges and limitations.

![The image depicts a person sitting in front of a massive, complex machine, composed of countless gears, levers, and screens, symbolizing a complex AI](complex_ai.webp)
### The complexity-performance tradeoff

The drive to enhance AI model performance often leads to increased complexity, a trend that rests on the assumed trade-off between complexity and performance. This perspective, however, merits critical examination. The lottery ticket hypothesis proposes that the efficacy of large models might stem from optimal parameter initialization rather than sheer size. Empirically, simpler models can be shown to achieve comparable performance to their more complex counterparts. 

Indeed, evidence shows that models with fewer parameters, such as Mistral-7B, can achieve performance levels comparable to those of their more complex counterparts, like GPT-3.5, with a fraction of the parameters (7 billion vs. 175 billion). This observation challenges the necessity of overly complex models, advocating for a more measured approach to model design where simplicity does not preclude effectiveness.

### Making Explanations Accessible

XAI predominantly features methods that cater to technical users, such as data scientists or machine learning engineers. This technical orientation creates a disconnect between the explanations provided and the comprehension of non-technical end-users, who are most impacted by AI's decisions.  Addressing this gap is essential for advancing the goal of transparency in AI.

### Faithfulness and the Limits of Explanation

Post-hoc explanations are obtained by locally approximating the behavior of a complex model. These explanations, while useful, can not fully explain the model's rationale. This introduces concerns regarding the faithfulness and completeness of these explanations. 

Moreover, as models evolve in complexity with time, the challenge of crafting explanations that accurately reflect their decision-making processes intensifies. This complexity underscores a pivotal question: Is there a theoretical limit to our ability to explain AI, and if so, how close are we to reaching it?

## Conclusion

XAI plays a critical role in addressing the opacity of complex machine learning models, enabling trust in AI-based decisions. By explaining model behavior, XAI helps identify biases, debug models, comply with regulations, and could even help generate new knowledge. Nonetheless, it's important to recognize the limitations and challenges associated with XAI and favor simpler models whenever possible.

## Resources

### Books

- [Explanatory Model Analysis](https://ema.drwhy.ai/)
- [Intrepretable Machine Learning](https://christophm.github.io/interpretable-ml-book/)

### Software

- [SHAP](https://shap.readthedocs.io/en/latest/)
- [LIME](https://github.com/marcotcr/lime)
- [DALEX](https://github.com/ModelOriented/DALEX)
- [XAITK](https://xaitk.org/)
- [InterpretML](https://interpret.ml/)

## Bibliography


- [Ebers. Regulating Explainable AI in the European Union. An Overview of the Current Legal Framework(s). (2021)](https://dx.doi.org/10.2139/ssrn.3901732)

- [Frankle and Carbin. The Lottery Ticket Hypothesis: Finding Sparse, Trainable Neural Networks. (2018)](https://doi.org/10.48550/arXiv.1803.03635)

- [Gunning et al. DARPA's explainable AI (XAI) program: A retrospective. (2021)](https://doi.org/10.1002/ail2.61)

- [Linardatos et al. Explainable AI: A Review of Machine Learning Interpretability Methods. (2020)](https://doi.org/10.3390/e23010018)

- [Lungberg and Lee. A Unified Approach to Interpreting Model Predictions. (2017)](https://doi.org/10.48550/arXiv.1705.07874)

- [McCoy et al. Believing in black boxes: machine learning for healthcare does not need explainability to be evidence-based. (2022)](https://doi.org/10.1016/j.jclinepi.2021.11.001)

- [Ng et al. The benefits and pitfalls of machine learning for biomarker discovery. (2023)](https://doi.org/10.1007/s00441-023-03816-z)

- [Obermeyer et al. Dissecting racial bias in an algorithm used to manage the health of populations. (2019)](https://doi.org/10.1126/science.aax2342)

- [Ribeiro et al. "Why Should I Trust You?": Explaining the Predictions of Any Classifier. (2016)](https://doi.org/10.48550/arXiv.1602.04938)

- [Rudin. Stop Explaining Black Box Machine Learning Models for High Stakes Decisions and Use Interpretable Models Instead. (2019)](https://doi.org/10.1038%2Fs42256-019-0048-x)

- [Sarkar. Is explainable AI a race against model complexity? (2022)](https://doi.org/10.48550/arXiv.2205.10119)

- [Sevilla. Parameter counts in Machine Learning. (2021)](https://towardsdatascience.com/parameter-counts-in-machine-learning-a312dc4753d0)
