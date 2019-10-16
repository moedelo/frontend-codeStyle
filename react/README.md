# Руководство по написанию React кода

## Оглавление

  1. [Основные правила](#basic-rules)
  1. [Class против `React.createClass` против компонента без состояния (stateless)](#class-vs-reactcreateclass-vs-stateless)
  1. [Наследование классовых компонентов](#react-class-extends)
  1. [Примеси (mixins)](#mixins)
  1. [Именование](#naming)
  1. [Объявление](#declaration)
  1. [Выравнивание](#alignment)
  1. [Кавычки](#quotes)
  1. [Пробелы](#spacing)
  1. [Свойства (Props)](#props)
  1. [Ссылки (Refs)](#refs)
  1. [Теги](#tags)
  1. [React.Fragments](#react-fragments)
  1. [Методы](#methods)
  1. [Последовательность](#ordering)
  1. [`isMounted`](#ismounted)

## <a name="basic-rules">Основные правила</a>

  - Включайте только один React компонент в файл.
  - Всегда используйте JSX синтаксис.
  - Не используйте `React.createElement`, если вы только не инициализируете программу из файла, который не является JSX.

## <a name="#class-vs-reactcreateclass-vs-stateless">Class против `React.createClass` против компонента без состояния (stateless)</a>

  - Если у вас есть внутреннее состояние (`state`) и/или ссылки (`refs`), отдавайте предпочтение `class extends React.Component` вместо `React.createClass`. eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // плохо
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // хорошо
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    И если у вас нет состояния (`state`) или ссылок (`refs`), отдавайте предпочтение нормальным функциям (не стрелочным функциям), а не классам:

    ```jsx
    // плохо
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // плохо (не рекомендуется делать выводы, опираясь на название функции)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // хорошо
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## <a name="react-class-extends">Наследование классовых компонентов</a>

  - Наследуйте классовые компоненты от `React.Component`
  
## <a name="mixins">Примеси (mixins)</a>

  - [Не используйте примеси](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

    > Почему? Примеcи вносят неявные зависимости, становятся причиной конфликтов имен и быстрого роста сложностей. Для большинства случаев, в которых используются примеси, можно более эффективно применить компоненты, компоненты высшего порядка или вспомогательные модули.

## <a name="naming">Именование</a>

  - **Расширения**: Используйте расширение `.js` для компонентов React.
  - **Имя файла**: Используйте `PascalCase` для названий файлов, например, `ReservationCard.js`.
  - **Именование переменной**: Используйте `PascalCase` для компонентов React и `camelCase` для их экземпляров. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // плохо
    import reservationCard from './ReservationCard';

    // хорошо
    import ReservationCard from './ReservationCard';

    // плохо
    const ReservationItem = <ReservationCard />;

    // хорошо
    const reservationItem = <ReservationCard />;
    ```

  - **Именование компонента**: Называйте файлы так же как и компоненты. Например, `ReservationCard.js` должен содержать внутри компонент `ReservationCard`. Однако корневые компоненты в директории должны лежать в файле `index.js`, и в этом случае название папки должно быть таким же, как название компонента:

    ```jsx
    // плохо
    import Footer from './Footer/Footer';

    // плохо
    import Footer from './Footer/index';

    // хорошо
    import Footer from './Footer';
    ```
  - **Именование компонента высшего порядка**: Используйте сочетание имени компонента высшего порядка и имени переданного в него компонента как свойство `displayName` сгенерированного компонента. Например, из компонента высшего порядка `withFoo()`, которому передан компонент `Bar`, должен получаться компонент с `displayName` равным `withFoo(Bar)`.

    > Почему? Свойство `displayName` может использоваться в инструментах разработчика или сообщениях об ошибках, и если оно ясно выражает связь между компонентами, это помогает понять, что происходит.

    ```jsx
    // плохо
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // хорошо
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

  - **Названия свойств**: Избегайте использования названий свойств DOM-компонента для других целей.

    > Почему? Люди ожидают, что такие свойства как `style` и `className` имеют одно определенное значение. Изменение этого API в вашем приложении ухудшает читабельность и поддержку кода, что может приводить к ошибкам.

    ```jsx
    // плохо
    <MyComponent style="fancy" />

    // хорошо
    <MyComponent variant="fancy" />
    ```

## <a name="declaration">Объявление</a>

  - Не используйте свойство `displayName` для именования компонентов. Вместо этого задавайте имя классу компонента.

    ```jsx
    // плохо
    export default React.createClass({
      displayName: 'ReservationCard',
      // здесь начинается работа
    });

    // хорошо
    export default class ReservationCard extends React.Component {
    }
    ```

## <a name="alignment">Выравнивание</a>

  - Следуйте приведенным ниже стилям для JSX-синтаксиса. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // плохо
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // хорошо
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // если свойства помещаются на одну строку, оставляйте их на одной строке
    <Foo bar="bar" />

    // отступ у дочерних элементов задается как обычно
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
    ```

## <a name="quotes">Кавычки</a>

  - Всегда используйте двойные кавычки (`"`) для JSX-атрибутов, а одинарные кавычки (`'`) для всего остального JS. eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

    > Почему? Для стандартных HTML-атрибутов обычно используются двойные кавычки, а не одинарные, и JSX-атрибуты тоже следуют этому соглашению.

    ```jsx
    // плохо
    <Foo bar='bar' />

    // хорошо
    <Foo bar="bar" />

    // плохо
    <Foo style={{ left: "20px" }} />

    // хорошо
    <Foo style={{ left: '20px' }} />
    ```

## <a name="spacing">Пробелы</a>

  - Всегда вставляйте один пробел в ваш самозакрывающийся тег. eslint: [`no-multi-spaces`](http://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```jsx
    // плохо
    <Foo/>

    // very bad
    <Foo                 />

    // плохо
    <Foo
     />

    // хорошо
    <Foo />
    ```

  - Не отделяйте фигурные скобки пробелами в JSX. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // плохо
    <Foo bar={ baz } />

    // хорошо
    <Foo bar={baz} />
    ```

## <a name="props">Свойства (Props)</a>

  - Всегда используйте `camelCase` для названий свойств.

    ```jsx
    // плохо
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // хорошо
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - Не указывайте значение свойства, когда оно явно `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // плохо
    <Foo
      hidden={true}
    />

    // хорошо
    <Foo
      hidden
    />
    ```

  - Всегда добавляйте свойство `alt` для тегов `<img>`. Если изображение является презентационным, `alt` может быть пустой строкой или `<img>` обязан иметь `role="presentation"`. eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

    ```jsx
    // плохо
    <img src="hello.jpg" />

    // хорошо
    <img src="hello.jpg" alt="Me waving hello" />

    // хорошо
    <img src="hello.jpg" alt="" />

    // хорошо
    <img src="hello.jpg" role="presentation" />
    ```

  - Не используйте такие слова как "изображение" ("image"), "фото" ("photo"), или "картинка" ("picture") в свойстве `alt` тега `<img>`. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > Почему? Скринридеры уже сообщают что `img` элементы являются картинками, так что нет необходимости включать эту информацию в текст свойства `alt`.

    ```jsx
    // плохо
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // хорошо
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Используйте только валидные, не абстрактные [ARIA роли](https://www.w3.org/TR/wai-aria/roles#role_definitions). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // плохо - не ARIA роль
    <div role="datepicker" />

    // плохо - асбтрактная ARIA роль
    <div role="range" />

    // хорошо
    <div role="button" />
    ```

  - Не используйте `accessKey` на элементах. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

    > Почему? Несоответствия между сочетанием комбинаций клавиш и командами с клавиатуры затрудняют доступ для людей, которые пользуются экранными считывателями и клавиатурами.

    ```jsx
    // плохо
    <div accessKey="h" />

    // хорошо
    <div />
    ```

  - Не используйте индексы элементов массива в качестве свойства `key`. Отдавайте предпочтение уникальному ID. ([почему?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

    ```jsx
    // плохо
    {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
    )}

    // хорошо
    {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
    ))}
    ```

## <a name="refs">Ссылки (Refs)</a>

  - Всегда используйте функции обратного вызова. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // плохо
    <Foo
      ref="myRef"
    />

    // хорошо
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```

## <a name="tags">Теги</a>

  - Всегда используйте самозакрывающиеся теги, если у элемента нет дочерних элементов. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // плохо
    <Foo className="stuff"></Foo>

    // хорошо
    <Foo className="stuff" />
    ```

  - Если ваш компонент имеет множество свойств, которые располагаются на нескольких строчках, то закрывайте тег на новой строке. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // плохо
    <Foo
      bar="bar"
      baz="baz" />

    // хорошо
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## <a name="react-fragments">React.Fragments</a>

  - При использовании `React.Fragments` указывайте полное наименование тега, не используйте сокращенный синтаксис (`<>...</>` - не надо так)
  - Избегайте излишнего использования `React.Fragments` ([eslint правило](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-useless-fragment.md))
  

## <a name="methods">Методы</a>

  - Используйте методы со стрелочными функциями для привязывания обработчиков событий для метода `render`. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

    > Почему? Вызов `bind` в методе `render` создает новую функцию при каждой перерисовке.

    ```jsx
    // плохо
    class extends React.Component {
      onClickDiv() {
        // ...
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // средненько
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // ...
      }

      render() {
        return <div onClick={this.onClickDiv} />;
      }
    }

    // хорошо
    class extends React.Component {
      onClickDiv = () => {
        // ...
      }

      render() {
        return <div onClick={this.onClickDiv} />;
      }
    }
    ```

  -  Используйте префикс подчеркивания для именования внутренних методов React компонента.

    ```jsx
    // хорошо
        class extends React.Component {
          onClickSubmit() {
            // ...
          }

          // ...
        }

    // хорошо
    React.createClass({
      _onClickSubmit() {
        // ...
      },

      // ...
    });
    ```

  - Всегда возвращайте значение в методах `render`. eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

    ```jsx
    // плохо
    render() {
      (<div />);
    }

    // хорошо
    render() {
      return (<div />);
    }
    ```

## <a name="ordering">Последовательность</a>

  - Последовательность для `class extends React.Component`:

  1. произвольные `static` методы
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *обработчики кликов или событий*, такие как `onClickSubmit()` или `onChangeDescription()`
  1. *getter методы для `render`*, такие как `getSelectReason()` или `getFooterContent()`
  1. *произвольные render методы*, такие как `renderNavigation()` или `renderProfilePicture()`
  1. `render`

  - Как определять `propTypes`, `defaultProps`, `contextTypes`, и т.д.

    ```jsx
    import React from 'react';
    import Proptypes from 'prop-types';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - Последовательность для `React.createClass`: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *обработчики кликов или событий*, такие как `onClickSubmit()` или `onChangeDescription()`
  1. *getter методы для `render`*, такие как `getSelectReason()` или `getFooterContent()`
  1. *произвольные render методы*, такие как `renderNavigation()` или `renderProfilePicture()`
  1. `render`

## `isMounted`

  - Не используйте `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

    > Почему? [`isMounted` — это антипаттерн][anti-pattern], который недоступен при использовании ES6 классов и который планируют официально признать устаревшим.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[⬆ к оглавлению](#Оглавление)**
