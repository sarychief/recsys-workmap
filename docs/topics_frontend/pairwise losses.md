# **Pairwise**  
1. **Hinge Loss**  
   - Сравнивает пары (релевантный vs нерелевантный айтем).  
   - Формула: $$\mathcal{L} = \max(0, 1 - (\hat{y}_{u,i^+} - \hat{y}_{u,i^-}))$$
   - Пример: Ранжирование в SVM-подходах.  

2. **Bayesian Personalized Ranking (BPR)**  
   - Оптимизирует AUC через сравнение пар.  
   - Формула: $$\mathcal{L} = -\sum_{(u,i^+,i^-)} \log \sigma(\hat{y}_{u,i^+} - \hat{y}_{u,i^-})$$
   - Пример: Рекомендации с неявной обратной связью.  

3. **WARP (Weighted Approximate-Rank Pairwise)**  
   - Учитывает позицию в ранге через аппроксимацию.  
   - Формула: $$\mathcal{L} = \sum_{(u,i^+)} \log(1 + \sum_{i^-} \exp(\gamma (\hat{y}_{u,i^-} - \hat{y}_{u,i^+}))$$
   - Пример: Ранжирование с фокусом на топ выдачи.  
