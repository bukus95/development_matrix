# OOP in JS

1. [Что такое класс/экземпляр класса?**](#1.-chto-takoe-klass-ekzemplyar-klassa)
2. [Как наследовать дочерний класс от родительского класса?**](#2.-kak-nasledovat-dochernii-klass-ot-roditelskogo-klassa)
3. Что такое статические методы?***
4. Частные и публичные поля класса. Какой из них по умолчанию?***

## 1. Что такое класс/экземпляр класса

`Класс JavaScript` — это схема для создания объектов. 
`Класс` инкапсулирует данные и функции. Классы JavaScript — это синтаксический сахар над прототипным наследованием.
`Экземпляр` — это объект, содержащий данные и поведение, описанное классом

### Объявление класса

`Первый способ определения класса` — class declaration (объявление класса). Для этого необходимо воспользоваться ключевым словом class и указать имя класса (в примере — «Rectangle»).

```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```
>Разница между объявлением функции (function declaration) и объявлением класса (class declaration) в том, 
> что объявление функции совершает подъём (hoisting), в то время как объявление класса — нет. 
> Поэтому вначале необходимо объявить ваш класс и только затем работать с ним, 
> а код же вроде следующего сгенерирует исключение типа ReferenceError.

`Второй способ определения класса` — class expression (выражение класса). 
Можно создавать именованные и безымянные выражения. 
В первом случае имя выражения класса находится в локальной области видимости класса и может быть получено через свойства самого класса, а не его экземпляра.

```javascript
// безымянный
var Rectangle = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// отобразится: "Rectangle"

// именованный
var Rectangle = class Rectangle2 {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// отобразится: "Rectangle2"
```

### Тело класса и задание методов
Тело класса — это часть кода, заключённая в фигурные скобки `{}`. Здесь вы можете объявлять члены класса, такие как методы и конструктор.

#### Строгий режим
Тела объявлений классов и выражений классов выполняются в строгом режиме (strict mode).

#### Constructor
Метод `constructor` — специальный метод, необходимый для создания и инициализации объектов, 
созданных, с помощью класса. В классе может быть только один метод с именем `constructor`. 
Исключение типа `SyntaxError` будет выброшено, если класс содержит более одного вхождения метода `constructor`.

Ключевое слово super можно использовать в методе `constructor` для вызова конструктора родительского класса.

```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }

  get area() {
    return this.calcArea();
  }

  calcArea() {
    return this.height * this.width;
  }
}

const square = new Rectangle(10, 10);

console.log(square.area); // 100
```

## 2. Как наследовать дочерний класс от родительского класса

Ключевое слово `extends` используется в объявлениях классов и выражениях классов для создания класса, дочернего относительно другого класса.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} издаёт звук.`);
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name); // вызывает конструктор super класса и передаёт параметр name
  }

  speak() {
    console.log(`${this.name} лает.`);
  }
}

let d = new Dog('Митци');
d.speak(); // Митци лает
```
Если в подклассе присутствует конструктор, он должен сначала вызвать `super`, прежде чем использовать `this`.

Аналогичным образом можно расширять традиционные, основанные на функциях "классы":

```javascript
function Animal (name) {
  this.name = name;
}
Animal.prototype.speak = function () {
  console.log(`${this.name} издаёт звук.`);
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} лает.`);
  }
}

let d = new Dog('Митци');
d.speak(); // Митци лает

// Для аналогичных методов дочерний метод имеет приоритет над родительским.
```
