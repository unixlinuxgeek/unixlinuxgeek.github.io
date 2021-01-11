# Создаем первое приложение Symfony 5 + Docker

## Подготовка

Создадим файл <b>docker-compose.yml</b> и скопируем в нее следующее содержимое:

<details open>
<summary>
docker-compose.yml
</summary>

```yml
version: "3"
services:
    nginx:
        container_name: nginx-1
        image: nginx:latest
        ports:
            - "80:80"
        volumes:
            - ./code:/code
            - ./site.conf:/etc/nginx/conf.d/default.conf
        networks:
          network_45:
            ipv4_address: 172.45.0.2

    php:
        container_name: php-fpm-1
        image: php:fpm
        volumes:
            - ./code:/code
        networks:
          network_45:
            ipv4_address: 172.45.0.3

    mysql-8:
        container_name: mysql-1
        image: mysql:latest
        command: --default-authentication-plugin=mysql_native_password
        environment:
            - MYSQL_ROOT_PASSWORD=12345678
        ports:
            - "3306:3306"
        environment:
          MYSQL_DATABASE: app1
          MYSQL_USER: app1_db_user
          MYSQL_PASSWORD: 12345678
          MYSQL_ROOT_PASSWORD: 12345678
          SERVICE_TAGS: dev
          SERVICE_NAME: mysql
        networks:
          network_45:
            ipv4_address: 172.45.0.4

    phpmyadmin:
        image: phpmyadmin/phpmyadmin
        container_name: phpmyadmin
        environment:
            - PMA_ARBITRARY=1
            - PMA_HOST=mysql-8
        restart: always
        ports:
           - 2000:80
        volumes:
           - /sessions
        networks:
          network_45:
            ipv4_address: 172.45.0.5

    composer:
      build:
        context: .
        dockerfile: composer.dockerfile
      container_name: composer45
      volumes:
        - ./code:/var/www/html
      working_dir: /var/www/html
      depends_on:
        - php
      user: symfony
      networks:
        network_45:
          ipv4_address: 172.45.0.6
      entrypoint: ['composer']

networks:
  network_45:
    driver: bridge
    ipam:
      config:
        - subnet: 172.45.0.0/16
          gateway: 172.45.0.1

```

</details>

Также нам нужно создать файл composer.dockerfile

<details open>
<summary>
composer.dockerfile
</summary>
```bash
FROM composer:latest
RUN addgroup -g 1000 symfony && adduser -G symfony -g symfony -s /bin/sh -D symfony
WORKDIR /var/www/html
```
</details>

Создадим папку code и перейдем в нее далее создадим наш проект с помощью composer:

```bash
composer create-project symfony/website-skeleton project
```

Теперь введем в адресную строку браузера адрес localhost и увидим нечто похожее:

![symfony-website-skeleton](/static/md/symfony/introduction-1/images/demo.jpg)

Наше приложение готово!!!  
В слудующей статьях мы создадимь роуты, контроллеры, настроем подключение к БД и установим дополнительные пакеты.
