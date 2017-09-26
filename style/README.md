### <a name='tableOfContents'>Оглавление</a>
1. [Терминология](#terminology)
1. [CSS](#css)
- [Id's](#ids)
- [Атрибуты](#attrs)
- [Наименование селекторов](#selectorsName)
- [Структура селектора](#selectorStructure)
- [Свойство-значение](#rule)
- [Структура пары свойство-значение](#ruleStructure)
- [Контроллы, компоненты, js](#controlsAndJs)
- [Отступы и пробелы](#whitespaces)
- [!important](#important)
1. [Less](#preprocessor)


<h2 id="terminology">Терминология</h2>

<h3 id="rule-declaration">Объявление правил</h3>

"Объявление правил" это имя данное селектору (или группе селекторов) с сопутствующими ему свойствами. Например:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

<h3 id="selectors">Селекторы</h3>

В объявлении правил "селекторы" - это части, которые определяют, к какому элементу DOM дерева будут применены правила стилей. Селекторы могут соответствовать HTML элементу, а также классу элемента, ID или любому другому атрибуту этого элемента. Вот несколько примеров:


```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

<h3 id="properties">Свойства</h3>

И, наконец, свойства, которые придают выбранным элементам их стиль. Свойства объявляются в виде пары "ключ-значение", объявления правил могут содержать одно или несколько свойств. Объявление свойств может выглядеть так:


```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### <a name='ids'>Id's</a>
Не использовать id (\#) при написании стилей!!! Никогда, даже если не в моготу.
``` CSS
    // плохо
    #page-header {}

    // хорошо
    header {}

     // хорошо
    .pageHeader {}
```

### <a name='attrs'>Атрибуты</a>
Допускается использование атрибутов в стилях (прим: [data-number=float]) только в случае, если они не участвуют в js.

### <a name='selectorsName'>Наименование селекторов</a>
- Основная часть класса именуется в стиле lowerCamelCase.
``` CSS
    // плохо
    .BadClass {}

    // плохо
    .bad-class {}

    // плохо
    .bad_class {}

    // хорошо
    .goodClass {}
```

- Названия классов должны быть короткими насколько возможно, но не терять при этом смысл.
``` CSS
    // плохо(слишком длинный)
    .buttonWhichUseToOpenDialog {}

    // плохо(не ясен смысл)
    .atr {}

    // плохо(избыточный)
    .navigation {}

    // хорошо
    .author {}
    .nav {}
```

- Не использвать теги в связке с классами. Это делает их узкоспециализированными и снижает производительность.
``` CSS
    // плохо
    div.error {}

    // хорошо
    .error {}
```

- Названия классов должны быть абстрактны и выражать смысловое нежели визуальное значение.
``` CSS
    // плохо
    .greenButton {}

    // хорошо
    .successButton {}
```


### <a name='selectorStructure'>Структура селектора</a>
Содержимое селектора оборачивается в фигурные скобки ({}).
``` CSS
    // плохо
    .selector
        display: block;

    // хорошо
    .selector {
        display: block;
    }
```

Открывающая фигурная скобка находится на той же строке, что и селектор.
``` CSS
    // плохо
    .selector
    {
        display: block;
    }

    // хорошо
    .selector {
        display: block;
    }
```

Множественные селекторы пишутся в столбец через запятую
``` CSS
    // плохо
    .selector1, .selector2, .selector3, .selector4 {
        display: block;
    }

    // хорошо
    .selector1,
    .selector2,
    .selector3,
    .selector4 {
        display: block;
    }
```

### <a name='rule'>Свойство-значение</a>
- Старайтесь использовать однострочную запись свойств вместо многострочной.
``` CSS
    // плохо
    padding-bottom: 2em;
    padding-left: 1em;
    padding-right: 1em;
    padding-top: 0;

    // хорошо
    padding: 0 1em 2em;
```

- Не ставьте единицы измерения после нулевых значений. Это бессмыслено, но занимает место.
``` CSS
    // плохо
    margin: 0px

    // хорошо
    margin: 0
```


- Используйте одинарные ('') вместо двойных("") ковычек.
```
    // плохо
    font-family: "open sans", arial, sans-serif;

    // хорошо
    font-family: 'open sans', arial, sans-serif;
```

### <a name='ruleStructure'>Структура пары свойство-значение</a>
- Каждая пара свойство-значение должно заканчиваться точкой с запятой.
``` CSS
    //Плохо
    selector {
        name: value
    }

    //Хорошо
    selector {
        name: value;
    }
```

- **Многострочные**: Каждое свойство должно находиться на своей строке, начиная со строки после фигурной скобки.
Все свойства должны быть выровнены в один стобец.
``` CSS
    //Плохо
    selector {
    name: value;
        name: value;
    }

    //Плохо
    selector {    name: value;
        name: value;
    }

    // Хорошо
    selector {
        name: value;
        name: value;
    }
```

- **Селекторы с одним свойством**: Данные селекторы допускается писать в одну строку.
Свойство отбивается пробелами с обеих сторон.
``` CSS
    // Хорошо
    selector {
        name: value;
    }

     // Хорошо
    selector { name: value; }
```

### <a name='controlsAndJs'>Контроллы, компоненты, js</a>
- **Классы для js**: Старайтесь не использовать в js в качестве селлекторов классы, которые влияют на верстку.
Если в js необходимо привязаться к какому-нибудь элементу по классу, то следует добавить элементу дополнительный класс
с префиксом js.
``` CSS
    // плохо
    $('.mdButton')

    // хорошо
    $('.js-saveButton')
```

- **Контроллы и компоненты**: Классы в компонентах и контроллах должны задаваться по подобию БЭМ нотации.
``` CSS
   .mdSelect {}
   .mdSelect__textField {}
   .mdSelect--largeSize {}
```


### <a name='whitespaces'>Отступы и пробелы</a>
- Селекторы не отбиваются ничем.
- Свойстава отбиваются 4 пробелами
``` CSS
    // плохо
    .selector {
    ∙∙display: block;
    }

    // плохо
    .selector {
    display: block;
    }

    // хорошо
    .selector {
    ∙∙∙∙display: block;
    }
```

- Между многострочными селекторами должна быть пустая строка.
``` CSS
    // плохо
    .selector{
        display: block;
        color: #fff;
    }
    .selector2{
        display: block;
        color: #fff;
    }

    // хорошо
    .selector{
        display: block;
        color: #fff;
    }

    .selector2{
        display: block;
        color: #fff;
    }
```

- Перед открывающейся фигурной скобкой ставится пробел.
``` CSS
    // плохо
    .selector{
        display: block;
    }

    // хорошо
    .selector {
        display: block;
    }
```

- Между свойством и значением ставится пробел.
``` CSS
    // плохо
    .selector{
        display:block;
    }

    // хорошо
    .selector {
        display: block;
    }
```

- <a name='important'>!important</a>
Использовать !important можно только в стилях ядра, для вещей которые никогда не должны быть переопределены.
``` CSS
    // плохо
    .table{
        width: 100% !important;
    }

    // хорошо
    .error{
        color: #CB3338 !important;
    }
```

## <a name='preprocessor'>Less</a>
- Переменные пишутся сamelCase'ом.

- Не используйте глобальные переменные, только локальные(используются внутри одного файла).

- Вложеннные селекторы отбиваются 4 пробелами.
``` Less
    // плохо
    .table{
    ∙∙tr {
        width: 100% !important;
      }
    }

    // хорошо
    .table{
    ∙∙∙∙tr {
            width: 100% !important;
        }
    }
```


- Используйте вложенность для похожих селекторов.
``` Less
    // плохо
    .table > thead > tr > th {}
    .table > thead > tr > td {}

    // хорошо
    .table > thead > tr {
      > th { }
      > td { }
    }
```


- Избегайте неоправданной вложенности. Максимум 3 уровня.
``` Less
    // плохо
    .table{
        tbody {
            tr {
                td {
                    .button{}
                }
            }
        }
    }

    // хорошо
    .table .button{}
```

- Общие цвета, размеры и прочие хаарактеристики следует присваивать переменным.
``` Less
    // плохо
    .error { color: #CB3338; }
    .closeLink { background: #CB3338; }

    // хорошо
    @redColor: #CB3338;
    .error { color: @redColor; }
    .closeLink { background: @redColor; }
```

- Для однотипных кусков свойств целесообразно использовать миксины.
``` Less
    // плохо
    .block {
        -webkit-box-shadow: none;
        box-shadow: none;
    }

    // хорошо
    .box-shadow(@shadow) {
      -webkit-box-shadow: @shadow;
      box-shadow: @shadow;
    }

    .block {
        .box-shadow(none);
    }
```
