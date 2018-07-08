# Паттерны JavaScript

### Constructor

Позволяет создавать с помощью класса объект класса.
Использует обычные прототипы в JS или их обертку “class” в ES6

```javascript
function Person (name, age, country) {
	this.name = name
	this.age = age
	this.country = country
}

const person1 = new Person('John', 20, 'RU')
console.log(person1)
```

### Decorator

Позволяет динамически добавлять объектам поведение в виде новых свойства и методов без необходимости модифицировать поведение класса по-умолчанию.

Декоратор вызывает как функция, в которую передается объект и после вызова объект будет мутирован. Не модифицирует начальное поведение класса и позволяет не создавать огромное количество подклассов.


```javascript
class Coffee {
	cost () {
		return 5
	}
}

const sugar = coffee => {
	const cost = coffee.cost()
	coffee.cost = () => cost + 1
}

const small = coffee => {
	const cost = coffee.cost()
	coffee.cost = () => cost - 1
}


const large = coffee => {
	const cost = coffee.cost()
	coffee.cost = () => cost + 2
}

const withMilk = coffee => {
	const cost = coffee.cost()
	coffee.cost = () => cost + 1
}

const largeWithMilkAndSugar = coffee => {
	large(coffee)
	withMilk(coffee)
	sugar(coffee)
	const cost = coffee.cost()
}

const coffee = new Coffee()
largeWithMilkAndSugar(coffee)
console.log(coffee.cost())
```


### Facade

Позволяет создать простой интерфейс для сложной части кода, чтобы спрятать его сложность и вместо того чтобы знать все о его сложной части, можно знать только пару методов, которые будут делать то что нам нужно. Проще работать с простым интерфейсом и переиспользовать его.

```javascript
class Bank {
	verify (amount) {
		return amount < 999
	}
}

class CreditHistory {
	check (name) {
		return true
	}
}

class Balance {
	check (name) {
		return true
	}
}

class Credit {
	constructor (name) {
		this.name = name
	}

	applyFor(amount) {
		const isApprover = new Bank().verify(amount)
		const bankResult = isApprover ? 'approver' : 'denied'
		const isPositiveBalance = new Balance().check(this.name)
		const balance = isPositiveBalance ? 'positive balance' : 'negative balance'
		const isGoodCreidHistory = new CreditHistory().check(this.name)
		const creditHistory = isGoodCreidHistory ? 'good' : 'poor'

		return `${this.name} has been ${bankResult} for the ${amount}$ credit. With a ${creditHistory} credit standing and having a ${balance} `
	}
}

const credit = new Credit('John')
const creditSmall = credit.applyFor(99)
const creditMedium = credit.applyFor(199)
const creditLarge = credit.applyFor(99999)

console.log('creditSmall', creditSmall)
console.log('creditMedium', creditMedium)
console.log('creditLarge', creditLarge)
```

### Factory

Создает фабрику, которая может создавать нам объекты. Есть ситуации, когда мы хотим скрыть снаружи реализацию создания обьекта и в этом случае нам поможет паттерн Factory. Проще работать с простым интерфейсом и переиспользовать его.

```javascript
class Fulltime {
	constructor () {
		this.rate = '$12'
	}
}

class Parttime {
	constructor () {
		this.rate = '$11'
	}
}

class Temporary {
	constructor () {
		this.rate = '$10'
	}
}

class Contractor {
	constructor () {
		this.rate = '$15'
	}
}


class Employee {
	create (type) {
		let employee
		if (type === 'fulltime') {
			employee = new Fulltime()
		} else if (type === 'parttime') {
			employee = new Parttime()
		} else if (type === 'temporary') {
			employee = new Temporary()
		} else if (type === 'contractor') {
			employee = new Contractor()
		}
		employee.type = type
		employee.say = function() {
			console.log(`${this.type}: rate ${this.rate}/hour`)
		}
		return employee
	}
}


const factory = new Employee()
const fulltime = factory.create('fulltime');
const parttime = factory.create('parttime');
const temporary = factory.create('temporary');
const contractor = factory.create('contractor');

fulltime.say()
parttime.say()
temporary.say()
contractor.say()
```

### Observer

Позволяет синхронизировать реакцию нескольких компонентов приложения на определенные события. Создается один наблюдатель и потом в нескольких местах на него подписываются другие части приложения. Позволяет делать связи один ко многим.


```javascript
class EventObserver {
	constructor () {
		this.observers = []
	}

	subscribe (fn) {
		this.observers.push(fn)
	}

	unsubscribe (fn) {
		this.observers = this.observers.filter(
			subscriber => subscriber !== fn
		)
	}

	broadcast (data) {
		this.observers.forEach(subscriber => subscriber(data))
	}
}

const blogObserver = new EventObserver()

const textField = document.querySelector('.textField');
const countField = document.querySelector('.countField');

const getWordsCount = text =>
	text ? text.trim().split(/\s+/).length : 0

blogObserver.subscribe(text => {
	countField.innerHTML = getWordsCount(text)
})

textField.addEventListener('keyup', function(e) {
	blogObserver.broadcast(textField.value)
});
```

```html
<textarea name="" id="" cols="30" rows="10" class="textField"></textarea>
<div>
	Words Count:
	<p class="countField"></p>
</div>
```

### Publish&Subscribe 

Позволяет избегать зависимостей между компонентами системы. Создается событие без привязки к какому-то конкретному классу и затем создаются подписчики, которые подписываются на интересные им события.

Можно легко разбивать приложения на независимые части. Можно легко реализовывать слабую связанность классов. Мы никогда не узнаем, подписан ли на нас кто-то.

```javascript
const EventBus = {
	channels: {},
	subscribe (channelName, listener) {
		if (!this.channels[channelName]) {
			this.channels[channelName] = []
		}
		this.channels[channelName].push(listener)
	},
	publish(channelName, data) {
		const channel = this.channels[channelName]
		if (!channel || !channel.length) {
			return
		}
		channel.forEach(listener => listener(data))
	}
}


class Mailer {
	constructor () {
		EventBus.subscribe('order/new', this.sendPurchaseEmail)
	}

	sendPurchaseEmail (params) {
		console.log(`Email was send to ${params.userEmail}`)
	}
}


class Order {
	constructor (params) {
		this.params = params
	}

	save () {
		console.log('Order save')
		EventBus.publish('order/new', {
			userEmail: this.params.userEmail
		})
	}

}

const mailer = new Mailer()

const order = new Order({userEmail: 'john@gmail.com'})
order.save()

EventBus.subscribe('foo', (message) => console.log('foo was fired'))
EventBus.publish('foo', 'Hello world')
```

### Strategy

```javascript
const logger = (strategy, level, message, ...args) => {
	return strategy(level, message, ...args)
}

const logToConsoleStrategy = (level, message) =>
	console[level](message)

const logToDOMStrategy = (level, message, node) =>
	node.innerHTML = `<div class='${level}'>${message}</div>`

logger(
	logToConsoleStrategy,
	'log',
	'log first message to console'
)

logger(
	logToDOMStrategy,
	'warn',
	'log second message to DOM',
	document.querySelector('#log')
)
```

```html
<div id="log"></div>
```