# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #3 выполнил(а):
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
разработать оптимальный баланс для десяти уровней игры Dragon Picker

Ход работы:

## Задание 1
### Предложите вариант изменения найденных переменных для 10 уровней в игре. Визуализируйте изменение уровня сложности в таблице.


В качестве переменных, влияющих на баланс были обнаружены следующие:

speed - скорость дракона

timeBetweenEggDrops - время между сбросом яиц

leftRightDistance - ширина рамок поля, внутри которых может перемещаться дракон

chanceDirection - шанс изменения направления дракона

Так как по моему мнению успех в игре зависит от реакции и координации игрока, то я решил увеличивать сложность с каждым уровнем по экспоненциальному закону. Были определены оптимальные начальные значения, которые используются в первом уровне, а также их коэффициента приращения. Если переменная с каждым уровнем должна увеличиваться, то коэффициент больше единицы, иначе меньше. Чтобы построить график увеличения сложности я ввёл такое пониятие, как коэффициент сложности уровня. Его график должен расти экспоненциально.
Коэффициент сложности = speed * leftRightDistance * chanceDirection / timeBetweenEggDrops
Данная формула была выведена из следующих соображений:

1. Чем быстрее движется дракон-тем сложнее за ним уследить
2. Чем больше рамка, тем больше у дракона пространства для манёвра, что усложняет процесс
3. Чем выше шанс смены направления движения, тем сложнее предугадать направление движения дракона, вследствие чего полагаться игрок может лишь на свою реакцию
4. Чем меньше времени между сбросом яиц, тем более активно ими бомбордируется игрок и тем больше шанс ошибиться и уронить яйцо

Визуализация коэффициента сложности, а также данные для всех 10 уровней представлены в следующей таблице: https://docs.google.com/spreadsheets/d/1khEQMKRQgC2AHraCr6WpIOTglv0DAHn0LSPR1QAzZKA/edit?gid=0#gid=0

## Задание 2
### Создайте 10 сцен на Unity с изменяющимся уровнем сложности.

Подробнее с результатом выполнения этого задания вы можете ознакомиться в приложенном .package файле

## Задание 3
### Решение в 80+ баллов должно визуализировать данные из google-таблицы, и с помощью Python передавать в проект Unity. В Python данные также должны быть визуализированы.

модель заполнения мы возьмём из WorkShop2, в качестве визуализации мы будем использовать график, который будем строить и выводить с помощью модуля "matplotlib"
Скрипт получается следующий:

```py
import enum
import time

import gspread
import matplotlib.pyplot as plt
import numpy as np


class DifficultIndex(enum.Enum):
    speed = 0
    egg_drop_time = 1
    right_left_distance = 2
    change_direction_chance = 3


class DifficultData:
    values = [1.5, 2.3, 0.3, 0.01]
    increase_coef = [1.36, 0.76, 1.5, 1.21]
    column_names = ("B", "C", "D", "E")
    fields_count = 4
    difficulty_coef = []

    def __init__(self):
        self.append_difficult_coef()

    def append_difficult_coef(self):
        self.difficulty_coef.append(
            self.values[DifficultIndex.speed.value] * self.values[DifficultIndex.right_left_distance.value] /
            self.values[
                DifficultIndex.egg_drop_time.value] * self.values[DifficultIndex.change_direction_chance.value])

    def icrease_difficulty(self):
        for i in range(self.fields_count):
            self.values[i] *= self.increase_coef[i]
        if self.values[DifficultIndex.right_left_distance.value] > 10:
            self.values[DifficultIndex.right_left_distance.value] = 10
        self.append_difficult_coef()


gc = gspread.service_account(filename='universal-datascience-61c84fd63e70.json')
sh = gc.open("Dragon picker difficulty")

sh.sheet1.update([["Level number"]], 'A1')
sh.sheet1.update([["Dragon speed"]], 'B1')
sh.sheet1.update([["Egg drop time"]], 'C1')
sh.sheet1.update([["Right-left distance"]], 'D1')
sh.sheet1.update([["change_direction_chance"]], 'E1')

difficulty_data = DifficultData()

def update_sheet(row):
    for i in range(difficulty_data.fields_count):
        sh.sheet1.update([[row - 1]], f"A{str(row)}")
        sh.sheet1.update([[difficulty_data.values[i]]], f'{difficulty_data.column_names[i]}{str(row)}')


for i in range(1, 11):
    time.sleep(5)
    if i != 1: difficulty_data.icrease_difficulty()
    update_sheet(i + 1)

fig, ax = plt.subplots()
ax.plot(np.arange(1, 11), np.array(difficulty_data.difficulty_coef))
ax.grid()
plt.xticks(range(1, 11, 1), range(1, 11, 1))
plt.yticks(range(1, 50, 2), range(1, 50, 2))
plt.xlabel("Level number")
plt.ylabel("Difficulty")
plt.show()
```

Для передачи из Python в Unity мы будем использовать Google-таблицу как посредника, то есть Unity будет забирать данные для соответствующего уровня наппрямую из Google-таблицы. Реализация подобного функционала уже была произведена в WorkShop2, оттуда его и позаимствуем с небольшими изменениями для адаптации под текущие условия:

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;
using UnityEngine.SceneManagement;

public class EnemyDragon : MonoBehaviour
{
    public GameObject dragonEggPrefab;
    public float speed = 1;
    public float timeBetweenEggDrops = 1f;
    public float leftRightDistance = 10f;
    public float chanceDirection = 0.1f;
    void Start()
    {
        GoogleSheets();
        Invoke("DropEgg", 2f);
    }

    void DropEgg(){
        Vector3 myVector = new Vector3(0.0f, 5.0f, 0.0f);
        GameObject egg = Instantiate<GameObject>(dragonEggPrefab);
        egg.transform.position = transform.position + myVector;
        Invoke("DropEgg", timeBetweenEggDrops);
    }


    void GoogleSheets()
    {
        var isHeader = true;
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1khEQMKRQgC2AHraCr6WpIOTglv0DAHn0LSPR1QAzZKA/values/Лист1?key=AIzaSyA3C2vQRCesUftFSIkjnMzwIq-ZqyEFU2Y");
        curentResp.SendWebRequest();
        while (!curentResp.isDone) ;
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp)["values"][SceneManager.GetActiveScene().buildIndex];
        var selectRow = rawJson.AsStringList;
        speed = float.Parse(selectRow[1]);
        timeBetweenEggDrops = float.Parse(selectRow[2]);
        leftRightDistance = float.Parse(selectRow[3]);
        chanceDirection = float.Parse(selectRow[4]);
    }


    // Update is called once per frame
    void Update()
    {
        Vector3 pos = transform.position;
        pos.x += speed * Time.deltaTime;
        transform.position = pos;

        if (pos.x < -leftRightDistance){
            speed = Mathf.Abs(speed);
        }
        else if (pos.x > leftRightDistance){
            speed = -Mathf.Abs(speed);
        }
    }

    private void FixedUpdate() {
        if (Random.value < chanceDirection){
            speed *= -1;
        }
    }
}
```

## Выводы

В ходе лабораторной работы был разобран и доработан прототип игры Dragon Picker, а именно был создан дизайн-документ с помощью Python, была проведена визуализация динамики изменения данных средствами Python и Google Sheets, а также, в соответствии с дизайн-документом была добавлена система сожностей, которая представляет собой набор из 10 сцен, чем больше номер сцены, тем выше уровень сложности, при этом данные для каждой сцены берутся напрямую из Google-таблицы

Цели лабораторной работы были достигнуты.
## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
