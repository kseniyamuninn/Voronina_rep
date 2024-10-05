---
title: "Voronina1(1)"
author: "Voronina Kseniya"
date: "2024-09-21"
output: html_document
---

## Работа с функциями на языке програмирования Python

### Определение функции

Функции Python — это объекты первого класса. Их можно присваивать переменным, хранить в структурах данных, передавать в качестве аргументов другим функциям и даже возвращать в качестве значений из других функций.

#### Пример объявления функции

```         
def f(x):
  x = x ** 2
  return x
```

Для объявления функции в Python нужно прописать:

1)  Ключевое слово def
2)  Название функции
3)  Закрывающиеся круглые скобки
4)  Двоеточие — (:)

В теле функции пишется код, который будет выполняться при её вызове.

### Термины

Ключевое слово `def` в начале функции сообщает интерпретатору о том, что следующий за ним код — есть её определение. Всё вместе — это объявление функции.

Параметр — это переменная, которой будет присваиваться входящее в функцию значение. Аргумент — само это значение, которое передается в функцию при её вызове.

![](img/1.png)

### Время выполнения функции

Чтобы оценить время выполнения функции, можно поместить её вызов внутрь следующего кода:

```         
from datetime import datetime
import time

start_time = datetime.now()
time.sleep(5)

print(datetime.now() - start_time)
```

### Вывод

Так мы изучили основные понятия и термины для работы с функциями на языке программирования Python. Функции часто используются для упрощения работы с вычислениями, а также для уменьшения величины кода.