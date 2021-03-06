# HTML5 Forms

1. `placeholder`
2. input `file`
3. `checkbox` / `radio`
4. input `date`


## Placeholder

Для стилизации плэйсхолдеров используются следующие правила:

```css
::-webkit-input-placeholder  { color: blue; } /* Safari/Opera */
::-ms-input-placeholder      { color: blue; } /* Edge */
:-ms-input-placeholder       { color: blue; } /* IE */
::placeholder                { color: blue; } /* Chrome */
```

#### Sass `@mixin` для применения стилей к плэйсхолдерам

```sass
@mixin placeholder {
  &::-webkit-input-placeholder     { @content; }
  ::-ms-input-placeholder          { @content; }
  :-ms-input-placeholder           { @content; }
  &::placeholder                   { @content; }  
}
```

В зависимости от контекста миксин можно использовать как для глобального применения стилей, так и для отдельных элементов:

```sass
// глобально для каждого поля ввода
@include placeholder {
  color: blue;
}

// для определённых полей ввода
.input {
  @include placeholder {
    color: green;
  }
}
```

#### Хаки для некоторых особенностей браузеров


**Скрываем placeholder в Chrome при фокусе**
```sass
input:focus::-webkit-input-placeholder, textarea:focus::-webkit-input-placeholder
  color: transparent
```

**Убираем крестик очистки в IE**
```sass
input::-ms-clear
   display: none
```

#### Список свойств поддерживаемых плейсхолдером

`font и все связанные свойства (font-size, font-family и т.д.)`, `background и все связанные свойства (background-color, background-image и т.д.)`, `opacity`, `text-indent`, `text-overflow`, `color`, `text-transform`, `line-height`, `word-spacing`, `letter-spacing`, `text-decoration`, `vertical-align`.

### Анимации

Скорее всего, вы захотите применять анимации к плэйсхолдерам при фокусе на поле ввода. Делается всё это достаточно просто. Достаточно всего несколько раз использовать написанный ранее миксин placeholder:

```sass
.input {
  @include placeholder {
    // стили для нормального состояния
  }
  
  &:focus {
    @include placeholder {
      // стили после события focus
    }
  }
}
```

##### Изменение прозрачности

```sass
.input {
  @include placeholder {
    color: #aaa;
    opacity: 1;
    transition: opacity 300ms;
  }
  
  &:focus {
    @include placeholder {
      opacity: 0;
    }
  }
}
```

##### Сдвиг вправо и влево

Чем больше ширина поля ввода, тем больше должно быть значение свойства `text-indent`. Для стандартного поля ввода будет достаточно 500px, для более широких стоит подбирать вручную. От ширины поля ввода и значения `text-inden` зависит скорость анимации. Для сдвига влево нужно использовать отрицательные значения, например -500px.

```sass
.input {
  @include placeholder {
    text-indent: 0px;
    transition: text-indent 300ms;
  }
  
  &:focus {
    @include placeholder {
      text-indent: 500px;
    }
  }
}
```
##### Сдвиг вниз

Как и в прошлом примере анимация зависит от размеров воля ввода, но в этом случае внимание своит обратить на высоту. Для подавляющего большинства полей ввода искомое значение `line-height` будет находиться в пределах 100px. К сожалению, с помощью свойства `line-height` невозможно реализовать эффект сдвига вверх, так как свойство не принимает отрицательные значения.

```sass
.input {
  @include placeholder {
    text-indent: 0px;
    transition: text-indent 300ms;
  }
  
  &:focus {
    @include placeholder {
      text-indent: 500px;
    }
  }
}
```

##### Всё вместе
Чтобы использовать код анимаций для плэйсхолдеров было приятно и удобно, можно написать небольшую библиотеку миксинов для любого препроцессора. Библиотека выглядит следующим образом (посмотреть на SassMeister):

```sass
@mixin placeholder {
  &::-webkit-input-placeholder {@content}
  &:-moz-placeholder           {@content}
  &::-moz-placeholder          {@content}
  &:-ms-input-placeholder      {@content}  
}

@mixin pl-shift($side: left, $position: 500px, $duration: 200ms) {
  @include placeholder {
    text-indent: 0;
    transition: text-indent $duration;
  }
  &:focus {
    @include placeholder {
      text-indent: if($side == left, -$position, $position);
    }
  }
}

@mixin pl-slide-down($position: 5, $duration: 200ms) {
   @include placeholder {
    line-height: 1;
    transition: line-height $duration;
  }
  &:focus {
    @include placeholder {
      line-height: $position;
    }
  } 
}

@mixin pl-fade-out($duration: 200ms) {
  @include placeholder {
    opacity: 1;
    transition: opacity $duration;
  }
  &:focus {
    @include placeholder {
      opacity: 0;
    }
  } 
}
```

Использовать её очень просто. Достаточно подключить желаемый миксин к любому полю ввода или просто создать одно глобальное правило для всех пэйсхолдеров на странице:

```sass
// Для отдельных элементов
.pl-shift-right {
  @include pl-shift( right );  
}

.pl-fade-out {
  @include pl-fade-out; 
}

// Для всего остального
@include pl-shift( left );
```

##### Сдвиг вверх (плюс использование нескольких цветов для плейсхолдера)

**HTML:**
```html
<div class="form">
  <div class="form-group">
    <input class="form-control" type="text" required>
    <label>Имя</label>
</div>

<div class="form-group">
  <input class="form-control" type="text" required>
  <label>Телефон <span>*</span></label>
  
</div>
</div>
```
**SCSS:**
```scss
body {
  background: #ccc;
  padding: 15px;
}

.form {
  width: 300px;
  
  .form-group {
    position: relative;
  }
  
  .form-control {
    background: none;
    border: none;
    border-bottom: 1px solid #666;
    border-radius: 0;
    box-shadow: none;
    
    &:focus,
    &:valid {
      outline: 0;
      
      + label {
        top: -10px;
        font-size:12px;
      }
    }
  }
  
  label {
    position: absolute;
    left: 12px;
    top: 10px;
   color: rgba(51,51,51,.54);
    pointer-events: none;
    transition: all .2s ease;
    
    
    span {
      color: red;
      font-weight: 700;
    }
  }
}
```

#### Autoprefixer

Чтобы заставить плагин работать с пэйсхолдерами достаточно использовать псевдоэлемент `::placeholder`:

```sass
::placeholder { color: orange; }
.input::placeholder { color: blue; }
```

После парсинга стилей Autoprefixer создаст отдельный CSS файл, в котором пропишет все необходимые префиксы для всех указанных вами браузеров.

### Нюансы

##### Нюанс 1

При фокусе на инпут с плейсхолдером в хроме он не пропадает, а пропадает только после того как мы начинаем вводить что-либо в него.

Решение: победить эту проблему можно так же на чистом CSS. Добавьте в свой файл со стилями строку: 

```css
input:focus::-webkit-input-placeholder, textarea:focus::-webkit-input-placeholder {     
    color:transparent;
}
```

##### Нюанс 2

Для Webkit и EDGE:

```sass
input::-webkit-input-placeholder, textarea::-webkit-input-placeholder {
  font-style:italic;
  color:#c00;
}
```

Для Firefox:

```sass
input:-moz-placeholder, textarea::-moz-input-placeholder {
  font-style:italic;
  color:#c00;
}
```
Обратите внимание, что не смотря на то, что и в первой строке и во второй свойства у тегов прописаны одинаковые (`font-style` и `color`) - их все равно нужно писать отдельно. Иначе стили работать не будут. 

#### Использованные ресурсы

[http://jsraccoon.ru/css-placeholders](http://jsraccoon.ru/css-placeholders)


## Input file

Создаем разметку в `index.html`:

```html
<div class="file-upload">
     <label>
          <input type="file" name="file">
          <span>Выбрать файл</span>
     </label>
</div>
<input type="text" id="filename" class="filename" disabled>
```

Базовая стилизация в `main.sass`:

```sass
.file-upload
  position: relative
  overflow: hidden
  width: 20%
  height: 20px
  background: #6da047
  border-radius: 3px
  padding: 8px 4px
  color: #fff
  text-align: center
  &:hover
    background: #7aad55
  input[type="file"]
    display: none
  label
    display: block
    position: absolute
    top: 0
    left: 0
    width: 100%
    height: 100%
    cursor: pointer
  span
    line-height: 36px

.filename
     background: #fff
     border: 0
```

Код в файле `index.js` для дублирования названия файла из `input file` в текстовое поле.

```javascript
$(document).ready( function() {
    $(".file-upload input[type=file]").change(function(){
         var filename = $(this).val().replace(/.*\\/, "");
         $("#filename").val(filename);
    });
});
```

По материалам [https://webcareer.ru/stilizaciya-input-file.html](https://webcareer.ru/stilizaciya-input-file.html)

## checkbox / radio


Разметка в `index.html`:

```html
<label class="checkbox">
  <input type="checkbox">
  <span></span>
</label>
```

Базовые стили в `main.sass`:

```sass
.checkbox
  input
    display: none
    &:checked + span::before
      opacity: 1
  span
    position: relative
    display: inline-block
    width: 20px
    height: 20px
    border: 1px solid black
    border-radius: 5px
    &::before
      position: absolute
      top: 50%
      left: 50%
      transform: translate(-50%, -50%)
      content: "✔";
      opacity: 0
      transition: 0.2s
```

Вместо `✔` можно вставить все что угодно, в т.ч. вырезанное из макета изображение.

## Input date

Плагины для стилизации `input date`:

* [http://amsul.ca/pickadate.js/date/](http://amsul.ca/pickadate.js/date/) - гибкий настраиваемый плагин для стилизации поля выбора даты
* [daterangepicker](http://www.daterangepicker.com/)
* [bootstrap-datetimepicker](https://eonasdan.github.io/bootstrap-datetimepicker/#custom-icons)
* [PickMeUp](https://github.com/nazar-pc/PickMeUp)
* [jqueryui datepicker](http://api.jqueryui.com/datepicker/)

# HTML Responsive Tables

HTML:

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
	<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/css/materialize.min.css">
	<link rel="stylesheet" href="./main.css">
</head>
<body>

<table>
	<thead>
		<tr>
			<!--<th>title 0-1</th>-->
			<th>title 0-2</th>
			<th>title 0-3</th>
			<th>title 0-4</th>
			<th>title 0-5</th>
		</tr>
	</thead>
	<tbody>
	<tr>
		<!--<td>номер</td>-->
		<td>Индусов Альберт Ибн Хатабыч Викторович</td>
		<td>сообщение которое очень большое на две строки прямо писец ужас</td>
		<td>Mozilla/5.0 (Windows NT 6.1; Win64; x64) Apple WebKit/53 (KHTML, like Gecko) Chrome/66.0 Mozilla/5.0 (Windows NT 6.1; Win64; x64) Apple WebKit/53 (KHTML, like Gecko) Chrome/66.0…</td>
		<td>ссылка</td>
	</tr>
	<tr>
		<!--<td>номер</td>-->
		<td>Федоров Максим Викторович</td>
		<td>сообщение</td>
		<td>Mozilla/5.0 (Windows NT 6.1; Win64; x64) Apple WebKit/53 (KHTML, like Gecko) Chrome/66.0…</td>
		<td>ссылка</td>
	</tr>
	</tbody>
</table>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/materialize/1.0.0/js/materialize.min.js"></script>
<script src="./index.js"></script>

</body>
</html>
```

SCSS:
```scss

@media screen and (max-width: 767px) {
  thead {
    display: none;
  }
  tbody {
    display: flex;
    flex-direction: column;
  }
  tr {
    display: inline-block;
    min-height: 63px;
    position: relative;
  }
  td {
    padding: 0;
    position: relative;
    &:nth-child(1) {
      width: 50vw;
    }
    &:nth-child(2) {
      width: 50%;
      order: 30;
      float: left;
      max-height: 41px;
      overflow: hidden;
    }
    &:nth-child(3) {
      width: 50%;
      position: absolute;
      top: 0;
      right: 0;
      max-height: 41px;
      overflow: hidden;
    }
    &:nth-child(4) {
      width: 50%;
      position: absolute;
      top: 40px;
      right: 0;
    }
  }
}
```