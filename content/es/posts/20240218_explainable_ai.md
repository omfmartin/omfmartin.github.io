---
title: "Una Guía Rápida sobre la IA Explicable"
url: "/es/posts/2024-02-18-explainable-ai"
summary: "Abriendo la caja negra de la IA para entender cómo razonan los algoritmos y fomentar la confianza."
date: 2024-02-18
---

## ¿Qué es la IA Explicable?

Cada año, la inteligencia artificial (IA) y los modelos de aprendizaje automático se vuelven más complejos, gracias a ordenadores más rápidos, algoritmos más ágiles y más datos. Ciertos modelos ahora se componen de cientos de miles de millones de parámetros. Esta complejidad les hace difíciles, si no imposibles, de entender.

 <figure>
    <img style="margin: auto;" 
        src="/posts/2024-02-18-explainable-ai/increasing_number_of_parameters.png" 
        alt="El aumento del número de parámetros en modelos de IA entre 1954 y 2024.">
    <figcaption>El aumento en el número de parámetros del modelo entre 1954 y 2024. Datos de <a href="https://towardsdatascience.com/parameter-counts-in-machine-learning-a312dc4753d0">Jaime Sevilla</a>.
    </figcaption> 
</figure>

Aquí es donde entra en escena la IA explicable (XAI, por sus siglas en inglés). La IA explicable busca abrir cajas negras, proporcionando _insights_ sobre cómo y por qué los algoritmos hacen sus predicciones. Al permitirnos explorar cómo las entradas de datos influyen en las predicciones del modelo, la IA explicable hace que la IA sea más transparente, fomenta la confianza y permite que tanto los desarrolladores como los usuarios finales comprendan el razonamiento del modelo.

Este artículo ofrece una breve introducción a la IA explicable, destacando su importancia y las metodologías involucradas. Para aquellos que busquen un análisis más profundo del tema, la sección de recursos al final de este artículo ofrece una selección de libros y herramientas de software dedicadas a IA explicable.

## Casos de Uso para Abrir la Caja Negra

![Una persona se encuentra en una habitación tenuemente iluminada, su silueta delineada por el suave resplandor que emana de una misteriosa caja negra abierta que sostiene en sus manos.](/posts/2024-02-18-explainable-ai/opening_the_black_box.png)

### Depuración y Mejora de Modelos

Los modelos predictivos cometen errores. Comprender las causas raíz que conducen a estas inexactitudes es esencial para mejorar el rendimiento del modelo. Al ayudarnos a entender el razonamiento detrás de las predicciones, las técnicas de IA explicable nos ayudan a caracterizar las situaciones donde el modelo está cometiendo errores. Esto hace de la IA explicable una herramienta valiosa para la depuración de modelos y contribuye al refinamiento del modelo.

Consideremos un escenario que involucra una tarea de clasificación de animales. Las técnicas de IA explicable, como los [mapas de saliencia](https://en.wikipedia.org/wiki/Saliency_map), pueden resaltar las partes de una imagen que el clasificador considera significativas para hacer una predicción. Este análisis podría revelar que, en el caso de una imagen de un camello, el modelo erróneamente se concentra en la arena en lugar del camello en sí. Esto ayudaría a explicar por qué el modelo es incapaz de clasificar con precisión imágenes de camellos en otros entornos.

### Detección y Mitigación de Sesgos

Los modelos predictivos pueden estar sesgados, lo que conduce a desigualdades sociales y perjudica a los grupos minoritarios. Los sesgos algorítmicos suelen provenir del uso de datos sesgados o incompletos, o de las suposiciones integradas en los modelos.

Un ejemplo notable de tal sesgo se observó en un [algoritmo comercial utilizado por el sistema de salud de EE.UU.](https://www.science.org/doi/10.1126/science.aax2342#supplementary-materials). Específicamente, para idénticas puntuaciones de riesgo predichas, se encontró que los pacientes negros estaban más enfermos que sus homólogos blancos. Este sesgo provino del modelo que suponía que mayores costos de atención médica significaban mayores necesidades de atención médica. Dado que se gastaba menos dinero en la población negra, el algoritmo concluyó erróneamente que corrían menos riesgo.

Instancias similares de sesgo algorítmico han sido documentadas en el [sistema judicial](https://www.propublica.org/article/how-we-analyzed-the-compas-recidivism-algorithm) y [procesos de solicitud de empleo](https://www.bbc.com/news/technology-45809919). Estos ejemplos destacan la necesidad de responsabilidad en los sistemas de IA, especialmente cuando se despliegan en áreas con impactos sociales significativos.

Las técnicas de IA explicable pueden ayudar a identificar estos sesgos contextualizando por qué los modelos hacen ciertas predicciones. Por ejemplo, en el ejemplo de atención médica mencionado anteriormente, la IA explicable podría demostrar cómo ser negro bajaba la puntuación de riesgo. Por lo tanto, la IA explicable es instrumental no solo en la interpretación de predicciones de modelos sino también en impulsar el desarrollo de sistemas de IA más equitativos.

## Escenarios de Alto Riesgo

Predicciones erróneas o sesgadas pueden impactar profundamente las vidas y el bienestar de los individuos, especialmente cuando se utilizan en escenarios de alto riesgo. Por ejemplo, al usar algoritmos predictivos para sistemas de puntuación social, categorización individual, tecnologías de reconocimiento facial, herramientas de aplicación de la ley, procesos de selección de empleo y la provisión de servicios esenciales como la atención sanitaria.

En la práctica, la explicación puede ser tan importante como la predicción. Por esta razón, en sectores de alto riesgo como la atención sanitaria, finanzas y justicia penal, hay una marcada preferencia por emplear modelos inherentemente interpretables. Por ejemplo, la regresión logística, con su interpretabilidad y simplicidad, a menudo se prefiere sobre redes neuronales más complejas dentro de los entornos de atención sanitaria.

### Cumplimiento Regulatorio

A medida que el despliegue de la IA se vuelve más generalizado, las leyes y regulaciones están comenzando a requerir que las decisiones de IA sean transparentes y explicables. Este cambio tiene como objetivo garantizar que estos algoritmos operen dentro de límites éticos.

Por ejemplo, el Reglamento General de Protección de Datos (GDPR) de la Unión Europea exige una transparencia significativa sobre la lógica involucrada en las decisiones automatizadas, particularmente en decisiones que afectan significativamente a los individuos, como aquellas relacionadas con el empleo, la solvencia y asuntos legales. Esta regulación ha sido interpretada por algunos como un _derecho a la explicación_. Basándose en esta base, el más reciente _AI Act_ ha introducido disposiciones específicas para algoritmos de "alto riesgo", con requisitos relacionados con la "transparencia y provisión de información a los usuarios".

Sin embargo, estas regulaciones enfrentan desafíos en la aplicación e interpretación. La complejidad inherente de los algoritmos de IA puede hacer que sea desafiante proporcionar explicaciones que sean técnicamente precisas y fácilmente comprensibles para el público general. A pesar de estos desafíos, la presión por tales regulaciones destaca la creciente preocupación por la transparencia en las aplicaciones de IA y subraya la necesidad crítica de la IA explicable.

### Descubrimiento de Conocimiento

El uso de la IA explicable para generar nuevo conocimiento es probablemente la aplicación más pasada por alto. Esta capacidad es particularmente interesante dado que los modelos de aprendizaje automático son más aptos para identificar patrones y tendencias complejas en los datos en comparación con pruebas estadísticas tradicionales como las pruebas t o chi-cuadrado. Este enfoque fue sugerido para [el descubrimiento de biomarcadores](https://doi.org/10.1007/s00441-023-03816-z).

A pesar de su potencial, emplear la IA explicable para la generación de conocimiento no está exento de desafíos. Algunos de los problemas más significativos incluyen:

- **Ambigüedad del Umbral de Significancia**: A diferencia de los métodos estadísticos tradicionales, que tienen criterios bien definidos para la significancia estadística, la IA explicable carece de un marco estandarizado para determinar el nivel de significancia de las variables.
- **Limitaciones de Datos Observacionales**: La mayoría de los datos utilizados en el aprendizaje automático son observacionales, lo que es inherentemente menos efectivo para establecer relaciones causales.
- **Sobreajuste**: Existe el riesgo de que los modelos confundan el ruido aleatorio con patrones significativos, lo que potencialmente podría llevar a interpretaciones y conclusiones incorrectas sobre los datos.

### ¿Cuándo podemos prescindir de una explicación?

No todas las predicciones hechas por la IA necesitan ser explicadas. Generar explicaciones introduce complejidad adicional y necesidades computacionales, que se traducen en gastos financieros e impacto ambiental. Implementar IA explicable puede ser injustificado en ciertos escenarios, tales como:

- **Escenarios de Bajo Riesgo**: En situaciones donde las consecuencias de los errores son mínimas, como las recomendaciones en una plataforma de streaming. Los usuarios típicamente tienen poco interés en entender el funcionamiento interno de cómo se generan estas sugerencias.
- **Problemas Bien Entendidos**: Para problemas bien establecidos como el filtrado de spam en sistemas de correo electrónico, las explicaciones para cada decisión pueden no ser esenciales porque la fiabilidad y precisión general del sistema ya son bien comprendidas.
- **Riesgos de Manipulación**: En algunos casos, proporcionar explicaciones detalladas puede ayudar involuntariamente a aquellos que buscan explotar el sistema. Por ejemplo, los expertos en SEO podrían malusar información detallada sobre algoritmos de búsqueda para manipular los rankings y socavar la integridad del sistema.

## Enfoques para Explicar la IA

Los intrincados detalles matemáticos y cálculos de un algoritmo usualmente no revelan mucho sobre cómo toma decisiones. Esto se debe a que las representaciones aprendidas por el modelo predictivo típicamente están más allá de la capacidad de comprensión del cerebro humano. Debido a esto, los enfoques directos, como divulgar el código fuente o los cálculos matemáticos, no logran proporcionar comprensión.

Para superar este desafío, las técnicas de IA explicable utilizan modelos inherentemente interpretables o aproximan modelos complejos de caja negra con otros más simples y comprensibles. La IA explicable es un campo activo de investigación. Actualmente, no existe una manera universal de explicar algoritmos predictivos complejos, y podría nunca haberla.

Nuestro enfoque será en categorizar las técnicas utilizadas para explicar modelos predictivos, destacando algunos de los métodos más comúnmente empleados.

![Una representación visual abstracta de una clasificación de técnicas de IA explicable.](/posts/2024-02-18-explainable-ai/classification_of_xai.png)

### Etapa: Ante-hoc vs Post-hoc

- **Ante-hoc (Interpretabilidad Intrínseca)**: Este enfoque se centra en el uso de modelos que son inherentemente interpretables debido a su estructura simple y procesos de toma de decisiones transparentes. Ejemplos incluyen modelos lineales, árboles de decisión y sistemas basados en reglas. Aunque estos modelos ofrecen claridad desde el principio, su interpretabilidad puede disminuir a medida que la complejidad o el número de parámetros aumenta. Actualmente hay investigaciones para diseñar modelos de alto rendimiento pero inherentemente interpretables; sin embargo, hasta ahora no han tenido mucho éxito.
- **Post-hoc (Interpretabilidad Extrínseca)**: Los métodos post-hoc tienen como objetivo explicar decisiones tomadas por modelos complejos, cajas negras, después de haber sido entrenados. Estas explicaciones no están integradas en el modelo sino que se derivan de analizar el comportamiento del modelo. Este enfoque es particularmente útil para modelos de cajas negras, como las redes neuronales profundas. Las explicaciones post-hoc buscan ser localmente fieles, lo que significa que reflejan con precisión el razonamiento del modelo para instancias o decisiones específicas.

### Modelo: Específico del Modelo vs Agnóstico del Modelo

- **Específico del Modelo**: Estas técnicas solo son aplicables a tipos específicos de modelos, explotando sus arquitecturas únicas para proporcionar explicaciones. Para los modelos de aprendizaje profundo, técnicas como los Mapas de Saliencia, DeepLIFT y los Mapas de Activación de Clase (CAM) ilustran cómo diferentes características influencian las predicciones del modelo.
- **Agnóstico del Modelo**: Estas técnicas ofrecen una alternativa flexible. Los métodos agnósticos del modelo no requieren conocimiento del funcionamiento interno del modelo, haciéndolos aplicables a una amplia gama de sistemas de IA. Técnicas como [SHAP](https://doi.org/10.48550/arXiv.1705.07874) (SHapley Additive exPlanations) y [LIME](https://github.com/marcotcr/lime) (Explicaciones Locales Interpretables Agnósticas del Modelo) pueden usarse para explicar cualquier modelo aproximando su límite de decisión o salida de una manera interpretable.

### Alcance: Global vs Local

- **Explicaciones Locales**: Estas se centran en predicciones o decisiones individuales, proporcionando _insights_ sobre por qué el modelo tomó una elección particular para una instancia específica. Herramientas como SHAP y LIME ofrecen explicaciones locales y muestran la influencia de varias características en un único resultado.
- **Explicaciones Globales**: En contraste, las explicaciones globales buscan mostrar el comportamiento del modelo a través del conjunto de datos completo o en un sentido más general. Técnicas para lograr explicaciones globales incluyen rankings de importancia de características y gráficas de dependencia parcial, que agregan los efectos de las características a través de múltiples instancias para ofrecer una comprensión más amplia de la lógica del modelo.

## Las Limitaciones de la IA explicable

A medida que la IA continúa permeando varios aspectos de nuestras vidas, el imperativo de transparencia y comprensibilidad en los sistemas de IA nunca ha sido más pronunciado. Sin embargo, como en cualquier dominio en rápida evolución, la IA explicable no está exento de desafíos y limitaciones.

![La imagen muestra a una persona sentada frente a una máquina masiva y compleja, compuesta por innumerables engranajes, palancas y pantallas, simbolizando una IA compleja.](/posts/2024-02-18-explainable-ai/complex_ai.png)

### El _tradeoff_ entre complejidad y rendimiento

El _tradeoff_ entre complejidad y rendimiento sugiere que mejorar el rendimiento de los modelos de IA generalmente requiere aumentar su complejidad. Esta suposición subraya la necesidad percibida de equilibrar la complejidad contra el rendimiento. Sin embargo, esta perspectiva merece un examen crítico. La [hipótesis del boleto de lotería](https://doi.org/10.48550/arXiv.1803.03635) propone que la eficacia de los modelos grandes podría derivarse de una inicialización óptima de parámetros en lugar de su mero tamaño. Empíricamente, se ha demostrado que modelos más simples pueden alcanzar un rendimiento comparable al de sus contrapartes más complejas.

De hecho, la evidencia muestra que modelos con menos parámetros, como Mistral-7B, pueden alcanzar niveles de rendimiento comparables a los de sus contrapartes más complejas, como GPT-3.5, con una fracción de los parámetros (7 mil millones vs. 175 mil millones). Esta observación desafía la necesidad de modelos excesivamente complejos, abogando por un enfoque más medido en el diseño de modelos donde la simplicidad no excluye la efectividad.

[Algunos investigadores](https://doi.org/10.1038%2Fs42256-019-0048-x) argumentan que los modelos no inherentemente interpretables deberían evitarse por completo en escenarios de alto riesgo. Sin embargo, sigue sin estar claro si esto es factible en todos los contextos sin sacrificar el rendimiento.

### Explicando la Explicación

Para una implementación efectiva de las técnicas de IA explicable, es crucial entender quién es el público objetivo de las explicaciones. La IA explicable sirve principalmente a profesionales técnicos, como científicos de datos e ingenieros de aprendizaje automático, que tienen un profundo entendimiento de estadística.

Sin embargo, existe una distinción significativa entre las explicaciones adecuadas para expertos en IA y aquellas para usuarios no técnicos. Los detalles técnicos pueden abrumar a los usuarios no técnicos, llevando a la confusión o malinterpretación. Por ejemplo, alguien con conocimiento limitado de estadística podría interpretar erróneamente los valores SHAP como indicativos de causalidad, en lugar de mera correlación.

Reconocer el conocimiento, objetivos, habilidades y capacidades del usuario final es primordial al usar la IA explicable. Además, la comunicación de las explicaciones de IA debe estar alineada con el nivel de comprensión de la audiencia. Esto es necesario para evitar tanto los riesgos de simplificación excesiva, que pueden llevar a malentendidos, como la ofuscación, que puede alienar y frustrar a los usuarios.

### Fidelidad y los Límites de la Explicación

Las explicaciones post-hoc de modelos complejos se logran aproximando localmente su comportamiento alrededor de un punto de datos específico utilizando un modelo más simple y transparente. Aunque estas aproximaciones ofrecen _insights_, no logran capturar completamente el comportamiento del modelo original. Además, los métodos de IA explicable a menudo carecen de métricas para evaluar la calidad de estas aproximaciones, planteando preguntas sobre la fidelidad de tales explicaciones.

Adicionalmente, a medida que la complejidad de los modelos aumenta anualmente, nuestra capacidad para explicarlos con precisión disminuye. Esta creciente complejidad destaca una pregunta crucial: ¿Existe un límite teórico a nuestra capacidad para hacer transparentes los procesos de toma de decisiones de la IA? Y si tal límite existe, ¿cuán cerca estamos de alcanzarlo?

## Conclusión

La IA explicable juega un papel crítico en abordar la opacidad de los complejos modelos de aprendizaje automático, permitiendo confiar en las decisiones basadas en IA. La IA facilita identificar sesgos, depurar modelos, cumplir con regulaciones y generar nuevo conocimiento explicando el comportamiento del modelo. Existen numerosos recursos de código abierto disponibles para la implementación fácil y eficiente de IA explicable. No obstante, es importante reconocer las limitaciones y desafíos asociados con IA explicable y favorecer modelos más simples siempre que sea posible.

## Recursos

### Libros

- [Análisis de Modelos Explicativos](https://ema.drwhy.ai/)
- [Aprendizaje Automático Interpretable](https://christophm.github.io/interpretable-ml-book/)

### Software

- [SHAP](https://shap.readthedocs.io/en/latest/)
- [LIME](https://github.com/marcotcr/lime)
- [DALEX](https://github.com/ModelOriented/DALEX)
- [XAITK](https://xaitk.org/)
- [InterpretML](https://interpret.ml/)

## Bibliografía

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
