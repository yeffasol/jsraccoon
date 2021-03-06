#ES6: стрелочные функции

На первый взгляд может показаться, что стрелочные функции являются исключительно "синтаксическим сахаром" и не несут в себе никакой новой функциональности. Действительно, их использование выглядит очень удобным и значительно соркащает объемы необходимого кода, особенно если вы сторонник функционального JavaScript:

```javascript
// раньше
var sum = function() {
  return [].reduce.call(arguments, function(m, n) {
    return m + n;
  }, 0);
}

// сейчас
var sum = (...args) => args.reduce((m, n) => m + n, 0);
```

Чтобы разобраться с новой синтаксической конструкцией, рассмотрим простую функцию `add`, которая возвращает сумму двух переданных ей аргументов:

```javascript
// раньше
var add = function(a, b) {
  return a + b;
};

// сейчас 
var add = (a, b) => a + b;
```

Создание стрелочной функции состоит из имени `add`, передаваемых ей параметров `(a, b)` и тела функции `a + b`. 

##### Объявление
Все стрелочные функции создаются с помощью *function expression*, то есть они всегда будут эквиваленты следующему ES5 коду:
```javascript
// function expression: два способа в ES6
var f = function() { return 42; }
var f = () => 42;

// function declaration: один способ в ES6
function f() {
  return 42;
}
```

Поэтому следует помнить, что перед использованием стрелочной функции её всегда необходимо создать заранее.

##### Параметры
Обычно параметры помещаются в круглые скобки `(a, b)`. Если необходимо передать функции всего один параметр, то круглые скобки можно не использовать `var f = x => x * 2`, но лучше подобную возможность не использовать и всегда окружать параметр скобками `var f = (x) => x * 2`, чтобы избежать недопонимания. В случае, если в параметрах нет необходимости, объявление функции будет выглядеть следующим образом:
```javascript
var f = () => 42;

f(); // 42
```

##### Тело функции
В случае, когда необходимо выполнить всего одну инструкцию (как во всех примерах, рассмотренных выше) достаточно просто указать эту инструкцию, как тело функции:
```javascript
var add = (a, b) => a + b; // возвращает a + b
```

В ситуациях, когда одной инструкции оказывается недостаточно, тело функции необходимо заключить в фигурные скобки `{ ... }` и пользоваться ключевым словом `return`:
```javascript
var g = (a, b) => {
  let m = Math.pow(2, parseInt(a));
  return b + m;
}
```

Все возможности обычных функций могут быть использованы и со стрелочными, включая [параметры по умолчанию](http://jsraccoon.ru/es6-defaults), [остаточные параметры](http://jsraccoon.ru/es6-spread-rest), [реструктурирующее присваивание](http://jsraccoon.ru/es6-destructuring) и так далее.

##### Анонимные функции
Чаще всего анонимные стрелочные функции используются при итерациях по элементам массивов с методами `map`, `forEach`, `reduce`, `filter` и другими:
```javascript
const double = (arr) => arr.map( item => item * 2 );
double([1,2,3,4]); // [2, 4, 6, 8]

const filter = (arr) => arr.filter( item => !!item );
filter([false, undefined, 0, 1, 'str']); // [1, "str"]

const sum = (...args) => args.reduce((m, n) => m + n, 0);
sum(10, 15, 20); // 45
```

## this больше не проблема
Главная причина появления стрелочных функций в ES6 — недопонимание, которое возникает у большинства разработчиков при работе с ключевым словом `this`. Чтобы понять, что же такого чудесного принесли с собой стрелочные функции рассмотрим простой пример:
```javascript
// ES5
var obj = {
  btn: document.links[0],
  log: function (message) {
    console.log(message);
    return this;
  },
  init: function() {
    var self = this;
    self.btn.addEventListener('click', function() {
      self.log('Button Clicked!');
    }, false);
  }
};
```

Чтобы выполнить какой-либо метод из объекта до появления стрелочных функций необходимо было воспользоваться одним из методов замены ключевого слова `this`: как в этом примере записать его в переменную `self`, или воспользоваться методом функций `bind`. Данные способы, несмоненно, работали, но выглядели достаточно скверно и могли смутить других разработчиков, которые попытались бы разобраться в коде. Стрелочные функции полностью решают данную проблему. Теперь код, приведённый выше, будет выглядеть следующим образом:
```javascript
// ES6
var obj = {
  btn: document.links[0],
  log: function(message) {
    console.log(message);
    return this;
  },
  init: function() {
    this.btn.addEventListener('click', () => this.log('Button Clicked!'), false);
  }
};
```

Наиболее подробное описание принципов работы стрелочных функций с ключевым словом `this` можно найти в [этой статье](http://blog.getify.com/arrow-this/).
