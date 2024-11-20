# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Зотов Роман Сергеевич
- ФО-230006
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
становить необходимое программное обеспечение, которое пригодится для создания интеллектуальных моделей на Python. Рассмотреть процесс установки игрового движка Unity для разработки игр.

Ход работы:

## Задание 1
### Написать программу Hello World на Python с запуском в Jupiter Notebook.

-Запустить JupiterNotebook
-Создать файл формата ipynb
-Использоать встроенную функцию консольного вывода "рrint" для вывода сообщения.

![](data:image/png;base64,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)

```py
print("Hello World!!!")
```

## Задание 2
### Написать программу Hello World на C# с запуском на Unity.


```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class HelloWorld : MonoBehaviour
{
    // Update is called once per frame
    void Update()
    {
        if (Input.anyKeyDown)
        {
            Debug.Log("Hello World!!!");
        }
    }
}
```

## Задание 3
### Оформить отчет в виде документации на github (markdown-разметка)
Результат этого задания вы только что прочитали

## Выводы

В ходе проделанной работы были изучены такие инструменты, как: Anakonda, JupiterLab, Unity, Python, C#
В ходе лабораторной:

1) Я научился работать с Anakonda
2) Я смог запустить Hello World в JupiterNotebook
3) Я создал скрипт, который заставляет любой объект в Unity, с учётом прикрепления к нему скрипт-файла, писать в консоль "Hello World" по нажатию клавиши

Цели лабораторной работы были достигнуты.
## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
