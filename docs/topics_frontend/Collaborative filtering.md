Идея в получении векторного представления пользователя и продукта на основе данных об их взаимодействия 
$$\Large f^{\text{ф-я оценки}}_{ui}: \text{User} \times \text{Item} \rightarrow \text{Relevance}$$
Далее минимизируем лоссы $R$ матрицы фактических предпочтений (исходная матрица взаимодействий) и $\hat R$ предсказанных
$$\Large L(R, \hat R) \rightarrow \min$$

$$
\large
\begin{bmatrix}
\color{green}{1} & 
\color{blue}{2} & 
\color{blue}{...} & 
\color{blue}{n} & 
\\
\color{orange}{2} & 
&
\color{blue}\downarrow
& 
\color{blue}\downarrow
\\
\color{orange}{...} &
\color{orange}{\rightarrow}
\\
\color{orange}{m} & 
\color{orange}{\rightarrow} & 
\end{bmatrix}^{\color{white}R\in\mathbb{R}^{(m,n)}}

\approx

\color{blue}{\begin{bmatrix}
\color{blue}{\uparrow} & 1
\\
\color{blue}{|} & 2
\\
\color{blue}{|} & ..
\\
\color{blue}{\downarrow} & k
\end{bmatrix}}^{\color{white}P\in\mathbb{R}^{(k, 1)}}
\color{white}

\times

\color{orange}{\begin{bmatrix}
{\leftarrow} & - & - & \rightarrow 
\\
1 & 2 & ... & k
\end{bmatrix}^{\color{white}Q^T\in\mathbb{R}^{(1, k)}}}
$$


  - R: Матрица взаимодействий
  - P: Матрица векторов пользователей (эмбедингов)
  - Q: Матрица векторов объектов (эмбедингов)
  - k: размерность векторного представления

Функция оценки (просто скалярное произведение): $$\Large\hat r_{ui} = p_uq^T_i = \sum p_{ud}q_{di}$$
Функция ошибки (MSE + reg): $$\Large L = \sum_{u,i\in S}(r_{ui} - p_uq^T_i)^2 + \lambda(\sum||p_u||^2 + \sum||q_i||^2)$$

---
## SVD сингулярное разложение

- все значения не должны быть пустые (поэтому заполняем 0)
	- Поэтому не так хорош для предсказаний
- Хорош для топ-N предсказаний
	- Для каждого пользователя считаем скалярное произведение со всеми объектами
	- Выбираем топ объектов по полученным значениям

$$\LARGE A = U\Sigma V^T$$
$$
\begin{bmatrix}
\color{green}{} & 
\color{blue}{} & 
\color{blue}{} & 
\color{blue}{} & 
\\
\color{orange}{} & 
&
\color{blue}
& 
\color{blue}
\\
\color{orange}{} &
\color{orange}{}
\\
\color{orange}{} & 
\color{orange}{} & 
\end{bmatrix}^{\color{white}A \in\mathbb{R}^{(m, n)}}

=

\color{blue}{\begin{bmatrix}
\color{blue}{} & 
\\
\color{blue}{} & 
\\
\color{blue}{} &
\\
\color{blue}{} & 
\end{bmatrix}}^{\color{white}U\in\mathbb{R}^{(m, m)}}_{\color{white}\text{орт. матрица}}
\color{white}

\times

\color{green}{\begin{bmatrix}
\color{blue}{} & &
\\
\color{blue}{} & 

\end{bmatrix}}^{\color{white}\Sigma\in\mathbb{R}^{(m, n)}}_{\color{white}\begin{matrix}\text{диаг. матрица}\\ k = \min(m,n)\end{matrix}}
\color{white}

\times

\color{orange}{\begin{bmatrix}
 &  &  & & & 

\end{bmatrix}^{\color{white}V^T\in\mathbb{R}^{(n, n)}}_{\color{white}\text{орт. матрица}}}
$$
1. **Левые сингулярные векторы (матрица $( U )$)**:
   - Матрица $( U )$ содержит левые сингулярные векторы, которые представляют собой ортонормальные столбцы.
   - Если $( A )$ имеет размер $( m \times n )$, то $( U )$ имеет размер $( m \times m )$.
   - Левые сингулярные векторы являются собственными векторами матрицы $( A A^T )$.

2. **Правые сингулярные векторы (матрица $( V )$)**:
   - Матрица $( V )$ содержит правые сингулярные векторы, которые также являются ортонормальными столбцами.
   - Матрица $( V )$ имеет размер $( n \times n )$.
   - Правые сингулярные векторы являются собственными векторами матрицы $( A^T A )$.

но нам не нужно полное решение `(почему?: мб потому что много считать)` поэтому берем урезанную версию - __Для уменьшения размерности выбирают первые 𝑘 наибольших сингулярных значений. Это приводит к сокращенному разложению__: $$\Large A \approx U_k\Sigma_kV^T_k = \hat R$$
$$
\begin{bmatrix}
\color{green}{} & 
\color{blue}{} & 
\color{blue}{} & 
\color{blue}{} & 
\\
\color{orange}{} & 
&
\color{blue}
& 
\color{blue}
\\
\color{orange}{} &
\color{orange}{}
\\
\color{orange}{} & 
\color{orange}{} & 
\end{bmatrix}^{\color{white}A \in\mathbb{R}^{(m, n)}}

\approx

\color{blue}{\begin{bmatrix}
\color{blue}{} & 
\\
\color{blue}{} & 
\\
\color{blue}{} &
\\
\color{blue}{} & 
\end{bmatrix}}^{\color{white}U_k\in\mathbb{R}^{(m, k)}}_{\color{white}\text{орт. матрица}}
\color{white}

\times

\color{green}{\begin{bmatrix}
\color{blue}{} & &
\\
\color{blue}{} & 

\end{bmatrix}}^{\color{white}\Sigma_k\in\mathbb{R}^{(k, k)}}_{\color{white}\begin{matrix}\text{диаг. матрица}\\ k = \min(m,n)\end{matrix}}
\color{white}

\times

\color{orange}{\begin{bmatrix}
 &  &  & & & 

\end{bmatrix}^{\color{white}V^T_k\in\mathbb{R}^{(n, k)}}_{\color{white}\text{орт. матрица}}}
$$

- $\large U_k \in \mathbb{R}^{(m\times k)}$ — матрица размером $( m \times k )$, содержащая первые $( k )$ столбцов из$( U )$,
- $\large V_k \in \mathbb{R}^{(n\times k)}$ — диагональная матрица размером $( k \times k )$, содержащая первые $( k )$ сингулярных значений на диагонали,
- $\large \Sigma_k \in \mathbb{R}^{(k\times k)}$ — матрица размером $( n \times k )$, содержащая первые $( k )$ столбцов из $( V )$.

В $\Sigma$ хранятся сингулярные значения исходной матрицы. Они показывают степень важности соответствующих сингулярных векторов.

Оптимизация: $\Large ||A_{\text{исходная матрица}} - \hat R_{\text{Полученная урезанная матрица}}||^2_F\rightarrow \min$

Хранить мы можем только $A$ и $V$ потому что: $$\large\begin{matrix} AV_kV^T_k = U\Sigma V^TV_kV^T_k \\ AV_kV^T_k = U_k\Sigma_k V^T_k = \hat R\end{matrix}$$

