---
title: "A Quick Guide to Explainable AI"
url: "/2024-02-explainable-ai"
summary: "Opening AI's black box to understand how algorithms reason and foster trust."
date: 2024-02-18
---


## What is Explainable AI?

Imagine being denied a loan. This was not because of your credit score or financial history, but because an algorithm deemed you unfit, without any clear explanation. This is not a scene from a dystopian novel; this is a real possibility in today's world where AI systems, from financial services to healthcare, make decisions affecting millions of lives. 

Every year, artificial intelligence (AI) and machine learning models become more complex, thanks to faster computers, quicker algorithms, and more data. Certain models are now composed of hundreds of billions of parameters. This complexity makes them hard if not impossible to understand. 

 <figure> <img style="margin: auto;" src="increasing_number_of_parameters.png" alt="The increasing number of AI model parameters between 1954 and 2024."> <figcaption>The increase in the number of model parameters between 1954 and 2024. Data from <a href="https://towardsdatascience.com/parameter-counts-in-machine-learning-a312dc4753d0">Jaime Sevilla</a>.</figcaption> </figure>

This is where eXplainable AI (XAI) enters the scene. XAI seeks to open black boxes, providing insights into how and why algorithms make their predictions. By allowing us to explore how data inputs influence model outputs, XAI makes AI more transparent, fosters trust and enables both developers and end-users to understand the model's reasoning.

This blog post offers a short introduction to XAI, highlighting its importance and the methodologies involved. For those seeking a deeper dive into the subject, the resources section at the end of this post offers a selection of books and software tools dedicated to XAI.

## Use Cases for Opening the Black Box

![A person stands in a dimly lit room, their silhouette outlined by the soft glow emanating from an open, mysterious black box they hold in their hands.](opening_the_black_box.png)
### Debugging and Improving Models

Predictive models make mistakes. Understanding the root causes that lead to these inaccuracies is essential for enhancing model performance. By helping us understand the rationale behind predictions, XAI techniques help us characterize the situations where the model is making errors. This makes XAI a valuable tool for model debugging and contributes to model refinement.

Consider a scenario involving an animal classification task. XAI techniques, such as [saliency maps](https://en.wikipedia.org/wiki/Saliency_map), can highlight the parts of an image that the classifier considers significant for making a prediction. This analysis might reveal that, in the case of a camel image, the model erroneously concentrates on the sand rather than the camel itself. This would help explain why the model is unable to accurately classify images of camels in other environments.

### Bias Detection and Mitigation

Predictive models can be biased, leading to social inequalities and harming minority groups. Algorithmic biases usually come from using skewed or incomplete data, or from the models' built-in assumptions.

A notable example of such bias was observed in a commercial [algorithm used by the US healthcare system](https://www.science.org/doi/10.1126/science.aax2342#supplementary-materials). Specifically, for identical predicted risk scores, Black patients were found to be sicker than their White counterparts. This bias came from the model assuming that higher healthcare costs meant greater healthcare needs. Since less money was spent on the Black population, the algorithm erroneously concluded they were less at risk. 

Similar instances of algorithmic bias have been documented in the [judicial system](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm) and [job application processes](https://www.bbc.com/news/technology-45809919). These examples highlight the need for accountability in AI systems, especially when deployed in areas with significant societal impacts.

XAI techniques can help identify such biases by contextualizing why models make certain predictions. For instance, in the previously mentioned healthcare example, XAI could demonstrate how being Black lowered the risk score. Therefore, XAI is instrumental not only in interpreting model predictions but also in driving the development of more equitable AI systems.
 
## High Stake Scenarios

Erroneous or biased predictions can profoundly impact individuals' lives and well-being, particularly when used in high-risk scenarios. For instance, when using predictive algorithms for social scoring systems, individual categorization, facial recognition technologies, law enforcement tools, employment screening processes, and the provision of essential services like healthcare.

In practical applications, the explanation can be as important as the prediction itself. For this reason, in high-stakes sectors such as healthcare, finance, and criminal justice, there is a marked preference for employing inherently interpretable models. For example, logistic regression, with its interpretability and simplicity, is often preferred over more complex neural networks within healthcare settings.

### Regulatory Compliance

As the deployment of AI becomes more widespread, laws and regulations are starting to require that AI decisions should be transparent and explainable. This shift aims to guarantee that these algorithms operate within ethical boundaries.

For instance, the European Union's General Data Protection Regulation (GDPR) mandates significant transparency about the logic involved in automated decisions, particularly in decisions that significantly affect individuals, such as those related to employment, creditworthiness, and legal matters. This regulation has been interpreted by some as a _right to explanation_. Building on this foundation, the more recent AI Act has introduced specific provisions for "high-risk" algorithms, with requirements related to "transparency and provision of information to users". 

However, these regulations face challenges in enforcement and interpretation. The inherent complexity of AI algorithms can make it challenging to provide explanations that are both technically accurate and easily understandable to the general public. Despite these challenges, the push for such regulations highlights the growing concern for transparency in AI applications and underscores the critical need for XAI.

### Knowledge Discovery

Using XAI for generating new knowledge is probably the most overlooked application. This capability is particularly interesting given that machine learning models are more apt at identifying complex patterns and trends in data compared to traditional statistical tests such as t-tests or chi-squared tests. This approach was suggested for [biomarker discovery](https://doi.org/10.1007/s00441-023-03816-z).

Despite its potential, employing XAI for knowledge generation is not without challenges. Some of the most significant issues include:

- **Significance Threshold Ambiguity**: Unlike traditional statistical methods, which have well-defined criteria for statistical significance, XAI lacks a standardized framework to determine the significance level of variables.
- **Observational Data Limitations**: The majority of data used in machine learning is observational, which is inherently less effective at establishing causal relationships.
- **Overfitting**: There exists a risk of models mistaking random noise for significant patterns, potentially leading to incorrect interpretations and conclusions regarding the data.

### When Can We Get Away Without an Explanation?

Not all predictions made by AI should be explained. Generating explanations introduces additional software complexity and computational needs, which translate to financial expenses, and environmental impact. Implementing XAI may be unjustified in certain scenarios, such as:

- **Low-Stakes Scenarios**: In situations where the consequences of errors are minimal, such as recommendations on a streaming platform. Users typically have little interest in understanding the inner workings of how these suggestions are generated.
- **Well-Understood Problems**: For well-established problems like spam filtering in email systems, the explanations for each decision may not be essential because the overall reliability and accuracy of the system are already well understood.
- **Risks of Manipulation**: In some cases, providing detailed explanations can unintentionally aid those seeking to exploit the system. For instance, SEO experts might misuse in-depth information about search algorithms to manipulate rankings and undermine the system's integrity.

## Approaches to Explainaing AI

The intricate mathematical details and computations of an algorithm usually don't reveal much about how it makes decisions. This is because the representations learned by the predictive model are typically beyond the human brain's capacity to understand. Because of this, direct approaches, such as disclosing source code or mathematical computations, fall short of providing understanding.

To overcome this challenge, XAI techniques either use inherently interpretable models or approximate complex, black-box models with simpler, more understandable ones. XAI is an active field of research. Currently, there is no universal way to explain complex predictive algorithms, and there might never be one.

Our focus will be on categorizing the techniques used to explain predictive models, highlighting some of the most commonly employed methods.

![An abstract visual representation of a classification of XAI techniques.](classification_of_xai.png)

### Stage: Ante-hoc vs Post-hoc

- **Ante-hoc (Intrinsic Interpretability)**: This approach focuses on using models that are inherently interpretable due to their simple structure and transparent decision-making processes. Examples include linear models, decision trees, and rule-based systems. While these models offer clarity from the outset, their interpretability can diminish as complexity or the number of parameters increases. There is currently research to design high-performing yet inherently interpretable models, however, it has thus far not been very successful.
- **Post-hoc (Extrinsic Interpretability)**: Post-hoc methods aim to explain decisions made by complex, black-box models after they have been trained. These explanations are not built into the model but are derived from analyzing the model's behavior. This approach is particularly useful for black-box models, such as deep neural networks. Post-hoc explanations strive to be locally faithful, meaning they accurately reflect the model's reasoning for specific instances or decisions.

### Model: Model-specific vs Model-agnostic

- **Model-Specific**: These techniques are only applicable to specific types of models, exploiting their unique architectures to provide explanations. For deep learning models, techniques such as Saliency Maps, DeepLIFT, and Class Activation Maps (CAM) illustrate how different features influence the model's predictions.
    
- **Model-Agnostic**: Offering a flexible alternative, model-agnostic methods do not require knowledge of the model's internal workings, making them applicable across a wide range of AI systems. Techniques like [SHAP](https://doi.org/10.48550/arXiv.1705.07874) (SHapley Additive exPlanations) and [LIME](https://github.com/marcotcr/lime)) (Local Interpretable Model-agnostic Explanations) can be used to explain any model by approximating its decision boundary or output in an interpretable way.

### Scope: Global vs Local

- **Local Explanations**: These are focused on individual predictions or decisions, providing insights into why the model made a particular choice for a specific instance. Tools like SHAP and LIME offer local explanations and show the influence of various features on a single outcome.
    
- **Global Explanations**: In contrast, global explanations seek to show the model's behavior across the entire dataset or in a more general sense. Techniques for achieving global explanations include feature importance rankings and partial dependence plots, which aggregate the effects of features across multiple instances to offer a broader understanding of the model's logic.

## The Limitations of XAI

As AI continues to permeate various aspects of our lives, the imperative for transparency and understandability in AI systems has never been more pronounced. Yet, as with any rapidly evolving domain, XAI is not without its challenges and limitations.

![The image depicts a person sitting in front of a massive, complex machine, composed of countless gears, levers, and screens, symbolizing a complex AI.](complex_ai.png)
### The complexity-performance tradeoff

The complexity-performance tradeoff suggests that enhancing AI model performance typically requires increasing their complexity. This assumption underlines the perceived necessity of balancing complexity against performance. This perspective, however, merits critical examination. The [lottery ticket hypothesis](https://doi.org/10.48550/arXiv.1803.03635) proposes that the efficacy of large models might stem from optimal parameter initialization rather than sheer size. Empirically, simpler models can be shown to achieve comparable performance to their more complex counterparts. 

Indeed, evidence shows that models with fewer parameters, such as Mistral-7B, can achieve performance levels comparable to those of their more complex counterparts, like GPT-3.5, with a fraction of the parameters (7 billion vs. 175 billion). This observation challenges the necessity of overly complex models, advocating for a more measured approach to model design where simplicity does not preclude effectiveness.

[Some researchers](https://doi.org/10.1038%2Fs42256-019-0048-x) argue that non-inherently interpretable models should be avoided altogether in high-stakes scenarios. However, it remains unclear whether this is feasible across all contexts without sacrificing performance.

### Explaining the Explanation 

For effective implementation of XAI techniques, it is crucial to understand who is the target audience for the explanations. XAI primarily serves technical professionals, such as data scientists and machine learning engineers, who have a deep understanding of statistics.

However, a significant distinction exists between explanations suitable for AI experts and those for non-technical users. Technical details can overwhelm non-technical users, leading to confusion or misinterpretation. For example, someone with limited knowledge of statistics might mistakenly interpret SHAP values as indicating causality, rather than merely correlation.

Acknowledging the end-user's knowledge, goals, skills, and abilities is paramount when using XAI. Moreover, communication of AI explanations should be aligned with the audience's level of understanding. This is necessary to avoid both the risks of oversimplification, which can lead to misunderstandings, and obfuscation, which can alienate and frustrate users.

### Faithfulness and the Limits of Explanation

Post-hoc explanations of complex models are achieved by locally approximating their behavior around a specific data point using a simpler, transparent model. While these approximations offer insights, they fall short of fully capturing the original model's behavior. Furthermore, XAI methods often lack metrics to evaluate the quality of these approximations, raising questions about the faithfulness of such explanations.

Additionally, as the complexity of models increases annually, our ability to explain them with accuracy diminishes. This escalating complexity highlights a crucial question: Is there a theoretical limit to our capacity to make AI's decision-making processes transparent? And if such a limit exists, how close are we to reaching it?

## Conclusion

XAI plays a critical role in addressing the opacity of complex machine learning models, enabling trust in AI-based decisions. AI facilitates identifying biases, debugging models, complying with regulations, and generating new knowledge by explaining model behavior. Numerous open-source resources are available for the easy and efficient implementation of XAI. Nonetheless, it's important to recognize the limitations and challenges associated with XAI and favor simpler models whenever possible.

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

- [Jiang and Senge. On Two XAI Cultures: A Case Study of Non-technical Explanations in Deployed AI System. (2021)](https://doi.org/10.48550/arXiv.2112.01016)

- [Gunning et al. DARPA's explainable AI (XAI) program: A retrospective. (2021)](https://doi.org/10.1002/ail2.61)

- [Larson et al. How We Analyzed the COMPAS Recidivism Algorithm. (2016)](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm)

- [Linardatos et al. Explainable AI: A Review of Machine Learning Interpretability Methods. (2020)](https://doi.org/10.3390/e23010018)

- [Lungberg and Lee. A Unified Approach to Interpreting Model Predictions. (2017)](https://doi.org/10.48550/arXiv.1705.07874)

- [Ng et al. The benefits and pitfalls of machine learning for biomarker discovery. (2023)](https://doi.org/10.1007/s00441-023-03816-z)

- [Obermeyer et al. Dissecting racial bias in an algorithm used to manage the health of populations. (2019)](https://doi.org/10.1126/science.aax2342)

- [Ribeiro et al. "Why Should I Trust You?": Explaining the Predictions of Any Classifier. (2016)](https://doi.org/10.48550/arXiv.1602.04938)

- [Rudin. Stop Explaining Black Box Machine Learning Models for High Stakes Decisions and Use Interpretable Models Instead. (2019)](https://doi.org/10.1038%2Fs42256-019-0048-x)

- [Sarkar. Is explainable AI a race against model complexity? (2022)](https://doi.org/10.48550/arXiv.2205.10119)

- [Sevilla. Parameter counts in Machine Learning. (2021)](https://towardsdatascience.com/parameter-counts-in-machine-learning-a312dc4753d0)
