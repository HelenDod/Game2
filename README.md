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
Ход работы:
Создадим отдельные папки для скриптов, текстур, аудиофайлов, префабов и т.д.
![image](https://user-images.githubusercontent.com/90499063/197578063-bf3dfb36-885d-42f9-9bf3-3a54d92af4bd.png)

Пересоберем папки, добавив в них использованные ассеты.
Так выглядят папки с соответствующими компонентами:
![image (1)](https://user-images.githubusercontent.com/90499063/197578253-f0e1079c-8bd9-4daa-8abf-696746d40cbd.png)
![image (2)](https://user-images.githubusercontent.com/90499063/197578258-856f6baf-40fd-4d6c-bda2-2bc2c1a1819f.png)
![image (3)](https://user-images.githubusercontent.com/90499063/197578260-e2cd421b-b21a-4539-873a-43e6d5ccaf29.png)
![image (4)](https://user-images.githubusercontent.com/90499063/197578263-10c38ee2-0bd7-4acb-99be-968c2f6395ed.png)
![image (5)](https://user-images.githubusercontent.com/90499063/197578266-c75bbb2c-4869-42bc-8f87-0a78f4eca618.png)
![image (6)](https://user-images.githubusercontent.com/90499063/197578268-39e8cba8-60ec-4b32-9eef-01388e832436.png)

## Задание 3
### – 5 Практическая работа «Интеграция игровых сервисов в готовое приложение».
Ход работы:
Делаем билд нашей сцены.
![image (7)](https://user-images.githubusercontent.com/90499063/197579052-326230f0-81e5-4a2e-b1f5-bad326e58ec9.png)
Получчаем такой результат.
![Создан билд (1) (1)](https://user-images.githubusercontent.com/90499063/197580626-51aa65ef-7e99-45f2-87a9-8e7633fe8a32.gif)

Дальше подключаем к юнити YandexGame plugin, с помощью которого так же делаем сборку проекта, загружаем zip-архив на Яндекс.Игры. 
![image (8)](https://user-images.githubusercontent.com/90499063/197579006-2290f6f1-c023-4d1a-ad28-5c0ea6e9e614.png)
![image (9)](https://user-images.githubusercontent.com/90499063/197579030-90531a13-25f7-47f5-aa57-4a601c137743.png)
![image (10)](https://user-images.githubusercontent.com/90499063/197579034-3fdd2f6b-1164-46bd-b687-4644170fb6b0.png)

## Выводы
В данной работе мы реализовали ловлю объектов, подключили счётчик набранных игроком очков. Реализовали алгоритм, при котором при пропуске яйца у энергетического щита уменьшается количество жизней, а сцена в этот момент перезапускается. При потере 3-х яиц игра перезапускается и счетчик очков обновляется. Также мы добавили больше текстур в нашу игру, украсили задний фон горой и небом. В итоге мы структурировали все файлы игры по отдельным папкам для большего удобства, поработали с плагином Яндекса и сделали билд игры, которую загрузили на Яндекс.Игры.
