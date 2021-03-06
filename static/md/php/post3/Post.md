# Что нового в PHP 7.2

В этой статье мы обсудим некоторые новые части языка PHP, выпущенные во время его последней итерации, такие как улучшенная безопасность.

Как мы все знаем, большинство веб-разработчиков страстно любят или ненавидят PHP. Я один из тех разработчиков, которым PHP нравится. Я знаю PHP как свои пять пальцев. Теперь, когда вышла версия 7.2, мне очень интересно какие особенности принесет новейшая версия.

## Самое важное это Безопасность

Версия 7.2 предлагает некоторые очень необходимые улучшения безопасности.

## Libsodium - теперь это часть ядра

Криптографическая библиотека прикладного уровня Libsodium теперь является частью ядра PHP 7.2. Ранее библиотека была доступна через PECL.

Включение Libsodium делает PHP первым языком программирования, который добавляет современную криптографию в свою стандартную библиотеку. Это гарантирует, что кроссплатформенная и кросс-языковая библиотека обеспечит шифрование, дешифрование, подписи, хеширование паролей и многое другое.

## Аргон 2 в хеш-пароле

Argon 2 - это отмеченный наградами алгоритм хеширования. Он победил в конкурсе по хешированию паролей 2015 года, предоставив безопасную альтернативу алгоритму Bcrypt в предыдущей версии PHP. Он разработан для максимальной скорости заполнения памяти и эффективного использования нескольких вычислительных блоков, при этом обеспечивая защиту от атак компромисса.

## Производительность

Согласно тестам Phoronix, PHP 7.2 работает на 13% быстрее, чем 7.1, и на 20% быстрее, чем 7.0. Он на 250% быстрее, чем PHP 5.6, с которого до сих пор не обновились более 40% пользователей WordPress. Другие тесты подтверждают эти выводы. Официальные тесты PHP показывают, что PHP 7 в два раза быстрее, чем 5.6, с половиной задержки.

## Deprecations

Как и в каждом обновлении, есть несколько устаревших функций и возможностей, которые будут удалены не позднее PHP 8.0. Полный список устаревших функций можно найти здесь. Эти функции будут работать в PHP 7.2, но сообщения об ошибках будут появляться во время использования в файлах журнала. Разработчики должны проверять код, чтобы обновить устаревшие функции, прежде чем они станут обратно несовместимыми.

## Поддержка

Поддержка безопасности PHP 7.0 завершилась 3 декабря 2017 года. Критическая поддержка будет доступна до конца 2018 года, но сообщество PHP больше не предоставляет поддержку для устранения ошибок или мелких проблем. PHP 7.1 последует этому примеру 1 декабря 2018 года. Обновление до 7.2 гарантирует, что последние обновления безопасности будут постоянно поддерживаться сообществом.

## Улучшенные возможности языка программирования

Добавлено новое предупреждение при вызове функции count() с параметром, который является скаляром, параметром с нулевым значением или объектом, который не реализует интерфейс Countable.

Typehints(Контроль типа) объекта исправляют ситуацию, в которой разработчик не может объявить функцию, которой необходимо передать объект в качестве параметра, или объявить, что функция должна возвращать объект. Исправление использует объект как тип параметра и как возвращаемый тип.

Преобразование числовых ключей в приведения object/array решает проблему с Zend Engine, на котором работает PHP 7. У этого механизма есть случаи, когда хеш-таблицы массивов могут содержать числовые строки, а хэш-таблицы объектов могут иметь целочисленные ключи. В таких случаях код PHP не может найти ключи. С исправлением в PHP 7.2 ключи массивов или хеш-таблиц объектов преобразуются соответствующим образом, поэтому числовые строковые имена свойств в объектах становятся ключами целочисленных массивов и наоборот, решая проблему недоступных свойств.
