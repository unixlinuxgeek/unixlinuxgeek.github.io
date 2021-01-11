# Шпаргалка Docker.

## Команды управления контейнером.

<table class="t-1 w40">
  <thead>
    <tr>
      <th>Команда</th>
      <th>Описание</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>docker exec -it <container id> /bin/ash</td>
      <td>Запускает команду /bin/ash в работающем контейнере (Alpine Linux)</td>
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
      <td>docker network ls</td>
      <td>Список сетей, о которых знает демон Engine. Сюда входят сети, охватывающие несколько хостов в кластере.</td>
    </tr>
    <tr>
      <td>docker network create --driver=bridge --subnet=172.16.0.0/16 --gateway=172.16.0.1 network_name</td>
      <td>
      Создает новую сеть. Bridge являются встроенными сетевым драйвером. 
      Bridge сеть использует программный мост, который позволяет контейнерам, подключенным к одной и той же bridge сети, обмениваться данными, обеспечивая изоляцию от контейнеров, которые не подключены к этой сети моста.
      </td>
    </tr>
    <tr>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>docker create image [ command]</td>
      <td>создать контейнер</td>
    </tr>
     <tr>
      <td>docker run image... </td>
      <td>create + start</td>
    </tr>
     <tr>
      <td>docker start container... </td>
      <td>запустить контейнер </td>
    </tr>
     <tr>
      <td>docker stop container...</td>
      <td>остановить контейнер </td>
    </tr>
     <tr>
      <td>docker kill container...</td>
      <td>уничтожить (SIGKILL) контейнер</td>
    </tr>
    <tr>
      <td>docker restart container...</td>
      <td>stop + start</td>
    </tr>
      <tr>
        <td>docker pause container...</td>
        <td>приостановить работу контейнера</td>
    </tr>
      <tr>
        <td>docker unpause container...</td>
        <td>возобновить работу контейнера</td>
    </tr>
    <tr>
        <td>docker rm container...</td>
        <td>удалить контейнер</td>
    </tr>
    <tr>
      <td>docker rm -f container...</td>
      <td>позволяет удалять работающий контейнер</td>
    </tr>
  </tbody>
</table>

## Инспекция контейнера.

<table class="t-2">
  <tbody>
    <th>Команда</th>
    <th>Описание</th>
  </tbody>
  <tbody>
    <tr>
      <td>docker ps</td>
      <td>список запущенных контейнеров</td>
    </tr>
      <tr>
        <td>docker ps -a</td>
        <td>список всех контейнеров</td>
    </tr>
    <tr>
      <td>docker image prune</td>
      <td>Удалите старые висячие изображения</td>
    </tr>
    <tr>
      <td>docker system prune</td>
      <td>
        Удаляет все неиспользуемые контейнеры, сети, образы (как висячие, так и без ссылок) и при необходимости, тома </td>
    </tr>
    <tr>
        <td>docker logs -f container</td>
        <td>показать вывод контейнера</td>
    </tr>
      <tr>
        <td>docker top container [ps OPTIONS]</td>
        <td>перечислить процессы, запущенные внутри контейнеров</td>
    </tr>
      <tr>
        <td>docker diff container</td>
        <td>показать различия с образом (измененные файлы)</td>
    </tr>
    <tr>
      <td>docker inspect container...</td>
      <td>показать низкоуровневую информацию</td>
    </tr>
  </tbody>
</table>
