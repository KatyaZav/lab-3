# Реализация интерфейса пользователя.
Отчет по лабораторной работе #3 выполнила:
- Завьялова Екатерина Николаевна
- РИ300023
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | # | 60 |
| Задание 2 | # | 20 |
| Задание 3 | # | 20 |

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
- Код реализации выполнения задания. Визуализация результатов выполнения.
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения.
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения.
- Выводы.

## Цель работы
интеграция интерфейса пользователя в разрабатываемое интерактивное приложение.

## Задание 1
### Используя видео-материалы практических работ 1-5 повторить реализацию игровых механик:
 – 1 Практическая работа «Реализация механизма ловли объектов»
 – 2 Практическая работа «Реализация графического интерфейса с добавлением счетчика очков»
 – 3 Практическая работа «Уменьшение жизни. Добавление текстур»
 – 4 Практическая работа «Структурирование исходных файлов в папке»
 – 5 Практическая работа «Интеграция игровых сервисов в готовое приложение»

Ход работы:
1) Реализовать механизм ловли золота.

Добавлю код поимки золота вагонетке, воспользовавшись интерфейсом.
```c#
 private void OnCollisionEnter2D(Collision2D collision)
    {
        ICollectable collect = collision.gameObject.GetComponent<ICollectable>();
        
        if (collect != null)
        {
            collect.Collect();
        }
        else
        {
            Debug.LogWarning("Catched strange object: " + collision.ToString());
            Destroy(collision.gameObject);
        }
    }
```

Добавлю код сбора золоту.
```c#
[SerializeField] GameObject collectEffectPrefab;

    public void Collect()
    {
        var effect = Instantiate(collectEffectPrefab);
        effect.transform.position = transform.position;

        Debug.Log("Pick gold");
        Destroy(gameObject);
    }
```

Теперь сбор золота выглядит так, как представлено на гифке снизу.

![Фото](https://github.com/KatyaZav/lab-3/blob/main/Screens/1%20task/1.gif)

2)     


![Видео](https://github.com/KatyaZav/GameDev-lab2/blob/main/Screens/1%20task/2.1.gif)
![Фото](https://github.com/KatyaZav/GameDev-lab2/blob/main/Screens/1%20task/3.1.jpg)


```c#

```


## Задание 2
### Добавить в приложение интерфейс для вывода статуса наличия игрока в сети (онлайн или офлайн).

Ход работы:
1) 

## Задание 3
### Реализовать вывод в консоль количества времени отсутствия игрока в сети если пользователь офлайн.

Ход работы:


## Выводы

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
