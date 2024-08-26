### Normalized Discounted Cumulative Gain (NDCG)
NDCG оценивает качество ранжирования, учитывая релевантность и позиции документов в ранжированном списке.

- **DCG@k (Discounted Cumulative Gain)**: 
  $\Large\text{DCG@k} = \sum_{i=1}^k \frac{2^{\text{релевантность}(i)} - 1}{\log_2(i + 1)}$
  где $\text{релевантность}(i)$ — релевантность документа на позиции \(i\).

- **IDCG@k (Ideal DCG)**: DCG для идеально ранжированного списка.
- **NDCG@k**: $\Large\text{NDCG@k} = \frac{\text{DCG@k}}{\text{IDCG@k}}$
