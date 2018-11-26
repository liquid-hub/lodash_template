# Шаблонизатор библиотеки LoDash

В библиотеке LoDash есть метод для обработки шаблонов.

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

## Передача данных

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

[Пример на codepen.io](https://codepen.io/brainmurder/pen/QJxPWv)

[Пример с динамическими данными на codepen.io](https://codepen.io/brainmurder/pen/RqJOaN)
