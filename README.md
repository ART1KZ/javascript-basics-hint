# 1. СИНТАКСИС

### Комментарии

**Однострочные** — игнорируются интерпретатором, используются для пояснений:

```javascript
// Это однострочный комментарий
let age = 19; // комментарий после кода
```

**Многострочные** — для больших блоков текста:

```javascript
/*
  Это многострочный комментарий.
  Можно писать на нескольких строках.
*/
```

### Точки с запятой

JavaScript **автоматически вставляет** точки с запятой (ASI — Automatic Semicolon Insertion), но это может привести к ошибкам:

```javascript
// Работает, но опасно
let x = 5
let y = 10

// Ошибка из-за ASI
let a = 1
[1, 2].forEach(console.log) // попытается выполнить 1[1, 2]
```

**Лучшая практика:** всегда ставить точки с запятой явно для предсказуемости кода.

### Строгий режим

`'use strict'` включает строгую проверку синтаксиса и запрещает небезопасные конструкции:

```javascript
'use strict';

x = 10; // ReferenceError: x is not defined (без strict была бы глобальная переменная)
delete Object.prototype; // TypeError (в обычном режиме игнорируется)

function test() {
  'use strict'; // можно применить только к функции
  // строгие правила внутри функции
}
```

**Преимущества:**
- Ловит опечатки в именах переменных
- Запрещает использование устаревших конструкций
- Повышает безопасность кода

***

# 2. ПЕРЕМЕННЫЕ

### let — изменяемая переменная

Область видимости — **блок** (block scope):

```javascript
let name = 'Иван';
name = 'Пётр'; // можно переназначить

if (true) {
  let age = 25; // существует только внутри блока
}
console.log(age); // ReferenceError
```

### const — константа

Нельзя **переназначить**, но содержимое объектов/массивов можно изменять:

```javascript
const PI = 3.14;
PI = 3; // TypeError

const user = { name: 'Олег' };
user.name = 'Вася'; // ОК, меняем свойство
user = {}; // TypeError, нельзя переназначить саму переменную
```

**Лучшая практика:** используй `const` по умолчанию, `let` — только если нужно переназначение.

### Типы данных

**Примитивные:**

```javascript
let str = "строка"; // string
let num = 42; // number
let float = 3.14; // number (отдельного типа для дробных нет)
let bool = true; // boolean
let nothing = null; // null (намеренное отсутствие значения)
let undef; // undefined (переменная объявлена, но не присвоено значение)
let bigNum = 9007199254740991n; // BigInt (для чисел > 2^53)
let sym = Symbol('id'); // Symbol (уникальный идентификатор)
```

**Ссылочные:**

```javascript
let arr = [1, 2, 3]; // object (массив)
let obj = { key: 'value' }; // object
let func = function() {}; // function (технически тоже object)
```

**Проверка типа:**

```javascript
typeof "текст"; // "string"
typeof 123; // "number"
typeof true; // "boolean"
typeof undefined; // "undefined"
typeof null; // "object" (историческая ошибка в JS)
typeof []; // "object"
Array.isArray([]); // true (правильная проверка массива)
```

***

# 3. ОПЕРАТОРЫ

### Арифметические

```javascript
let a = 10, b = 3;

a + b;  // 13 (сложение)
a - b;  // 7  (вычитание)
a * b;  // 30 (умножение)
a / b;  // 3.333... (деление)
a % b;  // 1  (остаток от деления)
a ** b; // 1000 (возведение в степень)

// Инкремент/декремент
let x = 5;
x++; // 6 (постфиксный, возвращает старое значение)
++x; // 7 (префиксный, возвращает новое значение)
x--; // 6
--x; // 5
```

### Операторы сравнения

```javascript
5 == '5';   // true (нестрогое, приводит типы)
5 === '5';  // false (строгое, проверяет тип)
5 != '5';   // false
5 !== '5';  // true

10 > 5;     // true
10 >= 10;   // true
3 < 7;      // true
3 <= 3;     // true
```

**Всегда используй `===` и `!==`** — это предотвращает неожиданные преобразования типов.

### Логические

```javascript
true && false;  // false (И - оба должны быть true)
true || false;  // true  (ИЛИ - хотя бы один true)
!true;          // false (НЕ - инверсия)

// Короткое замыкание
let user = null;
let name = user && user.name; // null (если user false, дальше не проверяется)
let defaultName = name || 'Гость'; // 'Гость' (если name false, берётся правое)

// Оператор нулевого слияния
let count = 0;
count || 10;  // 10 (0 считается false)
count ?? 10;  // 0  (только null/undefined считаются отсутствием значения)
```

***

# 4. УСЛОВИЯ

### if...else

```javascript
let age = 18;

if (age >= 18) {
  console.log('Совершеннолетний');
} else if (age >= 14) {
  console.log('Подросток');
} else {
  console.log('Ребёнок');
}
```

### Тернарный оператор

Сокращённая форма для простых условий:

```javascript
let age = 20;
let status = age >= 18 ? 'взрослый' : 'несовершеннолетний';

// Можно вкладывать (но лучше не злоупотреблять)
let category = age < 12 ? 'ребёнок' : age < 18 ? 'подросток' : 'взрослый';
```

### switch

Для множественного выбора по одному значению:

```javascript
let day = 3;

switch (day) {
  case 1:
    console.log('Понедельник');
    break; // без break выполнится следующий case
  case 2:
    console.log('Вторник');
    break;
  case 3:
    console.log('Среда');
    break;
  case 6:
  case 7:
    console.log('Выходной'); // несколько case для одного блока
    break;
  default:
    console.log('Неизвестный день');
}
```

**Важно:** без `break` выполнение "провалится" в следующий case (fall-through).

***

# 5. ЦИКЛЫ

### for

Классический цикл с счётчиком:

```javascript
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// Можно пропускать части
for (;;) {
  // бесконечный цикл
}
```

### while

Выполняется, пока условие истинно:

```javascript
let count = 0;
while (count < 3) {
  console.log(count);
  count++;
}
```

### do...while

Выполняется **минимум один раз**, затем проверяет условие:

```javascript
let num = 0;
do {
  console.log(num); // выполнится хотя бы раз
  num++;
} while (num < 0); // условие false, но один раз уже выполнилось
```

### break и continue

```javascript
for (let i = 0; i < 10; i++) {
  if (i === 3) continue; // пропустить итерацию
  if (i === 7) break;    // выйти из цикла
  console.log(i); // 0, 1, 2, 4, 5, 6
}

// Метки для вложенных циклов
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (j === 2) break outer; // выйти из внешнего цикла
    console.log(i, j);
  }
}
```

***

# 6. ФУНКЦИИ

### Объявление функции (Function Declaration)

```javascript
function greet(name) {
  return `Привет, ${name}!`;
}

console.log(greet('Иван')); // Привет, Иван!

// Параметры по умолчанию
function sum(a, b = 0) {
  return a + b;
}
sum(5); // 5 (b = 0)
```

**Hoisting:** функции поднимаются наверх, можно вызвать до объявления:

```javascript
sayHi(); // работает

function sayHi() {
  console.log('Привет');
}
```

### Функциональное выражение (Function Expression)

```javascript
const multiply = function(x, y) {
  return x * y;
};

multiply(3, 4); // 12
```

**Не поднимается** — нельзя вызвать до инициализации.

### Стрелочные функции (Arrow Functions)

Короткий синтаксис для простых функций:

```javascript
// Полная форма
const add = (a, b) => {
  return a + b;
};

// Короткая форма (неявный return)
const add2 = (a, b) => a + b;

// Один параметр — скобки не нужны
const square = x => x * x;

// Без параметров
const getRandom = () => Math.random();
```

**Важные отличия от обычных функций:**
- Нет собственного `this` (берёт из внешней области)
- Нельзя использовать как конструктор
- Нет `arguments`

### Область видимости (Scope)

```javascript
let global = 'глобальная';

function outer() {
  let outerVar = 'внешняя';
  
  function inner() {
    let innerVar = 'внутренняя';
    console.log(global);   // доступ к глобальной
    console.log(outerVar); // доступ к внешней
    console.log(innerVar); // доступ к своей
  }
  
  inner();
  console.log(innerVar); // ReferenceError
}

outer();
```

***

# 7. МАССИВЫ

### Создание

```javascript
let arr1 = [1, 2, 3]; // литерал (используется чаще)
let arr2 = new Array(1, 2, 3); // конструктор
let arr3 = new Array(5); // массив из 5 пустых элементов
```

### Основные методы

```javascript
let fruits = ['яблоко', 'банан'];

// Добавление/удаление
fruits.push('апельсин');    // ['яблоко', 'банан', 'апельсин']
fruits.pop();               // 'апельсин', массив: ['яблоко', 'банан']
fruits.unshift('киви');     // ['киви', 'яблоко', 'банан']
fruits.shift();             // 'киви', массив: ['яблоко', 'банан']

// Поиск
fruits.indexOf('банан');    // 1
fruits.includes('киви');    // false
fruits.find(f => f.length > 5); // 'яблоко'

// Изменение
fruits.splice(1, 0, 'груша'); // вставить на индекс 1
fruits.splice(0, 1);          // удалить 1 элемент с индекса 0
```

### Перебор

```javascript
let numbers = [1, 2, 3, 4, 5];

// forEach — выполнить для каждого элемента
numbers.forEach((num, index) => {
  console.log(`${index}: ${num}`);
});

// map — создать новый массив с преобразованием
let doubled = numbers.map(num => num * 2); // [2, 4, 6, 8, 10]

// filter — отфильтровать элементы
let even = numbers.filter(num => num % 2 === 0); // [2, 4]

// reduce — свернуть в одно значение
let sum = numbers.reduce((acc, num) => acc + num, 0); // 15

// find — первый подходящий элемент
let first = numbers.find(num => num > 3); // 4

// some/every — проверка условия
numbers.some(num => num > 4);  // true
numbers.every(num => num > 0); // true
```

### Деструктуризация

```javascript
let colors = ['красный', 'зелёный', 'синий'];
let [first, second] = colors; // first = 'красный', second = 'зелёный'
let [, , third] = colors;     // third = 'синий'
```

***

# 8. ОБЪЕКТЫ

### Создание и доступ к свойствам

```javascript
let user = {
  name: 'Олег',
  age: 25,
  'full name': 'Олег Петров', // ключ с пробелом
  isActive: true
};

// Доступ
console.log(user.name);        // 'Олег'
console.log(user['age']);      // 25
console.log(user['full name']); // 'Олег Петров'

// Добавление/изменение
user.email = 'oleg@mail.ru';
user.age = 26;

// Удаление
delete user.isActive;

// Проверка наличия свойства
'name' in user;  // true
user.hasOwnProperty('age'); // true
```

### Методы объекта

```javascript
let calculator = {
  a: 10,
  b: 5,
  
  sum() { // короткий синтаксис метода
    return this.a + this.b;
  },
  
  multiply: function() { // классический синтаксис
    return this.a * this.b;
  }
};

calculator.sum();      // 15
calculator.multiply(); // 50
```

### this

Контекст выполнения — **объект, который вызвал метод**:

```javascript
let person = {
  name: 'Иван',
  greet() {
    console.log(`Привет, я ${this.name}`);
  }
};

person.greet(); // Привет, я Иван

let greet = person.greet;
greet(); // Привет, я undefined (this потерян)

// Привязка контекста
let boundGreet = person.greet.bind(person);
boundGreet(); // Привет, я Иван
```

### Перебор свойств

```javascript
let obj = { a: 1, b: 2, c: 3 };

for (let key in obj) {
  console.log(`${key}: ${obj[key]}`);
}

Object.keys(obj);    // ['a', 'b', 'c']
Object.values(obj);  // [1, 2, 3]
Object.entries(obj); // [['a', 1], ['b', 2], ['c', 3]]
```

***

# 9. КЛАССЫ

### Основы

```javascript
class User {
  // Конструктор вызывается при создании экземпляра
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  // Метод экземпляра
  greet() {
    return `Привет, я ${this.name}`;
  }
  
  // Геттер
  get info() {
    return `${this.name}, ${this.age} лет`;
  }
  
  // Сеттер
  set email(value) {
    this._email = value.toLowerCase();
  }
}

let user = new User('Олег', 19);
user.greet();    // Привет, я Олег
user.info;       // Олег, 19 лет
user.email = 'OLEG@MAIL.RU';
console.log(user._email); // oleg@mail.ru
```

### Наследование

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} издаёт звук`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // вызов конструктора родителя
    this.breed = breed;
  }
  
  // Переопределение метода
  speak() {
    return `${this.name} лает`;
  }
  
  // Вызов родительского метода
  parentSpeak() {
    return super.speak();
  }
}

let dog = new Dog('Бобик', 'Овчарка');
dog.speak();       // Бобик лает
dog.parentSpeak(); // Бобик издаёт звук
```

### Статические методы

Вызываются на **классе**, а не на экземпляре:

```javascript
class MathHelper {
  static PI = 3.14;
  
  static square(x) {
    return x * x;
  }
  
  static compare(a, b) {
    return a - b;
  }
}

MathHelper.square(5); // 25
MathHelper.PI;        // 3.14

let helper = new MathHelper();
helper.square(5); // TypeError: не является функцией
```

### Приватные поля (современный JS)

```javascript
class BankAccount {
  #balance = 0; // приватное поле
  
  deposit(amount) {
    this.#balance += amount;
  }
  
  getBalance() {
    return this.#balance;
  }
}

let account = new BankAccount();
account.deposit(100);
console.log(account.getBalance()); // 100
console.log(account.#balance);     // SyntaxError
```

***

# 10. ВЗАИМОДЕЙСТВИЕ С ПОЛЬЗОВАТЕЛЕМ

### alert

Показывает модальное окно с сообщением:

```javascript
alert('Привет, мир!');
// Останавливает выполнение скрипта до закрытия окна
```

**Особенности:**
- Блокирует интерфейс
- Нельзя стилизовать
- Используется редко в современных приложениях

### prompt

Запрашивает ввод от пользователя:

```javascript
let name = prompt('Как тебя зовут?', 'Аноним'); // второй параметр — значение по умолчанию

if (name === null) {
  console.log('Отменено'); // пользователь нажал Cancel
} else if (name === '') {
  console.log('Пустая строка');
} else {
  console.log(`Привет, ${name}!`);
}
```

**Возвращает:**
- Строку с введённым текстом
- `null`, если нажата кнопка "Отмена"

### confirm

Диалоговое окно с вопросом (OK / Cancel):

```javascript
let isConfirmed = confirm('Ты уверен?');

if (isConfirmed) {
  console.log('Подтверждено');
} else {
  console.log('Отменено');
}
```

**Возвращает:**
- `true` при нажатии OK
- `false` при нажатии Cancel

### Современная альтернатива

В реальных проектах используют **HTML-формы** и **модальные окна** на CSS/JS вместо встроенных диалогов:

```javascript
// Пример с input
let input = document.querySelector('#nameInput');
let submitBtn = document.querySelector('#submit');

submitBtn.addEventListener('click', () => {
  let name = input.value;
  console.log(`Привет, ${name}!`);
});
```

***

Эти темы покрывают **базовый синтаксис JavaScript** для начинающих. Следующий шаг — изучение асинхронности (Promise, async/await), работы с DOM, событий и современных фреймворков.