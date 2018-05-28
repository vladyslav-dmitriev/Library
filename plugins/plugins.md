# JavaScript Plugins

**JavaScript Plugins** - полезное руководство по настройке, подключению и использовании различных полезных JavaScript плагинов.

### Содержание

1. [Slick.js](http://kenwheeler.github.io/slick/) - мощный слайдер карусели с большим функционалом и гибкими настройками 
2. [Waypoints](http://imakewebthings.com/waypoints/) - плагин навешивает обработчики на указанные разработчиком элементы, которые срабатывают при скроле
3. [Select2](https://select2.org/) - самая популярная библиотека для стилизации выпадающих списков `select` и их гибкой настройки
4. [Simplebar](https://github.com/Grsmto/simplebar) - отличный плавный и кроссбраузерный плагин для стилизации скролбара внутри какого-то блока
5. [FullPage.js](https://alvarotrigo.com/fullPage/ru/#page2/1) - самая популярная, крутая и многофункциональная библиотека для создания поблочной прокрутки на сайте, вся официальная документация на русском языке


### Кандидат на добавление
* [superscrollorama](http://johnpolacek.github.io/superscrollorama/) -  jQuery-плагин для создания привлекательных эффектов для скроллинга
* [mmenu](http://mmenu.frebsite.nl/) - библиотека используется для добавления на сайт адаптивного многоуровневого меню
* [sweep](https://github.com/rileyjshaw/sweep) - библиотека используется для интересного и глубокого изменения цветов 
* [Magnific-Popup](https://github.com/dimsemenov/Magnific-Popup) - удобный и производительный jQuery плагин для создания и настройки всплывающих окон


## Slick.js

#### Установка для webpack 4

`npm install slick-carousel`

Код файла `index.js`

```javascript
import 'slick-carousel';
require('slick-carousel/slick/slick.css');
require('slick-carousel/slick/slick-theme.css');

$('.my-slider').slick();
```

#### Базовые настройки

**Прокрутка**

* `infinite: false` - активация бесконечной прокрутки (по-умолчанию `false`)
* `autoplay: true` - активация автоматической прокрутки (по-умолчанию `false`)
* `speed: 700` - установление скорости самой прокрутки в значение `700 мс` (по-умолчанию `300`)
* `autoplaySpeed: 500` - установление скорости автоматической смены слайдов в значение `500 мс` (по-умолчанию `3000`)

**Отображение**

* `initialSlide: 1` - устанавливает номер слайда (отсчет с 0), с которого начинать показ (по-умолчанию `0`)
* `adaptiveHeight: true` - включает адаптирование высоты для одиночного слайда горизонтальной карусели (по-умолчанию `false`)
* `fade: true` - включает эффект затухания при перелистывании слайдов (по-умолчанию `false`)
* `cssEase: 'linear'` - устанавливает тип анимации для настройки `fade` эффекта (по-умолчанию `ease`)
* `draggable: false` - отменяет возможность перелистывать слайды только мышкой (по-умолчанию `true`)
* `swipe: false` - отменяет возможность перелистывать слайды мышкой и пальцем (по-умолчанию `true`)

**Навигация**

* `dots: true` - активация точек навигации (по-умолчанию `false`)
* `arrows: false` - отключение стрелок переключения слайдов (по-умолчанию `true`)
* `prevArrow: '<button type="button">Назад</button>'` - замена стандартной кнопки Preview
* `nextArrow: '<button type="button">Вперед</button>'` - замена стандартной кнопки Next

**Стилизация стрелок и точек навигации**

Стрелки и/или точки будут вынесены за пределы слайдера в указанный контейнер и будут иметь любой настраиваемый вид, для этого в настройках плагина в файле `index.js`:

```javascript
// стрелки
arrows: true
appendArrows: '.my-arrows'
prevArrow: '<button type="button">Назад</button>'
nextArrow: '<button type="button">Вперед</button>'

// точки
dots: true
appendDots: '.my-dots'
```

На странице в `index.html`:

```html
<div class="my-arrows"></div>
<div class="my-dots"></div>
```

**Адаптивость**

```javascript
$('.responsive-slider').slick({
// другие настройки,
  responsive: [
    {
      breakpoint: 2000,
      settings: {
        slidesToShow: 4,
        slidesToScroll: 1
      },
    },
    {
      breakpoint: 991,
      settings: {
        slidesToShow: 3,
        slidesToScroll: 1
      }
    }
  ]
});
```

**Получение номера текущего слайда**

Номер будет выводится перед символом `/` и автоматически изменяться при каждом перелистывании. Для этого в файле `index.js`:

```javascript
$(function(){
  $('.slider').slick({    
    autoplay: true,
    autoplaySpeed: 2000,    
  });

  $(".slider").on('afterChange', function(event, slick, currentSlide){
     $("#cp").text(currentSlide + 1);
  });
});
```

А в `index.html`:

```html
<div class="slider">
  <div>your content 1</div>
  <div>your content 2</div>
  <div>your content 3</div>
</div>

<div><span id="cp">1</span>/3</div>
```

При этом, изначальный номер текущего слайда при загрузке страницы, который будет отображаться перед символом `/`, будет равен тому, что указан в самом `html` перед  `/`.

**Синхронизация нескольких слайдеров между собой**

Необходимо в свойстве `asNavFor` указать одинаковый для всех класс `.my-slider`, а каждый слайдер тогда можно стилизовать уже по своему уникальному модификатору. Настройки в  `index.js`:

```javascript
$('.my-slider--first').slick({
  slidesToShow: 1,
  asNavFor: '.my-slider'
});

$('.my-slider--second').slick({
  slidesToShow: 1,
  asNavFor: '.my-slider'
});

$('.my-slider--third').slick({
  slidesToShow: 3,
  asNavFor: '.my-slider'
});
```

Код страницы `index.html`:

```html
<div class="my-slider my-slider--first">
  <div class="my-slide"></div>
  <div class="my-slide"></div>
</div>

<div class="my-slider my-slider--second">
  <div class="my-slide"></div>
  <div class="my-slide"></div>
</div>

<div class="my-slider my-slider--third">
  <div class="my-slide"></div>
  <div class="my-slide"></div>
</div>
```

#### Методы и события слайдера


`slickCurrentSlide` — возвращает номер текущего слайда. Отсчёт ведётся с нуля.
`slickPause` — останавливает автоматическую прокрутку.

```javascript
$('.my-slider').slick('slickCurrentSlide'); // прокручивает на слайд с указанным номером.
$('.my-slider').slick('slickGoTo',4); // прокручивает на один слайд вперёд.
$('.my-slider').slick('slickNext'); // прокручивает на один слайд назад.
```

Например, после прокрутки к 5 слайду мы должны сделать какое-то действие, для этого включаем обработчик события `afterChange`. Если действие следует совершить перед осуществлением перехода, то задействуем событие `beforeChange`.

```javascript
$('#slick-slider').on('afterChange', function(event, slick, currentSlide){
  if (currentSlide == 5) { console.log('Осуществлён переход к 5му слайду'); }
});

$('#slick-slider').on('beforeChange', function(event, slick, currentSlide, nextSlide){
  console.log('Собираемся осуществить переход к '+nextSlide+' слайду');
});
```

#### Ссылки 

* Больше информации по настройкам `slick.js` тут:
[http://realadmin.ru/saytostroy/slick-slider.html](http://realadmin.ru/saytostroy/slick-slider.html)
* Разное размещение слайдов в пределех контейнера [https://toster.ru/q/440038](https://toster.ru/q/440038)


## Waypoints

#### Установка для webpack 4

Подключение в файле `index.js`:

```javascript
require('waypoints/lib/jquery.waypoints.min.js');
require('waypoints/lib/shortcuts/inview.min.js');

var inview = new Waypoint.Inview({
  element: $('#block'),
  enter: function(direction) {
    alert('Hello!');
    inview.destroy();
  }
})
```
Тут `alert` сработает, когда элемент `#block` попадет во `viewport`.

`inview.destroy();` - без этой строчки плагин будет срабатывать каждый раз, когда элемент входит в область viewport. А с этой строчкой - только один раз.

Так же у плагина есть возможность следить за направлением скрола в параметре `direction` (вверх - вниз).

## Select2

#### Установка для webpack 4

Подключение в файле `index.js`:

```javascript
import 'select2';
import 'select2/dist/css/select2.css';

$('#city').select2();
```

Подключение на странице `index.html`:

```html
<select name="" id="city">
  <option value="ua">Ukraine</option>
  <option value="usa">USA</option>
</select>
```

#### Базовые настройки

**width у select**
```javascript
$('#city').select2({
  width: '90%'
});
```

Параметр `width` настраивает ширину `select`, можно задавать в любых единицах.


**Стилизация**

Для настройки стилизации в `select2` используется свойство `theme`. По-умолчанию оно равно `default` и определяет базовые стили.

Сбросить практически все стилевые правила можно, задав собственную тему, например: `example`. Тогда можно стилизовать определенную тему и применять ее только к нужным `select`.

В стилевых правилах нужна тема задается как модификатор блока `.select-container` (например как: `.select2-container--my`). Таким образом можно по-разному стилизовать несколько select на странице, например: 


Пример установления и настройки собственной темы в `index.js`:

```javascript
$('#city').select2({
  width: '90%',
  theme: 'example'
});

$('#device').select2({
  width: '50%',
  theme: 'my'
});
```

Пример стилизации в `main.sass`:

```sass
.select2-container--example .select2-results__option--highlighted[aria-selected]
  background: #5897fb

.select2-container--my .select2-results__option--highlighted[aria-selected]
  background: #faa

.select2-container--my .select2-search__field
  display: none
```

**События и методы select2**

[На этой странице](https://select2.org/programmatic-control/events) можно увидеть события, которые генерирует `select2` и использовать их для необходимых настроек, например:

Настройки в `index.js`:

```javascript
var selectLang = $("#lang");
var arrow = $('.select2-container--lang .select2-selection__arrow');

selectLang.on("select2:open", function (e) {
  arrow.css('transform', 'rotate(180deg)');
});

selectLang.on("select2:close", function (e) {
  arrow.css('transform', 'rotate(0)');
});
```

Пример стилизации `select` в файле `main.sass`:

```sass
.select2-container--lang .select2-selection__arrow
  display: block
  position: absolute
  top: 7px
  right: 0
  width: 0
  height: 0
  border-style: solid
  border-width: 6px 6px 0 6px
  border-color: #fff transparent transparent transparent
  transition: all 0.1s ease 
```

## Simplebar

Проверен в IE, в мобильном хроме и в родном браузере на xiaomi - полная кроссбраузерность  и идентичность на всех браузерах.

#### Установка для webpack 4

Подключение в файле `index.js`:

```javascript
import test from 'script-loader!simplebar';
import 'simplebar/dist/simplebar.css';

new SimpleBar(document.getElementById('scrollEl'), {
    autoHide: false,
    classNames: {
    content: 'simplebar-content', // представляет собой контейнер, содержащий элементы, которые прокручиваются
    scrollContent: 'simplebar-scroll-content', // представляет собой оболочку для прокручиваемого содержимого
    scrollbar: 'simplebar-scrollbar', // определяет стиль полосы прокрутки, с которой пользователь может взаимодействовать для прокрутки содержимого
    track: 'simplebar-track' // стилирует область вокруг полосы прокрутки
  }
})
```

Пример со стилями `main.sass`:

```sass
.simplebar
  &-content
    background: olive
    padding: 10px 20px 10px 10px
  &-scroll-content
    background: #faa
  &-scrollbar
    background: green
    border: none
  &-track
    background: blue
```

Подключение на странице `index.html`:

```html
<div id="scrollEl">Lorem 1000</div>
```

## FullPage.js

Как альтернатива этой библиотеке существует плагин [OnePageScroll.js](https://github.com/peachananr/onepage-scroll).

#### Установка для webpack 4

Подключение в файле `index.js`:

```javascript
import 'fullpage.js/dist/jquery.fullpage.min.css';
import 'fullpage.js';

$(document).ready(function() {
  $('#fullpage').fullpage({
    navigation: true, // отображаение точек навигации
    navigationPosition: 'right', // расположение точек навигации (по-умолчанию - слева)
    navigationTooltips: ['Главная', 'О продукте', 'Цены', 'Контакты'], // отображаемые названия разделов
    showActiveTooltip: true, // показывать название экранов по-умолчанию
    responsiveWidth: '767', // ширина, ниже которой плагин fullpage перестает работать и страница отображается в обычном режиме с обычным скролом
    anchors: ['firstPage', 'secondPage', 'thirdPage', 'fourthPage'],
    menu: '#myMenu', // класс для дополнительного меню
    paddingTop: '80px' // отступ сверху на каждом экране чтобы там можно было расположить меню 
  });
});
```

Пример настроек в `main.sass`:

```sass
#myMenu
  z-index: 100
  position: fixed
  top: 0
  right: 0
```

Подключение в `index.html`:

```html
<div id="fullpage">
  <div class="section">Определённый раздел 1</div>
  <div class="section">Определённый раздел 2</div>
  <div class="section">Lorem1000</div>
  <div class="section">Определённый раздел 4</div>
</div>
```
Так же в `html` пишется еще `#myMenu`.

#### Базовые настройски

[Вот тут пример](https://alvarotrigo.com/fullPage/examples/scrolling.html#secondPage/1) как сделать, чтобы на одной странице секции шли то с обычным скроллом, то с поблочным.

**Совместимость с WOW.js**

Небольшое замечание совместимости `fullPage.js` и `wow.js` (скрипт популярный, так что ньюанс стоит отметить). C дефолтными настройками анимация не работает.  Для срабатывания анимации нужно использовать параметр `scrollBar:true`. Если скроллбар не нужен, добавляем в SASS:

```sass
body, html
  overflow: hidden !important;
```