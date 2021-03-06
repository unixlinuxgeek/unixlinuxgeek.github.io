# Что нового в php 7.3

## Новые возможности и функции

- В ядре PHP: более гибкий синтаксис Heredoc и Nowdoc, деструктурирование массива поддерживает присвоение по ссылкам, оператор instanceof принимает литералы, исключение CompileError вместо некоторых ошибок компиляции, в вызовах разрешена завершающая запятая, поддержка Argon2id.
- В функции BC Math
- В облегченном протоколе доступа к каталогам (LDAP)
  -- В функции мультибайтовых строк: полная поддержка case-mapping и case-folding, нечувствительные к регистру операции используют case-folding, MB_CASE_TITLE выполняет преобразование title-Case,поддержка Unicode 11, поддержка больших строк, улучшение производительности, поддержка именованных фрагментов.
- В Readline
- В дате и времени
- В вычислениях с увеличенной точностью (GNU Multiple Precision)
- В функциях интернационализации
- В облегченном протоколе доступа к каталогам (LDAP)
- В OpenSSL
- В сокетах

## Новые глобальные константы

- В ядре PHP
- В библиотеке Client URL (cURL)
- В фильтрации данных
- В объектной нотации JavaScript (JSON)
- В облегченном протоколе доступа к каталогам (LDAP)
- В мультибайтовых строках
- В OpenSSL
- В PostgreSQL

## Функционал, объявленный устаревшим в PHP 7.3.x

- В ядре PHP: нечуствительные к регистру константы, использование assert() внутри пространств имен, поиск строк для нестрочного параметра needle, изменения в удалении тегов.
- В фильтрации данных
- В обработке изображений и GD
- В функции интернационализации
- В мультибайтовых строках
- В функции ODBC и DB2 (PDO_ODBC)

## Прочие изменения

- В ядре PHP: Функция set(raw)cookie принимает аргумент $option, Новые ini-директивы syslog, Сборщик мусора и т.д.
- В интерактивном отладчике PHP
- В библиотеке Client URL (cURL)
- В фильтрации данных
- В функции интернационализации
- В объектной нотации JavaScript (JSON)
- В мультибайтовых строках
- В FTP, ODBC (Unified), OPcache, OpenSSL, Tidy, XML-парсере, Zip и Сжатие Zlib
- В регулярных выражениях (совместимые с Perl)
- В Microsoft SQL Server и функции Sybase (PDO_DBLIB)
- В функции SQLite (PDO_SQLITE)
- В обработке сессий
