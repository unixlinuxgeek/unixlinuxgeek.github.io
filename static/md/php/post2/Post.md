# Что нового в PHP 7.1

## Обнуляемые типы

---

Типы для параметров и возвращаемых значений могут быть помечены как обнуляемые путём добавления префикса в виде знака вопроса. Это означает, что указанные параметры и возвращаемые значения, могут быть как указанного типа, так и null.

```php
function getTitle(): ?string
{
    return 'Main Page';
}
var_dump(getTitle());
function getDesc(): ?string
{
    return null;
}
var_dump(getDesc());
```

Выведет:

```php
string(9) "Main Page"
NULL

```

## Ничего не возвращающие функции

---

Был добавлен тип возвращаемого значения void. Функции с таким заданным типом возвращаемого значения не должны ничего возвращать. То есть либо вообще не содержать ни одного оператора return, либо использовать его без параметра. null не является корректным значением для возврата в таких функциях.

```php
function indexPage() : void {
    return;
    // Ok
}

function homePage() : void {
    // Ok
}

function aboutPage() : void {
    return true;
    // Fatal Error...
}
```

## Симметричная деструктуризация массива

---

Можно использовать короткий синтаксис ([]) для деструктуризации массивов с целью присвоения (в том числе в foreach), как альтернатива функции list().

```php
$data = [
    [1, 'John'],
    [2, 'David'],
];

// используя list()
list($id1, $name1) = $data[0];

// используя []
[$id1, $name1] = $data[0];

// используя list()
foreach ($data as list($id, $name)) {
    // код, содержащий $id и $name
}

// используя []
foreach ($data as [$id, $name]) {
    // код, содержащий $id и $name
}
```

## Видимость констант класса

---

Добавлена поддержка определения области видимости для констант класса.

```php

class Nums {
    // Начиная с PHP 7.1.0
    public const ONE = 'one';
    private const TWO = 'two';
}
echo Nums::ONE, PHP_EOL;
echo Nums::TWO, PHP_EOL;
// Вывод
// one
// Fatal error: Uncaught Error: Cannot access private const Nums::TWO in …
```

## Псевдотип iterable

---

Был добавлен новый псевдотип (похожий на callable), названный iterable. Он может использоваться как параметр, так и в качестве возвращаемого значения там, где используется массив или объект, реализующий интерфейс Traversable. Что касается подтипов, типы параметров из дочерних классов могут расширить декларацию родителей типа array или Traversable до iterable. Для типов возврата, дочерние классы могут сужать тип возвращаемого значения с iterable до array или объекта реализующего Traversable.

```php
function iterator(iterable $iter)
{
    foreach ($iter as $val) {
        //
    }
}
```

## Обработка нескольких исключений в одном блоке catch

---

В блоке catch теперь можно обрабатывать несколько исключений, перечисляя их через символ вертикальной черты (|). Это может быть полезно, если различные исключения обрабатываются одинаково.

```php
try {
    // Какой то код
} catch (FirstException | SecondException $e) {
    // Обрабатываем оба исключения
}
```

## Поддержка ключей в list()

---

Теперь вы можете указывать ключи в операторе list() или в его новом коротком синтаксисе []. Это позволяет деструктурировать массивы с нечисловыми или непоследовательными ключами.

```php
$data = [
    ["id" => 1, "name" => 'Tom'],
    ["id" => 2, "name" => 'Fred'],
];

// стиль list()
list("id" => $id1, "name" => $name1) = $data[0];

// стиль []
["id" => $id1, "name" => $name1] = $data[0];

// стиль list()
foreach ($data as list("id" => $id, "name" => $name)) {
    // logic here with $id and $name
}

// стиль []
foreach ($data as ["id" => $id, "name" => $name]) {
    // logic here with $id and $name
}
```

## Поддержка отрицательных смещений для строк

---

Поддержка отрицательных смещений для строк добавлена в функции для работы со строками, а также в индексацию строк с помощью [] или {}. В этих случаях отрицательные смещения интерпретируются как смещения относительно конца строки.

```php
var_dump("abcdef"[-2]);
var_dump(strpos("aabbcc", "b", -3));
```

Вывод:

```php
string (1) "e"
int(3)
```

## Преобразование callable в Closure с помощью Closure::fromCallable()

---

В класс Closure добавлен новый статический метод для возможности легко преобразовать callable в объекты типа Closure.

```php
class Util
{
    public function exposeFunction()
    {
        return Closure::fromCallable([$this, 'privateFunction']);
    }

    private function privateFunction($param)
    {
        var_dump($param);
    }
}

$privFunc = (new Util)->exposeFunction();
$privFunc('значение');
```

Вывод:

```php
string(16) "значение"

```

## Асинхронная обработка сигналов

---

Новая функция pcntl_async_signals() была добавлена для разрешения асинхронной обработки сигналов без использования тиков (которые производят много накладных расходов).

```php
pcntl_async_signals(true); // включает асинхронные сигналы

pcntl_signal(SIGHUP,  function($sig) {
    echo "SIGHUP\n";
});

posix_kill(posix_getpid(), SIGHUP);
```

Вывод:

```php
SIGHUP
```

## Поддержка HTTP/2 server push в ext/curl ¶

---

Поддержка "server push" добавлена в расширение CURL (требуется версия 7.46 и выше). Использовать можно в функции curl_multi_setopt() с новой константой CURLMOPT_PUSHFUNCTION. Также добавлены константы CURL_PUSH_OK и CURL_PUSH_DENY для определения, был ли принят или отклонён "server push".
