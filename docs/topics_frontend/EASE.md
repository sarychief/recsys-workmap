# EASE

Алгоритм EASE (Embarrassingly Shallow Autoencoders) базируется на идее использования линейного регрессионного подхода для предсказания рейтингов пользователей. 

### Основная идея

Алгоритм EASE пытается предсказать рейтинг каждого пользователя для каждого товара, используя информацию о всех других товарах, которые пользователь оценил. Важным аспектом этого подхода является то, что он строит глобальную модель для всех пользователей сразу, в отличие от коллаборативной фильтрации, которая может строить отдельные модели для каждого пользователя.

### Математическое описание

Пусть у нас есть матрица рейтингов $R$ размерности $n \times m$, где  $n$ — количество пользователей, а $m$ — количество товаров. Элементы этой матрицы $R_{ui}$ обозначают рейтинг, который пользователь $u$ дал товару $i$.

Целью является построение модели, которая предсказывает рейтинги, основываясь на информации о других товарах. В контексте EASE модель будет предсказывать рейтинг $\hat{R}_{ui}$ как линейную комбинацию других товаров:

$$\hat R_{ui} = \sum_{j \neq i} W_{ij} R_{uj}$$

где $W$ — это матрица весов, определяющая вклад каждого товара $j$ в предсказание рейтинга для товара $i$.

### Оптимизация

Для нахождения оптимальных весов $W_{ij}$ используется метод минимизации функции потерь, которая измеряет разницу между предсказанными и фактическими рейтингами. В данном случае, используется обычная регрессия:

$$\min_W \sum_{u=1}^n \sum_{i=1}^m \left( R_{ui} - \hat R_{ui} \right)^2 + \lambda \sum_{i=1}^m \sum_{j=1}^m W_{ij}^2$$

где $\lambda$ — гиперпараметр регуляризации.

### Решение
Давайте рассмотрим пример, чтобы лучше понять, как работает алгоритм EASE.

### Исходные данные

Предположим, у нас есть матрица рейтингов $R$ размером $4 \times 5$, где 4 — это количество пользователей, а 5 — количество товаров. Элементы матрицы $R_{ui}$ обозначают рейтинг, который пользователь $u$ дал товару $i$. Если пользователь не оценивал товар, значение будет равно 0.

| Пользователь | Товар 1 | Товар 2 | Товар 3 | Товар 4 | Товар 5 |
|--------------|---------|---------|---------|---------|---------|
| 1            | 5       | 3       | 4       | 0       | 2       |
| 2            | 4       | 0       | 0       | 2       | 1       |
| 3            | 0       | 2       | 1       | 4       | 0       |
| 4            | 1       | 0       | 5       | 4       | 3       |

### Шаг 1: Формирование регрессионной модели для одного товара

Рассмотрим предсказание рейтинга для товара 1. Мы хотим выразить его как линейную комбинацию рейтингов для других товаров. Иными словами, мы ищем такие веса $W_{1j}$, чтобы:

$$\hat R_{u1} = W_{1 2} R_{u 2} + W_{1 3} R_{u 3} + W_{1 4} R_{u 4} + W_{1 5} R_{u 5}$$

### Шаг 2: Формирование матриц

Для товара 1 матрица $X$ (данные для других товаров) будет следующей:

$$X = \begin{pmatrix} 3 & 4 & 0 & 2 \\ 0 & 0 & 2 & 1 \\ 2 & 1 & 4 & 0 \\ 0 & 5 & 4 & 3 \end{pmatrix}$$

А вектор $y$ (оценки для товара 1) будет:

$$y = \begin{pmatrix} 5 \\ 4 \\ 0 \\ 1 \end{pmatrix}$$

### Шаг 3: Решение задачи линейной регрессии

Наша задача сводится к минимизации следующей функции:

$$\min_{W_{12}, W_{13}, W_{14}, W_{15}} \sum_{u=1}^4 \left( y_u - \sum_{j=2}^5 W_{1j} X_{uj} \right)^2 + \lambda \sum_{j=2}^5 W_{1j}^2$$

Решая эту задачу (что можно сделать численно), мы получаем оптимальные веса $W_{1j}$ для товара 1. В данном примере мы не будем вычислять конкретные значения вручную, но процесс сводится к умножению матриц и вычислению обратной матрицы для решения системы уравнений.

### Шаг 4: Предсказание

После нахождения весов $W_{ij}$ мы можем использовать их для предсказания недостающих значений. Например, если бы пользователь 1 не оценивал товар 1, мы могли бы предсказать его рейтинг, используя найденные веса и имеющиеся оценки для других товаров.

### Обобщение для всех товаров

Такой процесс повторяется для каждого товара, и в итоге мы получаем полную матрицу весов $W$, которая используется для предсказания всех недостающих значений в матрице $R$.

### Примерное значение матрицы весов $W$

Предположим, что после выполнения линейной регрессии мы получили следующую матрицу весов $W$:

$$ W = \begin{pmatrix} 0 & -0.2 & 0.3 & 0.1 & 0.4 \\ -0.2 & 0 & 0.5 & -0.1 & 0.2 \\ 0.3 & 0.5 & 0 & 0.4 & -0.3 \\ 0.1 & -0.1 & 0.4 & 0 & 0.6 \\ 0.4 & 0.2 & -0.3 & 0.6 & 0 \end{pmatrix} $$

### Пример предсказанияe

Теперь, если мы хотим предсказать рейтинг пользователя 1 для товара 4 (который он не оценивал), мы можем использовать веса $W$ и известные рейтинги:

$$\hat{R}_{14} = 0.1 \cdot 5 + (-0.1) \cdot 3 + 0.4 \cdot 4 + 0.6 \cdot 2 = 0.5 - 0.3 + 1.6 + 1.2 = 3$$

Таким образом, предсказанный рейтинг пользователя 1 для товара 4 будет равен 3.

### Преимущества

1. **Простота**: Модель чрезвычайно проста в реализации и не требует сложной настройки гиперпараметров.
2. **Эффективность**: Благодаря линейной природе, EASE является очень быстрым и масштабируемым методом.
3. **Интерпретируемость**: Весовые коэффициенты $W$ могут интерпретироваться как меры влияния одного товара на другой.

### Заключение

Алгоритм EASE представляет собой простой и эффективный подход для задач рекомендательных систем, который сочетает в себе идеи линейной регрессии и $\ell_2$-регуляризации. Этот метод демонстрирует хорошие результаты на практике и может служить отличной альтернативой более сложным моделям, особенно в условиях ограниченных вычислительных ресурсов.
