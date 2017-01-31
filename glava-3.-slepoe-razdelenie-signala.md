# Глава 3. Слепое разделение сигнала

В этой главе рассмотрим детальнее алгоритмы реализующие BSS. Проанализируем достоинства и недостатки и возможности улучшения. Приведем список нерешенных проблем в этой области.

# 3.1. Independent component analysis \(ICA\)

Слепое разделение сигнала можно выполнить с помощью алгоритма независимого анализа компонент.

Предположим что у нас есть N источников сигнала $$s_1 (t),s_2 (t),...,s_N (t)$$, где t – это дискретный момент времени. В разных точках пространства, но неизвестно где именно, существует M сенсоров $$x_1 (t),x_2 (t),...,x_M (t)$$.

Таким образом задача разделения сигнала – это задача по M значениям сигнала с сенсоров восстановить значения оригинальных N сигналов.

Далее введем вектор-столбец $$\overline{s(t)} = [s_1 (t),s_2 (t),...,s_N (t)]^T$$ входного сигнала и вектор-столбец сенсоров $$\overline{x(t)} = [x_1 (t),x_2 (t),...,x_M (t)]^T$$. Процесс смешивания входных сигналов в среде передачи данных можно представить как линейный оператор $$A[*]$$, тогда $$\overline{x(t)}=A[\overline{s(t)}]+\epsilon(t)$$, где $$\epsilon(t)$$ - аддитивный шум.
