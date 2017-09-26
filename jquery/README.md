# Руководство по написанию jQuery кода

  <a name="jquery--no-jquery"></a><a name="26.1"></a>
  - [26.1](#jquery--no-jquery) Не используйте jquery в "новом коде"

  > Почему? Библиотека досточно тяжелая, а многое ее фичи реализуются посредством нативного js / css


  <a name="jquery--dollar-prefix"></a><a name="26.2"></a>
  - [26.2](#jquery--dollar-prefix) Начинайте названия переменных, хранящих объект jQuery, со знака `$`.

    ```javascript
    // плохо
    const sidebar = $('.sidebar');

    // хорошо
    const $sidebar = $('.sidebar');

    // хорошо
    const $sidebarBtn = $('.sidebar-btn');
    ```

  <a name="jquery--cache"></a><a name="26.3"></a>
  - [26.3](#jquery--cache) Кэшируйте jQuery-поиски.

    ```javascript
    // плохо
    function setSidebar() {
      $('.sidebar').hide();

      // ...

      $('.sidebar').css({
        'background-color': 'pink',
      });
    }

    // хорошо
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...

      $sidebar.css({
        'background-color': 'pink',
      });
    }
    ```

  <a name="jquery--queries"></a><a name="26.4"></a>
  - [26.4](#jquery--queries) Для поиска в DOM используйте каскады `$('.sidebar ul')` или селектор родитель > ребенок `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="26.5"></a>
  - [26.5](#jquery--find) Используйте функцию `find` для поиска в сохраненных jQuery-объектах.

    ```javascript
    // плохо
    $('ul', '.sidebar').hide();

    // плохо
    $('.sidebar').find('ul').hide();

    // хорошо
    $('.sidebar ul').hide();

    // хорошо
    $('.sidebar > ul').hide();

    // хорошо
    $sidebar.find('ul').hide();
    ```
