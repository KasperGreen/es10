Стандартизация JS&nbsp;перешла на&nbsp;годичный цикл обновлений, а&nbsp;начало года&nbsp;&mdash; отличное время для того чтобы узнать, что нас ждёт в&nbsp;юбилейной&nbsp;&mdash; уже десятой редакции EcmaScript!

*ES*9&nbsp;&mdash; [актуальная версия спецификации](https://www.ecma-international.org/publications/standards/Ecma-262.htm)

*ES*10&nbsp;&mdash; всё ещё [черновик](https://tc39.github.io/ecma262/)


На&nbsp;сегодняшний день&nbsp;в [*Stage*&nbsp;**4**](https://habr.com/ru/post/437806/#-stage-4)&nbsp;&mdash; всего несколько предложений.

А&nbsp;в [*Stage*&nbsp;**3**](https://habr.com/ru/post/437806/#-stage-3)&nbsp;&mdash; целая дюжина!

Из&nbsp;них, на&nbsp;мой взгляд, самые интересные&nbsp;&mdash; [приватные поля классов](https://habr.com/ru/post/437806/#privatnyestaticheskiepublichnye-metodysvoystvaatributy-u-klassov), [шебанг грамматика для скриптов](https://habr.com/ru/post/437806/#shebang-grammatika), [числа произвольной точности](https://habr.com/ru/post/437806/#bolshie-chisla-s-bigint), [доступ к&nbsp;глобальному контексту](https://habr.com/ru/post/437806/#globalthis&mdash;novyy-sposob-dostupa-k-globalnomu-kontekstu) и&nbsp;[динамические импорты](https://habr.com/ru/post/437806/#dinamicheskiy-importdynamic).



![КДПВ: Жёлтый магнит с&nbsp;надписью &laquo;JS&nbsp;ES10&raquo; на&nbsp;экране монитора&nbsp;&mdash; от&nbsp;kasper.green &amp;&nbsp;elfafeya.art](https://habrastorage.org/webt/j-/g9/na/j-g9nan1y54iew_qzum-eodzfgw.png)
<sup><cite>Автор фото: kasper.green; Жёлтый магнит: elfafeya.art &amp;&nbsp;kasper.green</cite></sup>

<cut />

## Содержание

### [Пять стадий](#pyat-stadiy)

### [Stage 4&nbsp;&mdash; Final](#-stage-4)

&bull; [**```catch```** &mdash; аргумент стал необязательным](#neobyazatelnyy-argument-u-catch);

&bull; [**```Symbol().description```** &mdash; акцессор к&nbsp;описанию символа](#dostup-k-opisaniyu-simvolnoy-ssylki);

&bull; [**```&rsquo;строки EcmaScript&rsquo;```** &mdash; улучшенная совместимость с&nbsp;**JSON** форматом](#stroki-ecmascript-sovmestimye-s-json);

&bull; [**```.toString()```** &mdash; прототипный метод обновлён](#dorabotka-prototipnogo-metoda-tostring).

-------------

### [Stage 3&nbsp;&mdash; Pre-release](#-stage-3)

&bull; [**```#```** &mdash; приватное всё у&nbsp;классов, через октоторп](#privatnyestaticheskiepublichnye-metodysvoystvaatributy-u-klassov);

&bull; [**```#!/usr/bin/env node```** &mdash; шебанг грамматика для скриптов](#shebang-grammatika);

&bull; [**```BigInt()```** &mdash; новый примитив, для чисел произвольной точности](#bolshie-chisla-s-bigint);

&bull; [**```globalThis```** &mdash; новый способ доступа к&nbsp;глобальному контексту](#globalthis&mdash;novyy-sposob-dostupa-k-globalnomu-kontekstu);

&bull; [**```import(dynamic)```** &mdash; динамический импорт](#dinamicheskiy-importdynamic);

&bull; [**```import.meta```** &mdash; мета-информация о&nbsp;загружаемом модуле](#importmeta&mdash;meta-informaciya-o-zagruzhaemom-module);

&bull; [**```Object.fromEntries()```** &mdash; создание объекта из&nbsp;массива пар&nbsp;&mdash; ключ\значение](#sozdanie-obekta-metodom-objectfromentries);

&bull; [**```JSON.stringify()```** &mdash; фикс метода](#fiks-metoda-jsonstringify);

&bull; [**```RegExp```** &mdash; устаревшие возможности](#ustarevshie-vozmozhnosti-regexp);

&bull; [**```.trimStart()```** и **```.trimEnd()```** &mdash; прототипные методы строк](#prototipnye-metody-strok-trimstart-i-trimend);

&bull; [**```.matchAll()```** &mdash; **```.match()```** с&nbsp;глобальным флагом](#matchall&mdash;novyy-prototipnyy-metod-strok);

&bull; [**```.flat()```** и **```.flatMap()```** &mdash; прототипные методы массивов](#odnomernye-massivy-s-flat-i-flatmap).


### [Итоги](#itogi)



--------------------



## Пять стадий

<sub>*Stage*</sub> **0** &darr; &thinsp;**Strawman** <sup>**Наметка**</sup> &thinsp;Идея, которую можно реализовать через **Babel**-плагин.;

<sub>*Stage*</sub> **1** &darr; &thinsp;**Proposal** <sup>**Предложение**</sup> &thinsp;Проверка жизнеспособности идеи.;

<sub>*Stage*</sub> **2** &darr; &thinsp;**Draft** <sup>**Черновик**</sup> &thinsp;Начало разработки спецификации.;

<sub>*Stage*</sub> **3** &darr; &thinsp;**Candidate** <sup>**Кандидат**</sup> Предварительная версия спецификации.;

<sub>*Stage*</sub> **4** ֍ **Finished** <sup>**Завершён**</sup> &thinsp;Финальная версия спецификации на&nbsp;этот год.

----------------------------

Мы&nbsp;рассмотрим только *Stage* **4**&nbsp;&mdash; де-факто, вошедший в&nbsp;стандарт.

И&nbsp;*Stage* **3**&nbsp;&mdash; который вот-вот станет его частью.

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
} catch(unusedVariable) { // переменная не&nbsp;используется
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






## Доступ к&nbsp;описанию символьной ссылки

<https://tc39.github.io/proposal-Symbol-description/>

Описание символьной ссылки можно косвенно получить методом toString():
```javascript
const symbol_link = Symbol(&quot;Symbol description&laquo;)
String(symbol_link) // &laquo;Symbol(Symbol description)&raquo;
```

Начиная с&nbsp;*ES*10&nbsp;у символов появилось свойство description, доступное только для чтения. Оно позволяет без всяких танцев с&nbsp;бубном получить описание символа:
```javascript
symbol_link.description
// &laquo;Symbol description&raquo;
```


В&nbsp;случае если описание не&nbsp;задано, вернётся&nbsp;&mdash; `undefined`
```javascript
const without_description_symbol_link = Symbol()
without_description_symbol_link.description
// undefined

const empty_description_symbol_link = Symbol(&rsquo;&rsquo;)
empty_description_symbol_link.description
// &quot;&quot;
```




### Строки EcmaScript совместимые с&nbsp;JSON

<https://github.com/tc39/proposal-json-superset>

EcmaScript до&nbsp;десятой редакции утверждает, что *JSON* является подмножеством `JSON.parse`, но&nbsp;это неверно.

*JSON* строки могут содержать не&nbsp;экранированные символы разделителей линий <sup>`U+2028` *LINE SEPARATOR*</sup> и&nbsp;абзацев <sup>**`U+2029`** *PARAGRAPH SEPARATOR*</sup>.

Строки *ECMAScript* до&nbsp;десятой версии&nbsp;&mdash; нет.

Если в&nbsp;*Edge* вызвать `eval()` со&nbsp;строкой `&quot;\u2029&laquo;`,
он&nbsp;ведёт себя так, словно мы&nbsp;сделали перенос строки&nbsp;&mdash; прямо посреди кода.

![](https://habrastorage.org/webt/vc/yg/-v/vcyg-vy7larz8y4kswiqinjhfh4.png)



C&nbsp;*ES*10 строками&nbsp;&mdash; всё в&nbsp;порядке:

![](https://habrastorage.org/webt/tc/he/tm/tchetmze4axxtp5wghm7pmz-0xc.png)






### Доработка прототипного метода `.toString()`

<http://tc39.github.io/Function-prototype-toString-revision/>

<spoiler title="Цели изменений">
* убрать обратно несовместимое требование:

&gt; Если реализация не&nbsp;может создать строку исходного кода, соответствующую этим критериям, она должна вернуть строку, для которой eval будет выброшено исключение с&nbsp;ошибкой синтаксиса.&laquo;;

* уточнить &bdquo;функционально эквивалентное&ldquo; требование;

* стандартизировать строковое представление встроенных функций и&nbsp;хост-объектов;

* уточнить требования к&nbsp;представлению на&nbsp;основе &bdquo;фактических характеристик&ldquo; объекта;

* убедиться, что синтаксический анализ строки содержит то&nbsp;же тело функции и&nbsp;список параметров, что и&nbsp;оригинал;

* для функций, определенных с&nbsp;использованием кода ECMAScript, toString должен возвращать фрагмент исходного текста от&nbsp;начала первого токена до&nbsp;конца последнего токена, соответствующего соответствующей грамматической конструкции;

* для встроенных функциональных объектов toStringне должны возвращать ничего, кроме NativeFunction;

* для вызываемых объектов, которые не&nbsp;были определены с&nbsp;использованием кода ECMAScript, toString необходимо вернуть NativeFunction;

* для функций, создаваемых динамически (конструкторы функции или генератора) toString, должен синтезировать исходный текст;

* для всех других объектов, toString должен бросить TypeError исключение.
</spoiler>


```javascript
// Пользовательская функция
function () { console.log(&rsquo;My Function!&rsquo;); }.toString();
// function () { console.log(&rsquo;My Function!&rsquo;); }

// Метод встроенного объекта объект
Number.parseInt.toString();
// function parseInt() { [native code] }

// Функция с&nbsp;привязкой контекста
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

// .call теперь обязательно ждёт, в&nbsp;качестве аргумента, функцию
Function.prototype.toString.call({});
// Function.prototype.toString requires that &rsquo;this&rsquo; be&nbsp;a&nbsp;Function&raquo;
```



----------






## ֍ Stage 3

Предложения вышедшие из&nbsp;статуса черновика, но&nbsp;ещё не&nbsp;вошедшие в&nbsp;финальную версию стандарта.






### Приватные\статические\публичные методы\свойства\атрибуты у&nbsp;классов

<https://github.com/tc39/proposal-class-fields><br />
<https://github.com/tc39/proposal-private-methods><br />
<https://github.com/tc39/proposal-static-class-features><br />

В&nbsp;некоторых языках есть договорённость, называть приватные методы через видимый пробел <sup>( &laquo;**_**&raquo; &mdash; такая_штука, ты&nbsp;можешь знать этот знак под неверным названием&nbsp;&mdash; нижнее подчёркивание)</sup>

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
		if($name === &rsquo;content&rsquo;) {
			return &quot; (age: &laquo;.$this-&gt;_age.&raquo;) &rarr; &laquo;.$this-&gt;_getContent().&bdquo;\r\n&ldquo;;
		}
		else {
			return &rsquo;without info&rsquo;;
		}
	}
	private function _getContent() {
		if($this-&gt;_contentIsAllowed()) {
			return $this-&gt;_content;
		}
		return &rsquo;Sorry. Content not for you.&rsquo;;
	}
	private function _contentIsAllowed() {
		return $this-&gt;_age &gt;= 18;
	}
	function __toString() {
		return $this-&gt;content;
	}
}
echo &raquo;<pre>&laquo;;

echo strval(new AdultContent(10));
// (age: 10) &rarr; Sorry. Content not for you

echo strval(new AdultContent(25));
// (age: 25) &rarr; ...is dummy example content (&bull;)(&bull;) &mdash;3 only for adults...

$ObjectAdultContent = new AdultContent(32);
echo $ObjectAdultContent-&gt;content;
// (age: 32) &rarr; ...is dummy example content (&bull;)(&bull;) &mdash;3 only for adults...
?&gt;

```


Напомню&nbsp;&mdash; это только договорённость. Ничто не&nbsp;мешает использовать префикс для других целей, использовать другой префикс, или не&nbsp;использовать вовсе.

<sup>Лично мне импонирует идея использовать видмый пробел в&nbsp;качестве префикса для функций, возвращающих `this`. Так их&nbsp;можно объединять в&nbsp;цепочку вызовов.</sup>

Разработчики спецификации *EcmaScript* пошли дальше и&nbsp;сделали префикс-**октоторп** <sup>( &raquo;**#**&quot; &mdash;решётка, хеш )</sup> частью синтаксиса.

Предыдущий пример на&nbsp;*ES*10 можно переписать так:

```javascript
export default class AdultContent {

// Приватные атрибуты класса
#age = 0
#adult_content = &rsquo;...is dummy example content (&bull;)(&bull;) &mdash;3 (.)(.) only for adults...&rsquo;

constructor(age) {
this.#setAge(age)
}

// Статический приватный метод
static #userIsAdult(age) {
return age &gt; 18
}

// Публичное свойство
get content () {
return `(age: ${this.#age}) &rarr; ` + this.#allowed_content
}

// Приватное свойство
get #allowed_content() {
if(AdultContent.userIsAdult(this.age)){
		return this.#adult_content
	}
	else {
		return &rsquo;Sorry. Content not for you.&rsquo;
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
// (age: 10) &rarr; Sorry. Content not for you.

console.log(AdultContentForKid.content)
// (age: 10) &rarr; Sorry. Content not for you.

const AdultContentForAdult = new AdultContent(25)

console.log(String(AdultContentForAdult))
// (age: 25) &rarr; ...is dummy example content (&bull;)(&bull;) &mdash;3 (.)(.) only for adults...

console.log(AdultContentForAdult.content)
// (age: 25) &rarr; ...is dummy example content (&bull;)(&bull;) &mdash;3 (.)(.) only for adults...

```

Пример излишне усложнён для демонстрации приватных свойств\методов\атрибутов разом. Но&nbsp;в&nbsp;целом&nbsp;JS радует глаз своей лаконичностью по&nbsp;сравнению с&nbsp;PHP вариантом.
Никаких тебе private function _..., ни&nbsp;точек с&nbsp;запятой в&nbsp;конце строки, и&nbsp;точка вместо &laquo;-&gt;&raquo; для перехода вглубь объекта.
Геттеры именованные. Для динамических имён&nbsp;&mdash; прокси-объекты.

Вроде&nbsp;бы мелочи, но&nbsp;после перехода на&nbsp;JS, всё меньше желания возвращаться к&nbsp;PHP.

К&nbsp;слову приватные акцессоры доступны только с&nbsp;Babel&nbsp;7.3.0 и&nbsp;старше. Крайняя версия на&nbsp;npmjs.com&nbsp;&mdash; 7.2.2

Ждём в&nbsp;Stage&nbsp;4!





### Шебанг грамматика

https://github.com/tc39/proposal-hashbang

Хешбэнг&nbsp;&mdash; знакомый юниксойдам способ указать интерпретатор для исполняемого файла
```javascript
#!/usr/bin/env node
// в&nbsp;скрипте
&rsquo;use strict&rsquo;;
console.log(1);
```
```javascript
#!/usr/bin/env node
// в&nbsp;модуле
export {};
console.log(1);
```

<sup>в&nbsp;данный момент на&nbsp;подобный фортель *Chrome* выбрасывает `SyntaxError: Invalid or&nbsp;unexpected token`</sup>




### Большие числа с&nbsp;BigInt

https://github.com/tc39/proposal-bigint

<sup>*работает в&nbsp;Chrome*</sup>

Максимальное целое число, которое можно безопасно использовать в&nbsp;JavaScript (2⁵&sup3; - 1).
```javascript
console.log(Number.MAX_SAFE_INTEGER)
// 9007199254740991
```

BigInt нужен для использования чисел произвольной точности.

Объявляется этот тип несколькими способами:
```javascript
// используя &rsquo;n&rsquo; постфикс в&nbsp;конце более длинных чисел
910000000000000100500n
// 910000000000000100500n

// напрямую передав в&nbsp;конструктор примитива BigInt() без постфикса
BigInt( 910000000000000200500 )
// 910000000000000200500n

// или передав строку в&nbsp;тот-же конструктор
BigInt( &laquo;910000000000000300500&raquo; )
// 910000000000000300500n

// пример очень большого числа длиной 1642 знака
BigInt( &laquo;9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999&raquo; )
\\ 9999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999999n
```


Это новый примитивный тип:
```javascript
typeof 123;
// &rarr; &rsquo;number&rsquo;
typeof 123n;
// &rarr; &rsquo;bigint&rsquo;
```

Его можно сравнивать с&nbsp;обычными числами:
```javascript
42n === BigInt(42);
// &rarr; true
42n == 42;
// &rarr; true
```

Но&nbsp;математические операции нужно проводить в&nbsp;пределах одного типа:

```javascript
20000000000000n/20n
// 1000000000000n

20000000000000n/20
// Uncaught TypeError: Cannot mix BigInt and other types, use explicit conversions

```


Поддерживается унарный минус, унарный плюс возвращает ошибку


```javascript
-2n
// &minus;2n

+2n
// Uncaught TypeError: Cannot convert a&nbsp;BigInt value to&nbsp;a&nbsp;number
```


### `globalThis` &mdash; новый способ доступа к&nbsp;глобальному контексту

<https://github.com/tc39/proposal-global>

<sup>*работает в&nbsp;Chrome*</sup>

Поскольку реализации глобальной области видимости зависят от&nbsp;конкретного движка, раньше приходилось делать что-то вроде этого:

```javascript
var getGlobal = function () {
	if&nbsp;(typeof self !== &rsquo;undefined&rsquo;) { return self; }
	if&nbsp;(typeof window !== &rsquo;undefined&rsquo;) { return window; }
	if&nbsp;(typeof global !== &rsquo;undefined&rsquo;) { return global; }
	throw new Error(&rsquo;unable to&nbsp;locate global object&rsquo;);
};
```

И&nbsp;даже такой вариант не&nbsp;гарантировал, что всё точно будет работать.

`globalThis` &mdash; общий для всех платформ способ доступа к&nbsp;глобальной области видимости:

```javascript
// Обращение к&nbsp;глобальному конструктору массива
globalThis.Array(1,2,3)
// [1, 2, 3]

// Запись собственных данных в&nbsp;глобальную область видимости
globalThis.myGLobalSettings = {
	it_is_cool: true
}

// Чтение собственных данных из&nbsp;глобальной области видимости
globalThis.myGLobalSettings
// {it_is_cool: true}
```






### Динамический `import(dynamic)`

<https://github.com/tc39/proposal-dynamic-import>

<sup>*работает в&nbsp;Chrome*</sup>

Хотелось переменные в&nbsp;строках импорта‽ С&nbsp;динамическими импортами это стало возможно:
```javascript
import(`./language-packs/${navigator.language}.js`)
```

Динамический импорт&nbsp;&mdash; асинхронная операция. Возвращает промис, который после загрузки модуля возвращает его в&nbsp;функцию обратного вызова.

Поэтому загружать модули можно&nbsp;&mdash; отложенно, когда это необходимо:
```javascript
element.addEventListener(&rsquo;click&rsquo;, async () =&gt; {
// можно использовать await синтаксис для промиса
const module = await import(`./events_scripts/supperButtonClickEvent.js`)
module.clickEvent()
})
```

Синтаксически, это выглядит как вызов функции `import()`, но&nbsp;не&nbsp;наследуется от&nbsp;Function.prototype, а&nbsp;значит вызвать через call или apply&nbsp;&mdash; не&nbsp;удастся:
```javascript
import.call(&quot;example this&laquo;, &laquo;argument&raquo;)
// Uncaught SyntaxError: Unexpected identifier
```




### import.meta&nbsp;&mdash; мета-информация о&nbsp;загружаемом модуле.

https://github.com/tc39/proposal-import-meta

<sup>*работает в&nbsp;Chrome*</sup>

В&nbsp;коде загружаемого модуля стало возможно получить информацию по&nbsp;нему:
```javascript
console.log(import.meta);
// { url: &laquo;file:///home/user/my-module.js&raquo; }
```
Сейчас это только адрес по&nbsp;которому модуль был загружен.






### Создание объекта методом `Object.fromEntries()`

https://github.com/tc39/proposal-object-from-entries

Аналог `_.fromPairs` из `lodash`:
```javascript
Object.fromPairs([[&rsquo;key_1&prime;, 1], [&rsquo;key_2&prime;, 2]])
// {key_1: 1; key_2: 2}
```






### Фикс метода `JSON.stringify()`

https://github.com/tc39/proposal-well-formed-stringify

В&nbsp;разделе [8.1&nbsp;RFC 8259](https://tools.ietf.org/html/rfc8259#section-8.1) требуется, чтобы текст *JSON*, обмениваемый за&nbsp;пределами замкнутой экосистемы,
кодировался с&nbsp;использованием UTF-8, но&nbsp;JSON.stringify может возвращать строки, содержащие кодовые точки, которые не&nbsp;представлены в&nbsp;UTF-8 (в&nbsp;частности, суррогатные кодовые точки от&nbsp;U+D800 до&nbsp;U+DFFF)

Так строка `\uDF06\uD834` после обработки JSON.stringify() превращается в `\\udf06\\ud834`:
```javascript
/* Непарные суррогатные единицы будут сериализованы с&nbsp;экранированием последовательностей */
JSON.stringify(&rsquo;\uDF06\uD834&prime;)
&rsquo;&quot;\\udf06\\ud834&quot;&rsquo;
JSON.stringify(&rsquo;\uDEAD&rsquo;)
&rsquo;&quot;\\udead&quot;&rsquo;
```

Такого быть не&nbsp;должно, и&nbsp;новая спецификация это исправляет. *Edge* и&nbsp;*Chrome* уже обновились.






### Устаревшие возможности RegExp

https://github.com/tc39/proposal-regexp-legacy-features

Спецификация для устаревших функций *RegExp*, вроде `RegExp.$1а` также `RegExp.prototype.compile` метода.






### Прототипные методы строк `.trimStart()` и `.trimEnd()`

<https://github.com/tc39/proposal-string-left-right-trim>

<sup>*работает в&nbsp;Chrome*</sup>


По&nbsp;аналогии с&nbsp;методами `.padStart()` и `.padEnd()`, обрезают пробельные символы в&nbsp;начале и&nbsp;конце строки соответственно:
```javascript
const one = &quot; hello and let &laquo;;
const two =&nbsp;&laquo;us begin. &raquo;;
console.log( one.trimStart() + two.trimEnd() )
// &laquo;hello and let&nbsp;us begin.&raquo;
```






### .matchAll() &mdash; новый прототипный метод строк.

https://github.com/tc39/proposal-string-matchall

<sup>*работает в&nbsp;Chrome*</sup>

Работает похоже на&nbsp;метод `.match()` с&nbsp;включенным флагом `g`, но&nbsp;возвращает итератор:
```javascript
const string_for_searh = &rsquo;olololo&rsquo;

// Вернёт первое вхождение с&nbsp;дополнительной информацией о&nbsp;нём
string_for_searh.match(/o/)
// [&laquo;o&raquo;, index: 0, input: &laquo;olololo&raquo;, groups: undefined]


//Вернёт массив всех вхождений без дополнительной информации
string_for_searh.match(/o/g)
// [&laquo;o&raquo;, &laquo;o&raquo;, &laquo;o&raquo;, &laquo;o&raquo;]

// Вернёт итератор
string_for_searh.matchAll(/o/)
// {_r: /o/g, _s: &laquo;olololo&raquo;}

// Итератор возвращает каждое последующее вхождение с&nbsp;подробной информацией,
// как если&nbsp;бы мы&nbsp;использовали .match без глобального флага
for(const item of&nbsp;string_for_searh.matchAll(/o/)) {
console.log(item)
}
// [&laquo;o&raquo;, index: 0, input: &laquo;olololo&raquo;, groups: undefined]
// [&laquo;o&raquo;, index: 2, input: &laquo;olololo&raquo;, groups: undefined]
// [&laquo;o&raquo;, index: 4, input: &laquo;olololo&raquo;, groups: undefined]
// [&laquo;o&raquo;, index: 6, input: &laquo;olololo&raquo;, groups: undefined]
```


Аргумент должен быть регулярным выражением, иначе будет выброшено исключение:
```javascript
&rsquo;olololo&rsquo;.matchAll(&rsquo;o&rsquo;)
// Uncaught TypeError: o&nbsp;is&nbsp;not a&nbsp;regexp!
```






### Одномерные массивы с `.flat()` и `.flatMap()`

https://github.com/tc39/proposal-flatMap

<sup>*работает в&nbsp;Chrome*</sup>

Массив обзавёлся прототипами `.flat()` и `.flatMap()`, которые в&nbsp;целом похожи на&nbsp;реализации в&nbsp;*lodash*,
но&nbsp;всё&nbsp;же имеют некоторые отличия. Необязательный аргумент&nbsp;&mdash; устанавливает максимальную глубину обхода дерева:
```javascript
const deep_deep_array = [
&rsquo;&ge;0&nbsp;&mdash; первый уровень&rsquo;,
[
&rsquo;&ge;1&nbsp;&mdash; второй уровень&rsquo;,
[
&rsquo;&ge;2&nbsp;&mdash; третий уровень&rsquo;,
[
&rsquo;&ge;3&nbsp;&mdash; четвёртый уровень&rsquo;,
[
&rsquo;&ge;4&nbsp;&mdash; пятый уровень&rsquo;
]
]
]
]
]

// 0&nbsp;&mdash; вернёт массив без изменений
deep_deep_array.flat(0)
// [&laquo;&ge;0&nbsp;&mdash; первый уровень&raquo;, Array(2)]

// 1&nbsp;&mdash; глубина по&nbsp;умолчанию
deep_deep_array.flat()
// [&laquo;первый уровень&raquo;, &laquo;второй уровень&raquo;, Array(2)]

deep_deep_array.flat(2)
// [&laquo;первый уровень&raquo;, &laquo;второй уровень&raquo;, &laquo;третий уровень&raquo;, Array(2)]

deep_deep_array.flat(100500)
// [&laquo;первый уровень&raquo;, &laquo;второй уровень&raquo;, &laquo;третий уровень&raquo;, &laquo;четвёртый уровень&raquo;, &laquo;пятый уровень&raquo;]
```

`.flatMap()` не&nbsp;эквивалентен последовательному вызову `.flat().map()`. Функция обратного вызова, передаваемая в&nbsp;метод, должна возвращать массив который станет частью общего плоского массива:
```javascript
[&rsquo;Hello&rsquo;, &rsquo;World&rsquo;].flatMap(word =&gt; [...word])
// [&laquo;H&raquo;, &laquo;e&raquo;, &laquo;l&raquo;, &laquo;l&raquo;, &laquo;o&raquo;, &laquo;W&raquo;, &laquo;o&raquo;, &laquo;r&raquo;, &laquo;l&raquo;, &laquo;d&raquo;]
```





---------






## Итоги

*Stage* **4**&nbsp;привнёс скорее косметические изменения. Интерес представляет *Stage*&nbsp;**3**. Большинство из&nbsp;предложений в&nbsp;*Chrome* уже реализованы, за&nbsp;исключением пожалуй `Object.fromEntries()` наличие которого не&nbsp;критично, а&nbsp;приватные свойства очень ждём.




--------



## Исправления в&nbsp;статье

Если заметил в&nbsp;статье неточность, ошибку или есть чем дополнить&nbsp;&mdash; ты&nbsp;можешь написать мне [личное сообщение](https://habr.com/ru/conversations/KasperGreen/), а&nbsp;лучше самому, воспользоваться репозиторием статьи <https://github.com/KasperGreen/es10>


## Материалы по&nbsp;теме
[Актуальная версия стандарта Ecma-262](https://www.ecma-international.org/publications/standards/Ecma-262.htm)

[Черновик следующей версии стандарта Ecma-262](https://tc39.github.io/ecma262/)

[Новые #приватные поля классов в JavaScript](https://medium.com/devschacht/javascripts-new-private-class-fields-c60daffe361b)

[ECMAScript](https://ru.wikipedia.org/wiki/ECMAScript)

[Обзор возможностей стандартов ES7, ES8 и ES9](https://habr.com/ru/company/ruvds/blog/431872/)

[Шебанг](https://ru.wikipedia.org/wiki/%D0%A8%D0%B5%D0%B1%D0%B0%D0%BD%D0%B3_(Unix))
https://habr.com/ru/post/354930/

-------------






![Альтернативный вариант КДПВ с&nbsp;жёлтым магнитом от&nbsp;elfafeya.art](https://habrastorage.org/webt/nt/a4/y7/nta4y72u_f_kgtwon8jio9ghiwg.png)
<sup><cite>Автор фото: kasper.green; Жёлтый магнит: elfafeya.art &amp;&nbsp;kasper.green</cite></sup>
