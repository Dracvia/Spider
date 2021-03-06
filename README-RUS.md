# Библиотека Spider
Библиотека создана для простых программ на Arduino с использованием [Multiservo shield](https://github.com/amperka/Multiservo) для управления одновременно 18-тю сервоприводами которые являются частями ног робота. Рассчитано что у робота есть 6 ног с 3-мя степенями свободы(То есть за движение каждой ноги отвечают 3 сервопривода).

## Начало работы
Библиотека рассчитана на Multiservo shield и основывается на их библиотеке. Поэтому для работы данной библиотеки необходимо предварительно установить библиотеку Multiservo. 

Добавление библиотеки можно сделать следующим образом предварительно скачав их в как архив

![Rus](https://github.com/Antrismus/Spider/blob/master/Illustration/Rus.png)

## Библиоткека
В библиотеке есть два класса, первый это Leg для характеристики и управление каждой ноги отдельно и второй класс Spider который соответственно управляет пауком в целом.

### Leg
```C++
#include <Leg.h>
```
Класс Leg, как уже говорилось ранее класс создан для управления ногой робота. Нога должна складываться с трех моторов (Бедро – верхний сервопривод, Колено – средний сервопривод, Ступня – нижний сервопривод)

#### Конструктор
1. Пустая нога
2. Инициалиизация ноги при создании

```C++
#include <Leg.h>

void setup() {
  Leg a();
  Leg b(15, 16, 17, 90, 130, 115, 'r');
}
```

#### Методы
1.	setupStaticAngle (Установить стандартные углы поворота для ноги)
```C++
#include <Leg.h>

void setup() {
  Leg b(15, 16, 17, 90, 130, 115, 'r');
  b.setupStaticAngle(90, 130, 115);
}
```
2.	setupSide (Устанавливает сторону ноги)
```C++
#include <Leg.h>

void setup() {
  Leg b(15, 16, 17, 90, 130, 115, 'r');
  b.setupSide('l');
}
```
3.	moving (Ставит ногу в определенное положение с помещу структуры Angle) 
Структура Angle состоит из трех углов соответственно по углу на каждый сервопривод
```C++
#include <Leg.h>

void setup() {
  Leg b(15, 16, 17, 90, 130, 115, 'r');
  // const Angle LEG_UP = {1, 60, 70}; - Стандартная константа
  b.moving(LEG_UP);
  b.moving({1, 60, 70});
}
```
4.	getCurentAngle (возвращает Angle в котором находиться нога в данный момент)
```C++
#include <Leg.h>

void setup() {
  Leg b(15, 16, 17, 90, 130, 115, 'r');
  // const Angle LEG_DOWN = {1, -30, 70}; - Стандартная константа
  b.moving(LEG_DOWN);
  Angle a = b.getCurentAngle();
}
```
5.	getStaticAngle (возвращает стандартные углы для ноги)
```C++
#include <Leg.h>

void setup() {
  Leg b(15, 16, 17, 90, 130, 115, 'r');
  // const Angle LEG_DOWN = {1, -30, 70}; - Стандартная константа
  b.moving(LEG_DOWN);
  Angle a = b.getStaticAngle();
}
```

### Spider
```C++
#include <Spider.h>
```
Класс которые объединят 6 ног в паука

#### Конструктор
1. Инициализация объекта  ногами.
2. С помощю структуры SpiderAdapter.
SpiderAdapter это структура которая содержит ноги которые мы можем переопределить в паука.
3. Создание объекта.
```C++
#include <Spider.h>

Spider mySpider;
 
void setup() {
  Leg rightTop(15, 16, 17, 90, 130, 115, 'r');
  Leg leftTop(2, 1, 0, 90, 55, 60, 'l'); 
  Leg rightCenter(12, 13, 14, 95, 130, 130, 'r');
  Leg leftCenter(5, 4, 3, 90, 50, 40, 'l');
  Leg rightBottom(6, 7, 8, 90, 130, 90, 'r');
  Leg leftBottom(9, 10, 11, 90, 40, 50, 'l'); 
  mySpider = Spider(leftTop, leftCenter, leftBottom, rightTop, rightCenter, rightBottom);
}
```

Варнинг – Будьте внимательны при создании ног передаются копии элементов а не их ссылка поэтому стоит позаботиться о том чтоб лишнее объекты ног, если они уже записаны в Spider, были уничтожены.

#### Методы
1. setLegs (Устанавливает ноги)
```C++
#include <Spider.h>

Spider mySpider;
 
void setup() {
  Leg rightTop(15, 16, 17, 90, 130, 115, 'r');
  Leg leftTop(2, 1, 0, 90, 55, 60, 'l'); 
  Leg rightCenter(12, 13, 14, 95, 130, 130, 'r');
  Leg leftCenter(5, 4, 3, 90, 50, 40, 'l');
  Leg rightBottom(6, 7, 8, 90, 130, 90, 'r');
  Leg leftBottom(9, 10, 11, 90, 40, 50, 'l'); 
  mySpider = Spider(leftTop, leftCenter, leftBottom, rightTop, rightCenter, rightBottom);
  mySpider.setLegs(leftTop, leftCenter, leftBottom, rightTop, rightCenter, rightBottom);
}
```
2. moving (На вход принимает структуру Mover – структура которая хранит внутри себя углы для каждой ноги)
```C++
#include <Spider.h>

Spider mySpider;
 
void setup() {
  Serial.begin(9600); 
  Leg rightTop(15, 16, 17, 90, 130, 115, 'r');
  Leg leftTop(2, 1, 0, 90, 55, 60, 'l'); 
  Leg rightCenter(12, 13, 14, 95, 130, 130, 'r');
  Leg leftCenter(5, 4, 3, 90, 50, 40, 'l');
  Leg rightBottom(6, 7, 8, 90, 130, 90, 'r');
  Leg leftBottom(9, 10, 11, 90, 40, 50, 'l'); 
  mySpider = Spider(leftTop, leftCenter, leftBottom, rightTop, rightCenter, rightBottom);
}

void crouch(int del = 200) {
  mySpider.moving(MOVE_CROUCH);
  delay(del);
  mySpider.moving(MOVE_STANDUP);
}
```
