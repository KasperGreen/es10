![КДПВ: Жёлтый магнит с надписью «JS ES10» на экране монитора —  от kasper.green & elfafeya.art](https://habrastorage.org/webt/nt/a4/y7/nta4y72u_f_kgtwon8jio9ghiwg.png)
        <cite>Фото: kasper.green; Жёлтый магнит: elfafeya.art & kasper.green</cite>

# Stage 4 — Final

•      **```catch```** — аргумент стал необязательным;

•      **```Symbol().description```** — акцессор к описанию символа;

•      **```'строки EcmaScript'```** — улучшеная совместимость с **JSON** форматом;

•      **```.toString()```** — прототипный метод обновлён.

# Stage 3 — Pre-release

•      **```#```** —  приватное всё у классов, через октоторп;

•      **```#!/usr/bin/env node```**  — шебанг грамматика для скриптов;

•      **```BigInt()```** — новый примитив, для чисел произвольной точности;

•      **```globalThis```** — новый способ доступа к глобальному контексту;

•      **```import(dynamic)```** — динамический импорт;

•      **```import.meta```** — мета-информация о загружаемом модуле;

•      **```Object.fromEntries()```** — создание объекта через массив пар — ключ\значение;

•      **```RegExp```** — устаревшие возможности;

•      **```.trimStart()```** и **```.trimEnd()```** — прототипные методы строк;

•      **```.matchAll()```** — **```.match()```** с глобальным флагом;

•      **```.flat()```** и **```.flatMap()```** — прототипные методы массивов.



<cut />

----------------------

### Всего пять стадий:

   <sub>*Stage*</sub> **0**   ↓  &thinsp;**Strawman**  <sup>**Наметка**</sup>          &thinsp;Идея которую можно реализовать через **Babel**-плагин.;

   <sub>*Stage*</sub> **1**   ↓  &thinsp;**Proposal**  <sup>**Предложение**</sup>    &thinsp;Проверка жизнеспособности идеи.;

   <sub>*Stage*</sub> **2**   ↓  &thinsp;**Draft**  <sup>**Черновик**</sup>                 &thinsp;Начало разработки спецификации.;

   <sub>*Stage*</sub> **3**   ↓  &thinsp;**Candidate**  <sup>**Кандидат**</sup>         Предварительная версия спецификации.;

   <sub>*Stage*</sub> **4**  ֍  **Finished**  <sup>**Завершён**</sup>          &thinsp;Финальная версия спецификации на этот год.

----------------------------

Мы рассмотрим только Stage 4 — дефакто вошедший в стандарт.
И Stage 3 — который вот-вот станет его частью.

----------------------------


# Stage 4

Эти изменения уже вошли в стандарт.

---------


### Необязательный аргумент у catch

<https://github.com/tc39/proposal-optional-catch-binding>

До ES10 блок ```catch``` требовал обязательного аргумента, для сбора информации об ошибке, даже если она не используется:

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

Начиная с редакции ES10, круглые скобки можно опустить и ```catch``` стенет похож на ```try```:
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
