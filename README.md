# Лабораторная работа. Интеграция сервиса для получения данных профиля пользователя.
Отчет по лабораторной работе #1 выполнила:
- Додонова Елена Игоревна
- РИ-300002
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
![1 видео_1 (1)](https://user-images.githubusercontent.com/90499063/197333309-2f12a0c8-63cd-4312-9be1-fbe349844ec4.gif)

### – 2 Практическая работа «Реализация графического интерфейса с добавлением счетчика очков».
Ход работы:

Создадим счетчик набранных очков.
![Снимок экрана (1775)](https://user-images.githubusercontent.com/90499063/197333330-ce28f442-b85e-46f7-8991-549b5799c0b8.png)

Настроим расположение и вид счетчика на экране.
![Снимок экрана (1776)](https://user-images.githubusercontent.com/90499063/197333354-648aad9a-cbb5-4c43-ad3c-66218cafdb31.png)

Напишем код для отображения счетчика очков.

```
public TextMeshProUGUI scoreGT;

    void Start(){
        GameObject scoreGO = GameObject.Find("Score");
        scoreGT = scoreGO.GetComponent<TextMeshProUGUI>();
        scoreGT.text = "0";
    }
```
![Счетчик отображает 0](https://user-images.githubusercontent.com/90499063/197333381-24b5eec4-3ec6-4ed7-87bd-d7cc69014b78.JPG)

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
 ![Счетчик работает (1)](https://user-images.githubusercontent.com/90499063/197333450-82e2fd45-c727-4063-a55b-af1865b6ab1b.gif)

 
 
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
![Сброс сцены (1)](https://user-images.githubusercontent.com/90499063/197333475-480f1728-e38f-4ef9-9e51-70759d4aa0bf.gif)

Напишем код уменьшения жизней энергетического щита.
```
public List<GameObject> shieldList;

void Start()
{
    shieldList = new List<GameObject>();
    for (int i = 1; i <= numEnergyShield; i++){
        GameObject tShiedGo = Instantiate<GameObject>(energyShieldPrefab);
        tShiedGo.transform.position = new Vector3(0, energyShieldBottomY, 0);
        tShiedGo.transform.localScale = new Vector3(1*i, 1*i, 1*i);
        shieldList.Add(tShiedGo);
    }
        
}

public void DragonEggDestroyed(){
        GameObject[] tDragonEggArray = GameObject.FindGameObjectsWithTag("Dragon Egg");
        foreach (var tGO in tDragonEggArray)
        {
            Destroy(tGO);
        }
        int shieldIndex = shieldList.Count - 1;
        GameObject tShieldGo = shieldList[shieldIndex];
        shieldList.RemoveAt(shieldIndex);
        Destroy(tShieldGo);
        if (shieldList.Count == 0){
            SceneManager.LoadScene("_0Scene");
        }
    }
```
У нас происходит уменьшение количества жизней при промахах игрока. После 3-х промахов игра перезапускается и счетчик обновляется.
![Уменьшение жизней (1)](https://user-images.githubusercontent.com/90499063/197335806-0b733a45-ff5b-46eb-8c30-a8ccf8e16201.gif)


Добавим текстуры на дальний план. 
Настроим расположение префаба горы.
![Снимок экрана (1777)](https://user-images.githubusercontent.com/90499063/197335791-225e0b9c-0641-4c05-8f96-0bfacaf4541a.png)

Настроим текстуру неба.
![Снимок экрана (1778)](https://user-images.githubusercontent.com/90499063/197335793-b227ed03-20a4-4cd6-86f5-9aea67fd7665.png)

Игра приобрела такой вид:
![Итог видео 3 (1) (1) (1) (1)](https://user-images.githubusercontent.com/90499063/197336477-0313c853-9bc1-4f18-8e1a-0915cf7fbadd.gif)


### – 4 Практическая работа «Структурирование исходных файлов в папке».






## Задание 3
### – 5 Практическая работа «Интеграция игровых сервисов в готовое приложение».






## Выводы
В данной работе мы создали прототип игры Dragon Picker. Реализовали основные методы движения, поработали со скриптами, префабами, объектами и текстурами. Также поработали с Yandex Games. Сравнили основные принципы работы Yandex.Games и VK Games. Выяснили, что на платформа Яндекс больше подходит для разработчиков, так как для них больше возможностей.
