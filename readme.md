# Шаблонизатор библиотеки LoDash

- [Создание рендер функции и передача данных](https://github.com/liquid-hub/lodash_template#%D1%81%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%80%D0%B5%D0%BD%D0%B4%D0%B5%D1%80-%D1%84%D1%83%D0%BD%D0%BA%D1%86%D0%B8%D0%B8-%D0%B8-%D0%BF%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0-%D0%B4%D0%B0%D0%BD%D0%BD%D1%8B%D1%85)
- [Примеры использования javascript кода внутри шаблона](https://github.com/liquid-hub/lodash_template#%D0%BF%D1%80%D0%B8%D0%BC%D0%B5%D1%80%D1%8B-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F-javascript-%D0%BA%D0%BE%D0%B4%D0%B0-%D0%B2%D0%BD%D1%83%D1%82%D1%80%D0%B8-%D1%88%D0%B0%D0%B1%D0%BB%D0%BE%D0%BD%D0%B0)
- [Вывод массива товаров](https://github.com/liquid-hub/lodash_template#%D0%B2%D1%8B%D0%B2%D0%BE%D0%B4-%D0%BC%D0%B0%D1%81%D1%81%D0%B8%D0%B2%D0%B0-%D1%82%D0%BE%D0%B2%D0%B0%D1%80%D0%BE%D0%B2-commonv2)

---

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


## Вывод массива товаров (Common.V2)

```HTML
<script type="text/template" id="products-template">
<% products.forEach(function(item, product) { %>
  <form class="card" data-product-id="<%= product.id %>">
    <a href="<%= product.url %>" class="card-image">
      <img src="<%= product.first_image.large_url %>">
    </a>

    <div class="card-title">
      <a href="<%= product.url %>">
        <%= product.title %>
      </a>
    </div>

    <div class="card-prices">
      <div class="card-price">
        <%= Shop.money.format(product.variants[0].price) %>
      </div>
      <% if (product.variants[0].old_price){ %>
        <div class="card-old_price">
          <%= Shop.money.format(product.variants[0].old_price) %>
        </div>
        <% } %>
      </div>

      <div class="card-action">
        <input type="hidden" name="variant_id" value="<%= product.variants[0].id %>" >

        <div data-quantity>
          <input type="text" name="quantity" value="1" />
          <span data-quantity-change="-1">-</span>
          <span data-quantity-change="1">+</span>
        </div>
      </div>

      <div class="card-buy">
        <% if (product.variants.size > 1){ %>
          <a href="<%= product.url %>" class="btn">Подробнее</a>
        <% }else{ %>
          <button data-item-add class="btn" type="submit">В корзину</button>
        <% } %>
      </div>
  </form>
<% }); %>

<!-- Если нужно узнать какие переменные переданы в рендер функцию, нужно вывести переменную obj.
<% console.log(obj); %> -->
</script>
```
