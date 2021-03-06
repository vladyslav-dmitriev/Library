# AJAX

**AJAX (Asynchronous Javascript and XML)** - способ асинхронной организации `javascript` кода, предназначенный для общения клиентского кода с серверным, для этого использует встроенный в браузер объект `XMLHttpRequest`. Является синтезом технологий `Javascript` и `XML` и позволяет изменять части web-страницы без необходимости перезагрузки всей страницы.


## HTTP протокол

**HTTP (Hypertext Transfer Protocol)** - это прикладной протокол для передачи гипертекстовых документов, таких как HTML и создан для связи между веб-браузерами и веб-серверами. Протокол следует классической клиент-серверной модели, когда клиент открывает соединение, создаёт запрос, а затем ждет ответа.

`HTTP` - это `stateless`-протокол, то есть сервер не сохраняет никаких данных (состояние) между двумя парами "запрос-ответ". Несмотря на то, что `HTTP` основан на `TCP/IP`, он так же может использовать любой транспорт, который не теряет молча сообщения (то есть он обязан знать дошло ли сообщение до адресата). 

### HTTP методы

* `GET` - запрос содержимого указанного ресурса, клиент может передавать параметры запроса в `URI`, а сервер отправляет тело ответного сообщения.

* `POST` - применяется для передачи пользовательских данных указанному ресурсу, при этому они помещяются в тело запроса

* `PUT` - применяется для загрузки содержимого запроса на указанный в запросе `URI`. Если по заданному `URI` не существует ресурс, то сервер создаёт его и возвращает статус 201 (Created). Если же был изменён ресурс, то сервер возвращает 200 (Ok) или 204 (No Content). Фундаментальное различие методов `POST` и `PUT` заключается в понимании предназначений `URI` ресурсов. Метод `POST` предполагает, что по указанному `URI` будет производиться обработка передаваемого клиентом содержимого. Используя `PUT`, клиент предполагает, что загружаемое содержимое соответствует находящемуся по данному `URI` ресурсу. 

* `DELETE` - удаляет указанный ресурс.

* `HEAD` - идентичен `GET`, за исключением того, что сервер не возвращает содержимое `HTTP`-ответа. Когда вы отправляете запрос `HEAD`, это означает, что вас интересуют только код ответа и `HTTP headers`, а не сам документ. С помощью этого метода браузер может проверить, был ли документ изменён для целей `caching` или существует ли документ вообще.

* `PATCH` - аналогично `PUT`, но применяется только к фрагменту ресурса.

* `CONNECT` - преобразует соединение запроса в прозрачный `TCP/IP`-туннель, обычно чтобы содействовать установлению защищённого `SSL`-соединения через нешифрованный прокси.


`Идемпотентность` - свойство объекта или операции при повторном применении операции к объекту давать тот же результат, что и при первом. К идемпотентным методам относят: GET, 


Спецификация `HTTP` не обязывает сервер понимать все методы, обязателнымя является только `GET`, а также не указывает серверу, что он должен делать при получении запроса с тем или иным методом. А это значит, что сервер в ответ на запрос DELETE /index.php HTTP/1.1 не обязан удалять страницу index.php на сервере, так же как на запрос GET /index.php HTTP/1.1 не обязан возвращать вам страницу index.php, он может ее удалять.

### Коды состояния

`Код состояния` является частью первой строки ответа сервера и представляет собой целое число из трёх цифр, где первая цифра указывает на класс состояния. За кодом ответа обычно следует отделённая пробелом поясняющая фраза, разъясняющая причину такого ответа, например: `201 Webpage Created`.

| Код | Класс           | Назначение  |
| :--- |:---------------| :-----|
| 1хх | Информационный  | Информирование о процессе передачи, сами сообщения от сервера содержат только стартовую строку ответа и, если требуется, несколько специфичных для ответа полей заголовка |
| 2хх | Успех           | Информирование о случаях успешного принятия и обработки запроса клиента, в зависимости от статуса, сервер может ещё передать заголовки и тело сообщения |
| 3хх | Перенаправление | Сообщает клиенту, что для успешного выполнения операции необходимо сделать другой запрос (как правило по другому URI) |
| 4хх | Ошибка клиента  | Сервер должен вернуть в теле сообщения гипертекстовое пояснение для пользователя |
| 5хх | Ошибка сервера  | Сервер должен включать в тело сообщения объяснение, которое клиент отобразит пользователю |

* `200 OK` - отправляется в ответ на успешный запрос
* `404 Not Found` - отправляется сервером, когда запрашиваемая страница или файл не найден
* 


### Заголовки

`Заголовки HTTP (HTTP Headers)` - это строки в HTTP-сообщении, содержащие разделённую двоеточием пару имя-значение, например `Content-Language: ru`.

Все заголовки разделяются на четыре основных группы, также именно в таком порядке рекомендуется посылать заголовки получателю:

* `General Headers (основные заголовки)` - могут включаться в любое сообщение клиента и сервера;
* `Request Headers (заголовки запроса)` - используются только в запросах клиента;
* `Response Headers (заголовки ответа)` - только для ответов от сервера;
* `Entity Headers (заголовки сущности)` - сопровождают каждую сущность сообщения.

Все необходимые для функционирования `HTTP` заголовки описаны в основных `RFC`. Если не хватает существующих, то можно вводить свои. Традиционно к именам таких дополнительных заголовков добавляют префикс `X-` для избежания конфликта имён с возможно существующими.

#### Основные заголовки

* 

#### Заголовки HTTP в запросах HTTP

* `Access-Control-Allow-Origin` - устанавливает список доверенных доменных имен, для которых ограничения принципа одинаковго источника не будут действовать, пример: `Access-Control-Allow-Origin: *`

* `Host` - это в основном имя `host`, включая домен и поддомен, пример: `Host: net.tutsplus.com`

* `User-Agent` - может содержать несколько частей информации, таких как: иия и версия браузера, название и версия операционной системы, язык по умолчанию. Например, можно определить, использует ли пользователь мобильный браузер и после перенаправить на мобильную версию сайтю. Пример: `User-Agent: Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US; rv:1.9.1.5) Gecko/20091102 Firefox/3.5.5 (.NET CLR 3.5.30729)`

* `Accept-Language` - отображает настройки языка по умолчанию. Например, если сайт имеет разные языковые версии, он может перенаправить пользователя на основе этих данных на версию сайта, предназначенную для его языка. Может содержать несколько языков, разделённых запятыми. Первый - это предпочтительный язык, и каждый из перечисленных языков может иметь значение `q`, которое представляет собой оценку предпочтения пользователя для языка (min. 0 max. 1). Пример: `Accept-Language: en-us,en;q=0.5`

* `Cookie` - отправляет файлы cookie, хранящиеся в вашем браузере для этого домена, пример: `Cookie: PHPSESSID=r2t5uvjq435r4q7ib3vtdjq120; foo=bar`

#### Заголовки HTTP в ответах HTTP

* `Content-Type` - указывает `mime-type` документа и затем браузер определяет, как интерпретировать содержимое на основании этого. Например, страница html может возвращать это: `Content-Type: text/html; charset=UTF-8`, где `text` - это тип, а `html` - подтип документа. Заголовок также может содержать больше информации, такой как `charset`

* `Content-Length` - когда контент будет передаваться браузеру, сервер может указать его размер (в байтах), используя этот заголовок, пример: `Content-Length: 89123`

* `Last-Modified` - указывает дату последнего изменения документа в формате `GMT`, пример: `Last-Modified: Sat, 28 Nov 2009 03:50:37 GMT`

* `Content-Encoding` - устанавливается, когда возвращаемое содержимое сжимается, пример: `Content-Encoding: gzip`

#### Заголовки сущности

* `Content-Language` - указывает один или несколько естественных языков содержимого, для носителей которых оно предназначается. Языки перечисляются через запятую, порядок значения не имеет. Если данный заголовок опущен, то предполагается, что содержимое предназначено для людей, понимающих любой язык (или же язык вообще значения не имеет).

Более **подробный список заголовков** с описанием можно посмотреть [здесь](https://zametkinapolyah.ru/servera-i-protokoly/tema-10-spravochnik-polej-http-zagolovkov-spisok-polej-http-zagolovka-zagolovki-http-soobshhenij-zaprosov-i-otvetov.html).

### Тело сообщения



## RESTful API

`REST` - это стиль архитектуры программного обеспечения для построения распределенных масштабируемых веб-сервисов, которая подразумевает использование стандартных `HTTP` методов так, чтобы придавать запросам определённый смысл. Таким образом, данные `HTTP`-запросы будут иметь различный смысловую нагрузку в `REST`:

```javascript
GET /object/list
POST /object/list
PUT /object/list
```

Итак, одна транзакция по такому API будет состоять, как минимум, из следующего:
* Метод запроса, например, GET
* Путь запроса, например, /object/list
* Тело запроса, например, форма
* Код ответа, например, 200 ОК
* Тело ответа, например, данные в формате JSON

Это и есть то, что называется `RESTful API`.

**URI (Uniform Resource Identifier)** - единообразный идентификатор ресурса.

`REST` описывает принципы взаимодействия клиента и сервера, основанные на понятиях `ресурса` и `глагола`. В случае `HTTP` ресурс определяется своим `URI`, а глагол — это `HTTP`-метод. `REST` предлагает отказаться от использования одинаковых `URI` для разных ресурсов (то есть адреса двух разных статей вроде `/index.php?article_id=10` и `/index.php?article_id=20` — это не `REST`-way) и использовать разные `HTTP`-методы для разных действий. Веб-приложение, написанное с использованием `REST` подхода будет удалять ресурс при обращении к нему с `HTTP`-методом `DELETE` (любой запрос на удаление в приложении должен использовать `HTTP`-метод `DELETE)`.

### `CRUD`

`CRUD` - сокращенное название четырех базовых операция для работы с базой данных (Create, Read, Update, Delete). В `AJAX` это соответствует методам `POST`, `GET`, `PUT` и `DELETE`.

### JSON

Можно получить ответ в виде `XML`, обычного текста или `JSON`.

**JSON (Javascript Object Notation)** - формат кодирования данных.

**JSONP (Javascript Object Notation)** - это `JSON` с дополнением, который позволяет запрашивать ресурсы `JSON` с другого домена. Это просто данные в формате `JSON`, завернутые в функцию обратного вызова.

Данные в фомате `JSONP` выглядят так:

```javascript
someFunction({"key": "value"})
```

Про `JSONP` можно почитать [тут](https://learn.javascript.ru/ajax-jsonp#%D1%80%D0%B5%D0%B5%D1%81%D1%82%D1%80-callbackregistry).

**JSON и XML**

`JSON` - похож на `XML`, так как может использоваться для описания объектов, но имеет преимущество перед `XML` в том, что он фактически является `javascript`, так как может быть свободно преобразован в обычный `javascript` объект.

По умолчанию `AJAX` запросы должны выполняться в том же домене страницы, на который возникает запрос.

**CORS** - новая и улучшенная альтернатива использованию `JSONP`.

## CORS

`Принцип одинакового источника (Same Origin Policy)` - это важная концепция безопасности для некоторых языков программирования на стороне клиента, таких как `JavaScript`. Политика разрешает сценариям, находящимся на страницах одного сайта, доступ к методам и свойствам друг друга без ограничений, но предотвращает доступ к большинству методов и свойств для страниц на разных сайтах.

`Одинаковые источники` - это источники, у которых совпадают три признака: домен, порт и протокол.

Обзор типичных проверок можно посмотреть [здесь](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%BE_%D0%BE%D0%B3%D1%80%D0%B0%D0%BD%D0%B8%D1%87%D0%B5%D0%BD%D0%B8%D1%8F_%D0%B4%D0%BE%D0%BC%D0%B5%D0%BD%D0%B0).

`CORS (Cross-origin resource sharing)` - это совместное использование ресурсов между разными источниками. Технология современных браузеров, которая позволяет предоставить веб-странице доступ к ресурсам другого домена.

До недавнего времени основным способом преодоления ограничений, наложенных в `same-origin-policy` относительно `XSS` запросов, было использование `JSONP`. Сам `JSONP` имеет неустранимое ограничение - позволяет только получение данных `GET` методом, то есть отправка данных через `POST` метод остается недоступной.


По умолчанию веб-браузеры не разрешают `AJAX`-запросы на сайты, кроме сайта, который вы посещаете. Это называется политикой единого происхождения, и это важная часть модели веб-безопасности. Совместное использование ресурсов между разными источниками `(cross-origin resource sharing)` - это механизм `HTML 5`, который дополняет политику единого происхождения для упрощения совместного использования ресурсов домена между различными веб-приложениями.

Спецификация `CORS` определяет набор заголовков, которые позволяют серверу и браузеру определять, какие запросы для междоменных ресурсов (изображения, таблицы стилей, сценарии, данные и т. д.) разрешены, а какие нет. `CORS` является техникой для ослабления правила одного источника, позволяя `JavaScript` на web странице обрабатывать `REST API` запросы от другого источника.

Взаимодействие ресурсов начинается с отправки `GET`, `POST` или `HEAD` запросу к тому или иному ресурсу на сервере. Тип содержимого `POST` запроса ограничен `application/x-www-form-urlencoded`, `multipart/form-data` или `plaintext`. Запрос включает заголовок `Origin`, который и указывает на происхождение клиентского кода.

Веб приложение проверяет происхождение запроса и на основании `Origin` либо принимает запрос, либо отвергает его. Если запрос принят, запрашиваемые сервер ответит заголовком `Access-Control-Allow-Origin`. Этот заголовок будет указывать клиенту с каким происхождением будет разрешен доступ. Принимая во внимание, что `Access-Control-Allow-Origin` соответствует `Origin` запроса, браузер разрешит запрос.

При запросе на `site.ru/resource` с `site.com/some` будут следующие заголовки:

```javascript
GET /resource
Host:site.ru
Origin: http://site.com
```

Если запрос принят, запрашиваемый сервер добавляет к ответу заголовок `Access-Control-Allow-Origin`, содержащий домен запроса `site.com`.

`Access-Control-Allow-Origin` - указывает, какие домены могут обращаться к ресурсам сайта. Например, если компания имеет домены `site.ru` и `site.com`, то ее разработчики могут использовать этот заголовок, чтобы предоставить `site.com` доступ к ресурсам `site.ru`.

`Access-Control-Allow-Methods` - определяет, какие `HTTP`-запросы (`GET`, `PUT`, `DELETE` и т. д.) могут быть использованы для доступа к ресурсам. Этот заголовок позволяет повысить безопасность, указав какие методы действительны, когда `site.com` обращается к ресурсам `site.ru`.

`Access-Control-Max-Age` - указывает время жизни предзапроса (также он называется "предполетным") доступности того или иного метода, после которого должен быть выполнен новый запрос на тот или иной метод.

Хорошая статья на эту тему [здесь](https://habr.com/company/pentestit/blog/337146/).
Еще можно почитать [тут](https://developer.mozilla.org/ru/docs/%D0%A1%D0%BB%D0%BE%D0%B2%D0%B0%D1%80%D1%8C/CORS), [тут](https://learn.javascript.ru/xhr-crossdomain), [тут](http://spring-projects.ru/understanding/cors/) и [тут](http://grishaev.me/cors).

## Организация

#### 1. Использование `callback hell`

#### 2. Использование `Promise`

#### 3. Ипользование `async/await`


## Использование

---

#### 1. Работать со встроенным функционалом напрямую

##### 1.1. Работать напрямую со встроенным объектом `XHR`

Подробная работа со встроенным объектом `XHR` описана [здесь](https://habr.com/post/14246/).

[https://xmlhttprequest.ru/](https://xmlhttprequest.ru/) - описание работы `XHR` от Ильи Кантора

##### 1.2. Использовать новый встроенный метод `fetch()` 

`Fetch` - это улучшеный XMLHttpRequest, который по умолчанию использует промисы и более простое и чистое API.

**Синтаксис метода fetch:**

```javascript
let promise = fetch(url[, options]);
```

* `url` - `URL`, на который сделать запрос,
* `options` - необязательный объект с настройками запроса. Свойства `options`:
  * `method` - метод запроса,
  * `headers` - заголовки запроса (объект),
  * `body` - тело запроса: FormData, Blob, строка и т.п.
  * `mode` - одно из: «same-origin», «no-cors», «cors», указывает, в каком режиме кросс-доменности предполагается делать запрос.
  * `credentials` - одно из: «omit», «same-origin», «include», указывает, пересылать ли куки и заголовки авторизации вместе с запросом.
  * `cache` - одно из «default», «no-store», «reload», «no-cache», «force-cache», «only-if-cached», указывает, как кешировать запрос.
  * `redirect` - можно поставить «follow» для обычного поведения при коде 30x (следовать редиректу) или «error» для интерпретации редиректа как ошибки.

При вызове `fetch` возвращает промис, который, когда получен ответ, выполняет коллбэки с объектом `Response` или с ошибкой, если запрос не удался.

```javascript
const status = function (response) {
	if (response.status !== 200) {
			return Promise.reject(new Error(response.statusText))
		}
		return Promise.resolve(response)
}

const json = (function(response) {
	return response.json()
})

fetch('http://www.mocky.io/v2/5b40cff22f00004f0079e103', {
	method: 'post',
	body: 'hello'
})
	.then(status)
	.then(json)
	.then(function (data) {
		console.log('data', data )
	})
	.catch(function (error) {
		console.log('error', error)
	})

```

Ниже еще один пример использования метода `fetch()`.

Создается класс `PostApi`, который определяет методы для работы с `AJAX`.

```javascript
class PostApi {
	static fetch() {
		return fetch('/api/film', {
			method: 'get'
		}).then(res => res.json())
	}

	static create(film) {
		return fetch('/api/film', {
			method: 'post',
			body: JSON.stringify(film),
			headers: {
				'Accept': 'application/json',
				'Content-Type': 'application/json'
			} 
		}).then(res => res.json())
	}

	static remove(id) {
		return fetch(`/api/film/${id}`, {
			method: 'delete'
		}).then(res => res.json())
	}
}
```

Вызываем метод `showAllFilms`, отображающий все посты:

```javascript
showAllFilms() {
    PostApi.fetch().then(backendFilms => {
        this.films = backendFilms.concat();
        this.film = this.films[this.selectFilmIndex];
    })
}
```

Вызываем метод, добавляющий новый пост:

```javascript
var addNewFilm = {
    id: this.films[this.films.length - 1].id + 1,
    name: this.newFilm[1],
    year: this.newFilm[2],
    format: this.newFilm[3],
    actors: this.newFilm[4]
}
PostApi.create(addNewFilm).then(film => {
    this.films.push(film);
})
```

Вызываем метод, удаляющий пост по `id`:

```javascript
PostApi.remove(id).then(() => {
    const filmIndex = this.films.findIndex(film => film.id === id)
    this.films.splice(filmIndex, 1);
})
```


#### 2. Использовать полифилы и библиотеки, являющиеся обертками над стандартным `XHR`

##### 2.1. Функция `$.ajax()` в `jQuery`

##### 2.2. Библиотека `axios`

## Postman

## Полезные ресурсы

* [https://www.mocky.io/](https://www.mocky.io/) - сервис для генерации тестовых данных, за которыми можно обращаться по `API` 

* Этапы `http` запроса - [http://bourabai.kz/dbt/web/http.html](http://bourabai.kz/dbt/web/http.html)