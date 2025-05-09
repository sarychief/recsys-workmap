# singular-value decomposition 

Сингулярное разложение (SVD) позволяет представить любую матрицу $A$ в виде произведения трёх специальных матриц:

$A = U \Sigma V^T$

где:

- **$U$** – матрица левых сингулярных векторов (ортогональная),
- **$\Sigma$** – диагональная матрица, содержащая неотрицательные сингулярные значения,
- **$V^T$** – транспонированная матрица правых сингулярных векторов (также ортогональная).

## Алгоритм
Сначала находим собственные значения, на них собственные векторы, а уже по ним восстанавливаем левую и правую матрицу сингулярных векторов

1. **Подготовка матрицы $A$:**  
    Начинаем с матрицы интеракций AA. Если матрица является _разреженной_, то есть имеет отсутствующие (пустые) значения, заполняем их нулями. Это отличается от неполной матрицы, где отсутствующие элементы остаются неопределёнными.
    - Заполняем пробелы
    
2. **Вычисление $A^T A$:**  
    Находим матрицу $A^T A$. Обратите внимание, что эта матрица симметрична и положительно полуопределена, что гарантирует наличие всех собственных значений неотрицательными.
    
4. **Нахождение собственных значений:**  
    Решаем характеристическое уравнение:
    $$\det(A^T A - \lambda I) = 0$$
    
    для определения собственных значений $\lambda$. Эти значения отражают "энергию" каждой компоненты.
    
5. **Сортировка собственных значений:**  
    Полученные собственные значения сортируются в порядке убывания. Это позволяет выделить наиболее значимые компоненты, особенно если используется усечённое разложение для понижения размерности.
    
6. **Нахождение собственных векторов:**  
    Для каждого собственного значения решаем систему уравнений
    $$(A^T A - \lambda I)v = 0$$
    для получения соответствующего собственных вектора $v$. После нормировки векторы становятся столбцами матрицы $V$.
    
7. **Определение сингулярных значений:**  
    Сингулярные значения $\sigma$ матрицы $A$ вычисляются как квадратные корни из собственных значений матрицы $A^T A$:
    $$\sigma = \sqrt{\lambda}$$
    Они располагаются по диагонали матрицы $\Sigma$ в порядке убывания.
    
8. **Вычисление матрицы $U$:**  
    После того как матрицы $V$ и $\Sigma$ известны, матрицу $U$ можно вычислить по формуле:
    
    $$U = A V \Sigma^{-1}$$
    
    Это обеспечивает, что столбцы $U$ будут нормированными векторами, удовлетворяющими ортогональности.
    

Таким образом, алгоритм SVD сводится к поиску собственных значений и собственных векторов матрицы $A^T A$, после чего с их помощью восстанавливаются сингулярные значения и матрицы $V$ и $U$. 
