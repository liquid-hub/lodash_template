# Шаблонизатор библиотеки LoDash

В библиотеке LoDash есть метод для обработки шаблонов.

Шаблоны имеют простой **синтаксис**:

`<% javascript code; %>` – javascript код
Между разделителями <% ... %> записывается и выполняется javascript код.

`<%= HTMLString %>` – вывод строки как HTML
Содержимое переменной будет выводиться как HTML.

`<%- text %>` – вывод строки в виде текста.
Теги будут будут преобразованы в текст. Например ``<hr>`` -> `&lt;hr&gt;`

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
