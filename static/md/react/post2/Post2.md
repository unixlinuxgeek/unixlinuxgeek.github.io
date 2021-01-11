## Хранилище localStorage и sessionStorage в React.

Простое, и более подходящее решение это использование локального хранилища браузера. Здесь нет ни бэкенда, ни дополнительной библиотеки. В этой статье приводится пошаговая демонстрация того, как сохранить состояние в React с помощью локального хранилища, как использовать его в качестве кеша для данных в более сложном примере и как сделать так, чтобы срок его действия истек, используя хранилище сеансов вместо локального хранилища.

### Введение в localStorage

Локальное хранилище (localStorage) поддерживается большинством браузеров. Использование локального хранилища довольно простое. В вашем JavaScript-коде, запущенном в браузере, вы имеете доступ к экземпляру localStorage, в котором есть сеттер и геттер для хранения и извлечения данных из локального хранилища. В локальном хранилище также есть методы для удаления элементов и очистки всех элементов:

```js
// сеттер
localStorage.setItem('data', data)
// геттер
localStorage.getItem('data')
// удалить
localStorage.removeItem('data')
// удалить все
localStorage.clear()
```

Тогда как первый аргумент является ключом для хранения / извлечения данных, второй аргумент - при хранении данных - это фактические данные. После того, как вы закроете браузер и снова откроете приложение JavaScript, вы найдете данные в локальном хранилище.

### Local Storage в React

Давайте подойдем к локальному хранилищу в React на примере. В нашем сценарии у нас есть функциональный компонент с состоянием, который использует React Hooks для управления состоянием поля ввода. Также результат состояния отображается как вывод с тегом абзаца HTML:

```js
import React from 'react'
const App = () => {
    const [value, setValue] = React.useState('')
    const onChange = (event) => setValue(event.target.value)
    return (
        <div>
            <h1>React Local Storage Demo</h1>
            <input value={value} type="text" onChange={onChange} />
            <p>{value}</p>
        </div>
    )
}
export default App
```

Если вы начнете что-то вводить в поле ввода, это будет показано ниже в абзаце. Однако, даже если у вас есть состояние, оно теряется, когда вы закрываете вкладку браузера или обновляете ее. Так что насчет добавления локального хранилища в качестве промежуточного кеша для него? Прямое решение будет следующим:

```js
import React from 'react'
const App = () => {
    const [value, setValue] = React.useState('')
    const onChange = (event) => {
        localStorage.setItem('myValueInLocalStorage', event.target.value)
        setValue(event.target.value)
    }
    return (
        <div>
            <h1>Hello React with Local Storage!</h1>
            <input value={value} type="text" onChange={onChange} />
            <p>{value}</p>
        </div>
    )
}
export default App
```

However, using the local storage in React's function components is a side-effect which is best implemented with the Effect Hook which runs every time the value property changes:

```js
import React from 'react'
const App = () => {
    const [value, setValue] = React.useState('')

    React.useEffect(() => {
        localStorage.setItem('myValueInLocalStorage', value)
    }, [value])

    const onChange = (event) => setValue(event.target.value)
    return (
        <div>
            <h1>Hello React with Local Storage!</h1>
            <input value={value} type="text" onChange={onChange} />
            <p>{value}</p>
        </div>
    )
}
export default App
```

Каждый раз, когда состояние значения изменяется через поле ввода, эффект запускается снова и сохраняет последнее значение в локальном хранилище. Однако один шаг отсутствует: мы только храним значение и никогда не получаем его. Давайте добавим это поведение на следующем шаге:

```js
import React from 'react'
const App = () => {
    const [value, setValue] = React.useState(
        localStorage.getItem('myValueInLocalStorage') || ''
    )
    React.useEffect(() => {
        localStorage.setItem('myValueInLocalStorage', value)
    }, [value])
    const onChange = (event) => setValue(event.target.value)
    return (
        <div>
            <h1>Hello React with Local Storage!</h1>
            <input value={value} type="text" onChange={onChange} />
            <p>{value}</p>
        </div>
    )
}
export default App
```

Теперь State Hook использует либо начальное значение из локального хранилища, либо пустую строку по умолчанию. Попробуйте сами, введя что-то в поле ввода и обновив браузер. Вы должны увидеть то же значение, что и раньше. Дополнительным шагом было бы извлечь эту функциональность как многократно используемый пользовательский хук:

```js
import React from 'react'
const localStorageState = (localStorageKey) => {
    const [value, setValue] = React.useState(
        localStorage.getItem(localStorageKey) || ''
    )
    React.useEffect(() => {
        localStorage.setItem(localStorageKey, value)
    }, [value])
    return [value, setValue]
}
const App = () => {
    const [value, setValue] = localStorageState('localStorageValue')
    const onChange = (event) => setValue(event.target.value)
    return (
        <div>
            <h1>React with Local Storage!</h1>
            <input value={value} type="text" onChange={onChange} />
            <p>{value}</p>
        </div>
    )
}
export default App
```

Теперь вы можете кэшировать свое состояние в React, используя один и тот же пользовательский хук для него. Вы можете найти окончательный исходный код этого проекта в этом репозитории GitHub. Если вам понравилась статья, не забудьте добавить звезду.
