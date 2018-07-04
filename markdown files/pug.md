# Шаблонизатор Pug

## Теория

## Синтаксис

#### Тип документа

```pug
doctype html // скомпилируется в <!DOCTYPE html>
```

#### Текст

Однострочный текст:

```pug
p Hello, my name is Vlad
```
Многострочный текст:
```pug
p.
  Hello
  My name is Vlad
```

#### Атрибуты 
Создание у элемента атрибутов:
```pug
img(src="./img/picture.png", alt="picture", class="my-img")
// то же что и
img(src="./img/picture.png" alt="picture" class="my-img")
```
классы можно записывать так:
```pug
img.my-img(src="./img/picture.png", alt="picture")
```

Можно передать массив с классами как значение атрибута и все значений из массивы станут отдельными классами элемента.

Пример на `Pug`:

```pug
- var classes = ['foo', 'bar', 'baz']
div(class=classes)
```
Что получится в `HTML`:

```html
<div class="foo bar baz"></div>
```

##### Варианты записи атрибутов

Атрибуты можно записывать в столбик, а атрибуты без значения записываются как и в `HTML` (без присвоения значения).

Код `Pug`:

```pug
input(
  type='checkbox'
  name='agreement'
  checked
)
```

Скомпилированный `HTML`:

```html
<input type="checkbox" name="agreement" checked="checked" />
```

##### Повторение атрибутов

Атрибуты можно повторять неограниченное количество раз, например, такой код в `Pug`:

```pug
img.my-popup__img(src="" alt="" class="my-img" class="this-img")
```

скомпилирует в `HTML`:

```html
<img class="my-popup__img my-img this-img" src="" alt="" />
```

#### Циклы

Если нужно использовать внутри цикла JS для Pug, то указывается оператор `=`.

**Первый вариант синтаксиса:**

Выведет внутри `<ul>` 5 тегов `<li>` с текстом `{$}`. Hi!:

```pug
div
  - var x = 5;
  ul
    - for (var i=0; i<=x; i++) {
      li= i + ". Hi!:
    - }
```

**Второй вариант синтаксиса:**

Выведет 3 раза тег `<li>`, а внутри модель телефона

```pug
div
  - var phones = ["iPhone7", "iPhone8", "iPhoneX"];
  ul
    for phone in phones
    li= phone
```

#### Интерполяция

**Первый вариант синтаксиса:**

```pug
- var x = 5;
  p Я купил #{x} яблок! // Я купил 5 яблок!
```

**Второй вариант синтаксиса:**

```pug
- var x = 5;
  p= "Я купил " + x + " яблок!" // Я купил 5 яблок! 
```
**Третий вариант синтаксиса:**

```pug
- var x = "<span>5</span>";
p Я купил !{x} яблок! //  Я купил <span>5</span> яблок!
```

#### Сложный текст

Написать такой html-код:
```html
<div class="pack">
  Осталось <span class="accent lastpack">5</span> упаковок <span class="block">по Акции</span>
</div>
```
можно, например, так:

```pug
.pack Осталось #[span.accent.lastpack 5] упаковок #[span.block по Акции]
```

### Вставка inline стилей

В атрибут `style` необходимо передать объект со стилями:

```pug
.block(style={color: 'red', background: 'green'}) text
```

Скомпилированный `HTML`:

```html
<div class="block" style="color:red;background:green;">text</div>
```

### Экранирование

Можно писать `HTML` код прямо в значениях, а для вывода неэкранированного кода, писать `!=`, например:

```pug
div(escaped="<code>") // <div escaped="&lt;code&gt;"></div>
div(escaped!="<code>") // <div escaped="<code>"></div>
```

### Миксины

Миксины, как функции, принимают параметры параметры в качестве входных данных и генерируют соответствующую разметку. Миксины позволяют создавать переиспользуемые блоки.

Pug:

```pug
mixin pet(name)
  li.pet= name
ul
  +pet('cat')
  +pet('dog')
  +pet('pig')
```

Скомпилированный HTML:

```html
<ul>
  <li class="pet">cat</li>
  <li class="pet">dog</li>
  <li class="pet">pig</li>
</ul>
```

### Условия

#### Обычное условие

Код на `Pug`:

```pug
- var x = false
- var y = false
if x
  h2 true
else if y
  h2 false
else
  h2 wtf
```

Выведет в `HTML`:

```html
<h2>wtf</h2>
```

#### Тернарный оператор

Код `Pug`:

```pug
- var authenticated = true
body(class=authenticated ? 'authed' : 'anon')
```

Скомпилированный `HTML`:

```html
<body class="authed"></body>
```

#### Switch Case
Pug поддерживает switch case, которая представляет собой более наглядный способ сравнить выражение сразу с несколькими вариантами.

```pug
- var friends = 10
case friends
  when 0
    p you have no friends
  when 1
    p you have a friend
  default
    p you have #{friends} friends
```

Конструкция выше выведет в HTML: 
```html
<p>you have 10 friends</p>
```

### Циклы

```pug
ul
  each val, index in ['zero', 'one', 'two']
    li= index + ': ' + val
```

Конструкция выше выведет в HTML:

```html
<ul>
  <li>0: zero</li>
  <li>1: one</li>
  <li>2: two</li>
</ul>
```

### Комментарии

Существуют различные комментариев: те, которые будут отображаться после компиляции (`//`), и те, которые пропадут (`//-`).

#### Однострочные комментарии

Код `Pug`:

```pug
// этот комментарий будет отображаться и в Pug и HTML
//- этот комментарий будет отображаться только в Pug
p foo
p bar
```

Скомпилированный `HTML`:

```html
<!-- этот комментарий будет отображаться и в Pug и HTML -->
<p>foo</p>
<p>bar</p>
```
#### Многострочные комментарии

Код `Pug`:

```pug
body
  //
    этот комментарий будет
    отображаться и в Pug и HTML
  //-
    этот комментарий будет
    отображаться только в Pug
```

Скомпилированный `HTML`:

```html
<body>
  <!--этот комментарий будет
  отображаться и в Pug и HTML-->
</body>
```

## Модульность

Pug позволяет подлкючать модули (одна и та же часть кода) к различным файлам. Например, имеется следующая структура проекта:
```pug
index.pug
modelus/
  price.pug
```

В index.pug в том месте, где необходимо подключить модуль `price.pug`, вызывается функция `include`, которая непосредственно подключает код из файла `price.pug` в то место, где была вызвана:
```pug
.header__price
  include modules/price
.header__form
```

## Примеры

#### Генерация списка с всевозможными параметрами

```pug
mixin features(title, text, src, cls)
  .block
    .block__item(class=cls)
      img.block__img(src="/img/block/" + src + ".jpg")
      .block__title= title
      p.block__capture= text

.header__block    
  +features('title1', 'text1', '1', 'block__item_first')
  +features('title2', 'text2', '2', 'block__item_second')
  +features('title3', 'text3', '3', 'block__item_third')
```

Или вот пример по-лучше:

```pug
- 
 var obj = [
    {title: 'first title', text: 'first text'},
    {title: 'second <span class="accent">title</span>', text: 'second text'},
    {title: 'third title', text: 'third text'}
  ]
mixin features(title, text, idx)
  .block
    .block__item
      img.block__img(src="/img/block/" + idx + ".jpg")
      .block__title!= title
      p.block__capture!= text
each val, idx in obj
  +features(val.title, val.text, idx+1)
```

Скомпилируется в следующий `HTML`:

```html
<div class="block">
    <div class="block__item"><img class="block__img" src="/img/block/1.jpg" />
        <div class="block__title">first title</div>
        <p class="block__capture">first text</p>
    </div>
</div>
<div class="block">
    <div class="block__item"><img class="block__img" src="/img/block/2.jpg" />
        <div class="block__title">second <span class="accent">title</span></div>
        <p class="block__capture">second text</p>
    </div>
</div>
<div class="block">
    <div class="block__item"><img class="block__img" src="/img/block/3.jpg" />
        <div class="block__title">third title</div>
        <p class="block__capture">third text</p>
    </div>
</div>
```

## Другое

* [онлайн редактор](http://jbt.github.io/markdown-editor/) Pug
* [дополнение](https://gist.github.com/neretin-trike/53aff5afb76153f050c958b82abd9228) к библиотеке