# Введение в PHP Reflection API

# Введение

Когда я начинал кодировать PHP, я не знал о мощи Reflection API, и основная причина заключалась в том, что он мне не понадобился для разработки моего простого класса, модуля или даже моего пакета, затем я начал находить его во многих областях, в которой он играл главную роль. Представляем вам Reflection API.

1. Что такое Reflection API.
2. Установка и настройка.
3. Использование.
4. Вывод.
5. Ссылки.

## Что такое Reflection API

> Информация из Википедий: отражение - это способность компьютерной программы исследовать, анализировать и изменять свою собственную структуру и поведение во время выполнения.

Точка, на которой вы можете встать и заглянуть в свой код
(реверс-инжиниринг), давайте проверим следующий фрагмент кода:

```php
/**
 * Class Profile
 */
class User {
   /**
    * @return string
    */
   public function getUserName(): string
   {
      return 'John';
   }
}
```

Теперь класс User представляет собой черный ящик, используя ReflectionClass, вы можете прочитать, что внутри него:

```php
// Создание объекта
$reflectionClass = new ReflectionClass('User');

// Получаем имя класса
var_dump($reflectionClass->getName());
=> output: string(7) "User"

// получаем документацию по классу
var_dump($reflectionClass->getDocComment());
=> output:
string(24) "/**
 * Class User
 */"
```

Итак, ReflectionClass подобен аналитику для нашего класса User, и это основная идея Reflection API.

PHP дает вам ключ к любому заблокированному ящику, поэтому у нас есть ключи для следующего:

[ReflectionClass](http://php.net/manual/en/class.reflectionclass.php): сообщает информацию о классе.

[ReflectionFunction](http://php.net/manual/en/class.reflectionfunction.php): сообщает информацию о функции.

[ReflectionParameter](http://php.net/manual/en/class.reflectionparameter.php): извлекает информацию о параметрах функции или метода.

[ReflectionClassConstant](http://php.net/manual/en/class.reflectionclassconstant.php): сообщает информацию о константе класса.

Ознакомьтесь с полным списком на [php.net](http://php.net/manual/ru/book.reflection.php).

## Установка и настройка

Чтобы использовать все классы Reflection API, установка или настройка не требуются, они входят в состав ядра.

## Использование

Здесь у нас есть несколько примеров, как мы можем использовать Reflection API:

Пример 1: Получить родительский класс для определенного класса:

```php
// здесь мы имеем дочерний класс
class Child extends User {
}

$class = new ReflectionClass('Child');

// здесь мы получаем список всех родителей
print_r($class->getParentClass()); // ['User']
```

Пример 2: Получаем документацию по функции getUserName():

```php
$method = new ReflectionMethod('User', 'getUserName');
var_dump($method->getDocComment());
=> вывод:
string(33) "/**
 * @return string
 */"
```

Пример 3: Он может действовать как instanceof и is_a() для проверки объектов:

```php
$class = new ReflectionClass('Profile');
$obj   = new Profile();
var_dump($class->isInstance($obj)); // bool(true)
// аналогичен
var_dump(is_a($obj, 'User')); // bool(true)
// аналогичен
var_dump($obj instanceof Profile); // bool(true)
```

Пример 4: В некоторых ситуациях вы обнаружите, что застряли в модульном тестировании и задаетесь вопросом, как я могу протестировать private функции ?! Не беспокойтесь, вот решение:

```php
// сделать область видимости getUserName () private
private function getUserName(): string
{
    return 'Foo';
}
$method = new ReflectionMethod('User', 'getUserName');
// проверим, является ли он private, поэтому мы устанавливаем его как доступный
if ($method->isPrivate()) {
    $method->setAccessible(true);
}
echo $method->invoke(new User()); // Foo
```

Предыдущие примеры довольно просты, вот некоторые области, в которых вы можете найти применение Reflection API:

1. Генератор документации: пакет laravel-apidoc-generator, он широко использует ReflectionClass и ReflectionMethod для получения информации о классах и блоках документации методов, а затем их обработки и проверки этого блока кода.
2. Контейнер внедрения зависимостей.

## Вывод

PHP предоставляет богатый API отражения, который упрощает доступ к любой области структур ООП.

## Ссылки

[http://php.net/manual/ru/book.reflection.php](http://php.net/manual/ru/book.reflection.php)
