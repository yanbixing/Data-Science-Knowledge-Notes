# ML High Level Concept

### ML vs Data Mining:

- Machine Learning: algorithms that can automatically learn with data, without being explicitly programmed.
- Data Mining: extract knowledge from unstructured data. Use a lot of techniques including ML.

### Supervised learning, unsupervised learning and reinforcement learning. (TBD)

- Supervised learning： you have label.
  - The goal of model is to predict label as accurate as possible.
  - Category according to type of label:
    - Classification:output variable is a category
    - Regression: output variable is a real value
- Unsupervised learning: don’t have label, 
  - The goal of model is to study the underlying structure or distribution in the data. (in order to learn more about the data.)
  - Main category according to purpose:
    - Clustering: discover the inherent grouping in the data.
    - Association: discover the rules/pattern in you data.
      - E.g. Collaborative filtering? Anomaly detection?
    - Ref: [Wiki-Unsupervised_learning](https://en.wikipedia.org/wiki/Unsupervised_learning)
    - Specific algorithms:
      - Anomaly detection: [Local_outlier_factor](https://en.wikipedia.org/wiki/Local_outlier_factor)
      - Neural Network: [Generative_adversarial_network](https://en.wikipedia.org/wiki/Generative_adversarial_network)
      - Approaches for learning latent variable ([Latent_variable_model](https://en.wikipedia.org/wiki/Latent_variable_model)), like 
        - [Method_of_moments](https://en.wikipedia.org/wiki/Method_of_moments_(statistics))
        - [PCA](https://en.wikipedia.org/wiki/Principal_component_analysis)
        - [SVD](https://en.wikipedia.org/wiki/Singular_value_decomposition)
- Reinforcement Learning (TBD)
- Semi-supervised learning
  - Use unsupervised learning to help supervised learning.
  - A good example is a photo archive where only some of the images are labeled, (e.g. dog, cat, person) and the majority are unlabeled.




