---

layout: yandex2

style: |
    /* собственные стили можно писать здесь!! */

    #question pre {
        margin: 0;
    }

    #async-errors pre {
        margin: 0;
    }

    #deb-and-thr img {
        width: 90%;
    }
---

# ![](themes/yandex2/images/logo-{{ site.presentation.lang }}.svg){:.logo}

## {{ site.presentation.title }}
{:.title}

### ![](themes/yandex2/images/title-logo-{{ site.presentation.lang }}.svg){{ site.presentation.service }}

{% if site.presentation.nda %}
![](themes/yandex2/images/title-nda.svg)
{:.nda}
{% endif %}

<div class="authors">

{% if site.author %}
<p>{{ site.author.name }}{% if site.author.position %}, {{ site.author.position }}{% endif %}</p>
{% endif %}

{% if site.author2 %}
<p>{{ site.author2.name }}{% if site.author2.position %}, {{ site.author2.position }}{% endif %}</p>
{% endif %}

</div>

<!-- Начало презентации -->


## Несколько дел независимо
{:.fullscreen}
![](pictures/async.jpg)
<figure markdown="1">
Несколько дел независимо
</figure>
{:style="width: 800px;"}


## В каком порядке покажутся сообщения?

```js
setTimeout(() => {
    console.log('setTimeout');
}, 0);

Promise.resolve().then(() => {
    console.log('promise1');
}).then(() => {
    console.log('promise2');
});

console.log('script end');
```
{:.next#question style="float:left;"}

- ...script end
- ...promise1
- ...promise2
- ...setTimeout
{:.image-right style="margin-top: -30px"}


## Event Loop
{:.section#event-loop}

### Цикл событий

## Цикл событий в JavaScript
{:.images}

![](pictures/eventloop.png)

### [Event loop explainer](https://github.com/atotic/event-loop#event-loop-explainer)

## Обработчики событий

...Добавятся в очередь, когда наступило событие

```js
input.addEventListener('focus', onFocus)
input.addEventListener('click', onClick)
```
{:.next}

```js
// события из кода выполняются в этом же тике
function onFocus () {
    input.click()
}
```
{:.next}

### [Порядок обработки событий](https://learn.javascript.ru/events-and-timing-depth)

## Таймеры в JavaScript

```js
const tId = setTimeout(() => {
    // поставить задачу в очередь через секунду
}, 1000)
```
{:.next}

```js
const iId = setInterval(() => {
    // добавлять задачу в очередь каждую секунду
}, 1000)
```
{:.next}

```js
// очистить таймаут или интервал
clearTimeout(tId)
clearInterval(iId)
```
{:.next}

### [Планирование: setTimeout и setInterval](https://learn.javascript.ru/settimeout-setinterval)

## setTimeout vs setInterval
{:.images}

![](pictures/timeouts.png)

### [Разница между setTimeout и setInterval](https://learn.javascript.ru/settimeout-setinterval#rekursivnyy-settimeout)

## Реальная частота срабатывания

```js
setTimeout(() => {
    // Выполни меня, так быстро, как только можешь!
}, 0)
```

- ...<b>setTimeout</b> все равно подождет минимум <b>N ms</b>
- ...В <b>неактивной вкладке</b> реальная частота срабатывания <b>гораздо реже</b>
- ...<b>[setImmediate](https://learn.javascript.ru/setimmediate)</b> - поставить задачу в очередь <b>немедленно</b>
- ...но он <b>отсутствует в стандарте</b>, зато есть в Node.js
- ...Кроме того, в Node.js есть <b>[process. nextTick](https://medium.com/devschacht/event-loop-timers-and-nexttick-18579cd122e0)</b>

### [Реальная частота срабатывания](https://learn.javascript.ru/settimeout-setinterval#settimeout-s-nulevoy-zaderzhkoy)

## Микрозадачи

```js
// then, catch, finally
Promise.resolve.then(microTask)
```
{:.next}

```js
// асинхронные ф-ции
(async () => { microTask() })()
```
{:.next}

```js
// встроенная ф-ция queueMicrotask
queueMicrotask(microTask)
```
{:.next}

```js
// ф-ции обратного вызова для Observer API
new MutationObserver(microTask)
    .observe(/* ... */)
```
{:.next}


### [Микрозадачи](https://learn.javascript.ru/microtask-queue) / [Событийный цикл: микрозадачи и макрозадачи](https://learn.javascript.ru/event-loop)


## requestAnimationFrame

...Запланировать перерисовку <b>JS анимаций</b> в <b>следующем кадре</b>

```js
requestAnimationFrame(animation)
```
{:.next}

```js
function animation () {
    // изменить что-то в DOM и сделать рекурсивный вызов
    requestAnimationFrame(animation)
}
```
{:.next}

### [Использование requestAnimationFrame](https://learn.javascript.ru/js-animation#ispolzovanie-requestanimationframe)

## requestIdleCallback

...Запланировать фоновую задачу в <b>период простоя</b> браузера

```js
requestIdleCallback(() => {
    // отправить данные аналитики
})
```
{:.next}

### [Как организовать выполнение фоновых задач в JavaScript](http://prgssr.ru/development/kak-organizovat-vypolnenie-fonovyh-zadach-v-javascript.html)



## Изолированность Event Loop

- ...У каждой вкладки свой <b>изолированный</b> и <b>независимый</b> Event Loop
- ...Также он есть у каждого <b>окна</b>, <b>фрейма</b>, <b>воркера</b>
- ...Есть возможность общения через механизм <b>postMessage</b>

### ...[Способы синхронизации вкладок браузера](https://habr.com/ru/company/rambler_and_co/blog/422545/)

## postMessage

...Механизм общения между <b>окнами</b>, <b>фреймами</b> и <b>воркерами</b>

```js
window.addEventListener('message', (e) => {
    console.log(e.data) // 📮
})

window.postMessage('📮')
```
{:.next}

...Событие происходит <b>без задержек</b>, быстрее, чем <b>setTimeout(..., 0)</b>

### [Общение между окнами](https://learn.javascript.ru/cross-window-communication)

## Блокировка Event Loop

```js
while (true) {
    // наслаждаемся отзывчивым UI
}
```
{:.next}

- ...Разбить задачу через setTimeout
- ...Использовать воркеры и postMessage
- ...Вынести вычисления на сервер


## Прямо в Callback Hell...
{:.fullscreen}
![](pictures/callback-hell.jpg)
<figure markdown="1">
Прямо в Callback Hell...
</figure>
{:style="width: 700px;"}


## Продолжение следует...
{:.section}

### Конец эпизода про Event Loop

## В предыдущих сериях

- ...Концепция Event Loop
- ...Обработчики событий, таймеры, postMessage
- ...Микрозадачи: Promise, queueMicrotask, Observer API
- ...requestAnimationFrame, requestIdleCallback


## Callback
{:.section#callback}

### Функция обратного вызова


## Callback Hell и Pyramid of Doom

```js
fetchUser(url, (user) => {
    fetchRole(user, (role) => {
        fetchToken(role, (token) => {
            fetchAccess(token, (access) => {
                fetchReport(access, (report) => {
                    fetchContent(report, (content) => {
                        // вот теперь-то поработаем с данными
                    })
                })
            })
        })
    })
})
```
{:.next}

## В каком порядке выполнится код?

```js
A(() => {
    C()

    D(() => {
        F()
    })

    E()
})

B()
```
{:.next style="float:left;"}

- ...A → B → C → D → E → F
- ...A → C → D → F → E → B
- ...A → B → C → D → F → E
- ...A → C → D → E → B → F
{:.image-right style="margin-top: 30px"}

## Не выпускайте Залго!
{:.blockquote}

### [Don't release Zalgo!](https://oren.github.io/articles/zalgo/)<br>[Designing APIs for Asynchrony](https://blog.izs.me/2013/08/designing-apis-for-asynchrony)

## Жесткая сцепленность

...Усложняет обработку <b>ошибок</b>

```js
step1((error, data) => {
    if (error) {
        // отменить этап №1
    }
```
{:.next}
```js
    step2((error, data) => {
        if (error) {
            // отменить этап №2, а затем №1
        }
    })
}))
```
{:.next}

## Проблема доверия

```js
thirdPartyCode(callback)
```
{:.next}

...<b>Инверсия управления</b> — передать контроль над кодом кому-то еще

- ...Cлишком <b>рано</b> или слишком <b>поздно</b>
- ...Cлишком <b>мало</b> или слишком <b>много</b>
- ...Передать <b>неверные аргументы</b>
- ...<b>Поглотить</b> ошибки и исключения
- ...Вообще <b>пропустить вызов</b> callback


## Впереди появилась надежда
{:.fullscreen}
![](pictures/promise.jpg)
<figure markdown="1">
Впереди появилась надежда
</figure>
{:style="width: 850px;"}


## Продолжение следует...
{:.section}

### Конец эпизода про Callback

## В предыдущих сериях

<b>Недостатки callback</b>

- ...Непонятно sync или async
- ...Жесткая сцепленность
- ...Проблема доверия

## Promise
{:.section#promise}

### Обещание

## Обещание — это талон
{:.fullscreen}
![](pictures/talon.jpg)
<figure markdown="1">
Обещание — это талон
</figure>
{:style="width: 700px;"}

## Работа с обещаниями

- ...Создать обещание при помощи <b>new</b>
- ...Передать в конструктор функцию: <b>(resolve, reject) => { ... }</b>
- ...Внутри функции вызвать <b>resolve</b> и/или <b>reject</b>
- ...<b>resolve</b> и <b>reject</b> принимают <b>только один аргумент</b>
- ...Обработать выполнение и/или отказ через метод <b>then</b> или <b>catch</b>
- ...<b>then</b> обрабатывает выполнение и/или отказ, <b>catch</b> только отказ

## Обработка выполнения

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => { resolve('👌') }, 1000)
})
```
{:.next}

```js
promise.then((value) => {
    console.log(value) // 👌
})
```
{:.next}


## Обработка отказа через then

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => { reject('💥') }, 1000)
})
```
{:.next}

```js
promise.then(
    (value) => {
        // ... проигнорировано
    },
    (error) => {
        console.log(value) // 💥
    }
)
```
{:.next}

## Обработка отказа через catch

```js
const promise = new Promise((resolve, reject) => {
    setTimeout(() => { reject('💥') }, 1000)
})
```

```js
promise.catch((error) => {
    console.log(value) // 💥
})
```

## Либо выполнение, либо отказ

Promise устанавливается <b>либо на выполнение, либо на отказ</b>

```js
const promise = new Promise((resolve, reject) => {
    resolve('👌')
    reject('💥') // никак не повлияет на состояние Promise
})
```
{:.next}


## Один раз и навсегда

Promise устанавливается <b>единственный раз</b>

```js
const promise = new Promise((resolve, reject) => {
    resolve('👌')
    reject('💥') // никак не повлияет на состояние Promise
})
```

```js
promise.then((value) => {
    console.log(value) // 👌
})
promise.then((value) => {
    console.log(value) // 👌
})
```
{:.next}

## Цепочки Promise

...Обработчики неявно <b>возвращают новый Promise</b>

```js
promise
    .then(() => {
        // return new Promise((resolve) => resolve())
    })
    .then(() => {
        // мы ничего не вернули из then, но код продолжает работать
    })
```
{:.next}


## Возвращение значения через return

Любое значение, <b>кроме Promise</b> будет <b>завернуто в Promise</b>

```js
promise
    .then(() => {
        return 'Заверните меня, пожалуйста!'
    })
    .then((value) => {
        // у String нет метода then, но код продолжает работать
        console.log(value) // "Заверните меня, пожалуйста!"
    })
```
{:.next}

## Возвращение Promise через return

Promise вернется <b>без изменений</b>

```js
promise
    .then(() => {
        return new Promise((resolve) => resolve('🔥'))
    })
    .then((value) => {
        console.log(value) // 🔥
    })
```
{:.next}

## Каждый then цепляется к предыдущему

```js
promise
    .then((url) => {
        return fetchHost(url).then((address) => {
            return fetchConnection(address).then((connection) => {
                return fetchData(connection)
            })
        })
    })
    .then((data) => {
        // данные успешно загружены! 🎉
    })
```
{:.next}

## Порядок исполнения всегда линейный

```js
promise
    .then((url) => {
        return fetchHost(url)
    })
    .then((address) => {
        return fetchConnection(address)
    })
    .then((connection) => {
        return fetchData(connection)
    })
    .then((data) => {
        // данные успешно загружены! 🎉
    })
```


## Проброс отказа

```js
promise
    .then(() => {
        return new Promise((_, reject) => reject('💥'))
    })
    .then(() => {
        // все обработчики на выполнение будут пропущены
    })
    .catch((e) => {
        console.log(e) // 💥
    })
    .then(() => {
        // продолжаем цепочку в штатном режиме
    })
```
{:.next}

## Можно и через then

```js
promise
    .then(() => {
        return new Promise((_, reject) => reject('💥'))
    }, (e) => {
        // проигнорирует отказ, потому что в этом же then
    })
    .then(() => {
        // обработчик на выполнение снова будет пропущен
    }, (e) => {
        console.log(e) // 💥
    })
```

## Обработчик отказа поймает любые ошибки

```js
promise
    .then(() => {
        undefined.toString()
    })
    .catch((e) => {
        console.log(e) // TypeError: Cannot read property 'toString' of undefined
    })
```
{:.next}

## Спрятанный try/catch

```js
promise
    .then(() => {
        try {
            undefined.toString()
        } catch (e) {
            return new Promise((_, reject) => reject(e));
        }
    })
    .catch((e) => {
        console.log(e) // TypeError: Cannot read property 'toString' of undefined
    })
```

## Поставь catch в конце!
{:.blockquote}

## Кто поймает ошибку в последнем catch?

...Сборщик мусора! 🗑️

```js
window.addEventListener('unhandledrejection', (event) => {
    console.log('Необработанная ошибка Promise. Позор вам!')
    console.log(event) // PromiseRejectionEvent
    console.log(event.reason) // 💥
})

new Promise((_, reject) => reject('💥'))
```
{:.next}

### [В некоторых библиотеках есть метод .done](http://bluebirdjs.com/docs/api/done.html#done)
{:.next}

## finally

- ...Выполнит какую-то работу, <b>независимо</b> от <b>успеха</b> или <b>неудачи</b>
- ...<b>Ничего не получает</b>, но <b>пробрасывает</b> результат через себя

```js
Promise.resolve('👌').finally(() => {
    // какая-то работа
}).then((value) => {
    console.log(value) // 👌
})
```
{:.next style="float:left;"}

```js
Promise.reject('💥').finally(() => {
    // какая-то работа
}).catch((error) => {
    console.log(error) // 💥
})
```
{:.next.image-right}

### [Promise.prototype.finally](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Promise/finally)


## Статические методы Promise
{:.section#static-methods}

### resolve, reject, all, race, allSelected, any

## Promise.resolve и Promise.reject

Обернут значение в <b>установленный Promise</b>

```js
const resolved = Promise.resolve('👌')
resolved.then((value) => {
    console.log(value) // 👌
})
```
{:.next}

```js
const rejected = Promise.reject('💥')
rejected.catch((value) => {
    console.log(value) // 💥
})
```
{:.next}

## Как Promise.resolve обработает Promise?

...<b>Promise.resolve</b> вернет его <b>без изменений</b>

```js
const promise = Promise.resolve('🍏');
```
{:.next}

```js
const resolved = Promise.resolve(promise)
```
{:.next}

```js
console.log(promise === resolved) // true
```
{:.next}

```js
resolved.then((value) => {
    console.log(value) // 🍏
})
```
{:.next}

## Как Promise.reject обработает Promise?

...<b>Promise.reject</b> как причину отказа еще раз его <b>обернет в Promise</b>

```js
const promise = Promise.resolve('🍏');
```
{:.next}

```js
const rejected = Promise.reject(promise)
```
{:.next}

```js
console.log(promise === rejected) // false
```
{:.next}

```js
rejected.catch((value) => {
    console.log(value) // Promise {<resolved>: 🍏}
})
```
{:.next}

## resolve и reject работают точно так же

```js
const promise = Promise.resolve('🍏');
```
{:.next}

```js
new Promise((resolve) => resolve(promise)).then((value) => {
    console.log(value) // 🍏
})
```
{:.next}

```js
new Promise((_, reject) => reject(promise)).catch((value) => {
    console.log(value) // Promise {<resolved>: 🍏}
})
```
{:.next}


## Thenable объект

...Объект, у которого <b>есть метод then</b>

```js
const thenable = {
    then () {
        return '🐵'
    }
}
```
{:.next}

...Скорее всего, это <b>полифилл для Promise</b> до ES6

## Всемогущая совместимость

- ...<b>Promise.resolve</b> и <b>resolve</b> распакуют значение <b>thenable объекта</b>
- ...Обернут распакованное значение в полноценный <b>ES6 Promise</b>

```js
const awesomeES6Promise = Promise.resolve(thenable)
```
{:.next}

```js
awesomeES6Promise.then((value) => {
    console.log(value) // 🐵
})
```
{:.next}

## Promise.all
- ...Принимает <b>массив</b> со <b>значениями</b> и/или <b>Promise</b>
- ...Возвращает <b>массив значений</b> или <b>первый отказ</b>

```js
Promise.all([
    Promise.resolve('🍍'),
    '🍉',
]).then((value) => {
    console.log(value) // [🍍, 🍉]
})
```
{:.next}

## Пример Promise.all

```js
Promise.all([
    fetchChats(),
    fetchPhotos(),
    fetchContacts(),
]).then(([chats, photos, contacts]) => {
    // получили параллельно все данные 👌
}).catch((error) => {
    // обработали первый отказ 💥
})
```

## Promise.race
- ...Принимает <b>массив</b> со <b>значениями</b> и/или <b>Promise</b>
- ...Возвращает <b>первое значение</b> или <b>первый отказ</b>

```js
Promise.race([
    Promise.resolve('🍍'),
    '🍉',
]).then((value) => {
    console.log(value) // 🍉
})
```
{:.next style="float:left;"}

```js
Promise.race([
    Promise.reject('💥'),
    '🍉',
]).catch((value) => {
    console.log(value) // 💥
})
```
{:.next.image-right}

## Пример Promise.race

```js
// запрос к самому быстрому дата-центру
Promise.race([
    fetchReplicaA(),
    fetchReplicaB(),
    fetchReplicaC(),
]).then((first) => {
    // получили ответ от самого быстрого дата-центра
}).catch((error) => {
    // обработали первый отказ 💥
})
```

## Крайние случаи

```js
// вернет пустой массивы
Promise.all([]).then((value) => {
    console.log(value) // []
})
```
{:.next}

```js
// зависнет навсегда в pending
Promise.race([]).then(
    () => { /* resolve не выполнится никогда */ },
    () => { /* reject тоже */ },
)
```
{:.next}

## Promise.allSettled

...Дождется <b>всех</b> и вернет <b>массив специальных объектов</b>

```js
Promise.allSettled([
    Promise.resolve('👌'),
    Promise.reject('💥'),
]).then(([resolved, rejected]) => {
    console.log(resolved) // { status: "fulfilled", value: 👌 }
    console.log(rejected) // { status: "rejected", reason: 💥 }
})
```
{:.next}

### [Promise.allSettled](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)

## Promise.any

...Вернет <b>первое значение</b> или <b>массив с причинами отказа</b>

```js
Promise.any([
    fetchPrimaryDB(),
    fetchReplicaDB(),
]).then((first) => {
    // самый быстрый ответ
}).catch(([primaryError, replicaError]) => {
    // список ошибок
})
```
{:.next}

### [Promise.any](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Promise/any)

## Пора выбраться из Callback Hell

```js
fetchToken(url, (token) => {
    fetchUser(token, (user) => {
        fetchRole(user, (role) => {
            fetchAccess(role, (access) => {
                fetchReport(access, (report) => {
                    fetchContent(report, (content) => {
                        // как же выбраться из Роковой Пирамиды?
                    })
                })
            })
        })
    })
})
```
{:.next}

## Всемогущий Promise
```js
fetchToken(url)
    .then(fetchUser)
    .then(fetchRole)
    .then(fetchAccess)
    .then(fetchReport)
    .then(fetchContent)
    .then((content) => {
        // Ура! У нас получилось!
    })
    .catch(errorHandler)
```
{:.next}

## Promise решает недостатки callback

- ...Асинхронный (прощай, Залго)
- ...Линейный (прощай, жесткая сцепленность)
- ...Одноразовый (проблема доверия решена)

## Как быть, когда Promise завис?

```js
Promise.race([
    fetchLongRequest(),
    new Promise((_, reject) => setTimeout(reject, 3000)),
]).then((data) => {
    // получили данные
}).catch((error) => {
    // или отказ по таймеру
})
```
{:.next}

## Генераторы
{:.fullscreen}
![](pictures/generator.jpg)
<figure markdown="1">
Генераторы
</figure>
{:style="width: 500px;"}

## Продолжение следует...
{:.section}

### Конец эпизода про Promise

## В предыдущих сериях

- ...Основные возможности Promise: then, catch, finally
- ...Неявности: возврат Promise, try/catch
- ...Цепочки и проброс ошибок
- ...Статические методы: resolve, reject, all, race, allSelected, any
- ...Выбрались из Callback Hell

## Итераторы<br>Генераторы
{:.section#iterator-generator}

### Как они могут улучшить Promise?

## Философия

- ...Итератор — <b>интерфейс</b> с методом <b>next</b> и признаком <b>done</b>
- ...Генератор — <b>подход</b>, когда вычисляется только следующий элемент

## Итератор в JavaScript

...Это штука с методом <b>next</b>, которая возвращает `{ value, done }`

```js
const iterator = ['🍎', '🍏'][Symbol.iterator]()
```
{:.next}

```js
iterator.next() // {value: "🍎", done: false}
```
{:.next}

```js
iterator.next() // {value: "🍏", done: false}
```
{:.next}
```js
iterator.next() // {value: undefined, done: true}
```
{:.next}
```js
iterator.next() // {value: undefined, done: true}
```
{:.next}

## Генератор в JavaScript

...Штука, которая <b>возвращает итератор</b>

```js
function* generator () {
    yield '🍎'
    yield '🍏'
}
```
{:.next}

```js
const iterator = generator()
```
{:.next}

```js
iterator.next() // {value: "🍎", done: false}
iterator.next() // {value: "🍏", done: false}
iterator.next() // {value: undefined, done: true}
```
{:.next}

## Общение итератора с внешним миром

...Через <b>аргумент</b> в методе <b>next</b>

```js
function* generator () {
    const value = yield '🍎'
    yield 42 + value
}
```
{:.next}

```js
const iterator = generator()
```
{:.next}

```js
iterator.next()   // { value: "🍎", done: false }
iterator.next(17) // { value: 59, done: false }
iterator.next()   // { value: undefined, done: true }
```
{:.next}

## Общение итератора с внешним миром

...Через <b>аргумент</b> в методе <b>throw</b>

```js
// где-то в генераторе
try {
    choice = yield "It's time to choose" // "🍎"
} catch (e) {
    choice = e
}
yield choice
```
{:.next}

```js
// текущая реальность
iterator.next() // { value: "It's time ...", done: false }
iterator.next('🍎') // { value: "🍎", done: false }
```
{:.next}

## Общение итератора с внешним миром

Через <b>аргумент</b> в методе <b>throw</b>

```js
// где-то в генераторе
try {
    choice = yield "It's time to choose"
} catch (e) {
    choice = e // "🍏"
}
yield choice
```

```js
// альтернативная реальность
iterator.next() // { value: "It's time ...", done: false }
iterator.throw('🍏') // { value: "🍏", done: false }
```

## Как связать итераторы и обещания?
{:.fullscreen}
![](pictures/how.jpg)
<figure markdown="1">
Как связать итераторы и обещания?
</figure>
{:style="width: 1000px;"}

## Итератор — это корутина (сопрограмма)

...**Cooperative concurrently executing routines**

...Корутина — это функция, которая:
- ...приостанавливает работу
- ...запоминает текущее состояние
- ...имеет несколько точек входа и выхода

### [Demystifying Async Programming](https://yunchi.dev/posts/demystifying-async/)

## Магическая функция async

```js
function async(generator) {
    const iterator = generator()

    function handle({ done, value }) {
        return done ? value : Promise.resolve(value)
            .then((x) => handle(iterator.next(x)))
            .catch((e) => handle(iterator.throw(e)))
    }

    return handle(iterator.next());
}
```
{:.next}

### [Generators by Forbes Lindesay](https://www.promisejs.org/generators/)

## Ничего не напоминает?

```js
async(function* () {
    const response = yield fetch('something.com')
    const json = yield response.json()

    // обработать json
})
```

## async/await
{:.section#async-await}

### Синтаксис для работы с Promise

## Знакомьтесь, Async

...Асинхронная функция <b>заворачивает</b> свой результат в <b>Promise</b>

```js
async function asyncFunction () {
    return '🔥'
}

asyncFunction().then((value) => {
    console.log(value) // 🔥
})
```
{:.next style="float:left;"}

```js
function asyncFunction () {
    return Promise.resovle('🔥')
}
```
{:.next.image-right}

### [async/await](https://learn.javascript.ru/async-await)

## Знакомьтесь, Await

...Await <b>получает результат</b> из <b>Promise</b>
<br>и вызывается <b>только внутри</b> асинхронной функции

```js
(async () => {
    const value = await Promise.resolve('🔥')
    console.log(value) // 🔥
})()
```
{:.next}

## Асинхронность токсична
{:.fullscreen}
![](pictures/toxic.jpg)
<figure markdown="1">
Асинхронность токсична
</figure>
{:style="width: 750px;"}

## Привычные формы
{:.fullscreen}
![](pictures/forms.jpg)
<figure markdown="1">
Привычные формы
</figure>
{:style="width: 600px;"}

## Top level await

...Работает только для <b>ES модулей</b> или в <b>DevTools</b>

```js
const connection = await dbConnector();
```
{:.next}

```js
const jQuery = await import('http://cdn.com/jquery');
```
{:.next}

### [Top-level await in modules](https://exploringjs.com/impatient-js/ch_modules.html#top-level-await)

## Как работает top level await?

...Он делает модуль <b>асинхронным</b>

```js
// module.mjs
const value =
    await Promise.resolve('🔥')

export { value }
```
{:.next style="float:left;"}

```js
// main.mjs
import {
    value
} from './module.mjs'

console.log(value) // 🔥
```
{:.next.image-right}

## Как работает top level await?

```js
// module.mjs
export let value
export const promise = (async () => {
    value = await Promise.resolve('🔥')
})()

export { value, promise }
```
{:.next style="float:left;"}

```js
// main.mjs
import {
    value,
    promise
} from './module.mjs'

(async () => {
    await promise
    console.log(value) // 🔥
})()
```
{:.next.image-right}


## await — это просто оператор...

```js
console.log(await 42 + await Promise.resolve(42)) // 84
```
{:.next}

...однако скрытую опасность в себе он таит 😮

## Не все await одинаково полезны

```js
const articles = await fetchArticles()
const pictures = await fetchPictures()

// ... какие-то действия с articles и pictures
```
{:.next}

...Запросы <b>независимы</b>, но выполняются <b>последовательно</b>

### [Как избежать async/await ада](https://medium.com/@stasonmars/как-избежать-async-await-ада-dde39291bdb8)

## Делаем запросы параллельными

```js
// решение №1
const articlesPromise = fetchArticles()
const picturesPromise = fetchPictures()

const articles = await articlesPromise;
const pictures = await picturesPromise;
```
{:.next}

```js
// решение №2
const [articles, pictures] = await Promise.all([
    fetchArticles(),
    fetchPictures(),
])
```
{:.next}

## Обработка ошибок
{:#async-errors}

```js
// Вариант №1: try/catch
async function asyncFunction () {
    try {
        await Promise.reject('💥')
    } catch (e) {
        // перехватить ошибку
    }
}

asyncFunction().then((value) => {
    // мы в безопасности
})
```
{:.next style="float:left;"}

```js
// Вариант №2: catch
async function asyncFunction () {
    await Promise.reject('💥')
}

asyncFunction()
    .then((value) => {
        // вдруг ошибка?
    })
    .catch((error) => {
        // тогда её поймает catch
    })
```
{:.next.image-right}

## async функции и массивы
{:.section}

### forEach, map, filter, reduce

## forEach + async/await

```js
const wtf = ['🤯']

wtf.forEach(async (emoji) => {
    console.log(emoji)
})

console.log('It is asynchronous, baby')
```
{:.next}

```js
// 'It is asynchronous, baby'
// 🤯
```
{:.next}

## map + async/await

```js
const urls = ['/data', '/meta']

const result = urls.map(async (url) => {
    const response = await fetch(url)
    return await response.json()
})
```
{:.next}

```js
console.log(result) // [Promise, Promise]
```
{:.next}

## map + async/await, попытка №2

```js
const urls = ['/data', '/meta']

const promises = urls.map(async (url) => {
    return (await fetch(url)).json()
})
```

```js
const result = await Promise.all(promises)
console.log(result) // [{...}, {...}]
```
{:.next}

## filter + async/await

```js
const values = [null, 0, 42]

const wtf = values.filter(async (value) => {
    return Boolean(value)
})
```
{:.next}

```js
console.log(wtf) // [null, 0, 42]
```
{:.next}

## reduce + async/await

```js
const numbers = [4, 8, 15, 16, 23, 42]

const result = numbers.reduce(async (sum, number) => {
    return sum + number
}, 0)
```
{:.next}

```js
console.log(result) // Promise
```
{:.next}

```js
console.log(await result) // [object Promise]42
```
{:.next}

## reduce + async/await, попытка №2

```js
const numbers = [4, 8, 15, 16, 23, 42]

const result = numbers.reduce(async (sum, number) => {
    return (await sum) + number
}, 0)
```
{:.next}

```js
console.log(await result) // 108
```
{:.next}

## Асинхронный генератор

```js
async function* generator () {
    yield '🍎'
    yield '🍏'
}
```
{:.next}

```js
const iterator = generator()
```
{:.next}

```js
console.log(await iterator.next()) // "🍎"

for await (const value of iterator) {
    console.log(value) // "🍏"
}
```
{:.next}

## Callback, Promise или async/await?

- ...Обработчики событий работают только с функциями обратного вызова
- ...Во всех остальных случаях — async/await и обещания

## Race Condition
{:.fullscreen}
![](pictures/humster-race.jpg)
<figure markdown="1">
Race Condition
</figure>
{:style="width: 500px;"}

## Продолжение следует...
{:.section}

### Конец эпизода про итераторы, генераторы и async/await

## В предыдущих сериях

- ...Итераторы и Генераторы
- ...Корутины
- ...async/await
- ...Не все await одинаково полезны

## Race Condition
{:.section#race-condition}

### Состояние гонки

## Пример Race Condition
{:.images .three}

![](pictures/rc-1.png)
{:.next}

![](pictures/rc-2.png)
{:.next}

![](pictures/rc-3.png)
{:.next}

## Объяснение Race Condition
{:.images}

![](pictures/diagram.png)

## Решение Race Condition

- ...При отправке запроса <b>прерываем</b> все предыдущие
- ...При обработке ответа <b>игнорируем</b> все предыдущие

```js
fromEvent(inputElem, 'keyup') // RxJS в действии
    .pipe(
        pluck('target', 'value'),
        switchMap(value => ajax.getJSON(`${URL}?search=${value}`))
    )
    .subscribe((response) => {
        // обновить DOM
    })
```
{:.next}

### ...[Reactive Extensions Library for JavaScript](https://rxjs.dev/)

## Итоги

- ...Event Loop
- ...Callback Hell
- ...Promise
- ...Итераторы, генераторы, корутины
- ...async/await
- ...Race Condition

## Рекомендации

<b>Книга</b>
- [Симпсон — {Вы не знаете JS} Асинхронная обработка и оптимизация](https://www.piter.com/product_by_id/141929485)

<b>Видео</b>
- [Джейк Арчибальд. В цикле - JSConf.Asia](https://youtu.be/cCOL7MC4Pl0)

<b>Статьи</b>
- [Задачи, микрозадачи, очереди и планы](https://habr.com/ru/post/264993/)
- [Визуализация промисов и async/await](https://habr.com/ru/post/501702/)
- [Оптимизация производительности фронтенда. Часть 2](https://habr.com/ru/post/517594/)


## Финальные титры
{:.fullscreen}
![](pictures/sunset.jpg)
<figure markdown="1">
Финальные титры
</figure>
{:style="width: 600px;"}

## The End 👏
{:.section}

### Спасибо за внимание!


<!-- ## Контакты
{:.contacts}

{% if site.author %}

<figure markdown="1">

### {{ site.author.name }}

{% if site.author.position %}
{{ site.author.position }}
{% endif %}

</figure>

{% endif %} -->

<!-- {% if site.author2 %}

<figure markdown="1">

### {{ site.author2.name }}

{% if site.author2.position %}
{{ site.author2.position }}
{% endif %}

</figure>

{% endif %} -->

<!-- разделитель контактов -->
<!-- ------- -->

<!-- left -->
<!-- - {:.skype}author -->
<!-- - {:.mail}author@yandex-team.ru -->
<!-- - {:.github}author -->

<!-- right -->
<!-- - {:.twitter}@author -->
<!-- - {:.facebook}author -->
<!-- - {:.telegram}@author -->

<!--

- {:.mail}author@yandex-team.ru
- {:.phone}+7-999-888-7766
- {:.github}author
- {:.bitbucket}author
- {:.twitter}@author
- {:.telegram}author
- {:.skype}author
- {:.instagram}author
- {:.facebook}author
- {:.vk}@author
- {:.ok}@author

-->
