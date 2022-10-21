# Лабораторная работа. Интеграция сервиса для получения данных профиля пользователя.
Отчет по лабораторной работе #1 выполнила:
- Додонова Елена Игоревна
- РИ300002
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

## Цель работы
Интеграция интерфейса пользователя в разрабатываемое интерактивное приложение.

## Задание 1
### Используя видео-материалы практических работ 1-5 повторить реализацию игровых механик:
### – 1 Практическая работа «Реализация механизма ловли объектов».
Ход работы:

Создадим скрипт и подключим его к энергетическому щиту. С помощью данного кода игрок сможет управлять щитом с помощью мыши.

```
void Update()
    {
        Vector3 mousePos2D = Input.mousePosition;
        mousePos2D.z = - Camera.main.transform.position.z;
        Vector3 mousePos3D = Camera.main.ScreenToWorldPoint(mousePos2D);
        Vector3 pos = this.transform.position;
        pos.x = mousePos3D.x;
        this.transform.position = pos;
    }
```

Затем реализуем ловлю яиц энергетическим щитом.

```
private void OnCollisionEnter(Collision coll) {
        GameObject Collided = coll.gameObject;
        if (Collided.tag == "Dragon Egg"){
            Destroy(Collided);
        }
    }
```
Вот, что получилось в итоге:


### – 2 Практическая работа «Реализация графического интерфейса с добавлением счетчика очков».

Создадим счетчик набранных очков.


Настроим расположение и вид счетчика на экране.

Напишем код для отображения счетчика очков.

```
public TextMeshProUGUI scoreGT;

    void Start(){
        GameObject scoreGO = GameObject.Find("Score");
        scoreGT = scoreGO.GetComponent<TextMeshProUGUI>();
        scoreGT.text = "0";
    }
```
Напишем код, который будет выполнять подсчёт пойманных яиц
```
private void OnCollisionEnter(Collision coll) {
        GameObject Collided = coll.gameObject;
        if (Collided.tag == "Dragon Egg"){
            Destroy(Collided);
        }
        int score = int.Parse(scoreGT.text);
        score +=1;
        scoreGT.text = score.ToString();
    }
 ```
 В итоге при столкновении яйца с щитом счетчик увеличивается, а при пропуске остается неизменным.
 
 
 
## Задание 2
### – 3 Практическая работа «Уменьшение жизни. Добавление текстур».
Ход работы:
Напишем код, который будет отвечать за перезапуск сцены, если игрок промахивается при ловле яиц.

 ```
{
        if (transform.position.y < bottomY){
            Destroy(this.gameObject);
            DragonPicker apScript = Camera.main.GetComponent<DragonPicker>();
            apScript.DragonEggDestroyed();
        }
    }
```

```
public void DragonEggDestroyed(){
        GameObject[] tDragonEggArray = GameObject.FindGameObjectsWithTag("Dragon Egg");
        foreach (var tGO in tDragonEggArray)
        {
            Destroy(tGO);
        }
    }
```
Таким образом, при промахе игрока происходит сброс сцены, где последующее выпадающее яйцо пропадает.



### – 4 Практическая работа «Структурирование исходных файлов в папке».






## Задание 3
### – 5 Практическая работа «Интеграция игровых сервисов в готовое приложение».






## Выводы
В данной работе мы создали прототип игры Dragon Picker. Реализовали основные методы движения, поработали со скриптами, префабами, объектами и текстурами. Также поработали с Yandex Games. Сравнили основные принципы работы Yandex.Games и VK Games. Выяснили, что на платформа Яндекс больше подходит для разработчиков, так как для них больше возможностей.
