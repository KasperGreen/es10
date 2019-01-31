
Стандартизация JS перешла на годичный цикл обновлений, а начало года — отличное время для того чтобы узнать, что нас ждёт в юбилейной — уже десятой редакции EcmaScript!

*ES*9 - [актуальная версия спецификации](https://www.ecma-international.org/publications/standards/Ecma-262.htm)

*ES*10 — всё ещё [черновик](https://tc39.github.io/ecma262/)


На сегодняшний день в [*Stage* **4**](https://habr.com/ru/post/437806/#-stage-4) — всего несколько предложений.

А в [*Stage* **3**](https://habr.com/ru/post/437806/#-stage-3) — целая дюжина!

Из них, на мой взгляд, самые интересные — [приватные поля классов](https://habr.com/ru/post/437806/#privatnyestaticheskiepublichnye-metodysvoystvaatributy-u-klassov), [шебанг грамматика для скриптов](https://habr.com/ru/post/437806/#shebang-grammatika), [числа произвольной точности](https://habr.com/ru/post/437806/#bolshie-chisla-s-bigint), [доступ к глобальному контексту](https://habr.com/ru/post/437806/#globalthis--novyy-sposob-dostupa-k-globalnomu-kontekstu) и [динамические импорты](https://habr.com/ru/post/437806/#dinamicheskiy-importdynamic).


 
![КДПВ: Жёлтый магнит с надписью «JS ES10» на экране монитора —  от kasper.green & elfafeya.art](https://habrastorage.org/webt/j-/g9/na/j-g9nan1y54iew_qzum-eodzfgw.png)
        <sup><cite>Автор фото: kasper.green; Жёлтый магнит: elfafeya.art & kasper.green</cite></sup>

<cut />

## Содержание

### [Пять стадий](#pyat-stadiy)

### [Stage 4 — Final](#-stage-4)

 •      [**```catch```** — аргумент стал необязательным](#neobyazatelnyy-argument-u-catch);

 •      [**```Symbol().description```** — акцессор к описанию символа](#dostup-k-opisaniyu-simvolnoy-ssylki);

 •      [**```'строки EcmaScript'```** — улучшенная совместимость с **JSON** форматом](#stroki-ecmascript-sovmestimye-s-json);

 •      [**```.toString()```** — прототипный метод обновлён](#dorabotka-prototipnogo-metoda-tostring).

-------------

### [Stage 3 — Pre-release](#-stage-3)

 •      [**```#```** —  приватное всё у классов, через октоторп](#privatnyestaticheskiepublichnye-metodysvoystvaatributy-u-klassov);

 •      [**```#!/usr/bin/env node```**  — шебанг грамматика для скриптов](#shebang-grammatika);

 •      [**```BigInt()```** — новый примитив, для чисел произвольной точности](#bolshie-chisla-s-bigint);

 •      [**```globalThis```** — новый способ доступа к глобальному контексту](#globalthis--novyy-sposob-dostupa-k-globalnomu-kontekstu);

 •      [**```import(dynamic)```** — динамический импорт](#dinamicheskiy-importdynamic);

 •      [**```import.meta```** — мета-информация о загружаемом модуле](#importmeta--meta-informaciya-o-zagruzhaemom-module);

 •      [**```Object.fromEntries()```** — создание объекта из массива пар — ключ\значение](#sozdanie-obekta-metodom-objectfromentries);

 •      [**```JSON.stringify()```** — фикс метода](#fiks-metoda-jsonstringify);

 •      [**```RegExp```** — устаревшие возможности](#ustarevshie-vozmozhnosti-regexp);

 •      [**```.trimStart()```** и **```.trimEnd()```** — прототипные методы строк](#prototipnye-metody-strok-trimstart-i-trimend);

 •      [**```.matchAll()```** — **```.match()```** с глобальным флагом](#matchall--novyy-prototipnyy-metod-strok);

 •      [**```.flat()```** и **```.flatMap()```** — прототипные методы массивов](#odnomernye-massivy-s-flat-i-flatmap).


### [Итоги](#itogi)



--------------------



## Пять стадий

    <sub>*Stage*</sub> **0**   ↓  &thinsp;**Strawman**  <sup>**Наметка**</sup>          &thinsp;Идея, которую можно реализовать через **Babel**-плагин.;

    <sub>*Stage*</sub> **1**   ↓  &thinsp;**Proposal**  <sup>**Предложение**</sup>    &thinsp;Проверка жизнеспособности идеи.;

    <sub>*Stage*</sub> **2**   ↓  &thinsp;**Draft**  <sup>**Черновик**</sup>                 &thinsp;Начало разработки спецификации.;

    <sub>*Stage*</sub> **3**   ↓  &thinsp;**Candidate**  <sup>**Кандидат**</sup>         Предварительная версия спецификации.;

    <sub>*Stage*</sub> **4**  ֍  **Finished**  <sup>**Завершён**</sup>          &thinsp;Финальная версия спецификации на этот год.

 ----------------------------

 Мы рассмотрим только *Stage* **4** — де-факто, вошедший в стандарт.

 И *Stage* **3** — который вот-вот станет его частью.

 ----------------------------


 


## ֍ Stage 4

Эти изменения уже вошли в стандарт.


 
 


### Необязательный аргумент у `catch`

<https://github.com/tc39/proposal-optional-catch-binding>

До *ES*10 блок ```catch``` требовал обязательного аргумента для сбора информации об ошибке, даже если она не используется:
```javascript
function isValidJSON(text) {
  try {
    JSON.parse(text);
    return true;
  } catch(unusedVariable) { // переменная не используется
    return false;
  }
}
```


![](https://habrastorage.org/webt/ez/l2/2d/ezl22di9ciu-4g60nlqyuqne7lk.png)
<sup>*Edge* пока не обновлён до *ES*10, и ожидаемо валится с ошибкой</sup>


Начиная с редакции *ES*10, круглые скобки можно опустить и ```catch``` станет как две капли воды похож на ```try```.

![](https://habrastorage.org/webt/yi/ia/qg/yiiaqgiclyxz_i7bf3gq14dj-8m.png)
<sup>Мой Chrome уже обновился до *ES*10, а местами и до *Stage* **3**. Дальше скриншоты будут из *Chrome*</sup>

<spoiler title="исходный код">
```javascript
function isValidJSON(text) {
  try {
    JSON.parse(text);
    return true;
  } catch { // без аргумента
    return false;
  }
}
```
</spoiler>


 
 


## Доступ к описанию символьной ссылки

<https://tc39.github.io/proposal-Symbol-description/>

Описание символьной ссылки можно косвенно получить методом toString():
```javascript
const symbol_link = Symbol("Symbol description")
String(symbol_link) // "Symbol(Symbol description)"
```

Начиная с *ES*10 у символов появилось свойство description, доступное только для чтения. Оно позволяет без всяких танцев с бубном получить описание символа:
```javascript
symbol_link.description
// "Symbol description"
```


В случае если описание не задано, вернётся — `undefined`
```javascript
const without_description_symbol_link = Symbol()
without_description_symbol_link.description
// undefined

const empty_description_symbol_link = Symbol('')
empty_description_symbol_link.description
// ""
```
 
 


### Строки EcmaScript совместимые с JSON

<https://github.com/tc39/proposal-json-superset>

EcmaScript до десятой редакции утверждает, что *JSON* является подмножеством `JSON.parse`, но это неверно.

*JSON* строки могут содержать не экранированные символы разделителей линий <sup>`U+2028` *LINE SEPARATOR*</sup> и абзацев <sup>**`U+2029`** *PARAGRAPH SEPARATOR*</sup>.

Строки *ECMAScript* до десятой версии  — нет.

Если в *Edge* вызвать `eval()` со строкой `"\u2029"`,
он ведёт себя так, словно мы сделали перенос строки — прямо посреди кода.

![](https://habrastorage.org/webt/vc/yg/-v/vcyg-vy7larz8y4kswiqinjhfh4.png)

 

C *ES*10 строками — всё в порядке:

![](https://habrastorage.org/webt/tc/he/tm/tchetmze4axxtp5wghm7pmz-0xc.png)


 
 


### Доработка прототипного метода `.toString()`

<http://tc39.github.io/Function-prototype-toString-revision/>

<spoiler title="Цели изменений">
* убрать обратно несовместимое требование:

> Если реализация не может создать строку исходного кода, соответствующую этим критериям, она должна вернуть строку, для которой  eval будет выброшено исключение с ошибкой синтаксиса.»;

* уточнить «функционально эквивалентное» требование;

* стандартизировать строковое представление встроенных функций и хост-объектов;

* уточнить требования к представлению на основе «фактических характеристик» объекта;

* убедиться, что синтаксический анализ строки содержит то же тело функции и список параметров, что и оригинал;

* для функций, определенных с использованием кода ECMAScript, toString должен возвращать фрагмент исходного текста от начала первого токена до конца последнего токена, соответствующего соответствующей грамматической конструкции;

* для встроенных функциональных объектов toStringне должны возвращать ничего, кроме NativeFunction;

* для вызываемых объектов, которые не были определены с использованием кода ECMAScript, toString необходимо вернуть NativeFunction;

* для функций, создаваемых динамически (конструкторы функции или генератора) toString, должен синтезировать исходный текст;

* для всех других объектов, toString должен бросить TypeError исключение.
</spoiler>


```javascript
// Пользовательская функция
function () { console.log('My Function!'); }.toString();
// function () { console.log('My Function!'); }

// Метод встроенного объекта объект
Number.parseInt.toString();
// function parseInt() { [native code] }

// Функция с привязкой контекста
function () { }.bind(0).toString();
// function () { [native code] }


// Встроенные вызываемые функциональный объекты
Symbol.toString();
// function Symbol() { [native code] }

// Динамически создаваемый функциональный объект
Function().toString();
// function anonymous() {}

// Динамически создаваемый функциональный объект-генератор
function* () { }.toString();
// function* () { }

// .call теперь обязательно ждёт, в качестве аргумента, функцию
Function.prototype.toString.call({});
// Function.prototype.toString requires that 'this' be a Function"
```



----------


 
 


## ֍ Stage 3

Предложения вышедшие из статуса черновика, но ещё не вошедшие в финальную версию стандарта.


 
 


### Приватные\статические\публичные методы\свойства\атрибуты у классов

<https://github.com/tc39/proposal-class-fields><br />
<https://github.com/tc39/proposal-private-methods><br />
<https://github.com/tc39/proposal-static-class-features><br />

В некоторых языках есть договорённость, называть приватные методы через видимый пробел <sup>( «**_**» — такая_штука, ты можешь знать этот знак под неверным названием — нижнее подчёркивание)</sup>

Например так:
```php
<?php
class AdultContent {
	private $_age = 0;
	private $_content = '…is dummy example content (•)(•) —3 (.)(.) only for adults…';
	function __construct($age) {
		$this->_age = $age;
	}
	function __get($name) {
		if($name === 'content') {
			return " (age: ".$this->_age.") → ".$this->_getContent()."\r\n";
		}
		else {
			return 'without info';
		}
	}
	private function _getContent() {
		if($this->_contentIsAllowed()) {
			return $this->_content;
		}
		return 'Sorry. Content not for you.';
	}
	private function _contentIsAllowed() {
		return $this->_age >= 18;
	}
	function __toString() {
		return $this->content;
	}
}
echo "<pre>";

echo strval(new AdultContent(10));
//  (age: 10) →  Sorry. Content not for you

echo strval(new AdultContent(25));
// (age: 25) →  …is dummy example content (•)(•) —3 only for adults…

$ObjectAdultContent = new AdultContent(32);
echo $ObjectAdultContent->content;
// (age: 32) →  …is dummy example content (•)(•) —3 only for adults…
?>

```


Напомню — это только договорённость. Ничто не мешает использовать префикс для других целей, использовать другой префикс, или не использовать вовсе.

<sup>Лично мне импонирует идея использовать видмый пробел в качестве префикса для функций, возвращающих `this`. Так их можно объединять в цепочку вызовов.</sup>

Разработчики спецификации *EcmaScript* пошли дальше и сделали префикс-**октоторп** <sup>( «**#**» —решётка, хеш )</sup> частью синтаксиса.

Предыдущий пример на *ES*10 можно переписать так:

```javascript
export default class AdultContent {

  // Приватные атрибуты класса
  #age = 0
  #adult_content = '…is dummy example content (•)(•) —3 (.)(.) only for adults…'

  constructor(age) {
    this.#setAge(age)
  }

  // Статический приватный метод
  static #userIsAdult(age) {
    return age > 18
  }

  // Публичное свойство
  get content () {
    return `(age: ${this.#age}) → ` + this.#allowed_content
  }

  // Приватное свойство
  get #allowed_content() {
      if(AdultContent.userIsAdult(this.age)){
		return this.#adult_content
	}
	else {
		return 'Sorry. Content not for you.'
	}
  }

  // Приватный метод
  #setAge(age) {
	  this.#age = age
  }

  toString () {
    return this.#content
  }
}

const AdultContentForKid = new AdultContent(10)

console.log(String(AdultContentForKid))
// (age: 10) → Sorry. Content not for you.

console.log(AdultContentForKid.content)
// (age: 10) → Sorry. Content not for you.

const AdultContentForAdult = new AdultContent(25)

console.log(String(AdultContentForAdult))
// (age: 25) → …is dummy example content (•)(•) —3 (.)(.) only for adults…

console.log(AdultContentForAdult.content)
// (age: 25) → …is dummy example content (•)(•) —3 (.)(.) only for adults…

```

Пример излишне усложнён для демонстрации приватных свойств\методов\атрибутов разом. Но в целом JS радует глаз своей лаконичностью по сравнению с PHP вариантом.
Никаких тебе private function _…, ни точек с запятой в конце строки, и точка вместо «->»  для перехода вглубь объекта.
Геттеры именованные. Для динамических имён — прокси-объекты.

Вроде бы мелочи, но после перехода на JS, всё меньше желания возвращаться к PHP.

К слову приватные акцессоры доступны только с Babel 7.3.0 и старше. Крайняя версия на npmjs.com — 7.2.2

Ждём в Stage 4!


 
 

### Шебанг грамматика

https://github.com/tc39/proposal-hashbang

Хешбэнг — знакомый юниксойдам способ указать интерпретатор для исполняемого файла
```javascript
#!/usr/bin/env node
// в скрипте
'use strict';
console.log(1);
```
```javascript
#!/usr/bin/env node
// в модуле
export {};
console.log(1);
```

<sup>в данный момент на подобный фортель *Chrome* выбрасывает `SyntaxError: Invalid or unexpected token`</sup>
 
 


### Большие числа с BigInt

https://github.com/tc39/proposal-bigint

<sup>*работает в Chrome*</sup>

Максимальное целое число, которое можно безопасно использовать в JavaScript (2⁵³ - 1).
```javascript
console.log(Number.MAX_SAFE_INTEGER)
// 9007199254740991
```

BigInt нужен для использования чисел произвольной точности.

Объявляется этот тип несколькими способами:
```javascript
// используя 'n' постфикс в конце более длинных чисел
910000000000000100500n
// 910000000000000100500n

// напрямую передав в конструктор примитива BigInt() без постфикса
BigInt( 910000000000000200500 )
// 910000000000000200500n

// или передав строку в тот-же конструктор
BigInt( "910000000000000300500" )
// 910000000000000300500n

// пример очень большого числа длиной 1642 знака
BigInt( "9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999" )
\\ 9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999n
```


Это новый примитивный тип:
```javascript
typeof 123;
// → 'number'
typeof 123n;
// → 'bigint'
```

Его можно сравнивать с обычными числами:
```javascript
42n === BigInt(42);
// → true
42n == 42;
// → true
```

Но математические операции нужно проводить в пределах одного типа:

```javascript
20000000000000n/20n
// 1000000000000n

20000000000000n/20
// Uncaught TypeError: Cannot mix BigInt and other types, use explicit conversions

```


Поддерживается унарный минус, унарный плюс возвращает ошибку

 
 ```javascript
 -2n
 // -2n

 +2n
 // Uncaught TypeError: Cannot convert a BigInt value to a number
 ```


### `globalThis` — новый способ доступа к глобальному контексту

<https://github.com/tc39/proposal-global>

<sup>*работает в Chrome*</sup>

Поскольку реализации глобальной области видимости зависят от конкретного движка, раньше приходилось делать что-то вроде этого:

```javascript
var getGlobal = function () {
	if (typeof self !== 'undefined') { return self; }
	if (typeof window !== 'undefined') { return window; }
	if (typeof global !== 'undefined') { return global; }
	throw new Error('unable to locate global object');
};
```

И даже такой вариант не гарантировал, что всё точно будет работать.

`globalThis` — общий для всех платформ способ доступа к глобальной области видимости:

```javascript
// Обращение к глобальному конструктору массива
globalThis.Array(1,2,3)
// [1, 2, 3]

// Запись собственных данных в глобальную область видимости
globalThis.myGLobalSettings = {
	it_is_cool: true
}

// Чтение собственных данных из глобальной области видимости
globalThis.myGLobalSettings
// {it_is_cool: true}
```


 
 


### Динамический `import(dynamic)`

<https://github.com/tc39/proposal-dynamic-import>

<sup>*работает в Chrome*</sup>

Хотелось переменные в строках импорта‽ С динамическими импортами это стало возможно:
```javascript
import(`./language-packs/${navigator.language}.js`)
```

Динамический импорт — асинхронная операция. Возвращает промис, который после загрузки модуля возвращает его в функцию обратного вызова.

Поэтому загружать модули можно — отложенно, когда это необходимо:
```javascript
element.addEventListener('click', async () => {
    // можно использовать await синтаксис для промиса
    const module = await import(`./events_scripts/supperButtonClickEvent.js`)
    module.clickEvent()
})
```

Синтаксически, это выглядит как вызов функции `import()`, но не наследуется от Function.prototype, а значит вызвать через call или apply — не удастся:
```javascript
import.call("example this", "argument")
// Uncaught SyntaxError: Unexpected identifier
```
 
 


### import.meta — мета-информация о загружаемом модуле.

https://github.com/tc39/proposal-import-meta

<sup>*работает в Chrome*</sup>

В коде загружаемого модуля стало возможно получить информацию по нему:
```javascript
console.log(import.meta);
// { url: "file:///home/user/my-module.js" }
```
Сейчас это только адрес по которому модуль был загружен.


 
 


### Создание объекта методом `Object.fromEntries()`

https://github.com/tc39/proposal-object-from-entries

Аналог `_.fromPairs` из `lodash`:
```javascript
Object.fromPairs([['key_1', 1], ['key_2', 2]])
// {key_1: 1; key_2: 2}
```


 
 


###  Фикс метода `JSON.stringify()`

https://github.com/tc39/proposal-well-formed-stringify

В разделе [8.1 RFC 8259](https://tools.ietf.org/html/rfc8259#section-8.1) требуется, чтобы текст *JSON*, обмениваемый за пределами замкнутой экосистемы,
кодировался с использованием UTF-8, но JSON.stringify может возвращать строки, содержащие кодовые точки, которые не представлены в UTF-8 (в частности, суррогатные кодовые точки от U+D800 до U+DFFF)

Так строка `\uDF06\uD834` после обработки JSON.stringify() превращается в `\\udf06\\ud834`:
```javascript
/* Непарные суррогатные единицы будут сериализованы с экранированием последовательностей */
JSON.stringify('\uDF06\uD834')
'"\\udf06\\ud834"'
JSON.stringify('\uDEAD')
'"\\udead"'
```

Такого быть не должно, и новая спецификация это исправляет. *Edge* и *Chrome* уже обновились.


 
 


### Устаревшие возможности RegExp

https://github.com/tc39/proposal-regexp-legacy-features

Спецификация для устаревших функций *RegExp*, вроде `RegExp.$1а` также `RegExp.prototype.compile` метода.


 
 


### Прототипные методы строк `.trimStart()` и `.trimEnd()`

<https://github.com/tc39/proposal-string-left-right-trim>

<sup>*работает в Chrome*</sup>


По аналогии с методами `.padStart()` и `.padEnd()`, обрезают пробельные символы в начале и конце строки соответственно:
```javascript
const one = "      hello and let ";
const two = "us begin.        ";
console.log( one.trimStart() + two.trimEnd() )
// "hello and let us begin."
```


 
 


### .matchAll() — новый прототипный метод строк.

https://github.com/tc39/proposal-string-matchall

<sup>*работает в Chrome*</sup>

Работает похоже на метод `.match()` с включенным  флагом `g`, но возвращает итератор:
```javascript
const string_for_searh = 'olololo'

// Вернёт первое вхождение с дополнительной информацией о нём
string_for_searh.match(/o/)
// ["o", index: 0, input: "olololo", groups: undefined]


//Вернёт массив всех вхождений без дополнительной информации
string_for_searh.match(/o/g)
// ["o", "o", "o", "o"]

// Вернёт итератор
string_for_searh.matchAll(/o/)
// {_r: /o/g, _s: "olololo"}

// Итератор возвращает каждое последующее вхождение с подробной информацией,
// как если бы мы использовали .match без глобального флага
for(const item of string_for_searh.matchAll(/o/)) {
  console.log(item)
}
// ["o", index: 0, input: "olololo", groups: undefined]
// ["o", index: 2, input: "olololo", groups: undefined]
// ["o", index: 4, input: "olololo", groups: undefined]
// ["o", index: 6, input: "olololo", groups: undefined]
```


Аргумент должен быть регулярным выражением, иначе будет выброшено исключение:
```javascript
'olololo'.matchAll('o')
// Uncaught TypeError: o is not a regexp!
```


 
 


### Одномерные массивы с `.flat()` и `.flatMap()`

https://github.com/tc39/proposal-flatMap

<sup>*работает в Chrome*</sup>

Массив обзавёлся прототипами `.flat()` и `.flatMap()`, которые в целом  похожи на реализации в *lodash*,
но всё же имеют некоторые отличия. Необязательный аргумент — устанавливает максимальную глубину обхода дерева:
```javascript
const deep_deep_array = [
  '≥0 — первый уровень',
  [
    '≥1 — второй уровень',
    [
      '≥2 — третий уровень',
      [
        '≥3 — четвёртый уровень',
        [
          '≥4 — пятый уровень'
        ]
      ]
    ]
  ]
]

// 0 — вернёт массив без изменений
deep_deep_array.flat(0)
//  ["≥0 — первый уровень", Array(2)]

// 1 — глубина по умолчанию
deep_deep_array.flat()
//  ["первый уровень", "второй уровень", Array(2)]

deep_deep_array.flat(2)
//  ["первый уровень", "второй уровень", "третий уровень", Array(2)]

deep_deep_array.flat(100500)
// ["первый уровень", "второй уровень", "третий уровень", "четвёртый уровень", "пятый уровень"]
```

`.flatMap()`  не эквивалентен последовательному вызову `.flat().map()`. Функция обратного вызова, передаваемая в метод, должна возвращать массив который станет частью общего плоского массива:
```javascript
['Hello', 'World'].flatMap(word => [...word])
// ["H", "e", "l", "l", "o", "W", "o", "r", "l", "d"]
```

 



---------

 
 



## Итоги

*Stage* **4** привнёс скорее косметические изменения. Интерес представляет *Stage* **3**. Большинство из предложений в *Chrome* уже реализованы, за исключением пожалуй `Object.fromEntries()` наличие которого не критично, а приватные свойства очень ждём.

 


--------

 

## Исправления в статье
 
Если заметил в статье неточность, ошибку или есть чем дополнить — ты можешь написать мне [личное сообщение](https://habr.com/ru/conversations/KasperGreen/), а лучше самому воспользоваться репозиторием статьи <https://github.com/KasperGreen/es10>


## Материалы по теме
https://www.ecma-international.org/publications/standards/Ecma-262.htm
https://tc39.github.io/ecma262/
https://developer.okta.com/blog/2019/01/22/whats-new-in-es2019
https://habr.com/ru/sandbox/113964/
https://www.sitepoint.com/javascript-private-class-fields/
https://ru.wikipedia.org/wiki/ECMAScript
https://habr.com/ru/company/ruvds/blog/431872/
https://medium.com/devschacht/javascripts-new-private-class-fields-c60daffe361b
https://ru.wikipedia.org/wiki/%D0%A8%D0%B5%D0%B1%D0%B0%D0%BD%D0%B3_(Unix)
https://habr.com/ru/post/354930/

-------------


 
 


![Альтернативный вариант КДПВ с жёлтым магнитом от elfafeya.art](https://habrastorage.org/webt/nt/a4/y7/nta4y72u_f_kgtwon8jio9ghiwg.png)
       <sup><cite>Автор фото: kasper.green; Жёлтый магнит: elfafeya.art & kasper.green</cite></sup>
