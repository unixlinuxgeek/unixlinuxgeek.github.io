# Локальная разработка с помощью Docker Compose

В этом посте мы создадим небольшой проект с помощью Docker Compose и настроим его для локальной
разработки.

## Что такое Docker Compose

Docker-compose - это инструмент, который используется для многоконтейнерных приложений работающие на одном хосте. Мы запускаем все контейнеры как службы одной командой.
Пошаговая инструкция:

- Установите Docker Compose если он не установлен
- Определите Dockerfile для службы / контейнера
- Определите файл docker-compose.yaml со всеми службами, портами и другими деталями
- Запустите команду docker-compose up -d

## Список часто используемых комонд docker-compose:

<table class="t-2">
  <thead>
    <tr>
      <th>Команда</th>
      <th>Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>docker-compose up</td>
      <td>Создание и запуск контейнеров. Можете остановить с помощью комбинации клавиш CTRL+C</td>
    </tr>
    <tr>
     <tr>
      <td>docker-compose up -d</td>
      <td>Создание и запуск контейнеров в фоновом режиме</td>
    </tr>
    <tr>
      <td>docker-compose down</td>
      <td>Остановливает и удаляет контейнеры, сети, образы и тома</td>
    </tr>
    <tr>
      <td>docker-compose restart</td>
      <td>Перезапускает сервисы</td>
    </tr>
    <tr>
      <td>docker-compose stop</td>
      <td>Остановливает сервисы</td>
    </tr>
    <tr>
      <td>docker-compose ps</td>
      <td> Выводит список контейнеров</td>
    </tr>
    <tr>
      <td>docker-compose exec</td>
      <td>Выполнить команду в работающем контейнере</td>
    </tr>
    <tr>
      <td>docker-compose --help</td>
      <td>Справка по командам</td>
    </tr>
  </tbody>
</table>
