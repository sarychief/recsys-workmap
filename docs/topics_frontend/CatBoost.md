# Отличие от градбуста

- Автоматическое кодирование категориальных признаков (и текстовых)
	- сам под капотом по-разному кодирует данные и на них обучает разные модели и потом выбирает лучшую
	- OHE/счетчики
- Использует симметричные деревья
Симметричные деревья в CatBoost имеют одинаковую структуру ветвлений на всех уровнях, что ускоряет обучение и предсказание, а также уменьшает переобучение за счёт меньшего числа возможных разбиений.
	- __не склонны к переобучению__ из-за неизменности структуры от размера трейна
	- __удобнее хранить в памяти__ в виде списка предикатов
- Встроенный детектор переобучения

# Ordered Boosting 
Придуман во избежания Data Shift (тип каждый раз одни и те же данные) и переобучения как следствие 

