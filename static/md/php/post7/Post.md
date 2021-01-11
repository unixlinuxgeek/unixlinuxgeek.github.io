# Микросервисы PHP - создание базового Restful Crud API

Один из самых распространенных способов создания доступа к микросервису сегодня - это RESTful API. REST имеет большие преимущества перед другими протоколами, такими как сокеты, которые включают:

1. Неблокирующий ввод-вывод для клиента при реализации на таких языках, как Javascript
2. Легко управлять ресурсами, а это означает, что вызовы легко организовывать и группировать вместе.
3. За синтаксисом легко следить, поскольку он соответствует стандартам HTTP.
4. Конечные точки не имеют состояния, то есть идемпотент. Это уменьшает побочные эффекты неожиданных выходов.
5. Легко реализовать операции CRUD - создание, чтение, обновление, удаление.

Restful over HTTP обычно разбивается на 4 типа запросов GET, POST, PUT и DELETE. Есть и другие типы запросов, которые выходят за рамки этой статьи. Запросы можно описать как:

GET: Normally maps to an operation of retrieving data. In CRUD, this is your READ.

-   GET: обычно используется для операций извлечения данных. В CRUD это ваша READ.
-   POST: используется для создания данных или в CRUD CREATE.
-   PUT: вызывается, когда пользователь хочет обновить данные. UPDATE в CRUD.
-   DELETE: используется для удаления данных. И наконец, DELETE в CRUD.

В этом руководстве мы собираемся создать небольшое приложение, которое выполняет эти 4 метода и представляет, как мы можем предоставить доступ к микросервису. В этой статье НЕ будут рассматриваться такие области, как безопасность или контроль доступа.

## Запустите код

Код этого руководства доступен по адресу: https://github.com/ProdigyView-Toolkit/Microservices-Examples-PHP

## Создание клиента

Клиенты могут соединяться с Restful через HTTP-запросы. Эти запросы могут поступать из нескольких источников, от Javacript в браузере до запроса CURL с сервера. В наших примерах наш запрос будет поступать с сервера.

Для начала, вот основной запрос CURL с использованием библиотеки curl ProdigyView.

```php
$curl = new Curl($host);
$curl-> send('get');
echo $curl->getResponse();
```

Это отправит запрос GET в определенную конечную точку, указанную в переменной \$host. Если мы хотим добавить данные в наш запрос, мы можем просто сделать это, передав массив.

```php
$data = array('id' => 1);
$curl = new Curl($host);
$curl->send('get', $data);
echo $curl->getResponse();
```

Очень просто. И POST, PUT и DELETE работают одинаково. Например, запрос PUT.

```php
$curl = new Curl($host);
$curl->send('put',array('id' => '2', 'title' => 'All About Me'));
echo $curl->getResponse();
```

И это все! Это относительно простой клиент для выполнения запросов. Полный пример ниже, прежде чем мы создадим наш сервер:

```php
<?php
include('vendor/autoload.php');

use prodigyview\network\Curl;


$host = '127.0.0.1:8080/server.php';

echo "\nStarting RESTFUL Tests\n\n";

//Получаем все истории
$curl = new Curl($host);
$curl-> send('get');
echo $curl->getResponse();
echo "\n\n";

//Получаем одиночную запись
$curl = new Curl($host);
$curl-> send('get', array('id'=>1));
echo $curl->getResponse();
echo "\n\n";

//Неудачный POST
$curl = new Curl($host);
$curl->send('post',array('POST' => 'CREATE'));
echo $curl->getResponse();
echo "\n\n";

//Успешно создаем запись
$curl = new Curl($host);
$curl-> send('post',array('title' => 'Robin Hood', 'description' => 'Steal from the rich and give to me.'));
echo $curl->getResponse();
echo "\n\n";

//Успешно обновляем историю
$curl = new Curl($host);
$curl-> send('put',array('id' => '2', 'title' => 'All About Me'));
echo $curl->getResponse();
echo "\n\n";

//Неудачная попытка удаления
$curl = new Curl($host);
$curl-> send('delete',array('delete' => 'me'));
echo $curl->getResponse();
echo "\n\n";

//Запись успешно удалена
$curl = new Curl($host);
$curl-> send('delete',array('id' => 1));
echo $curl->getResponse();
echo "\n\n";
```

### Restful сервер

Теперь мы можем перейти к нашему Restful Microservice. В нашем сервисе мы представим, что это база данных статей, которыми нужно манипулировать. Чтобы было ясно, REST НЕ является микросервисом, но предоставляет доступ к микросервису.

Например, в этом руководстве мы только обновляем статьи. Если бы мы хотели, мы могли бы создать совершенно другой API RESTFUL для другой службы, которая имеет свои собственные ресурсы (базы данных, серверы и т. д.) Для изменения информации о пользователях, но все еще использует REST и CRUD. Поскольку этот новый API будет изменять только пользователей и будет иметь свои собственные ресурсы, он будет рассматриваться как микросервис.

Переходя к коду, давайте сначала создадим фиктивные данные в качестве примера «базы данных».

```php
<?php
//Создаем фиктивные данные
$articles = array(
	1 => array('title' => 'Little Red Riding Hood', 'description' => 'A sweet innocent girl meets a werewolf'),
	2 => array('title' => 'Snow White and the Seven Dwarfs', 'description' => 'A sweet girl, a delicious apple.'),
	3 => array('title' => 'Gingerbread Man', 'description' => 'A man who actively avoids kitchens .'),
);
```

Отлично! Теперь нам нужно обработать входящий запрос. Все это обрабатывается в объекте запроса ProdigyView, который обрабатывает текущий запрос HTTP и извлекает метод и данные.

```php
//Создаем и обработываем текущий запрос
$request = new Request();
//Получаем метод запроса (GET, POST, PUT, DELETE)
$method = strtolower($request->getRequestMethod());
//ПОЛУЧАЕМ данные из запроса
$data = $request->getRequestData('array');
```

Далее нам нужен роутер. Маршрутизатор решит, как обрабатывать наши данные, в зависимости от типа запроса (GET, PUT, POST, DELETE).

```php
//Маршрутизируем запрос
if ($method == 'get') {
  get($data);
} else if ($method == 'post') {
  post($data);
} else if ($method == 'put') {
  parse_str($data,$data);
  put($data);
} else if ($method == 'delete') {
  parse_str($data,$data)
  delete($data);
}
```

Обратите внимание, что выше для PUT и DELETE мы используем parse_str ($data, $data). Это связано с тем, что запросы PUT и DELETE возвращаются как строки, а не как массивы. Строковые данные будут иметь вид id = 1. Мы должны превратить эти данные в массив с помощью функции parse_str.
И, наконец, мы пишем несколько функций для обработки данных. В качестве примера приведем функцию для обработки запроса GET.

```php
<?php
/**
 * Обработаем все данные GET, чтобы найти информацию
 */
function get($data) {
	global $articles;

	$response  = array();

	if(isset($data['id']) && isset($articles[$data['id']])) {
		$response = $articles[$data['id']];
	} else {
		$response = $articles;
	}

	echo Response::createResponse(200, json_encode($response));
	exit();
};
```

Функция выше ищет идентификатор. Если идентификатор найден, он возвращает эту статью. Если он не найден, он возвращает все статьи. GET в CRUD должен только читать информацию, не изменяя ее. И наш ответ:

```php
echo Response::createResponse(200, json_encode($response));
```

Response - это инструмент ProdigyView для создания валидного HTTP-ответа. При этом мы кодируем данные ответа в формате JSON, которые будут возвращены нашему клиенту, определенному выше.
Распространенное заблуждение: иногда считают, что REST должен возвращать JSON. Это неправда, REST может возвращать данные в любом формате клиенту. Например, мы могли бы вернуть ответ в XML, если бы захотели.
Полный пример сервера здесь:

```php
<?php
include ('vendor/autoload.php');

use prodigyview\network\Request;
use prodigyview\network\Response;

//Создать фиктивные данные
$articles = array(
	1 => array('title' => 'Little Red Riding Hood', 'description' => 'A sweet innocent girl meets a werewolf'),
	2 => array('title' => 'Snow White and the Seven Dwarfs', 'description' => 'A sweet girl, a delicious apple.'),
	3 => array('title' => 'Gingerbread Man', 'description' => 'A man who actively avoids kitchens.'),
);

//Создать и обработываем текущий запрос
$request = new Request();

//Получаем метод запроса (GET, POST, PUT, DELETE)
$method = strtolower($request->getRequestMethod());

//ПОЛУЧАЕМ данные из запроса
$data = $request->getRequestData('array');

//Маршрутизируем запрос
if ($method == 'get') {
	get($data);
} else if ($method == 'post') {
	post($data);
} else if ($method == 'put') {
	parse_str($data,$data);
	put($data);
} else if ($method == 'delete') {
	parse_str($data,$data);
	delete($data);
}

/**
 * Обработываем все данные GET, чтобы найти информацию
 */
function get($data) {
	global $articles;

	$response  = array();

	if(isset($data['id']) && isset($articles[$data['id']])) {
		$response = $articles[$data['id']];
	} else {
		$response = $articles;
	}

	echo Response::createResponse(200, json_encode($response));
	exit();
};

/**
 * Обработываем всех данные POST для создания данных
 */
function post($data) {
	global $articles;

	$response  = array();

	if(isset($data['title']) && isset($data['description'])) {
		$articles[] = $data;
		$response = array('status' => 'Article Successfully Added');
	} else {
		$response = array('status' => 'Unable To Add Article');
	}

	echo Response::createResponse(200, json_encode($response));
	exit();

};

/**
 * Обработываем все данные PUT для обновления информации
 */
function put($data) {
	global $articles;

	$response  = array();

	if(isset($data['id']) && isset($articles[$data['id']])) {
		if(isset($data['title'])) {
			$articles[$data['id']]['title'] = $data['title'];
		}

		if(isset($data['description'])) {
			$articles[$data['id']]['description'] = $data['description'];
		}
		$response = array('status' => 'Article Successfully Updated', 'content' => $articles[$data['id']]);
	} else {
		$response = array('status' => 'Unable To Update Article');
	}

	echo Response::createResponse(200, json_encode($response));
	exit();

};

/**
 * выполняем DELETE для удаления данных
 */
function delete($data) {
	global $articles;

	$response  = array();

	if(isset($data['id']) && isset($articles[$data['id']])) {
		unset($articles[$data['id']]);
		$response = array('status' => 'Article Deleted', 'content' => $articles);
	} else{
		$response = array('status' => 'Unable To Update Article');
	}

	echo Response::createResponse(200, json_encode($response));
	exit();

};
```

## Подведение итогов

Это основы создания RESTFul CRUD API для подключения к микросервису. В производственной среде он будет иметь больше функций, таких как безопасность, кеширование, контроль доступа, и, вероятно, будет находиться внутри MVC. API-интерфейсы REST также могут передавать данные другим микросервисам для выполнения своих задач.
Повторим, что REST существует как в монолитных приложениях, так и в микросервисах. Мы можем эффективно использовать REST и CRUD, разделяя наши операции на различные сервисы со своими собственными ресурсами и предоставляя доступ к ним через наш REST API.
