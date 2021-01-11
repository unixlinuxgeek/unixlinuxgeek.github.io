## React Router v5 ч2.

Еще один хук в выпуске 5.1 - это useLocation, которая возвращает объект текущего местоположения. Этот функционал полезен, когда вам нужно знать текущий URL.

Например, представьте хук useEffect, в которой вы хотите отправлять событие «просмотр страницы» в службу веб-аналитики каждый раз, когда изменяется URL-адрес. С useLocation это так же просто, как:

```js
import { Switch, useLocation } from 'react-router-dom'

function usePageViews() {
    let location = useLocation()

    useEffect(() => {
        ga.send(['pageview', location.pathname])
    }, [location])
}

function App() {
    usePageViews()
    return <Switch>{/* Ваши роуты здесь */}</Switch>
}
```

### useHistory

Для программной навигации мы предоставляем доступ к объекту истории через useHistory.

```js
import { useHistory } from 'react-router-dom'

function BackButton({ children }) {
    let history = useHistory()
    return (
        <button type="button" onClick={() => history.goBack()}>
            {children}
        </button>
    )
}
```

Хук useHistory - это временная мера для будущего хука, которая в данное время находится в альфа версии: useNavigate предоставит API, более тесно связанный с <Link>, и исправит несколько старых проблем с использованием API истории непосредственно в маршрутизаторе.

### useRouteMatch

Хук useRouteMatch полезен всякий раз, когда вы используете <Route>, просто для того, чтобы вы могли получить доступ к его данным совпадений, включая все случаи, когда вы могли бы визуализировать <Route> самостоятельно вне <Switch>. Он соответствует URL точно так же, как <Route>, включая точные, строгие и конфиденциальные параметры.

Таким образом, вместо рендеринга <Route>, просто используйте RouteMatch:

```js
// перед
import { Route } from 'react-router-dom'

function App() {
    return (
        <div>
            {/* ... */}
            <Route
                path="/BLOG/:slug/"
                strict
                sensitive
                render={({ match }) => {
                    return match ? <Post match={match} /> : <NotFound />
                }}
            />
        </div>
    )
}

// после
import { useRouteMatch } from 'react-router-dom'

function App() {
    let match = useRouteMatch({
        path: '/BLOG/:slug/',
        strict: true,
        sensitive: true,
    })

    return (
        <div>
            {/* ... */}
            {match ? <Post match={match} /> : <NotFound />}
        </div>
    )
}
```

```js
// перед
import { Route } from 'react-router-dom'

function App() {
    return (
        <div>
            {/* ... */}
            <Route
                path="/BLOG/:slug/"
                strict
                sensitive
                render={({ match }) => {
                    return match ? <BlogPost match={match} /> : <NotFound />
                }}
            />
        </div>
    )
}

// после
import { useRouteMatch } from 'react-router-dom'

function App() {
    let match = useRouteMatch({
        path: '/BLOG/:slug/',
        strict: true,
        sensitive: true,
    })

    return (
        <div>
            {/* ... */}
            {match ? <Post match={match} /> : <NotFound />}
        </div>
    )
}
```

Точно так же, как <Route>, если вы пропустите путь, этот хук вернет совпадение из ближайшего совпадения <Route> в дереве.
