
Стандартизация JS&nbsp;перешла на&nbsp;годичный цикл обновлений, а&nbsp;начало года&nbsp;&mdash; отличное время для того чтобы узнать, что нас ждёт в&nbsp;юбилейной&nbsp;&mdash; уже десятой редакции EcmaScript!

*ES*9 - [актуальная версия спецификации](https://www.ecma-international.org/publications/standards/Ecma-262.htm)

*ES*10 — всё ещё [черновик](https://tc39.github.io/ecma262/)


На сегодняшний день в [*Stage* **4**](https://habr.com/ru/post/437806/#-stage-4) — всего несколько предложений.

А в [*Stage* **3**](https://habr.com/ru/post/437806/#-stage-3) — целая дюжина!

Из них, на мой взгляд, самые интересные — [приватные поля классов](https://habr.com/ru/post/437806/#privatnyestaticheskiepublichnye-metodysvoystvaatributy-u-klassov), [шебанг грамматика для скриптов](https://habr.com/ru/post/437806/#shebang-grammatika), [числа произвольной точности](https://habr.com/ru/post/437806/#bolshie-chisla-s-bigint), [доступ к глобальному контексту](https://habr.com/ru/post/437806/#globalthis--novyy-sposob-dostupa-k-globalnomu-kontekstu) и [динамические импорты](https://habr.com/ru/post/437806/#dinamicheskiy-importdynamic).


 
![КДПВ: Жёлтый магнит с надписью «JS ES10» на экране монитора —  от kasper.green & elfafeya.art](https://habrastorage.org/webt/j-/g9/na/j-g9nan1y54iew_qzum-eodzfgw.png)
        <sup><cite>Автор фото: kasper.green; Жёлтый магнит: elfafeya.art &amp;&nbsp;kasper.green</cite></sup>

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

Эти изменения уже вошли в&nbsp;стандарт.


### Необязательный аргумент у `catch`

<https://github.com/tc39/proposal-optional-catch-binding>

До&nbsp;*ES*10 блок ```catch``` требовал обязательного аргумента для сбора информации об&nbsp;ошибке, даже если она не&nbsp;используется:
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
<sup>*Edge* пока не&nbsp;обновлён до&nbsp;*ES*10, и&nbsp;ожидаемо валится с&nbsp;ошибкой</sup>


Начиная с&nbsp;редакции *ES*10, круглые скобки можно опустить и ```catch``` станет как две капли воды похож на ```try```.

![](https://habrastorage.org/webt/yi/ia/qg/yiiaqgiclyxz_i7bf3gq14dj-8m.png)
<sup>Мой Chrome уже обновился до&nbsp;*ES*10, а&nbsp;местами и&nbsp;до&nbsp;*Stage*&nbsp;**3**. Дальше скриншоты будут из&nbsp;*Chrome*</sup>

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

Начиная с&nbsp;*ES*10&nbsp;у символов появилось свойство description, доступное только для чтения. Оно позволяет без всяких танцев с&nbsp;бубном получить описание символа:
```javascript
symbol_link.description
// "Symbol description"
```


В&nbsp;случае если описание не&nbsp;задано, вернётся&nbsp;&mdash; `undefined`:
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

&gt; Если реализация не&nbsp;может создать строку исходного кода, соответствующую этим критериям, она должна вернуть строку, для которой eval будет выброшено исключение с&nbsp;ошибкой синтаксиса.&laquo;;

* уточнить &laquo;функционально эквивалентное&raquo; требование;

* стандартизировать строковое представление встроенных функций и&nbsp;хост-объектов;

* уточнить требования к&nbsp;представлению на&nbsp;основе &laquo;фактических характеристик&raquo; объекта;

* убедиться, что синтаксический анализ строки содержит то&nbsp;же тело функции и&nbsp;список параметров, что и&nbsp;оригинал;

* для функций, определенных с&nbsp;использованием кода ECMAScript, toString должен возвращать фрагмент исходного текста от&nbsp;начала первого токена до&nbsp;конца последнего токена, соответствующего соответствующей грамматической конструкции;

* для встроенных функциональных объектов toStringне должны возвращать ничего, кроме NativeFunction;

* для вызываемых объектов, которые не&nbsp;были определены с&nbsp;использованием кода ECMAScript, toString необходимо вернуть NativeFunction;

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

Предложения вышедшие из&nbsp;статуса черновика, но&nbsp;ещё не&nbsp;вошедшие в&nbsp;финальную версию стандарта. 


### Приватные\статические\публичные методы\свойства\атрибуты у классов

<https://github.com/tc39/proposal-class-fields><br />
<https://github.com/tc39/proposal-private-methods><br />
<https://github.com/tc39/proposal-static-class-features><br />

В&nbsp;некоторых языках есть договорённость, называть приватные методы через видимый пробел <sup>( &laquo;**_**&raquo; &mdash; такая_штука, ты&nbsp;можешь знать этот знак под неверным названием&nbsp;&mdash; нижнее подчёркивание)</sup>.

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


Напомню&nbsp;&mdash; это только договорённость. Ничто не&nbsp;мешает использовать префикс для других целей, использовать другой префикс, или не&nbsp;использовать вовсе.

<sup>Лично мне импонирует идея использовать видмый пробел в&nbsp;качестве префикса для функций, возвращающих `this`. Так их&nbsp;можно объединять в&nbsp;цепочку вызовов.</sup>

Разработчики спецификации *EcmaScript* пошли дальше и&nbsp;сделали префикс-**октоторп** <sup>( &laquo;**#**&raquo; &mdash;решётка, хеш )</sup> частью синтаксиса.

Предыдущий пример на&nbsp;*ES*10 можно переписать следующим образом:
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

Пример излишне усложнён для демонстрации приватных свойств\методов\атрибутов разом. Но&nbsp;в&nbsp;целом&nbsp;JS радует глаз своей лаконичностью по&nbsp;сравнению с&nbsp;PHP вариантом.
Никаких тебе private function _..., ни&nbsp;точек с&nbsp;запятой в&nbsp;конце строки, и&nbsp;точка вместо &laquo;-&gt;&raquo; для перехода вглубь объекта.
Геттеры именованные. Для динамических имён&nbsp;&mdash; прокси-объекты.

Вроде&nbsp;бы мелочи, но&nbsp;после перехода на&nbsp;JS, всё меньше желания возвращаться к&nbsp;PHP.

К&nbsp;слову приватные акцессоры доступны только с&nbsp;Babel&nbsp;7.3.0 и&nbsp;старше. Крайняя версия на&nbsp;npmjs.com&nbsp;&mdash; 7.2.2

Ждём в&nbsp;Stage&nbsp;4!


 
 

### Шебанг грамматика

https://github.com/tc39/proposal-hashbang

Хешбэнг&nbsp;&mdash; знакомый юниксойдам способ указать интерпретатор для исполняемого файла:
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

<sup>в&nbsp;данный момент на&nbsp;подобный фортель *Chrome* выбрасывает `SyntaxError: Invalid or&nbsp;unexpected token`</sup>
 
 


### Большие числа с BigInt

https://github.com/tc39/proposal-bigint

<sup>*работает в Chrome*</sup>

Максимальное целое число, которое можно безопасно использовать в&nbsp;JavaScript (2⁵³ - 1):
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

Но&nbsp;математические операции нужно проводить в&nbsp;пределах одного типа:
```javascript
20000000000000n/20n
// 1000000000000n

20000000000000n/20
// Uncaught TypeError: Cannot mix BigInt and other types, use explicit conversions

```


Поддерживается унарный минус, унарный плюс возвращает ошибку:
 ```javascript
 -2n
 // -2n

 +2n
 // Uncaught TypeError: Cannot convert a BigInt value to a number
 ```


### `globalThis` — новый способ доступа к глобальному контексту

<https://github.com/tc39/proposal-global>

<sup>*работает в Chrome*</sup>

Поскольку реализации глобальной области видимости зависят от&nbsp;конкретного движка, раньше приходилось делать что-то вроде этого:
```javascript
var getGlobal = function () {
	if (typeof self !== 'undefined') { return self; }
	if (typeof window !== 'undefined') { return window; }
	if (typeof global !== 'undefined') { return global; }
	throw new Error('unable to locate global object');
};
```

И&nbsp;даже такой вариант не&nbsp;гарантировал, что всё точно будет работать.

`globalThis` &mdash; общий для всех платформ способ доступа к&nbsp;глобальной области видимости:
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

Хотелось переменные в&nbsp;строках импорта‽ С&nbsp;динамическими импортами это стало возможно:
```javascript
import(`./language-packs/${navigator.language}.js`)
```

Динамический импорт&nbsp;&mdash; асинхронная операция. Возвращает промис, который после загрузки модуля возвращает его в&nbsp;функцию обратного вызова.

Поэтому загружать модули можно&nbsp;&mdash; отложенно, когда это необходимо:
```javascript
element.addEventListener('click', async () => {
    // можно использовать await синтаксис для промиса
    const module = await import(`./events_scripts/supperButtonClickEvent.js`)
    module.clickEvent()
})
```

Синтаксически, это выглядит как вызов функции `import()`, но&nbsp;не&nbsp;наследуется от&nbsp;Function.prototype, а&nbsp;значит вызвать через `call` или `apply` &mdash; не&nbsp;удастся:
```javascript
import.call("example this", "argument")
// Uncaught SyntaxError: Unexpected identifier
```
 
 


### import.meta — мета-информация о загружаемом модуле.

https://github.com/tc39/proposal-import-meta

<sup>*работает в Chrome*</sup>

В&nbsp;коде загружаемого модуля стало возможно получить информацию по&nbsp;нему:
```javascript
console.log(import.meta);
// { url: "file:///home/user/my-module.js" }
```
Сейчас это только адрес по&nbsp;которому модуль был загружен.
 
 


### Создание объекта методом `Object.fromEntries()`

https://github.com/tc39/proposal-object-from-entries

Аналог `_.fromPairs` из `lodash`:
```javascript
Object.fromPairs([['key_1', 1], ['key_2', 2]])
// {key_1: 1; key_2: 2}
```


 
 


###  Фикс метода `JSON.stringify()`

https://github.com/tc39/proposal-well-formed-stringify

В&nbsp;разделе [8.1&nbsp;RFC 8259](https://tools.ietf.org/html/rfc8259#section-8.1) требуется, чтобы текст *JSON*, обмениваемый за&nbsp;пределами замкнутой экосистемы,
кодировался с&nbsp;использованием UTF-8, но&nbsp;JSON.stringify может возвращать строки, содержащие кодовые точки, которые не&nbsp;представлены в&nbsp;UTF-8 (в&nbsp;частности, суррогатные кодовые точки от&nbsp;U+D800 до&nbsp;U+DFFF)

Так строка `\uDF06\uD834` после обработки JSON.stringify() превращается в `\\udf06\\ud834`:
```javascript
/* Непарные суррогатные единицы будут сериализованы с экранированием последовательностей */
JSON.stringify('\uDF06\uD834')
'"\\udf06\\ud834"'
JSON.stringify('\uDEAD')
'"\\udead"'
```

Такого быть не&nbsp;должно, и&nbsp;новая спецификация это исправляет. *Edge* и&nbsp;*Chrome* уже обновились.


 
 


### Устаревшие возможности RegExp

https://github.com/tc39/proposal-regexp-legacy-features

Спецификация для устаревших функций *RegExp*, вроде `RegExp.$1`, а&nbsp;также `RegExp.prototype.compile` метода.


 
 


### Прототипные методы строк `.trimStart()` и `.trimEnd()`

<https://github.com/tc39/proposal-string-left-right-trim>

<sup>*работает в Chrome*</sup>


По&nbsp;аналогии с&nbsp;методами `.padStart()` и `.padEnd()`, обрезают пробельные символы в&nbsp;начале и&nbsp;конце строки соответственно:
```javascript
const one = "      hello and let ";
const two = "us begin.        ";
console.log( one.trimStart() + two.trimEnd() )
// "hello and let us begin."
```


 
 


### .matchAll() — новый прототипный метод строк.

https://github.com/tc39/proposal-string-matchall

<sup>*работает в Chrome*</sup>

Работает похоже на&nbsp;метод `.match()` с&nbsp;включенным флагом `g`, но&nbsp;возвращает итератор:
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

Массив обзавёлся прототипами `.flat()` и `.flatMap()`, которые в&nbsp;целом похожи на&nbsp;реализации в&nbsp;*lodash*,
но&nbsp;всё&nbsp;же имеют некоторые отличия. Необязательный аргумент&nbsp;&mdash; устанавливает максимальную глубину обхода дерева:
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

`.flatMap()` не&nbsp;эквивалентен последовательному вызову `.flat().map()`. Функция обратного вызова, передаваемая в&nbsp;метод, должна возвращать массив который станет частью общего плоского массива:
```javascript
['Hello', 'World'].flatMap(word => [...word])
// ["H", "e", "l", "l", "o", "W", "o", "r", "l", "d"]
```

 



---------

 
 



## Итоги

*Stage* **4**&nbsp;привнёс скорее косметические изменения. Интерес представляет *Stage*&nbsp;**3**. Большинство из&nbsp;предложений в&nbsp;*Chrome* уже реализованы, за&nbsp;исключением пожалуй `Object.fromEntries()` наличие которого не&nbsp;критично, а&nbsp;приватные свойства очень ждём.

 


--------

 

## Исправления в статье
 
Если заметил в&nbsp;статье неточность, ошибку или есть чем дополнить&nbsp;&mdash; ты&nbsp;можешь написать мне [личное сообщение](https://habr.com/ru/conversations/KasperGreen/), а&nbsp;лучше самому воспользоваться репозиторием статьи <https://github.com/KasperGreen/es10>


## Материалы по теме
[Актуальная версия стандарта Ecma-262](https://www.ecma-international.org/publications/standards/Ecma-262.htm)

[Черновик следующей версии стандарта Ecma-262](https://tc39.github.io/ecma262/)

[Новые #приватные поля классов в JavaScript](https://medium.com/devschacht/javascripts-new-private-class-fields-c60daffe361b)

[ECMAScript](https://ru.wikipedia.org/wiki/ECMAScript)

[Обзор возможностей стандартов ES7, ES8 и ES9](https://habr.com/ru/company/ruvds/blog/431872/)

[Шебанг](https://ru.wikipedia.org/wiki/%D0%A8%D0%B5%D0%B1%D0%B0%D0%BD%D0%B3_(Unix))
https://habr.com/ru/post/354930/


-------------


 
 


![Альтернативный вариант КДПВ с жёлтым магнитом от elfafeya.art](https://habrastorage.org/webt/nt/a4/y7/nta4y72u_f_kgtwon8jio9ghiwg.png)
       <sup><cite>Автор фото: kasper.green; Жёлтый магнит: elfafeya.art & kasper.green</cite></sup>
