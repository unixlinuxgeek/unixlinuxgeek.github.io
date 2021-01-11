# Основы разделения пакетов с помощью Webpack 4

В этой статье я представлю концепцию разделения пакетов и приведу два простых примера, которые иллюстрируют, как реализовать разделение пакетов, и его преимущества.

# Введение в optimization.splitChunks

Первоначально чанки (и модули, импортированные в них) были связаны отношениями родитель-потомок во внутреннем графе webpack.
CommonsChunkPlugin использовался, чтобы избежать дублирования зависимостей между ними, но дальнейшая оптимизация была невозможна.
Начиная с webpack v4, CommonsChunkPlugin был удален в пользу оптимизации optimization.splitChunks.

Параметр optimisation.splitChunks позволяет разделить main.js, сгенерированный веб-пакетом по умолчанию, на несколько фрагментов - другими словами, на несколько файлов. Давайте посмотрим на наиболее распространенный вариант использования: разделение исходных активов и ресурсов поставщика.

-   vendor assets: assets, используемые от внешних поставщиков, например, импортированные из node_modules. Код, который не является уникальным для вашего приложения.
-   исходные assets: assets, уникальные для вашего приложения, такие как бизнес-логика, которые вы, вероятно, со временем измените.

## Настройка

Во-первых, простой пример. Мы создаем приложение Hello World Vue и хотим использовать разделение пакетов, чтобы повысить производительность нашего приложения и снизить нагрузку на наш сервер.

```js
install webpack webpack-cli vue --save
```

И создайте конфигурацию webpack и точку входа:

```js
webpack.config.js
mkdir src
touch src/index.js
touch src/create-app.js
```

В src/create-app.js создайте новое приложение Vue следующим образом:

```js
import Vue from 'vue'

export default function createApp() {
    const el = document.createElement('div')
    el.setAttribute('id', 'app')

    document.body.appendChild(el)

    new Vue({
        el: '#app',
        render: (h) => h('div', 'Hello world'),
    })
}
```

Импортируйте create-app и выполните его в src/index.js:

```js
import createApp from './create-app'

document.addEventListener('DOMContentLoaded', () => {
    createApp()
})
```

Добавьте минимальную конфигурацию webpack:

```js
const webpack = require('webpack')

module.exports = {}
```

Теперь запустите npx webpack --mode development, чтобы связать с настройками по умолчанию:

```js
: 6aca10b38e4ad1df98f1
Version: webpack 4.44.1
Time: 355ms
Built at: 2020-06-09 15:56:50
  Asset     Size  Chunks             Chunk Names
main.js  236 KiB    main  [emitted]  main
[./node_modules/webpack/buildin/global.js] (webpack)/buildin/global.js 489 bytes {main} [built]
[./src/create-app.js] 246 bytes {main} [built]
[./src/index.js] 110 bytes {main} [built]
    + 4 hidden modules
```

main.js 236 КиБ - это большая полезная нагрузка, просто для Hello World. Однако мы написали всего около 10 строк кода. Все остальное - это vue.js. Мы можем разделить код vendor-а (vue.js) и наш код src/index.js.

## Разделение на две группы
