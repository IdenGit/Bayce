## Умный рейтинг на основе оценочной формулы Байеса
 
### Установка

`include_once 'rating.class.php'`
 
### Использование

Для рейтинга нужны две сущьности - учасники рейтинга и отзывы о них

Учасники должны содержать:

1) имя учасника
2) id учасника

Отзывы должны содержать:

1) Оценку от нуля до пяти
2) id отзыва
3) id учасника
4) Дату отзыва в формате `'Y-m-d H:i:s'`

Приммер:

```php

$peoples = [
    [
        'name'=>'Alice',
        'id' => 1,
    ],
    [
        'name'=>'Bob',
        'id' => 2,
    ]   
];


$reviews = [
    [
        'vote_rate'=>5,
        'id' => 1,
        'vote_date' => '2001-11-17 14:23:17',
        'user_id' => 1
    ],
    [
        'vote_rate'=>5,
        'id' => 2,
        'vote_date' => '2010-01-01 14:23:17',
        'user_id' => 1
    ],
    [
        'vote_rate'=>4,
        'id' => 3,
        'vote_date' => '2013-02-13 14:23:17',
        'user_id' => 1
    ],
    [
        'vote_rate'=>4,
        'id' => 4,
        'vote_date' => '2017-01-01 14:23:17',
        'user_id' => 2
    ],
    [
        'vote_rate'=>5,
        'id' => 5,
        'vote_date' => '2017-01-17 14:23:17',
        'user_id' => 2
    ],
    [
        'vote_rate'=>4,
        'id' => 6,
        'vote_date' => '2017-01-17 14:23:17',
        'user_id' => 2
    ]
];
```

Далее мы создаем сам класс

```php
  $aliasses = [
        // reviews
        'elem' => 'user_id',
        'rating' => 'vote_rate',
        'date' => 'vote_date',
        // peoples
        'eId' => 'id',
        'eName' => 'name'
   ];
   $oldDays = 90;
   $minCount = 3;
   $rate = new \Rating($aliasses, $peoples, $reviews, $oldDays, $minCount);
```

Где 
- `$aliasses` содержит массив конвертации полей отзывов и участников
- `$oldDays` определяет дату после который отзывы начинают устаривать 
- `$minCount` мнимальное поличество отзывов с которых человек попадаетв рейтинг

Как только класс создан рейтинги уже расчитаны

я запустил код в `2017-02-17 12:30:01`
и получил следующие результаты:

Алиса

oldRating = 7
newRating = 83.23

Боб

oldRating = 6,5
newRating = 83.29

Не смотря что средний статистический (oldRating) рейтинг Боба ниже рейтинга Алисы

6,5 < 7

Умный рейтинг(newRating) боба выше, так как у Алисы все отзывы достаточно старые и уже не актуальные

83.29 > 83.23



