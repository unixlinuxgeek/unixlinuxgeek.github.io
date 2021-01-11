# Локальная разработка с помощью Docker Compose

В этом посте мы создадим небольшой проект с помощью Docker Compose и настроим его для локальной
разработки.

## Что такое Docker Compose

Docker-compose - это инструмент, который используется для многоконтейнерных приложений работающие на одном хосте. Мы запускаем все контейнеры как службы одной командой.
Пошаговая инструкция:

- Установите Docker Compose если он не установлен
- Определите Dockerfile для службы / контейнера
- Определите файл docker-compose.yml со всеми службами, портами и другими деталями
- Запустите команду docker-compose up -d

## Список часто используемых комонд docker:

<table class="t-2">
  <thead>
    <tr>
      <th>Команда</th>
      <th>Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>docker ps -a</td>
      <td>Показывает все контейнеры (остановленые и работающие)</td>
    </tr>
    <tr>
      <td>docker exec -it <container id> /bin/ash</td>
      <td>Запускает команду /bin/ash в работающем контейнере</td>
    </tr>
    <tr>
      <td>docker image ls -a</td>
      <td>Показывает все образы (ключ -a отображает промежуточные изображения)</td>
    </tr>
    <tr>
      <td>docker rmi -f <image id></td>
      <td>Удаляет образы в принудительном режиме</td>
    </tr>
    <tr>
      <td>docker rm <container id></td>
      <td>Удаляет контайнер</td>
    </tr>
    <tr>
      <td>docker network ls</td>
      <td>Список сетей, о которых знает демон Engine. Сюда входят сети, охватывающие несколько хостов в кластере.</td>
    </tr>
  </tbody>
</table>

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
      <td>docker ps -a</td>
      <td>Показывает все контейнеры (остановленые и работающие)</td>
    </tr>
    <tr>
      <td>docker exec -it <container id> /bin/ash</td>
      <td>Запускает команду /bin/ash в работающем контейнере</td>
    </tr>
    <tr>
      <td>docker image ls -a</td>
      <td>Показывает все образы (ключ -a отображает промежуточные изображения)</td>
    </tr>
    <tr>
      <td>docker rmi -f <image id></td>
      <td>Удаляет образы в принудительном режиме</td>
    </tr>
    <tr>
      <td>docker rm <container id></td>
      <td>Удаляет контайнер</td>
    </tr>
    <tr>
      <td>docker network ls</td>
      <td>Список сетей, о которых знает демон Engine. Сюда входят сети, охватывающие несколько хостов в кластере.</td>
    </tr>
  </tbody>
</table>
