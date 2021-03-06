# Глава 3. Слепое разделение сигнала

В этой главе рассмотрим детальнее алгоритмы реализующие BSS. Проанализируем достоинства и недостатки и возможности улучшения. Приведем список нерешенных проблем в этой области.

## 3.1. Независимый анализ компонент \(ICA\)

Слепое разделение сигнала можно выполнить с помощью алгоритма независимого анализа компонент.

Предположим что у нас есть $$N$$ источников сигнала $$s_1 (t),s_2 (t),...,s_N (t)$$, где $$t$$ – это дискретный момент времени. В разных точках пространства, но неизвестно где именно, существует M сенсоров $$x_1 (t),x_2 (t),...,x_M (t)$$.

Таким образом задача разделения сигнала – это задача по M значениям сигнала с сенсоров восстановить значения оригинальных N сигналов.

Далее введем вектор-столбец $$\overline{s(t)} = [s_1 (t),s_2 (t),...,s_N (t)]^T$$ входного сигнала и вектор-столбец сенсоров $$\overline{x(t)} = [x_1 (t),x_2 (t),...,x_M (t)]^T$$. Процесс смешивания входных сигналов в среде передачи данных можно представить как линейный оператор $$A[*]$$, тогда $$\overline{x(t)}=A[\overline{s(t)}]+\epsilon(t)$$, где $$\epsilon(t)$$ - аддитивный шум.

Если предположить, что система обратима, то есть должен существовать обратный линейный оператор $$W[*]$$, тогда $$\overline{u(t)}=W[\overline{x(t)}]=W[A[\overline{s(t)}] + \epsilon(t)] \approx \overline{s(t)}$$.

Исходя из грубого предположения, что сенсоры мгновенно получают композицию всех сигналов, можно положить, что $$A$$ – матрица смешивания сигналов, тогда $$\overline{x(t)}=A*\overline{s(t)} + \epsilon(t)$$, где $$A$$ – матрица размера $$M$$ на $$N$$, $$x$$ – столбец размера $$M$$, $$s$$ – столбец размера $$N$$.

В самом простом случае принято считать, что у нас нет аддитивного шума и что количество сенсоров совпадает с количеством источников сигналов. Тогда $$N = M$$ и оператор разложения сигнала $$W$$ представляет собой матрицу, обратную квадратной матрице $$A$$. В реальности вводят параметр качества восстановления сигнала , которая должна быть очень близкой к единичной в идеале.

Чтобы найти матрицу разложения, нужно знать матрицу смешивания сигналов $$A$$, которая тоже неизвестна. Из всех данных доступных для анализа есть только вектор показаний сенсоров $$x$$. Возникает вопрос, как вообще тогда возможно что-либо восстановить. На помощь приходит анализ характеристик сигналов. В итоге исследователями было установлено, что должны выполняться следующие критерии:

1. Сигналы должны быть статистически независимы друг от друга.

2. Максимум только один сигнал из всех может иметь Гауссово распределение. Так как иначе не получится построить матрицу A.

Основными проблемами чистого ICA подхода являются:

1. Невозможно определить порядок независимых компонент. Это проблема неопределенности перестановок.

2. Невозможно вычислить дисперсию независимых компонент. Это проблема неопределенности масштаба. Так как $$A$$ и $$s$$ – неизвестны, то любое скалярное умножение s будет поглощено смешивание сигнала.

Таким образом, задача по восстановлению сигнала сводится к поиску каким-либо образом матрицы смешивания $$A$$ и приблизительных сигналов $$s$$. Основные подходы в решении этой задачи следующие:

1. Минимизация взаимной информации. Так как мы предположили, что сигналы независимы.

2. Минимизация Гауссиано-подобия, так как сигналы не имеют такое распределение по предположению. А по центральной предельной теореме сумма большого числа независимых случайных величин имеет распределение близкое к Гауссовому \(нормальному\).

## 3.2. Открытые проблемы разделения сигналов

Не смотря на то, что проблема разделения сигналов хорошо изучена и существуют успешные опыты по разделению звука, всё еще существует много нерешённых проблем, которые не позволяют широко применять эту технику. Рассмотрим некоторые из проблем.

В предыдущей части было сделано упрощение модели, где считается что в системе нет аддитивного шума или он незначителен. В реальности это конечно не так и такой шум приносит много проблем. Существует очень мало моделей, задача которых состоит в удалении аддитивного шума во время разделения сигнала.

Упрощение, что все сигналы приходят одновременно и больше не повторяются, было принято, чтобы не рассматривать проблему реверберации, т.е. множественного отражения сигнала в пространстве и повторное снятие сенсорами этих отражений. Получается так, что в системе больше не N источников сигналов, а $$N*k$$, где $$k$$ – это некоторый коэффициент, отражающий величину переотражений. Для борьбы с отражениями предлагается использовать предсказывающие модели.

Большее число источников сигналов чем сенсоров. Как показывает нам пример из биологии – необходимо и достаточно два уха, чтобы уметь различать звуки и ориентироваться в пространстве по звукам. В упрощенной схеме устанавливается что $$N = M$$ и матрица смешивания и разделения квадратные и обратимы. Неквадратную матрицу обращать невозможно, но есть алгоритмы по приближению. Кроме того сама математическая модель явно не задает такие ограничения, они лишь вводятся для упрощения вычислений.

Разделение сигнала в реальном времени требует либо очень больших вычислительных мощностей, либо быстрых алгоритмов разделения.

Нестационарность перемешивания сигналов. Предполагается что все микрофоны и источники сигналов неподвижны в пространстве, поэтому матрица смешивания не изменяется во времени и мощности сигналов можно предположить постоянными. Во время передвижения объектов характер перемешивания сигналов будет постоянно меняться. Необходимы новые алгоритмы решающие эту проблему.

