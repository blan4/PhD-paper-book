# Глава 4. Определние координат источника звука

В этой главе предлагается подробно рассмотреть методы формирования лучей направленности на источники звука.

## 4.1. Модель распространения звука

Для того чтобы проверять качество новых алгоритмов определения координат, необходимо построить математическую модель распространения звука в пространстве. Эта модель должна позволять легко управлять характеристиками среды в плане приближенности к реальности. Процесс создания и улучшения алгоритма будет происходить итеративно, от простого к сложному, поэтому нам нужна не максимально приближенная к реальности модель, а модель в которой можно отключить реверберацию, интерференцию, эффекты затухания звука, поглощения его препятствиями и так далее. Процесс разработки будет выглядеть следующим образом:

1. Строится базовый алгоритм, который умеет учитывать некоторый набор характеристик.
2. Включаем соответсвующий набор конфигураций в модели окружения, чтобы они соответствовалаи ожидаемым характеристикам.
3. Добиваемся функционирующего алгоритма с заданным интервалом точности.
4. Изменяем модель звука. Например, добавляем эхо.
5. Алгоритм выходит за рамки интервала точности, проводим модификации и снова добиваемся прохождения тестов качества.
6. Повторяем итерации до нужного уровня сложности системы.

Такой подход является адаптацией TDD - test driven development - разработка через тесты, где сначала создают "красные" тесты которые программа не проходит, потом модифицируют программу, чтобы она проходила тесты, потом снова уточняют тесты, так чтобы они стали красными и так далее. Такой подход позволит нам точно наблюдать за прогрессом системы, понять как каждая характеристика окружения влияет на сложность системы.


