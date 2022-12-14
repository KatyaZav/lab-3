# Реализация интерфейса пользователя.
Отчет по лабораторной работе #3 выполнила:
- Завьялова Екатерина Николаевна
- РИ300023
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
  1) Практическая работа «Реализация механизма ловли объектов» 
  2) Практическая работа «Реализация графического интерфейса с добавлением счетчика очков» 
 
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

2) Добавить UI счетчик очков.
Начну с создания общего класса, в котором будет храниься вся информация по игроку. Как можно увидеть, в коде есть применение ивентов (событий).

```c#
public static class Statistics 
{
    //public static int RecordCount { get; private set;}
    public static int GameCount { get; private set; }
    
    public static Action<int> UpdateGameCount;
    public static void AddCount(int count)
    {
        GameCount += count;
        UpdateGameCount?.Invoke(GameCount);
    }
}
```

Сделаем подпись на событие в классе UI счетчика очков:
```c#
public class CountText : MonoBehaviour
{
    TextMeshProUGUI text;

    void Start()
    {
        text = GetComponent<TextMeshProUGUI>();//GetComponent<TextMeshPro>();
        UpdateText(Statistics.GameCount);
        
        Statistics.UpdateGameCount += UpdateText;
    }

    void UpdateText(int count)
    {
        text.text = string.Format("Очки: {0}", count);
    }
}
```

Добавим добавление очков в классе золота.
```c#
Statistics.AddCount(1);
```

Осталось добавить сам элемент интерфейса счетчик очков, предварительно настроив канваз. По итогу все выглядит так:

![Видео](https://github.com/KatyaZav/lab-3/blob/main/Screens/1%20task/2.gif)

## Задание 2
### Сделать: 
  1) Практическая работа «Уменьшение жизни. Добавление текстур» 
  2) Практическая работа «Структурирование исходных файлов в папке» 
  
Ход работы:
1) Сделать уменьшение жизни и перезапуск сцены при проигрыше.      
Для начала нужно выбрать, какой вариант жизней мне больше нравится. Идеи две:
    1. Есть шкала, которая со временем будет убавляться. Поимка золота будет ее восполнять. 
    2. Шкала - просто отображение жизни. Любой пропуск будет убавлять значение. 

Первое будет более веселым, но реализовать второй вариант проще.
Добавим UI слайдер и его изображение. Пропишем ему скрипт:

```c#
public class HealthSlider : MonoBehaviour
{
    [SerializeField] float MaxHealth;
    [SerializeField] float waitTime;
    [SerializeField]float _health;
    Image image;
    void Start()
    {
        image = GetComponent<Image>();
        _health = MaxHealth;

        StartCoroutine(SliderWork());
        Statistics.UpdateGameCount += AddHealth;
    }

    IEnumerator SliderWork()
    {
        while (true)
        {
            if (_health <= 0)
            {
                Debug.Log("Lose");
                break;
            }
            if (_health < MaxHealth * 0.2)
                yield return new WaitForSeconds(waitTime * 2);
            else 
                yield return new WaitForSeconds(waitTime);
            
            _health--;
            UpdateSlider();
        }
    }
    void UpdateSlider()
    {
        image.fillAmount =_health / MaxHealth;
    }

    void AddHealth(int count)
    {
        _health += count * 2;

        if (_health > MaxHealth)
            _health = MaxHealth + 1;

        UpdateSlider();
    }
}
```

В итоге я реализовала первый варинт.

![Видео](https://github.com/KatyaZav/lab-3/blob/main/Screens/1%20task/3.gif)

Если полоска доходит до конца, сцена перезагружается.

2) Структурировать исходные файлы.

Создадим основные папки Animations, Scenes, Texture, Material, Prefab, Scripts. Перед названиями добавим пометку "_", чтобы системные папки были выше остальных.

![Фото](https://github.com/KatyaZav/lab-3/blob/main/Screens/1%20task/4.jpg)

## Задание 3
### Практическая работа «Интеграция игровых сервисов в готовое приложение»

Ход работы:
1) Добавить Яндекс плагин в проект.

Найдем пакет плагина и установим его в юнити. Далее добавим на сцену префаб Яндекс Игр. 

![Фото](https://github.com/KatyaZav/lab-3/blob/main/Screens/1%20task/5.jpg)

2) Добавить игру на Яндекс Игры, заполнить часть полей.

Черновик игры на ЯИграх: https://yandex.ru/games/app/190276?draft=true&lang=ru.


## Выводы
Я вспомнила, как работать с событиями, интерфейсами, корутинами и TMPro. Так же я просмотрела информацию по созданию слайдеров. 

Узнала, что можно добавлять "_" в  название важных системных папок, чтобы они не терялись в прочих папках и отображались наверху. 

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
