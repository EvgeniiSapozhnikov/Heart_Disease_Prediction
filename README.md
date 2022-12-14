# Построение модели машинного обучения для предсказания наличия у пациента сердечно-сосудистых заболеваний

__Цель исследования__ – научить модель машинного обучения выявлять пациентов, имеющих сердечно-сосудистые заболевания

__Задачи:__
Выбор датасета для обучения модели, предобработка данных. 
Определение метрики, по которой будут оцениваться результаты предсказаний модели.
Выбор модели , подготовка данных. 
Обучение модели, выбор оптимальных гиперпараметров. 

__Исходные данные__
Датасет №2: Heart Failure Prediction Dataset
Ссылка: https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction

___

## Предварительный анализ данных

На данном этапе были выполнены следующие мероприятия:
1. Загружен и изучен исходный датасет.
2. Получено графическое представление о данных по отдельным полям датасета.
3. Проведен поиск аномалий, дубликатов и выбросов в датасете.

__Изучение исходного датасета:__
1. Целевой признак находится в поле `HeartDisease`.
2. Матрица корреляции по Пирсону показала, что целевой признак может иметь линейную зависимость со следующими полями: `MaxHR`,`ExerciseAngina`, `Oldpeak`.
3. В исходном датасете отсутствуют пропуски.

__Графический анализ:__
1. Графический анализ показал, что в датасете содержится примерно одинаковое содержание пациентов с ССЗ и без них.
2. В поле `Cholesterol` содержится значительное число пациентов с нулевый уровнем, что, скорее всего, является аномалией.
3. Пациенты с ССЗ чаще встречаются в более возрастных группах.
4. Показания частоты пульса у пациентов с ССЗ в среднем выше, чему пациентов без ССЗ.

__Поиск дубликатов, аномалий и выбросов:__
1. В исходном датасете отсутствуют явные и неявные дубликаты.
2. В датасете присутствуют выбросы, их доля доходит по 20% (`Cholesterol`).


__По результатам анадиза данных принято решение не удалять из датасета выбросы и аномалии и не менять их значения, так как изменение значений может привести к тому, что обученная модель после обучения будет плохо справляться с аномалиями и выбросами и, как следствие, предсказаниям такой модели нельзя будет доверять.__

___

## Выбор метрики

В ходе исследования будет решаться задача бинарной классификации - модели необходимо будет определить, имеется ли у пациента СЗЗ (1) или нет(0).

Принимая во внимание, что модель будет предсказывать наличие или отсутствие болезни у пациента, крайне важно снизить количество случаев, когда пациент с СЗЗ принимается системой как здоровый, т.е. __снизить количество ложноотрицательных ответов__. Следовательно, в качестве целевой метрики принимаем полноту (`recall`).

___

## Обучение моделей

Для обучения возьмем следующие модели:
1. Решающее дерево.
2. Случайный лес.
3. Логистическая регрессия.
4. Градиентный бустинг (Catboost).

Для подбора гиперпараметров применим кросс валидацию средствами библиотек `skleart` (GridSearchCV) и `optuna`.

___

## Общие выводы

По результатам выполненной работы можно сделать следующие выводы:
1. Все выбранные модели показывают достаточно высокие значения метрики recall при любом методе подбора гиперпараметров (от 0.873 до 0.951).
2. Наибольшее значение полноты достигнуто при использовании градиентного бустинга Catboost - 0.95.
3. График зависимости свойств показал сильную зависимость от следующих признаков: `Cholesterol` и `ChestpainType`.

__В целом следует отметить, что исходный датасет имел всего 1000 значений, и при разделении его на 3 выборки - обучающую, валидационную и тестовую в соотношении 60:20:20 - в нашем распоряжении остается 200 значений для валидации и 200 значений для предсказаний и, следовательно, даже 1-2 дополнительных ложноотрицательных значения приводят с ощутимому изменению выбранной метрики.__
