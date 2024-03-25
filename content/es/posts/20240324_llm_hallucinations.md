---
title: "Más allá de la Ilusión: Entender y Mitigar las Alucinaciones de los LLMs"
url: "/es/posts/2024-03-24-llm-hallucination"
summary: "¿Cómo podemos detectar y mitigar las alucinaciones para un despliegue más seguro de los LLMs?"
date: 2024-03-24
---

**Los Modelos de Lenguaje de Gran Tamaño (LLMs) inventan cosas**. Ocasionalmente, estos modelos generarán contenido que, aunque parezca totalmente plausible, es fundamentalmente incorrecto. Este fenómeno se conoce como "alucinaciones".

**Las alucinaciones pueden tener consecuencias desastrosas**. Pueden difundir información errónea, potencialmente ofender a usuarios, e incluso comprometer información sensible. Este problema es un gran inconveniente de los LLMs y es la razón por la que muchos son cautelosos a la hora de emplearlos en entornos del mundo real.

En este artículo, discutiremos qué son las alucinaciones, explicaremos por qué ocurren y exploraremos estrategias para detectarlas y mitigarlas.

![Una pintura surrealista y psicodélica que retrata un modelo lingüístico embarcándose en un viaje alucinatorio y alterador de la mente.](/posts/2024-03-24-llm-hallucination/hallucinating_llm.png)

## ¿Qué son las Alucinaciones?

Las alucinaciones pueden definirse de manera general como respuestas que son **factualmente incorrectas, infieles a la información proporcionada, o sin sentido**. La mayoría de las veces, el modelo parecerá bastante seguro y su respuesta puede incluso parecer totalmente plausible. Esto hace que detectar alucinaciones sea una tarea difícil. Generalmente, las alucinaciones se dividen en dos categorías: intrínsecas y extrínsecas.

**Las alucinaciones intrínsecas** ocurren cuando la respuesta del modelo contradice directamente la información que se le dio. Por ejemplo:

>>> **Prompt**: La capital de Francia es París. ¿Cuál es la capital de Francia?
**Respuesta**: La capital de Francia es Lyon.

**Las alucinaciones extrínsecas** se producen cuando el modelo realiza afirmaciones que no pueden ser verificadas ni contradichas con la información proporcionada. Por ejemplo:

>>> **Prompt**: Describe el impacto económico de las fuentes de energía renovable.
**Respuesta**: Según un informe de 2045, las fuentes de energía renovable han llevado a la creación de 5 millones de nuevos empleos en todo el mundo.

## ¿Por qué los LLMs Alucinan?

Se considera que las alucinaciones en los modelos de lenguaje son inevitables. Pueden ocurrir debido a una variedad de factores, los cuales se pueden clasificar de manera general en problemas relacionados con los datos, el proceso de entrenamiento y la inferencia.

### Causas Relacionadas con los Datos

- **Inexactitudes en los Datos de Entrenamiento**: Los datos de entrenamiento son la base de los LLMs. Sin embargo, los datos no siempre son de la mejor calidad y pueden ser inexactos o sesgados. Dada la vasta cantidad de datos procesados, asegurar su precisión y relevancia es un desafío. Esto implica que parte de la información aprendida por el modelo lo llevará a generar respuestas engañosas o incorrectas.
- **Consultas Fuera de Distribución**: Los LLMs se entrenan en conjuntos de datos que cubren ciertos temas, períodos de tiempo y tipos de información. Cuando se enfrentan a preguntas o tareas que se encuentran fuera de estos límites, un modelo de lenguaje puede generar respuestas incorrectas. Por ejemplo, preguntar sobre las últimas noticias resultará en respuestas basadas en información desactualizada o irrelevante.
- **Problemas de Utilización de Datos**: Cómo los modelos de lenguaje utilizan su base de conocimientos también puede contribuir a las alucinaciones. Los modelos de lenguaje tienden a depender de patrones de co-ocurrencia de palabras para generar respuestas. Esta dependencia podría llevar a conclusiones incorrectas, como identificar a Sídney como la capital de Australia debido a su asociación más frecuente en comparación con la respuesta correcta, Canberra.

### Causas Relacionadas con el Entrenamiento

- **Sesgo de Exposición**: Este problema surge de una discrepancia entre cómo se entrena al modelo y el uso real del modelo. Durante el entrenamiento, el modelo aprende a predecir el siguiente _token_ confiando en la secuencia correcta anterior de los datos de entrenamiento, un método conocido como "enseñanza forzada". Sin embargo, durante la inferencia, el modelo depende de sus propias predicciones. Esto puede llevar a una acumulación de errores o a un efecto bola de nieve.
- **Limitaciones Arquitectónicas**: Los modelos de lenguaje causal generan texto prediciendo el siguiente token basándose únicamente en los _tokens_ precedentes. Este diseño limita la capacidad del modelo para captar algunas sutilezas contextuales, lo que se cree que contribuye a la generación de contenido alucinado.
- **Desalineación de Creencias**: Los LLM generalmente pasan por un proceso de entrenamiento en dos etapas. Inicialmente, son preentrenados para adquirir un conocimiento amplio. Posteriormente, son ajustados (_fine-tuned_) para alinearse mejor con instrucciones o indicaciones específicas de los usuarios, a menudo utilizando técnicas como el [aprendizaje por refuerzo](https://es.wikipedia.org/wiki/Aprendizaje_por_refuerzo). Este ajuste podría llevar inadvertidamente al modelo a priorizar las preferencias del usuario sobre la exactitud.

### Causas Relacionadas con la Inferencia

- **Aleatoriedad en el Muestreo**: Las estrategias de descodificación de los modelos de lenguaje a menudo incorporan cierta aleatoriedad para favorecer la creatividad y la calidad de la respuesta, por ejemplo, ajustando el parámetro de temperatura para favorecer _tokens_ menos probables. Este enfoque puede aumentar inadvertidamente la probabilidad de generar contenido sin sentido.
- **Cuello de Botella de Softmax**: La última capa de un LLM estima la probabilidad del siguiente _token_. La mayoría de los LLM utilizan una capa softmax, que se sabe que tiene una limitación llamada el cuello de botella de softmax. Esta limitación impide la capacidad del modelo para distinguir eficazmente entre resultados altamente probables.
- **Atención Diluida**: Con secuencias de entrada más largas, la eficiencia del mecanismo de atención del modelo puede disminuir, lo que lleva a una atención debilitada en detalles y contexto esenciales. Esta dilución de la atención compromete la capacidad del modelo para mantenerse coherente y preciso en textos extendidos.

## ¿Cómo podemos detectar las alucinaciones?

Detectar alucinaciones presenta desafíos significativos que las métricas estándar utilizadas en el Procesamiento de Lenguaje Natural (NLP) como [BLEU](https://es.wikipedia.org/wiki/BLEU), [ROUGE](https://es.wikipedia.org/wiki/ROUGE_(m%C3%A9trica)), o [BERTScore](https://en.wikipedia.org/wiki/Sentence_embedding) no solucionan. No solo estas métricas no predicen con precisión las alucinaciones, sino que también dependen de la respuesta de referencia, lo cual es impracticable para aplicaciones generalizadas.

Un enfoque para la detección de alucinaciones es emplear **métricas probabilísticas** que calculan la probabilidad de alucinaciones basadas en probabilidades de _tokens_ o medidas de entropía, por ejemplo, estimando la [perplejidad](https://es.wikipedia.org/wiki/Perplejidad). Sin embargo, estas métricas requieren acceso a las probabilidades de los _tokens_, que pueden no estar disponibles, lo que limita su adopción.

Una estrategia más efectiva ha sido el desarrollo de **métricas basadas en modelos**. Este enfoque implica utilizar otro modelo de lenguaje, por ejemplo, un LLM de última generación, para evaluar la probabilidad de contenido alucinatorio. Estas métricas se pueden clasificar aún más en métricas a nivel de _token_ y a nivel de frase.

Las **métricas a nivel de _token_** intentan predecir para cada _token_ si fue alucinado o no. Para implementar esta estrategia, los investigadores generalmente generan un conjunto de datos alucinados sintéticamente aplicando perturbaciones a conjuntos de datos de referencia libres de alucinaciones. Luego se entrena un clasificador de lenguaje binario, como uno basado en [BERT](https://es.wikipedia.org/wiki/BERT_(modelo_de_lenguaje)) o [XLNet](https://doi.org/10.48550/arXiv.1906.08237), para discernir entre _tokens_ alucinados y no alucinados. Proyectos que utilizan este enfoque son [HADES](https://github.com/microsoft/HaDes) y el pipeline de detección de alucinaciones [fairseq-detect-hallucination](https://github.com/violet-zct/fairseq-detect-hallucination).

Por otro lado, las **métricas a nivel de frase** evalúan la oración completa para determinar la presencia de alucinaciones. Un método destacado es [SelfCheckGPT](https://github.com/potsawee/selfcheckgpt), una técnica basada en el muestreo que se fundamenta en la suposición de que las alucinaciones son más probables de ocurrir cuando el modelo está incierto. Este método evalúa la autoconsistencia comparando varias respuestas muestreadas, utilizando métricas estándar como coincidencia de N-gramas o BERTScore, para estimar la probabilidad de contenido alucinatorio. Una fortaleza de este método es que no requiere ningún dato externo, lo que facilita su escalabilidad.

## Estrategias para Mitigar las Alucinaciones

Las estrategias de mitigación de alucinaciones tienen como objetivo reducir la probabilidad de que un LLM genere contenido alucinatorio. A continuación, exploramos varias técnicas, ordenadas desde ajustes sencillos hasta intervenciones más complejas.

![Visualiza un momento capturando las secuelas de un viaje alucinatorio para un modelo lingüístico.](/posts/2024-03-24-llm-hallucination/llm_waking_up.png)

### 1. Ajustar los Parámetros de Inferencia

Uno de los métodos más sencillos para reducir las alucinaciones es ajustar la configuración operativa del LLM. Por ejemplo, afinar parámetros como la temperatura de generación y el muestreo _top-k_ puede regular la aleatoriedad de la respuesta del LLM. Valores más bajos de temperatura o _top-k_ pueden producir respuestas más cautelosas y conservadoras, minimizando potencialmente las salidas alucinatorias.

### 2. Ingeniería de Prompts

La ingeniería de indicaciones efectiva es crucial para dirigir los LLM hacia respuestas precisas. Los siguientes consejos generales son esenciales para optimizar los _prompts_:

- **Claridad y Precisión**: Asegurar que el _prompt_ sea claro sobre el tipo exacto de información requerida.

- **Reconocimiento de Incertidumbre**: Solicitar explícitamente que el modelo evite inventar información y exprese incertidumbre cuando sea aplicable, por ejemplo diciendo "No lo sé".

- **Énfasis en la Información Clave**: Repetir detalles importantes dentro del _prompt_ puede ayudar a subrayar la importancia de estos elementos. Esto ayuda a asegurar que reciban la debida atención en la respuesta del modelo.

- **Colocación Estratégica de la Información**: Posicionar la información más crucial hacia el final del _prompt_ puede mejorar la atención del modelo en estos detalles clave.

- **Especificación del Formato de Salida**: Dirigir al modelo para que estructure su respuesta en un formato específico puede ayudar a obtener información que está organizada de manera que mejor se adapte a las necesidades del usuario.

Además, técnicas más avanzadas de [ingeniería de _prompts_](https://www.promptingguide.ai) ofrecen capas adicionales de sofisticación:

- **Indicaciones Múltiples**: involucra proporcionar al modelo múltiples ejemplos del resultado deseado. Este enfoque ayuda al modelo a comprender mejor el contexto y alinear sus respuestas con el formato de respuesta esperado.

- **Cadena de Pensamiento**: indica al modelo a mostrar su proceso de razonamiento paso a paso, ofreciendo a los usuarios percepciones sobre la progresión lógica que lleva a la respuesta.

- **Autoconsistencia**: implica generar varias respuestas y seleccionar la que demuestre el mayor grado de consistencia interna, mejorando así la fiabilidad.

- **Cadena de Verificación**: es una técnica donde el modelo se utiliza para verificar sus respuestas o hacer anotaciones, mejorando aún más la precisión y la confiabilidad de la información proporcionada.


### 3. Técnicas Basadas en Recuperación

Estas técnicas mejoran las respuestas del modelo incorporando fuentes de conocimiento externas y contexto a la indicación. Probablemente, la técnica más conocida es **Generación Aumentada por Recuperación (RAG, por sus siglas en inglés)**: Esta técnica codifica documentos externos utilizando [_sentence embeddings_](https://en.wikipedia.org/wiki/Sentence_embedding), generalmente mediante alguna red neuronal basada en transformadores como [BERT](https://es.wikipedia.org/wiki/BERT_(modelo_de_lenguaje)). Las _embeddings_ resultantes se almacenan luego en una [base de datos vectorial](https://en.wikipedia.org/wiki/Vector_database), que luego se puede consultar de manera eficiente usando el _prompt_ original (o alguna variación). Los datos recuperados se utilizan entonces para enriquecer el _prompt_. Una implementación popular es [FAISS](https://github.com/facebookresearch/faiss). El principal inconveniente de estos enfoques basados en recuperación es que requieren mantener una base de datos grande y curada.

### 4. Ajuste Fino de LLM

La calidad de la salida de un modelo depende directamente de sus datos de entrenamiento. Ajustar un LLM con datos actualizados o más relevantes puede abordar debilidades o lagunas específicas en su base de conocimiento. Aunque volver a entrenar desde cero es a menudo impracticable, el ajuste fino ofrece una alternativa rentable para mejorar la precisión del modelo en aplicaciones específicas. Técnicas como [LoRA](https://doi.org/10.48550/arXiv.2106.09685) ayudan a reducir el número de parámetros entrenables lo que acelera el ajuste fino haciéndolo una opción viable.

## Reflexión Final

Hay una línea fina entre la creatividad y las alucinaciones. Por un lado, un modelo que se inclina demasiado hacia la precaución puede evitar las alucinaciones pero a costa de producir respuestas que son poco interesantes o demasiado simplistas. Por otro lado, un modelo excesivamente creativo producirá salidas que se asemejan a [una entrevista con Salvador Dalí](https://www.youtube.com/watch?v=A3FAy0teMNo).

Encontrar el equilibrio correcto es clave para aprovechar el potencial completo de los LLM mientras se minimizan los riesgos asociados con las alucinaciones. En última instancia, entender y mitigar las alucinaciones no es solo sobre prevenir errores; se trata de construir confianza entre los humanos y la inteligencia artificial, asegurando que estas herramientas mejoren nuestros procesos de toma de decisiones, creatividad y acceso a la información.

## Bibliografía

- [Huang et al. A Survey on Hallucination in Large Language Models: Principles, Taxonomy, Challenges, and Open Questions. (2023)](https://doi.org/10.48550/arXiv.2311.05232)
- [Ji et al. Survey of Hallucination in Natural Language Generation. (2022)](https://doi.org/10.1145/3571730)
- [Luo et al. Hallucination Detection and Hallucination Mitigation: An Investigation. (2024)](https://doi.org/10.48550/arXiv.2401.08358)
- [Manakul et al. SelfCheckGPT: Zero-Resource Black-Box Hallucination Detection for Generative Large Language Models. (2023)](https://doi.org/10.48550/arXiv.2303.08896)
- [Xu et al. Hallucination is Inevitable: An Innate Limitation of Large Language Models. (2024)](https://doi.org/10.48550/arXiv.2401.11817)
- [Zhou et al. Detecting Hallucinated Content in Conditional Neural Sequence Generation. (2020)](https://doi.org/10.48550/arXiv.2011.02593)
