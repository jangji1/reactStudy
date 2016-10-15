# Ch 5. Todo App 1

## 3. 라우터

- [react-router](https://github.com/ReactTraining/react-router/tree/master/docs)
- [route-matching](https://github.com/ReactTraining/react-router/blob/master/docs/guides/RouteMatching.md)
- [react-router 4.0](https://react-router-website-xvufzcovng.now.sh/)


### 3-1) `hashHistory` vs. `browserHistory`
- `hashHistory`
  - url 중 hash 영역(`#`의 뒷부분)의 변화를 감지하여 routing.
  - server-side setting 불필요
  - IE10 미만에서도 동작
- `browserHistory` (pushState, replaceState)
  - [브라우저 히스토리 조작하기](https://developer.mozilla.org/ko/docs/Web/API/History_API)
  - server-side setting 필요
  - IE10 미만 미지원

### 3-2) 기본 구조

```html
<ul>
  <li><Link to="/">Home</Link></li>
  <li><Link to="/about">About</Link></li>
  <li><Link to="/about/name">About - Name</Link></li>
  <li><Link to="/about/redirect0">About - RedirectTo: Portfolio #0</Link></li>
  <li><Link to="/about/redirect1">About - RedirectTo: Portfolio #1</Link></li>
  <li><Link to="/portfolio">Portfolio - All</Link></li>
  <li><Link to="/portfolio/0">Portfoilo - #0</Link></li>
  <li><Link to="/portfolio/1">Portfoilo - #1</Link></li>
</ul>
```

#### 3-2-1. JSX type
```js
import { Router, Route, Link, IndexRoute, Redirect, browserHistory } from 'react-router';

render(
  <Router history={browserHistory}>
    <Route path="/" component={App}>
      <IndexRoute component={Home} />
      <Route path="about" component={About}>
        <Route path="name" component={Name} />
        <Route path="redirect0"
          onEnter={(nextState, replace) => replace('/portfolio/0')}
        />
        <Redirect from="redirect1" to="/portfolio/1" />
      </Route>
      <Route path="portfolio(/:id)" component={Portfolio} />
    </Route>
  </Router>
  , document.getElementById('root')
);
```

#### 3-2-2. plain Object type
```js
/* redirect 컴포넌트를 호출할 수 없으며, 대신 `onEnter` 콜백을 활용해야만 한다. */
import { Router, Route, Link, IndexRoute, browserHistory } from 'react-router';
const routes = {
  path: '/',
  component: App,
  indexRoute: { component: Home },
  childRoutes: [
    {
      path: 'about',
      component: About,
      childRoutes: [
        {
          path: 'name',
          component: Name
        },
        {
          path: 'redirect0',
          onEnter: (nextState, replace) => replace('/portfolio/0')
        }
      ]
    },
    {
      path: 'portfolio(/:id)',
      component: Portfolio
    }
  ]
}
render(
    <Router history={browserHistory} routes={routes}/>
    , document.getElementById('root')
);
```
