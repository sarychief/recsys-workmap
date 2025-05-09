# (Градиентный) Бустинг
_Дата: 30.03.2025 22:50_  
**Теги**: #машинное_обучение #

---

## Оглавление  
- [[#(Градиентный) Бустинг|Бустинг]]
	- [[#Алгоритм]]
- [[#Регуляризация]]

---
---

## Подглава 1  
> _Краткое описание (1-2 предложения)._

### Алгоритм  
- 1. Обучаем первое наилучшее неглубокое дерево __на всех данных__ на MSE$$\sum^l_{i=1}(b_1(x_i)-y_i)^2\rightarrow\min_b$$
1. Вычисляем вектор ошибок $s$ на каждом объекте трейна: $$s = [y_1 - b_1(x_1), y_2 - b_1(x_2), ...]=y-a(x)$$
2. Обучаем второе дерево на трейне, но заменяем $y$ на $s$, тем самым вторая модель обучается на ошибках первой: $$\sum^l_{i=1}(b_2(x_i) - s_i)^2\rightarrow\min_b$$
3. Вычисляем вектор ошибок модели из двух деревьев $a(x) = b_1(x) + b_2(x)$: $$s =[y_1 - a(x_1), y_2 - a(x_2), ...]=y-a(x)$$
4. в случае __градиентного__ бустинга мы считаем $s_i$ следующим образом: $$s_i = -\frac{\partial L}{\partial z} |_{z=a_{N-1}(x)}$$
	1. Следует добавить еще шаг градиента в построение и предсказание модели: $$a(x)=\eta\sum^N_{i=1}b_i(x)$$
	2. и лосс: $$L(a(x),y)=\frac{1}{l}\sum^l_{i=1}(s_i - b(x_i))^2\rightarrow\min_b$$

#### Предсказания
Регрессия: Каждая модель предсказывает и усредняем ответы + начальное приближение (обычно среднее значение таргета в обучающей выборке).
Бинарная классификация: Каждая модель предсказывает вероятность отношения к положительному классу объекта + начальный логит ($\log(\frac{p}{1-p})$) и отдаем в сигмоиду

### Важные особенности  
- _**Уменьшаем смещение, в теории увеличивая разброс (можно нет, если тюнить)**_
	- Используем небольшие деревья во избежания скорейшего переобучения
- При увеличении количества базовых моделей рано или поздно начнем переобучаться


### Q&A
##### Спрашиваю коротко
> Отвечаю не обязательно коротко


---
---

## Регуляризация
> _Краткое описание (1-2 предложения)._

### Полное описание  
1. Сокращение шага (медленно, но верно)
	- Если модель тупая (max_depth=3) то можно каждую $b_N(x)$ модель прибавлять с каким-то шагом $\eta \in \{0,1 \}$ $$a_N(x) = a_{N-1}(x) + \eta b_N(x)$$
2. Стохастический градиентный бустинг 
	- Давайте обучать базовые модели на случайных подвыборках


### Важные особенности  
- 
- 

### Q&A
##### Спрашиваю коротко
> Отвечаю не обязательно коротко

---
