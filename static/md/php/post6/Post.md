# Введение в лямбды и замыкания

Lambdas и Closures - относительно новые дополнения к PHP после выпуска версии 5.3. Оба предлагают некоторые новые функции и возможность рефакторинга старого кода, чтобы он был более чистым и интуитивно понятным. Однако я думаю, что многие разработчики не знают о Lambdas и Closures или не знают, что они на самом деле делают.

В этом посте я объясню Lambdas и Closures, дам вам несколько примеров кода, чтобы показать их использование, и приведу реальный пример их действия, чтобы показать вам их распространенность в современном PHP.

## Что такое лямбда?

Лямбда - это анонимная функция, которую можно присвоить переменной или передать другой функции в качестве аргумента. Если вы знакомы с другими языками программирования, такими как Javascript или Ruby, вы тогда хорошо знакомы с анонимными функциями.

### Анонимные функции

Анонимная функция - это просто функция без имени. Например, чтобы создать обычную функцию, вы можете написать что-то вроде этого:

```php
// Обычная функция
function speak() {
    return "Hello world";
}
```

Затем вы можете просто вызвать эту функцию следующим образом:

```php
echo speak();
// "Hello world"
```

Анонимная функция не имеет имени, поэтому вы можете определить ее так:

```php
// Анонимная функция
function () {
    return "Hello world";
}
```

## Использование лямбд

Поскольку у функции нет имени, вы не можете вызывать ее как обычную функцию. Вместо этого вы должны либо присвоить его переменной, либо передать другой функции в качестве аргумента.

```php
 // Анонимная функция
 // присвоение переменной
 $func = function () {
     return "Hello world";
 }

// вызов функций
 echo $func();
 // "Hello world"
```

Чтобы использовать анонимную функцию, мы присваиваем ее переменной, а затем вызываем эту переменную как функцию. Вы также можете передать функцию другой функции, например:

```php
// Передача лямбды, в функцию
function speak($msg){
    echo $msg();
}

// вызов функций
speak(function(){
    return "Hello world";
});
```

## Зачем вам использовать лямбду?

Лямбда-выражения полезны, потому что они выбрасывают функции, которые вы можете использовать один раз. Часто для выполнения работы вам понадобится функция, но нет смысла иметь ее в глобальной области или даже делать ее доступной как часть вашего кода. Вместо того, чтобы использовать функцию один раз, а затем оставить ее без дела, вы можете использовать лямбду.

Конечно, вы уже некоторое время можете использовать функцию create_function в PHP. Это в основном делает ту же работу.

```php
// использование create_function
$speak = create_function(”, ‘echo "Hello World!";’);

// вызов функций
$speak();
```

## Что такое замыкание?

Замыкание по сути то же самое, что и лямбда, за исключением того, что оно может обращаться к переменным вне области, в которой оно было создано. Например:

```php
// Создать пользователя
$user = "John";

// Создать замыкание
$speak = function() use ($user) {
echo "Hello $user";
};

// Поприветствовать пользователя
$speak(); // Возвращает "Hello John"
```

Как вы можете видеть выше, Closure может получить доступ к переменной \$user. потому что это было объявлено в предложении use определения функции Closure.

Если вы измените переменную \$user в Closure, это не повлияет на исходную переменную. Чтобы обновить исходную переменную, мы можем добавить амперсанд. Амперсанд перед переменной означает, что это ссылка, поэтому исходная переменная также обновляется.

Например:

```php
// Установить счетчик
$i = 0;
// Увеличение счетчика в в области видимости функции
$closure = function () use ($i){ $i++; };
// Запустить функцию
$closure();
// Глобальный count не изменился
echo $i; // Возвращает 0

// Сбросить счетчик
$i = 0;
// Увеличивает счетчик в области видимости функции, но передать его как ссылку
$closure = function () use (&$i){ $i++; };
// Запустить функцию
$closure();
// Глобальный счетчик увеличился
echo $i; // Возвращает 1

```

Closures are also useful when using PHP functions that accept a callback function like array_map, array_filter, array_reduce or array_walk.

The array_walk function takes an array and runs it through the callback function.

```php
// An array of names
$users = array("John", "Jane", "Sally", "Philip");

// Pass the array to array_walk
array_walk($users, function ($name) {
echo "Hello $name<br>";
});
// Returns
// -> Hello John
// -> Hello Jane
// -> ..
```

Again, you can access variables outside the scope of the Closure by using the use clause:

```php
// Set a multiplier
 $multiplier = 3;

// Create a list of numbers
 $numbers = array(1,2,3,4);

// Use array_walk to iterate
 // through the list and multiply
 array_walk($numbers, function($number) use($multiplier){
 echo $number * $multiplier;
 });
```

In the example above, it probably wouldn’t make sense to create a function to just multiply two numbers together. If you were to create function to do a job like this, then come back to the code a while later you will probably be thinking why did you bother create a globally accessible function only to be used once? By using a Closure as the callback, we can use the function once and then forget about it.

## Real life usage

So we’ve established that Lambdas and Closures are anonymous functions that can be used as throw away bits of functionality that don’t pollute the global namespace and are good to use as part of a callback.

A popular example of the use of these types of functions is in routing requests within modern frameworks. Laravel for example, allows you to do the following:

```php
Route::get(‘user/(:any)’, function($name){
    return "Hello " . $name;
});
```

The code above simply matches a URL like /user/philip and returns a greeting.

This is a very basic example, but it highlights how a Closure can be utilised in a very useful situation.

## Conclusion

So hopefully that was a good explanation of Lambdas and Closures.

Lambdas and Closures seem like two very deep Computer Science terms if you are a newbie programmer. However, it’s actually not that complicated at all. Both Lambdas and Closures are simply anonymous functions that are useful for one offs or where it doesn’t make sense to define a function.

Lambdas and Closures are fairly new to PHP and they don’t follow exactly the same usage as in other languages. If you are at all familiar with Javascript, you will see anonymous functions used a lot. In particular, you will see a lot of good examples in jQuery. Once you can recognise the pattern, it makes reading code a lot easier because you not only understand what is going on, but also understand why it was written like that in the first place and what the developer was trying to achieve through her decisions.
