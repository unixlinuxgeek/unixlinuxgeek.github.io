# Изучение Vue на простом примере.

Vue - это прогрессивный фреймворк для создания пользовательских интерфейсов. В отличие от других монолитных фреймворков, Vue разработан с нуля для постепенного внедрения. Основная библиотека ориентирована только на слой представления, и ее легко выбрать и интегрировать с другими библиотеками или существующими проектами. С другой стороны, Vue также отлично поддерживает сложные одностраничные приложения при использовании в сочетании с современными инструментами и вспомогательными библиотеками.

Преимущества Vue.js

- Делает создание пользовательского интерфейса и интерфейсных приложений намного проще
- Меньше кривая обучения, чем в других фреймворках
- Чрезвычайно быстрый и легкий
- Позволяет создавайть мощные одностраничные приложения
- Виртуальный DOM
- Большое активное сообщество

Для того чтобы понять вам как создавать приложения с помощью Vue.js давайте создадим небольшой проект «TodoList» (Список дел).

Прежде всего, необходимо настроить среду проекта. Я буду использовал Vue CLI. Он предоставляет официальный интерфейс командной строки для быстрого создания одностраничных приложений.
Чтобы использовать Vue CLI, вы должны сначала загрузить и установить Node.js. Затем откройте командную строку Node.js или CMD, выберите папку и запустите эту команду.

```sh
npm i @vue/cli
```

После устанавки Vue CLI, можно создать проект:

```sh
vue create app
```

![Vue Project](/static/md/vue/introduction-1/vueProject.jpg)

Содержимое папки практически одинаковы в React или Angular. Если вы знакомы с «React» или «Angular», то разобраться в структуре проета Vue будет несложно.

Здесь «App.vue» - это корневой компонент, который в основном отображает все эти компоненты. Взгляните на этот код.

```js
<template>
  <div id="app">
    <Header />
    <AddTodo />
    <Todo v-bind:todos="todos" v-on:del-todo="deleteTodo">
  </div>
</template>

<script>
import Header from './components/layout/Header'
import Todo from './components/Todo'
import AddTodo from './components/AddTodo'
export default {
  name: 'App',
  components: {
    Header,
    Todo,
    AddTodo
  },
  data () {
    return {
      todos: [
        {
          id: 1,
          title: 'Todo One',
          completed: true
        },
        {
          id: 1,
          title: 'Todo Two',
          completed: true
        },
        {
          id: 1,
          title: 'Todo Three',
          completed: false
        }
      ]
    }
  },
  methods: {
    deleteTodo (id) {
      this.todos = this.todos.filer(todo=> todo.id !== id)
    }
  }
}
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: Arial, Helvetica, sansserif;
  line-height: 1.4;
}
.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover {
  background: #666;
}
</style>

```

Компонент «App.vue состоит из трех частей: Шаблона, Сценария» и Стилей. Если вы вникнете в нее, я думаю, вы поймете некоторые основные части. Верхняя и нижняя часть шаблона посвящена HTML и стилям, а средняя часть «Script» - это основная часть Vue.js.

Теперь можно создать компонент «header» для нашего проекта.

```js
<template>
  <header class="header">
    <h1>Список дел</h1>
    <div id="nav">
      <router-link to="/">Home</router-link>
      <router-link tp="/about">About</router-link>
    </div>
  </header>
</template>

<script>
<template>
  <div id="app">
    <Header />
    <AddTodo />
    <Todo v-bind:todos="todos" v-on:del-todo="deleteTodo">
  </div>
</template>

<script>
import Header from './components/layout/Header'
import Todo from './components/Todo'
import AddTodo from './components/AddTodo'
export default {
  name: 'App',
  components: {
    Header,
    Todo,
    AddTodo
  },
  data () {
    return {
      todos: [
        {
          id: 1,
          title: 'Todo One',
          completed: true
        },
        {
          id: 1,
          title: 'Todo Two',
          completed: true
        },
        {
          id: 1,
          title: 'Todo Three',
          completed: false
        }
      ]
    }
  },
  methods: {
    deleteTodo (id) {
      this.todos = this.todos.filer(todo=> todo.id !== id)
    }
  }
}
</script>

<style>
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: Arial, Helvetica, sansserif;
  line-height: 1.4;
}
.btn {
  display: inline-block;
  border: none;
  background: #555;
  color: #fff;
  padding: 7px 20px;
  cursor: pointer;
}
.btn:hover {
  background: #666;
}
</style>
```

Теперь создадим компонент Todo

```js
<template>
  <div>
    <div v-bind:key="todo.id" v-for="todo in todos">
      <TodoItem v-bind:todo="todo" v-on:del-todo="$emit('del-todo', todo.id)" />
    </div>
  </div>
</template>
<script>
import TodoItem from "./TodoItem.vue";
export default {
  name: "Todo",
  components: {
    TodoItem,
  },
  props: ["todos"],
};
</script>

<style scoped></style>

```

В компоненте todo у нас есть дочерний компонент TodoItem, давайте реализуем это

```js
<template>
  <div class="todo-item" v-bind:class="{ 'is-complete': todo.completed }">
    <p>
      <input
        type="checkbox"
        v-on:change="markComplete"
        v-bind:checked="todo.completed"
      />
      {{ todo.title }}
      <button @click="$emit('del-todo', todo.id)" class="del">x</button>
    </p>
  </div>
</template>

<script>
export default {
  name: "TodoItem",
  props: ["todo"],
  methods: {
    markComplete() {
      this.todo.completed = !this.todo.completed;
    },
  },
};
</script>

<style scoped>
.todo-item {
  background: #f4f4f4;
  padding: 10px;
  border-bottom: 1px #ccc dotted;
}
.is-complete {
  text-decoration: line-through;
}
.del {
  background: #ff0000;
  color: #fff;
  border: none;
  padding: 5px 9px;
  border-radius: 50%;
  cursor: pointer;
  float: right;
}
</style>

```

Давайте добавим последний компонент - компонент AddTodo.

```js
<template>
  <div>
    <form>
      <input
        type="text"
        v-model="title"
        name="title"
        placeholder="Добавить..."
      />
      <input type="submit" value="Submit" class="btn" />
    </form>
  </div>
</template>

<script>
export default {
  name: "AddTodo",
  data() {
    return {
      title: "",
    };
  },
};
</script>

<style scoped>
form {
  display: flex;
}
input[type="text"] {
  flex: 10;
  padding: 5px;
}
input[type="submit"] {
  flex: 2;
}
</style>
```

Компилируем npm run build и запускаем npm run serve.
Проект должен быть приблезмтельно таким.

![Todo App](/static/md/vue/introduction-1/TodoApp.png)
