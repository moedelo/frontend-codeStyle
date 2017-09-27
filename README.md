# Руководство по написанию JavaScript кода

Другие руководства

 - [React](react/)
 - [jQuery (deprecated)](jquery/)
 - [Css && Less](style/)
 - [Html](html/)

## Оглавление

  1. [Типы](#types)
  1. [Объявление переменных](#references)
  1. [Объекты](#objects)
  1. [Массивы](#arrays)
  1. [Деструктуризация](#destructuring)
  1. [Строки](#strings)
  1. [Функции](#functions)
  1. [Стрелочные функции](#arrow-functions)
  1. [Классы и конструкторы](#classes--constructors)
  1. [Модули](#modules)
  1. [Итераторы и генераторы](#iterators-and-generators)
  1. [Свойства](#properties)
  1. [Переменные](#variables)
  1. [Подъем](#hoisting)
  1. [Операторы сравнения и равенства](#comparison-operators--equality)
  1. [Блоки](#blocks)
  1. [Управляющие операторы](#control-statements)
  1. [Комментарии](#comments)
  1. [Пробелы](#whitespace)
  1. [Запятые](#commas)
  1. [Точка с запятой](#semicolons)
  1. [Приведение типов](#type-casting--coercion)
  1. [Соглашение об именовании](#naming-conventions)
  1. [Аксессоры](#accessors)
  1. [События](#events)


## <a name="types">Типы</a>

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **Простые типы**: Когда вы взаимодействуете с простым типом, вы напрямую работаете с его значением.

    - `string`
    - `number`
    - `boolean`
    - `null`
    - `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **Сложные типы**: Когда вы взаимодействуете со сложным типом, вы работаете с ссылкой на его значение.

    - `object`
    - `array`
    - `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="references">Объявление переменных</a>

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) Используйте `const` для объявления переменных; избегайте `var`. eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)

    > Почему? Это гарантирует, что вы не сможете переопределять значения, т.к. это может привести к ошибкам и к усложнению понимания кода.

    ```javascript
    // плохо
    var a = 1;
    var b = 2;

    // хорошо
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) Если вам необходимо переопределять значения, то используйте `let` вместо `var`. eslint: [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

    > Почему? Область видимости `let` — блок, у `var` — функция.

    ```javascript
    // плохо
    var count = 1;
    if (true) {
      count += 1;
    }

    // хорошо, используйте let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) Помните, что у `let` и `const` блочная область видимости.

    ```javascript
    // const и let существуют только в том блоке, в котором они определены.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="objects">Объекты</a>

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) Для создания объекта используйте литеральную нотацию. eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // плохо
    const item = new Object();

    // хорошо
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.2](#es6-computed-properties) Используйте вычисляемые имена свойств, когда создаете объекты с динамическими именами свойств.

    > Почему? Они позволяют вам определить все свойства объекта в одном месте.

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // плохо
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // хорошо
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.3](#es6-object-shorthand) Используйте сокращенную запись метода объекта. eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

    ```javascript
    // плохо
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      }
    };

    // хорошо
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      }
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.4](#es6-object-concise) Используйте сокращенную запись свойств объекта. eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

    > Почему? Это короче и понятнее.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // плохо
    const obj = {
      lukeSkywalker: lukeSkywalker
    };

    // хорошо
    const obj = {
      lukeSkywalker
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.5](#objects--grouped-shorthand) Группируйте ваши сокращенные записи свойств в начале объявления объекта.

    > Почему? Так легче сказать, какие свойства используют сокращенную запись.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // плохо
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // хорошо
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.6](#objects--quoted-props) Только недопустимые идентификаторы помещаются в кавычки. eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

    > Почему? На наш взгляд, такой код легче читать. Это улучшает подсветку синтаксиса, а также облегчает оптимизацию для многих JS движков.

    ```javascript
    // плохо
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    };

    // хорошо
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    };
    ```

  <a name="objects--prototype-builtins"></a>
  - [3.7](#objects--prototype-builtins) Не вызывайте напрямую методы `Object.prototype`, такие как `hasOwnProperty`, `propertyIsEnumerable`, и `isPrototypeOf`.

    > Почему? Эти методы могут быть переопределены в свойствах объекта, который мы проверяем `{ hasOwnProperty: false }`, или этот объект может быть `null` (`Object.create(null)`).

    ```javascript
    // плохо
    console.log(object.hasOwnProperty(key));

    // хорошо
    console.log(Object.prototype.hasOwnProperty.call(object, key));

    // отлично
    const has = Object.prototype.hasOwnProperty; // Кэшируем запрос в рамках модуля.
    /* или */
    import has from 'has';
    // ...
    console.log(has.call(object, key));
    ```

  <a name="objects--rest-spread"></a>
  - [3.8](#objects--rest-spread) Используйте оператор расширения вместо [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) для поверхностного копирования объектов. Используйте синтаксис оставшихся свойств, чтобы получить новый объект с некоторыми опущенными свойствами.

    ```javascript
    // очень плохо
    const original = { a: 1, b: 2 };
    const copy = Object.assign(original, { c: 3 }); // эта переменная изменяет `original` ಠ_ಠ
    delete copy.a; // если сделать так

    // плохо
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

    // хорошо
    const original = { a: 1, b: 2 };
    const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

    const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="arrays">Массивы</a>

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) Для создания массива используйте литеральную нотацию. eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // плохо
    const items = new Array();

    // хорошо
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) Для добавления элемента в массив используйте [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) вместо прямого присваивания.

    ```javascript
    const someStack = [];

    // плохо
    someStack[someStack.length] = 'abracadabra';

    // хорошо
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) Для копирования массивов используйте оператор расширения `...`.

    ```javascript
    // плохо
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i += 1) {
      itemsCopy[i] = items[i];
    }

    // хорошо
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a><a name="4.4"></a>
  - [4.4](#arrays--from) Чтобы преобразовать массиво-подобный объект в массив, используйте [Spread operator](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_operator).

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = [ ...foo ];
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.5](#arrays--callback-return) Используйте операторы `return` внутри функций обратного вызова в методах массива. Можно опустить `return`, когда тело функции состоит из одной инструкции, возврщающей выражение без побочных эффектов. [8.2](#arrows--implicit-return). eslint: [`array-callback-return`](http://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map(x => x + 1);

    // плохо
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      flat[index] = flatten;
    });

    // хорошо
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      flat[index] = flatten;
      return flatten;
    });

    // плохо
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // хорошо
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

  <a name="arrays--bracket-newline"></a><a name="4.6"></a>
  - [4.6](#arrays--bracket-newline) Если массив располагается на нескольких строках, то используйте разрывы строк после открытия и перед закрытием скобок.

    ```javascript
    // плохо
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ];

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }];

    const numberInArray = [
      1, 2,
    ];

    // хорошо
    const arr = [[0, 1], [2, 3], [4, 5]];

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ];

    const numberInArray = [
      1,
      2,
    ];
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="destructuring">Деструктуризация</a>

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) При обращении к нескольким свойствам объекта используйте деструктуризацию объекта.

    > Почему? Деструктуризация избавляет вас от создания временных переменных для этих свойств.

    ```javascript
    // плохо
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // хорошо
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // отлично
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) Используйте деструктуризацию массивов.

    ```javascript
    const arr = [1, 2, 3, 4];

    // плохо
    const first = arr[0];
    const second = arr[1];

    // хорошо
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) Используйте деструктуризацию объекта для множества возвращаемых значений, но не делайте тоже самое с массивами. jscs: [`disallowArrayDestructuringReturn`](http://jscs.info/rule/disallowArrayDestructuringReturn)

    > Почему? Вы cможете добавить новые свойства через некоторое время или изменить порядок без последствий.

    ```javascript
    // плохо
    function processInput(input) {
      // затем происходит чудо
      return [left, right, top, bottom];
    }

    // при вызове нужно подумать о порядке возвращаемых данных
    const [left, __, top] = processInput(input);

    // хорошо
    function processInput(input) {
      // затем происходит чудо
      return { left, right, top, bottom };
    }

    // при вызове выбираем только необходимые данные
    const { left, top } = processInput(input);
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="strings">Строки</a>

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) Используйте одинарные кавычки `''` для строк. eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

    ```javascript
    // плохо
    const name = "Capt. Janeway";

    // плохо - литерал шаблонной строки должен содержать интерполяцию или переводы строк
    const name = `Capt. Janeway`;

    // хорошо
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) Строки, у которых в строчке содержится более 150 символов, не пишутся на нескольких строчках с использованием конкатенации.

    > Почему? Работать с разбитыми строками неудобно и это затрудняет поиск по коду.

    ```javascript
    // плохо
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // плохо
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // хорошо
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.3"></a>
  - [6.3](#es6-template-literals) При создании строки программным путем используйте шаблонные строки вместо конкатенации. eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)

    > Почему? Шаблонные строки дают вам читабельность, лаконичный синтаксис с правильными символами перевода строк и функции интерполяции строки.

    ```javascript
    // плохо
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // плохо
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // плохо
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // хорошо
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.4"></a>
  - [6.4](#strings--eval) Никогда не используйте `eval()`, т.к. это открывает множество уязвимостей. eslint: [`no-eval`](http://eslint.org/docs/rules/no-eval)

  <a name="strings--escaping"></a><a name="6.5"></a>
  - [6.5](#strings--escaping) Не используйте в строках необязательные экранирующие символы. eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

    > Почему? Обратные слеши ухудшают читабельность, поэтому они должны быть только при необходимости.

    ```javascript
    // плохо
    const foo = '\'this\' \i\s \"quoted\"';

    // хорошо
    const foo = '\'this\' is "quoted"';
    const foo = `my name is '${name}'`;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="functions">Функции</a>

  <a name="functions--iife"></a><a name="7.1"></a>
  - [7.1](#functions--iife) Оборачивайте в скобки немедленно вызываемые функции. eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

    > Почему? Немедленно вызываемая функция представляет собой единый блок. Чтобы четко показать это — оберните функцию и вызывающие скобки в еще одни скобки. Обратите внимание, что в мире с модулями вам больше не нужны немедленно вызываемые функции.

    ```javascript
    // Немедленно вызываемая функция
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.2"></a>
  - [7.2](#functions--in-blocks) Никогда не объявляйте фукнции в нефункциональном блоке (`if`, `while`, и т.д.). Вместо этого присвойте функцию переменной. Браузеры позволяют выполнить ваш код, но все они интерпретируют его по-разному. eslint: [`no-loop-func`](http://eslint.org/docs/rules/no-loop-func.html)

  <a name="functions--note-on-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--note-on-blocks) **Примечание:** ECMA-262 определяет `блок` как список инструкций. Объявление функции не является инструкцией. [Подробнее в документе ECMA-262](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // плохо
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // хорошо
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.4"></a>
  - [7.4](#functions--arguments-shadow) Никогда не называйте параметр `arguments`. Он будет иметь приоритет над объектом `arguments`, который доступен для каждой функции.

    ```javascript
    // плохо
    function foo(name, options, arguments) {
      // ...
    }

    // хорошо
    function foo(name, options, args) {
      // ...
    }
    ```

  <a name="es6-rest"></a><a name="7.5"></a>
  - [7.5](#es6-rest) Никогда не используйте `arguments`, вместо этого используйте синтаксис оставшихся параметров `...`. eslint: [`prefer-rest-params`](http://eslint.org/docs/rules/prefer-rest-params)

    > Почему? `...` явно говорит о том, какие именно аргументы вы хотите извлечь. Кроме того, такой синтаксис создает настоящий массив, а не массиво-подобный объект как `arguments`.

    ```javascript
    // плохо
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // хорошо
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.6"></a>
  - [7.6](#es6-default-parameters) Используйте синтаксис записи аргументов по умолчанию, а не изменяйте аргументы функции.

    ```javascript
    // очень плохо
    function handleThings(opts) {
      // Нет! Мы не должны изменять аргументы функции.
      // Плохо вдвойне: если переменная opts будет ложной,
      // то ей присвоится пустой объект, а не то что вы хотели.
      // Это приведет к коварным ошибкам.
      opts = opts || {};
      // ...
    }

    // все еще плохо
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // хорошо
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.7"></a>
  - [7.7](#functions--default-side-effects) Избегайте побочных эффектов с параметрами по умолчанию.

    > Почему? И так все понятно.

    ```javascript
    var b = 1;
    // плохо
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.8"></a>
  - [7.8](#functions--defaults-last) Всегда вставляйте последними параметры по умолчанию.

    ```javascript
    // плохо
    function handleThings(opts = {}, name) {
      // ...
    }

    // хорошо
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.9"></a>
  - [7.9](#functions--constructor) Никогда не используйте конструктор функций для создания новых функий. eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

    > Почему? Создание функции в таком духе вычисляет строку подобно eval(), из-за чего открываются уязвимости.

    ```javascript
    // плохо
    var add = new Function('a', 'b', 'return a + b');

    // всё ещё плохо
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="7.10"></a>
  - [7.10](#functions--signature-spacing) Отступы при определении функции. eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

    ```javascript
    // плохо
    const x = function () {};
    const g = function (){};
    const h = function(){};

    // хорошо
    const f = function() {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.11"></a>
  - [7.11](#functions--mutate-params) Никогда не изменяйте параметры. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

    > Почему? Манипуляция объектами, приходящими в качестве параметров, может вызывать нежелательные побочные эффекты в вызывающей функции.

    ```javascript
    // плохо
    function f1(obj) {
      obj.key = 1;
    }

    // хорошо
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    }
    ```

  <a name="functions--reassign-params"></a><a name="7.12"></a>
  - [7.12](#functions--reassign-params) Никогда не переназначайте параметры. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

    > Почему? Переназначенные параметры могут привести к неожиданному поведению, особенно при обращении к `arguments`. Это также может вызывать проблемы оптимизации, особенно в V8.

    ```javascript
    // плохо
    function f1(a) {
      a = 1;
      // ...
    }

    function f2(a) {
      if (!a) { a = 1; }
      // ...
    }

    // хорошо
    function f3(a) {
      const b = a || 1;
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.13"></a>
  - [7.13](#functions--spread-vs-apply) Отдавайте предпочтение использованию оператора расширения `...` при вызове вариативной функции. eslint: [`prefer-spread`](http://eslint.org/docs/rules/prefer-spread)

    > Почему? Это чище, вам не нужно предоставлять контекст, и не так просто составить `new` с `apply`.

    ```javascript
    // плохо
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // хорошо
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // плохо
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

    // хорошо
    new Date(...[2016, 8, 5]);
    ```

  <a name="functions--signature-invocation-indentation"></a><a name="7.14"></a>
  - [7.14](#functions--signature-invocation-indentation) Функции с многострочным определением или запуском должны содержать такие же отступы, как и другие многострочные списки в этом руководстве: с каждым элементом на отдельной строке, с запятой в конце элемента.

    ```javascript
    // плохо
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // хорошо
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // плохо
    console.log(foo,
      bar,
      baz);

    // хорошо
    console.log(
      foo,
      bar,
      baz,
    );
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="arrow-functions">Стрелочные функции</a>

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) Когда вам необходимо использовать функциональное выражение (например, анонимную функцию), используйте стрелочную функцию. eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html) jscs: [`requireArrowFunctions`](http://jscs.info/rule/requireArrowFunctions)

    > Почему? Таким образом создается функция, которая выполняется в контексте `this`, который мы обычно хотим, а также это более короткий синтаксис.

    > Почему бы и нет? Если у вас есть довольно сложная функция, вы можете переместить её логику внутрь собственного объявления.

    ```javascript
    // плохо
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) Если тело функции состоит из одного оператора, возвращающего [выражение](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Expressions_and_Operators#Выражения) без побочных эффектов, то опустите фигурные скобки и используйте неявное возвращение. В противном случае, сохраните фигурные скобки и используйте оператор `return`. eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam), [`requireShorthandArrowFunctions`](http://jscs.info/rule/requireShorthandArrowFunctions)

    > Почему? Синтаксический сахар. Когда несколько функций соединены вместе, то это читается лучше.

    ```javascript
    // плохо
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // хорошо
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // хорошо
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // хорошо
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }));

    // Неявный возврат с побочными эффектами
    function foo(callback) {
      const val = callback();
      if (val === true) {
        // Сделать что-то, если функция обратного вызова вернет true
      }
    }

    let bool = false;

    // плохо
    foo(() => bool = true);

    // хорошо
    foo(() => {
      bool = true;
    });
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) В случае, если выражение располагается на нескольких строках, то необходимо обернуть его в скобки для лучшей читаемости.

    > Почему? Это четко показывает, где функция начинается и где заканчивается.

    ```javascript
    // плохо
    ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    );

    // хорошо
    ['get', 'post', 'put'].map(httpMethod => (
      Object.prototype.hasOwnProperty.call(
        httpMagicObjectWithAVeryLongName,
        httpMethod,
      )
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) Если ваша функция принимает один аргумент и не использует фигурные скобки, то опустите круглые скобки. В противном случае, всегда оборачивайте аргументы круглыми скобками для ясности и последовательности. Примечание: также допускается всегда использовать круглые скобки, в этом случае используйте [вариант "always"](http://eslint.org/docs/rules/arrow-parens#always) для eslint или не включайте  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam) для jscs. eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)
    > Почему? Меньше визуального беспорядка.

    ```javascript
    // плохо
    [1, 2, 3].map((x) => x * x);

    // хорошо
    [1, 2, 3].map(x => x * x);

    // хорошо
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ));

    // плохо
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // хорошо
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) Избегайте схожести стрелочной функции (`=>`) с операторами сравнения (`<=`, `>=`). eslint: [`no-confusing-arrow`](http://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // плохо
    const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

    // плохо
    const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

    // хорошо
    const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

    // хорошо
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height > 256 ? largeSize : smallSize;
    };
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="classes--constructors">Классы и конструкторы</a>

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) Всегда используйте `class`. Избегайте прямых манипуляций с `prototype`.

    > Почему? Синтаксис `class` является кратким и понятным.

    ```javascript
    // плохо
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };

    // хорошо
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) Используйте `extends` для наследования.

    > Почему? Это встроенный способ наследовать функциональность прототипа, не нарушая `instanceof`.

    ```javascript
    // плохо
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this.queue[0];
    };

    // хорошо
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) Методы могут возвращать `this`, чтобы делать цепочки вызовов.

    ```javascript
    // плохо
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // хорошо
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```

  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) Вы можете определить свой собственный метод `toString()`, просто убедитесь, что он успешно работает и не создает никаких побочных эффектов.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) У классов есть конструктор по умолчанию, если он не задан явно. Можно опустить пустой конструктор или конструктор, который только делегирует выполнение родительскому классу. eslint: [`no-useless-constructor`](http://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // плохо
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // плохо
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // хорошо
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) Избегайте дублирующих членов класса. eslint: [`no-dupe-class-members`](http://eslint.org/docs/rules/no-dupe-class-members)

    > Почему? Если объявление члена класса повторяется, без предупреждения будет использовано последнее. Наличие дубликатов скорее всего приведет к ошибке.

    ```javascript
    // плохо
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // хорошо
    class Foo {
      bar() { return 1; }
    }

    // хорошо
    class Foo {
      bar() { return 2; }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="modules">Модули</a>

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) Всегда используйте модули (`import`/`export`) вместо нестандартных модульных систем. Вы всегда сможете транспилировать код в вашу любимую модульную систему.

    > Почему? Модули — это будущее. Давайте начнем использовать будущее уже сейчас!

    ```javascript
    // плохо
    const StyleGuide = require('./StyleGuide');
    module.exports = StyleGuide.es6;

    // ok
    import StyleGuide from './StyleGuide';
    export default StyleGuide.es6;

    // отлично
    import { es6 } from './StyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) Не используйте импорт через `*`.

    > Почему? Это гарантирует, что у вас есть единственный экспорт по умолчанию.

    ```javascript
    // плохо
    import * as StyleGuide from './StyleGuide';

    // хорошо
    import StyleGuide from './StyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) Не экспортируйте прямо из импорта.

    > Почему? Несмотря на то, что запись в одну строку является краткой, разделение на отдельные строки делает вещи последовательными.

    ```javascript
    // плохо
    // файл es6.js
    export { es6 as default } from './StyleGuide';

    // хорошо
    // файл es6.js
    import { es6 } from './StyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports) Импортируйте из пути только один раз.
 eslint: [`no-duplicate-imports`](http://eslint.org/docs/rules/no-duplicate-imports)
    > Почему? Наличие нескольких строк, которые импортируют из одного и того же файла, может сделать код неподдерживаемым.

    ```javascript
    // плохо
    import foo from 'foo';
    // … какие-то другие импорты … //
    import { named1, named2 } from 'foo';

    // хорошо
    import foo, { named1, named2 } from 'foo';

    // хорошо
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports) Не экспортируйте изменяемые переменные.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > Почему? Вообще, следует избегать мутации, в особенности при экспорте изменяемых переменных. Несмотря на то, что эта техника может быть необходима в редких случаях, в основном только константа должна быть экспортирована.

    ```javascript
    // плохо
    let foo = 3;
    export { foo };

    // хорошо
    const foo = 3;
    export { foo };
    ```

  <a name="modules--prefer-default-export"></a>
  - [10.6](#modules--prefer-default-export) В модулях с единственным экспортом предпочтительнее использовать экспорт по умолчанию, а не экспорт по имени.
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > Почему? Для того чтобы поощрять создание множества файлов, которые бы экспортировали одну сущность, т.к. это лучше для читабельности и поддержки кода.

    ```javascript
    // плохо
    export function foo() {}

    // хорошо
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) Поместите все импорты выше остальных инструкций.
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > Почему? Так как `import` обладает подъемом, то хранение их всех в начале файла предотвращает от неожиданного поведения.

    ```javascript
    // плохо
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // хорошо
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

  <a name="modules--multiline-imports-over-newlines"></a>
  - [10.8](#modules--multiline-imports-over-newlines) Импорты на нескольких строках должны быть с отступами как у многострочных литералов массива и объекта.

    > Почему? Фигурные скобки следуют тем же правилам отступа как и любая другая фигурная скобка блока в этом руководстве, тоже самое касается висячих запятых.

    ```javascript
    // плохо
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

    // хорошо
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path';
    ```

  <a name="modules--no-webpack-loader-syntax"></a>
  - [10.9](#modules--no-webpack-loader-syntax) Не используйте синтаксис загрузчика Webpack в импорте.
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > Почему? Использование Webpack синтаксиса связывает код с упаковщиком модулей. Предпочтительно использовать синтаксис загрузчика в `webpack.config.js`.

    ```javascript
    // плохо
    import fooSass from 'css!sass!foo.scss';
    import barCss from 'style!css!bar.css';

    // хорошо
    import fooSass from 'foo.scss';
    import barCss from 'bar.css';
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="iterators-and-generators">Итераторы и генераторы</a>

  <a name="iterators--nope"></a><a name="11.1"></a>
  - [11.1](#iterators--nope) Не используйте итераторы. Применяйте функции высшего порядка вместо таких циклов как `for-in` или `for-of`. eslint: [`no-iterator`](http://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](http://eslint.org/docs/rules/no-restricted-syntax)

    > Почему? Это обеспечивает соблюдение нашего правила о неизменности переменных. Работать с чистыми функциями, которые возвращают значение, проще, чем с функциями с побочными эффектами.

    > Используйте `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... для итерации по массивам, а `Object.keys()` / `Object.values()` / `Object.entries()` для создания массивов, с помощью которых можно итерироваться по объектам.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // плохо
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }
    sum === 15;

    // хорошо
    let sum = 0;
    numbers.forEach((num) => {
      sum += num;
    });
    sum === 15;

    // отлично (используйте силу функций)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;

    // плохо
    const increasedByOne = [];
    for (let i = 0; i < numbers.length; i++) {
      increasedByOne.push(numbers[i] + 1);
    }

    // хорошо
    const increasedByOne = [];
    numbers.forEach((num) => {
      increasedByOne.push(num + 1);
    });

    // отлично (продолжайте в том же духе)
    const increasedByOne = numbers.map(num => num + 1);
    ```

  <a name="generators--nope"></a><a name="11.2"></a>
  - [11.2](#generators--nope) Не используйте пока генераторы.

    > Почему? Они не очень хорошо транспилируются в ES5.

  <a name="generators--spacing"></a>
  - [11.3](#generators--spacing) Если все-таки необходимо использовать генераторы, или вы не обратили внимания на [наш совет](#generators--nope), убедитесь, что `*` у функции генератора расположена должным образом. eslint: [`generator-star-spacing`](http://eslint.org/docs/rules/generator-star-spacing)

    > Почему? `function` и `*` являются частью одного и того же ключевого слова. `*` не является модификатором для `function`, `function*` является уникальной конструкцией, отличной от `function`.

    ```javascript
    // плохо
    function * foo() {
      // ...
    }

    const bar = function * () {
      // ...
    };

    const baz = function *() {
      // ...
    };

    const quux = function*() {
      // ...
    };

    function*foo() {
      // ...
    }

    function *foo() {
      // ...
    }

    // очень плохо
    function
    *
    foo() {
      // ...
    }

    const wat = function
    *
    () {
      // ...
    };

    // хорошо
    function* foo() {
      // ...
    }

    const foo = function* () {
      // ...
    };
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="properties">Свойства</a>

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) Используйте точечную нотацию для доступа к свойствам. eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // плохо
    const isJedi = luke['jedi'];

    // хорошо
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) Используйте скобочную нотацию `[]`, когда название свойства хранится в переменной.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="variables">Переменные</a>

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) Всегда используйте `const` или `let` для объявления переменных. Невыполнение этого требования приведет к появлению глобальных переменных. Необходимо избегать загрязнения глобального пространства имен. eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef) [`prefer-const`](http://eslint.org/docs/rules/prefer-const)

    ```javascript
    // плохо
    superPower = new SuperPower();

    // хорошо
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) Используйте объявление `const` или `let` для каждой переменной. eslint: [`one-var`](http://eslint.org/docs/rules/one-var.html) jscs: [`disallowMultipleVarDecl`](http://jscs.info/rule/disallowMultipleVarDecl)

    > Почему? Таким образом проще добавить новые переменные. Также вы никогда не будете беспокоиться о перемещении `;` и `,` и об отображении изменений в пунктуации. Вы также можете пройтись по каждому объявлению с помощью откладчика, вместо того, чтобы прыгать через все сразу.

    ```javascript
    // плохо
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // плохо
    // (сравните с кодом выше и попытайтесь найти ошибку)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // хорошо
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) В первую очередь группируйте `const`, а затем `let`.

    > Почему? Это полезно, когда в будущем вам понадобится создать переменную, зависимую от предыдущих.

    ```javascript
    // плохо
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // плохо
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // хорошо
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) Создавайте переменные там, где они вам необходимы, но помещайте их в подходящее место.

    > Почему? `let` и `const` имеют блочную область видимости, а не функциональную.

    ```javascript
    // плохо - вызов ненужной функции
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // хорошо
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```
  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment) Не создавайте цепочки присваивания переменных.

    > Почему? Такие цепочки создают неявные глобальные переменные.

    ```javascript
    // плохо
    (function example() {
      // JavaScript интерпретирует это, как
      // let a = ( b = ( c = 1 ) );
      // Ключевое слово let применится только к переменной a;
      // переменные b и c станут глобальными.
      let a = b = c = 1;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // 1
    console.log(c); // 1

    // хорошо
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // throws ReferenceError
    console.log(b); // throws ReferenceError
    console.log(c); // throws ReferenceError

    // тоже самое и для `const`
    ```

  <a name="variables--unary-increment-decrement"></a><a name="13.6"></a>
  - [13.6](#variables--unary-increment-decrement) Избегайте использования унарных инкрементов и декрементов (++, --). eslint [`no-plusplus`](http://eslint.org/docs/rules/no-plusplus)

    > Почему? Согласно документации eslint, унарные инкремент и декремент автоматически вставляют точку с запятой, что может стать причиной трудноуловимых ошибок при инкрементировании и декрементировании значений. Также нагляднее изменять ваши значения таким образом `num += 1` вместо `num++` или `num ++`. Запрет на унарные инкремент и декремент ограждает вас от непреднамеренных преждевременных инкрементаций/декрементаций значений, которые могут привести к непредсказуемому поведению вашей программы.

    ```javascript
    // плохо

    const array = [1, 2, 3];
    let num = 1;
    num++;
    --num;

    let sum = 0;
    let truthyCount = 0;
    for (let i = 0; i < array.length; i++) {
      let value = array[i];
      sum += value;
      if (value) {
        truthyCount++;
      }
    }

    // хорошо

    const array = [1, 2, 3];
    let num = 1;
    num += 1;
    num -= 1;

    const sum = array.reduce((a, b) => a + b, 0);
    const truthyCount = array.filter(Boolean).length;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="hoisting">Подъем</a>

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) Объявления `var` поднимаются к началу своей области видимости, а их присвоение нет. Объявления `const` и `let` работают по новой концепции называемой [Временные Мертвые Зоны (Temporal Dead Zone)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let). Важно знать, почему использовать [typeof больше не безопасно](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

    ```javascript
    // мы знаем, что это не будет работать
    // (если нет глобальной переменной notDefined)
    function example() {
      console.log(notDefined); // => выбросит ошибку ReferenceError
    }

    // обращение к переменной до ее создания
    // будет работать из-за подъема.
    // Примечание: значение true не поднимается.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // интерпретатор понимает объявление
    // переменной в начало области видимости.
    // это означает, что наш пример
    // можно переписать таким образом:
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // использование const и let
    function example() {
      console.log(declaredButNotAssigned); // => выбросит ошибку ReferenceError
      console.log(typeof declaredButNotAssigned); // => выбросит ошибку ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) Для анонимных функциональных выражений наверх области видимости поднимается название переменной, но не ее значение.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous не является функцией

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expresions) Для именованных функциональных выражений наверх области видимости поднимается название переменной, но не имя или тело функции.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named не является функцией

      superPower(); // => ReferenceError superPower не определена

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // тоже самое справедливо, когда имя функции
    // совпадает с именем переменной.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named не является функцией

      var named = function named() {
        console.log('named');
      };
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) При объявлении функции ее имя и тело поднимаются наверх области видимости.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - Более подробно можно прочитать в статье [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) от [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ к оглавлению](#Оглавление)**

## <a name="comparison-operators--equality">Операторы сравнения и равенства</a>

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) Используйте `===` и `!==` вместо `==` и `!=`. eslint: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) Условные операторы, такие как `if`, вычисляются путем приведения к логическому типу `Boolean` через абстрактный метод `ToBoolean` и всегда следуют следующим правилам:
    - **Object** соответствует **true**
    - **Undefined** соответствует **false**
    - **Null** соответствует **false**
    - **Boolean** соответствует **значению булева типа**
    - **Number** соответствует **false**, если **+0, -0, or NaN**, в остальных случаях **true**
    - **String** соответствует **false**, если строка пустая `''`, в остальных случаях **true**

    ```javascript
    if ([0] && []) {
      // true
      // Массив (даже пустой) является объектом, а объекты возвращают true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) Используйте сокращения для булевских типов, а для строк и чисел применяйте явное сравнение.

    ```javascript
    // плохо
    if (isValid === true) {
      // ...
    }

    // хорошо
    if (isValid) {
      // ...
    }

    // плохо
    if (name) {
      // ...
    }

    // хорошо
    if (name !== '') {
      // ...
    }

    // плохо
    if (collection.length) {
      // ...
    }

    // хорошо
    if (collection.length > 0) {
      // ...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>
  - [15.4](#comparison--moreinfo) Более подробную информацию можно узнать в статье [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) от Angus Croll.

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) Используйте фигурные скобки для `case` и `default`, если они содержат лексические декларации (например, `let`, `const`, `function`, и `class`). eslint: [`no-case-declarations`](http://eslint.org/docs/rules/no-case-declarations.html).

    > Почему? Лексические декларации видны во всем `switch` блоке, но инициализируются только при присваивании, которое происходит при входе в блок `case`. Возникают проблемы, когда множество `case` пытаются определить одно и то же.

    ```javascript
    // плохо
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {
          // ...
        }
        break;
      default:
        class C {}
    }

    // хорошо
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {
          // ...
        }
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) Тернарные операторы не должны быть вложены и в большинстве случаев должны быть расположены на одной строке. eslint: [`no-nested-ternary`](http://eslint.org/docs/rules/no-nested-ternary.html).

    ```javascript
    // плохо
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // лучше
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // отлично
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) Избегайте ненужных тернарных операторов. eslint: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary.html).

    ```javascript
    // плохо
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // хорошо
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="blocks">Блоки</a>

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) Используйте фигурные скобки, когда блок кода занимает несколько строк.

    ```javascript
    // плохо
    if (test)
      return false;

    // хорошо
    if (test) return false;

    // хорошо
    if (test) {
      return false;
    }

    // плохо
    function foo() { return false; }

    // хорошо
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) Если блоки кода в условии `if` и `else` занимают несколько строк, расположите оператор `else` на той же строчке, где находится закрывающая фигурная скобка блока `if`. eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style.html) jscs:  [`disallowNewlineBeforeBlockStatements`](http://jscs.info/rule/disallowNewlineBeforeBlockStatements)

    ```javascript
    // плохо
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // хорошо
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="control-statements">Управляющие операторы</a>

   <a name="control-statements"></a>
   - [17.1](#control-statements) Если ваш управляющий оператор (`if`, `while` и т.д.) слишком длинный или превышает максимальную длину строки, то каждое (сгруппированное) условие можно поместить на новую строку. Вам решать, где будет располагаться логический оператор (в начале или в конце строки).

     ```javascript
     // плохо
     if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
       thing1();
     }

     // плохо
     if (foo === 123 &&
       bar === 'abc') {
       thing1();
     }

     // плохо
     if (foo === 123
       && bar === 'abc') {
       thing1();
     }

     // хорошо
     if (
       (foo === 123 || bar === "abc") &&
       doesItLookGoodWhenItBecomesThatLong() &&
       isThisReallyHappening()
     ) {
       thing1();
     }

     // хорошо
     if (foo === 123 && bar === 'abc') {
       thing1();
     }

     // хорошо
     if (
       foo === 123 &&
       bar === 'abc'
     ) {
       thing1();
     }

     // хорошо
     if (
       foo === 123
       && bar === 'abc'
     ) {
       thing1();
     }
     ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="comments">Комментарии</a>

  <a name="comments--multiline"></a><a name="18.1"></a>
  - [18.1](#comments--multiline) Используйте конструкцию `/** ... */` для многострочных комментариев.

    ```javascript
    // плохо
    // make() возвращает новый элемент
    // соответствующий переданному названию тега
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="18.2"></a>
  - [18.2](#comments--singleline) Используйте двойной слэш `//` для однострочных комментариев. Располагайте такие комментарии отдельной строкой над кодом, который хотите пояснить. Если комментарий не является первой строкой блока, добавьте сверху пустую строку.

    ```javascript
    // плохо
    const active = true;  // это текущая вкладка

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    function getType() {
      console.log('fetching type...');
      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // хорошо
    function getType() {
      console.log('fetching type...');

      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }

    // тоже хорошо
    function getType() {
      // установить по умолчанию тип 'no type'
      const type = this.type || 'no type';

      return type;
    }
    ```

  - [18.3](#comments--spaces) Начинайте все комментарии с пробела, так их проще читать. eslint: [`spaced-comment`](http://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // плохо
    //это текущая вкладка
    const active = true;

    // хорошо
    // это текущая вкладка
    const active = true;

    // плохо
    /**
     *make() возвращает новый элемент
     *соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }

    // хорошо
    /**
     * make() возвращает новый элемент
     * соответствующий переданному названию тега
     */
    function make(tag) {

      // ...

      return element;
    }
    ```

  <a name="comments--actionitems"></a><a name="18.4"></a>
  - [18.4](#comments--actionitems) Если комментарий начинается со слов `FIXME` или `TODO`, то это помогает другим разработчикам быстро понять, когда вы хотите указать на проблему, которую надо решить, или когда вы предлагаете решение проблемы, которое надо реализовать. Такие комментарии, в отличие от обычных, побуждают к действию: `FIXME: -- нужно разобраться с этим` или `TODO: -- нужно реализовать`.

  <a name="comments--fixme"></a><a name="18.5"></a>
  - [18.5](#comments--fixme) Используйте `// FIXME:`, чтобы описать проблему.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: здесь не должна использоваться глобальная переменная
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="18.6"></a>
  - [18.6](#comments--todo) Используйте `// TODO:`, чтобы описать решение проблемы.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: нужна возможность задать total через параметры
        this.total = 0;
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="whitespace">Пробелы</a>

  <a name="whitespace--spaces"></a><a name="19.1"></a>
  - [19.1](#whitespace--spaces) Используйте мягкую табуляцию (символ пробела) шириной в 4 пробела. eslint: [`indent`](http://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)

    ```javascript
    // плохо
    function foo() {
    ∙∙∙∙let name;
    }

    // плохо
    function bar() {
    ∙let name;
    }

    // хорошо
    function baz() {
    ∙∙let name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="19.2"></a>
  - [19.2](#whitespace--before-blocks) Ставьте 1 пробел перед открывающей фигурной скобкой у блока. eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)

    ```javascript
    // плохо
    function test(){
      console.log('test');
    }

    // хорошо
    function test() {
      console.log('test');
    }

    // плохо
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // хорошо
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="19.3"></a>
  - [19.3](#whitespace--around-keywords) Ставьте 1 пробел перед открывающей круглой скобкой в операторах управления (`if`, `while` и т.п.). Не оставляйте пробелов между списком аргументов и названием в объявлениях и вызовах функций. eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing.html) jscs: [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)

    ```javascript
    // плохо
    if(isJedi) {
      fight ();
    }

    // хорошо
    if (isJedi) {
      fight();
    }

    // плохо
    function fight () {
      console.log ('Swooosh!');
    }

    // хорошо
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="19.4"></a>
  - [19.4](#whitespace--infix-ops) Разделяйте операторы пробелами. eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

    ```javascript
    // плохо
    const x=y+5;

    // хорошо
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="19.5"></a>
  - [19.5](#whitespace--newline-at-end) В конце файла оставляйте одну пустую строку. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // плохо
    import { es6 } from './StyleGuide';
      // ...
    export default es6;
    ```

    ```javascript
    // плохо
    import { es6 } from './StyleGuide';
      // ...
    export default es6;↵
    ↵
    ```

    ```javascript
    // хорошо
    import { es6 } from './StyleGuide';
      // ...
    export default es6;↵
    ```

  <a name="whitespace--chains"></a><a name="19.6"></a>
  - [19.6](#whitespace--chains) Используйте переносы строк и отступы, когда делаете длинные цепочки методов (больше 2-х методов). Ставьте точку в начале строки, чтобы дать понять, что это не новая инструкция, а продолжение цепочки. eslint: [`newline-per-chained-call`](http://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // плохо
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // плохо
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // хорошо
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // плохо
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // хорошо
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led);

    // хорошо
    const leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="19.7"></a>
  - [19.7](#whitespace--after-blocks) Оставляйте пустую строку между блоком кода и следующей инструкцией. jscs: [`requirePaddingNewLinesAfterBlocks`](http://jscs.info/rule/requirePaddingNewLinesAfterBlocks)

    ```javascript
    // плохо
    if (foo) {
      return bar;
    }
    return baz;

    // хорошо
    if (foo) {
      return bar;
    }

    return baz;

    // плохо
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // хорошо
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // плохо
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // хорошо
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="19.8"></a>
  - [19.8](#whitespace--padded-blocks) Не добавляйте отступы до или после кода внутри блока. eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html) jscs:  [`disallowPaddingNewlinesInBlocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)

    ```javascript
    // плохо
    function bar() {

      console.log(foo);

    }

    // also bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // хорошо
    function bar() {
      console.log(foo);
    }

    // хорошо
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-parens"></a><a name="19.9"></a>
  - [19.9](#whitespace--in-parens) Не добавляйте пробелы между круглыми скобками и их содержимым. eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html) jscs: [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)

    ```javascript
    // плохо
    function bar( foo ) {
      return foo;
    }

    // хорошо
    function bar(foo) {
      return foo;
    }

    // плохо
    if ( foo ) {
      console.log(foo);
    }

    // хорошо
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="19.10"></a>
  - [19.10](#whitespace--in-brackets) Не добавляйте пробелы между квадратными скобками и их содержимым. eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

    ```javascript
    // плохо
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // хорошо
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="19.11"></a>
  - [19.11](#whitespace--in-braces) Добавляйте пробелы между фигурными скобками и их содержимым. eslint: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html) jscs: [`requireSpacesInsideObjectBrackets`](http://jscs.info/rule/requireSpacesInsideObjectBrackets)

    ```javascript
    // плохо
    const foo = {clark: 'kent'};

    // хорошо
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="19.12"></a>
  - [19.12](#whitespace--max-len) Старайтесь не допускать, чтобы строки были длиннее 150 символов (включая пробелы). Замечание: согласно [пункту выше](#strings--line-length), длинные строки с текстом освобождаются от этого правила и не должны разбиваться на несколько строк. eslint: [`max-len`](http://eslint.org/docs/rules/max-len.html) jscs: [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

    > Почему? Это обеспечивает удобство чтения и поддержки кода.

    ```javascript
    // плохо
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // плохо
    $.ajax({ method: 'POST', url: 'https://google.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // хорошо
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // хорошо
    $.ajax({
      method: 'POST',
      url: 'https://google.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="commas">Запятые</a>

<a name="commas--leading-trailing"></a><a name="20.1"></a>
  - [20.1](#commas--leading-trailing) Не начинайте строку с запятой. eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html) jscs: [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)

    ```javascript
    // плохо
    const story = [
        once
      , upon
      , aTime
    ];

    // хорошо
    const story = [
      once,
      upon,
      aTime
    ];

    // плохо
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // хорошо
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers'
    };
    ```

  <a name="commas--dangling"></a><a name="20.2"></a>
  - [20.2](#commas--no-dangling) Не добавляйте висячие запятые. eslint: [`no-comma-dangle`](https://eslint.org/docs/rules/no-comma-dangle)

    ```diff
    // плохо
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
         inventorOf: ['coxcomb chart', 'modern nursing'],
    };

    // хорошо
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale'
         lastName: 'Nightingale',
         inventorOf: ['coxcomb chart', 'modern nursing']
    };
    ```


**[⬆ к оглавлению](#Оглавление)**

## <a name="semicolons">Точка с запятой</a>

  <a name="semicolons--required"></a><a name="21.1"></a>
  - [21.1](#semicolons--required) **Да.** eslint: [`semi`](http://eslint.org/docs/rules/semi.html)

    ```javascript
    // плохо
    (function () {
      const name = 'Skywalker'
      return name
    })()

    // хорошо
    (function () {
      const name = 'Skywalker';
      return name;
    }());

    // хорошо, но уже устарело
    // (такая защита функций нужна, когда конкатенируются два файла, содержащие немедленно вызываемые функции)
    ;((() => {
      const name = 'Skywalker';
      return name;
    })());
    ```

    [Читать подробнее](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214).

**[⬆ к оглавлению](#Оглавление)**

## <a name="type-casting--coercion">Приведение типов</a>

  <a name="coercion--explicit"></a><a name="22.1"></a>
  - [22.1](#coercion--explicit) Выполняйте приведение типов в начале инструкции.

  <a name="coercion--strings"></a><a name="22.2"></a>
  - [22.2](#coercion--strings)  Строки:

    ```javascript
    // => this.reviewScore = 9;

    // плохо
    const totalScore = this.reviewScore + ''; // вызывает this.reviewScore.valueOf()

    // плохо
    const totalScore = this.reviewScore.toString(); // нет гарантии что вернется строка

    // хорошо
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="22.3"></a>
  - [22.3](#coercion--numbers) Числа: Используйте `Number` и `parseInt` с основанием системы счисления. eslint: [`radix`](http://eslint.org/docs/rules/radix)

    ```javascript
    const inputValue = '4';

    // плохо
    const val = new Number(inputValue);

    // плохо
    const val = +inputValue;

    // плохо
    const val = inputValue >> 0;

    // плохо
    const val = parseInt(inputValue);

    // хорошо
    const val = Number(inputValue);

    // хорошо
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--bitwise"></a><a name="22.4"></a>
  - [22.5](#coercion--bitwise) Не используйте побитовыми операциями.

    ```javascript
    2147483647 >> 0; // => 2147483647
    2147483648 >> 0; // => -2147483648
    2147483649 >> 0; // => -2147483647
    ```

  <a name="coercion--booleans"></a><a name="22.5"></a>
  - [22.6](#coercion--booleans) Логические типы:

    ```javascript
    const age = 0;

    // плохо
    const hasAge = new Boolean(age);

    // хорошо
    const hasAge = Boolean(age);

    // отлично
    const hasAge = !!age;
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="naming-conventions">Соглашение об именовании</a>

  <a name="naming--descriptive"></a><a name="23.1"></a>
  - [23.1](#naming--descriptive) Избегайте названий из одной буквы. Имя должно быть наглядным. eslint: [`id-length`](http://eslint.org/docs/rules/id-length)

    ```javascript
    // плохо
    function q() {
      // ...
    }

    // хорошо
    function query() {
      // ...
    }
    ```

  <a name="naming--camelCase"></a><a name="23.2"></a>
  - [23.2](#naming--camelCase) Используйте `camelCase` для именования объектов, функций и экземпляров. eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

    ```javascript
    // плохо
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // хорошо
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="23.3"></a>
  - [23.3](#naming--PascalCase) Используйте `PascalCase` только для именования конструкторов и классов. eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)

    ```javascript
    // плохо
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // хорошо
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="23.4"></a>
  - [23.4](#naming--leading-underscore) Используйте `_` в начале названий, для обозначения приватных переменных.

    ```javascript
    this._firstName = 'Panda';
    ```

  <a name="naming--self-this"></a><a name="23.5"></a>
  - [23.5](#naming--self-this) Не сохраняйте ссылку на `this`. Используйте стрелочные функции или [метод bind()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind). jscs: [`disallowNodeTypes`](http://jscs.info/rule/disallowNodeTypes)

    ```javascript
    // плохо
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // плохо
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // хорошо
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="23.6"></a>
  - [23.6](#naming--filename-matches-export) Название файла точно должно совпадать с именем его экспорта по умолчанию.

    ```javascript
    // содержание файла 1
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // содержание файла 2
    export default function fortyTwo() { return 42; }

    // содержание файла 3
    export default function insideDirectory() {}

    // в других файлах
    // плохо
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // плохо
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // хорошо
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ поддерживает оба варианта: insideDirectory.js и insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="23.7"></a>
  - [23.7](#naming--camelCase-default-export) Используйте `camelCase`, когда экспортируете функцию по умолчанию. Ваш файл должен называться так же, как и имя функции.
    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="23.8"></a>
  - [23.8](#naming--PascalCase-singleton) Используйте `PascalCase`, когда экспортируете конструктор / класс / синглтон / библиотечную функцию / объект.

    ```javascript
    const StyleGuide = {
      es6: {
      },
    };

    export default StyleGuide;
    ```

  <a name="naming--Acronyms-and-Initialisms"></a>
  - [23.9](#naming--Acronyms-and-Initialisms) Сокращения или буквенные аббревиатуры всегда должны писаться заглавными буквами или строчными.

    > Почему? Имена для удобства чтения, а не для удовлетворения компьютерных алгоритмов.

    ```javascript
    // плохо
    import SmsContainer from './containers/SmsContainer';

    // плохо
    const HttpRequests = [
      // ...
    ];

    // хорошо
    import SMSContainer from './containers/SMSContainer';

    // хорошо
    const HTTPRequests = [
      // ...
    ];

    // отлично
    import TextMessageContainer from './containers/TextMessageContainer';

    // отлично
    const Requests = [
      // ...
    ];
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="accessors">Аксессоры</a>

  <a name="accessors--not-required"></a><a name="24.1"></a>
  - [24.1](#accessors--not-required) Функции-аксессоры для свойств объекта больше не нужны.

  <a name="accessors--no-getters-setters"></a><a name="24.2"></a>
  - [24.2](#accessors--no-getters-setters) Не используйте геттеры/сеттеры, т.к. они вызывают неожиданные побочные эффекты, а также их тяжело тестировать, поддерживать и понимать. Вместо этого создавайте методы `getVal()` и `setVal('hello')`.

    ```javascript
    // плохо
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // хорошо
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="24.3"></a>
  - [24.3](#accessors--boolean-prefix) Если свойство/метод возвращает логический тип, то используйте названия `isVal()` или `hasVal()`.

    ```javascript
    // плохо
    if (!dragon.age()) {
      return false;
    }

    // хорошо
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  <a name="accessors--consistent"></a><a name="24.4"></a>
  - [24.4](#accessors--consistent) Можно создавать функции `get()` и `set()`, но нужно быть последовательным.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ к оглавлению](#Оглавление)**

## <a name="events">События</a>

  <a name="events--hash"></a><a name="25.1"></a>
  - [25.1](#events--hash) Когда привязываете данные к событию (например, события `DOM` или какие-то собственные события, как `Backbone` события), передавайте объект вместо простого значения. Это позволяет другим разработчикам добавлять больше данных без поиска и изменения каждого обработчика события. К примеру, вместо:

    ```javascript
    // плохо
    $(this).trigger('listingUpdated', listing.id);

    // ...

    $(this).on('listingUpdated', (e, listingId) => {
      // делает что-то с listingId
    });
    ```

    предпочитайте:

    ```javascript
    // хорошо
    $(this).trigger('listingUpdated', { listingId: listing.id });

    // ...

    $(this).on('listingUpdated', (e, data) => {
      // делает что-то с data.listingId
    });
    ```

  **[⬆ к оглавлению](#Оглавление)**
