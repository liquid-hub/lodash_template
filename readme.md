# Шаблонизатор библиотеки LoDash

[Создание рендер функции и передача данных](https://github.com/liquid-hub/lodash_template#%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85)

Шаблоны имеют простой **синтаксис**:

`<% javascript code; %>` – javascript код

Между разделителями <% ... %> записывается и выполняется javascript код.

---

`<%= HTMLString %>` – вывод строки как HTML

Содержимое переменной будет выводиться как HTML.

---

`<%- text %>` – вывод строки в виде текста.

Теги будут будут преобразованы в текст. Например ``<hr>`` -> `&lt;hr&gt;`

---

## script type="text/template"

Теги script с типом `type="text/template"` служат контейнерами для шаблонов.

Данные записанные в данном теге не будут отображаться на странице, таким образом мы прячем шаблон.

## Пример

```html
<script type="text/template" id="items-template">
<div class="items">
  <% items.forEach(function(item) { %>
  <div class="item-title">
    <%- item.title %>
  </div>
  <div class="item-description">
    <%= item.description %>
  </div>
  <% }); %>
</div>
</script>
```

## Создание рендер функции и передача данных

Чтобы отрисовать шаблон на основе данных, нам нужно создать рендер функцию.

Рендер функция создается из написанного нами шаблона.

Это нужно для того чтобы заменить <%- title %> на данные из javascript объектов.

Пример:

```js
var compiled = _.template('hello <%= user %>!');
compiled({ 'user': 'fred' });
// => 'hello fred!'
```

Чтобы создать рендер функцию нужно передать в метод `_.template` содержимое нашего шаблона.

Для удобства мы записываем шаблоны в тег `script type="text/template"`.

Получившийся результат можем вывести на страницу в виде html кода.

Пример:

```HTML
<div class="js-dinamic"></div>

<script type="text/template" id="hello">
hello <%= user %>!
</script>

<script type="text/javascript">
var compiled = _.template($('#hello').html());
$('.js-dinamic').html(compiled({ 'user': 'fred' }));
</script>
```

[🖥️ Пример на codepen.io](https://codepen.io/brainmurder/pen/QJxPWv)

[🖥️ Пример с динамическими данными на codepen.io](https://codepen.io/brainmurder/pen/RqJOaN)


## Примеры использования javascript кода внутри шаблона

```HTML
<script type="text/template" id="main-template">
  Цикл из массива items.
  <% items.forEach(function(item, index) { %>
  <div class="item">
    (<%- index %>) <%- item %>
  </div>
  <% }); %>

  <div>
    Используя внутри шаблона javascript методы, обязательно ставьте точку с запятой (;).
    <% console.log(items); %>
    <br>
    Но если вызываем функцию которая возвращает значение то точка с запятой не ставится и в разделитель добавляется знак равно (=).
    <%= textUpperCase('text') %>
    => TEXT
  </div>

  <div>
    Операторы сравнения.
    <% if (true) { %>
      Истина
    <% }else{ %>
      Ложь
    <% } %>
    <% if (true) { %>
      Только истина
    <% } %>
  </div>

  Использование кода в атрибутах.
  <% items.forEach(function(item, index) { %>
  <div class="item" data-inex="<%- index %>">
    <%- item %>
  </div>
  <% }); %>
</script>

<script type="text/javascript">
  // Функция делает все буквы заглавными
  function textUpperCase(text) {
    return text.toUpperCase();
  }
</script>
```

[🖥️ Пример на codepen.io](https://codepen.io/brainmurder/pen/GwGLGN)
