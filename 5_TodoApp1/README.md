# Ch 5. Todo App 1

## 1. 분석 및 기획

### 1-1) 분석

[todomvc](http://todomvc.com/examples/react/)

- 입력창 "What needs to be done"
- 입력후 엔터로 등록
- 아이템이 하나 이상 있으면 좌측 '전체완료'버튼 활성화
- 아이템별로 '완료'버튼 토글
- 아이템별로 '삭제'버튼
- 아이템 더블클릭시 수정
- 다른곳 클릭시 수정 취소
- 수정후 엔터로 저장
- 화면 하단 좌측 'xx items left'에 미완료 아이템 카운트 ( 단수형/복수형 표기 )
- 하단 중간 'All / Active / Completed' 필터 버튼
- 완료된 아이템이 있을 경우 'clear completed' 버튼

### 1-2) 필요한 동작(기능) 정의

- addTodo        : 새 글 등록(엔터)
- editTodo       : 더블클릭시 수정모드로 전환
- saveTodo       : 수정한 내용 저장(엔터)
- cancelEditTodo : 수정취소
- deleteTodo     : 삭제
- toggleAll      : 전체 Todo의 Complete / Active 상태 변경 토글
- toggleTodo     : Completed / Active 상태 변경 토글
- selectFilter   : 필터 선택
- filterView     : TodoList 필터링
- deleteCompleted: Completed 상태의 Todo 전체 삭제


### 1-3) [구조] 잡기 및 <기능> 분배

- main.js     : app과 무관한 나머지 레이아웃
  - App.js      : \<filterView>
    - Header.js   : Title, \<addTodo>
    - TodoList.js : \<toggleAll>
      - Todo.js     : \<editTodo>, \<saveTodo>, \<cancelEditTodo>, \<deleteTodo>, \<toggleTodo>, \<deleteCompleted>
    - Footer.js   : counter, \<selectFilter>


## 2. 작성

### 2-1) 기본 기능

- list 불러오기
- addTodo
- deleteTodo

### 2-2) 수정 기능

- editTodo
- saveTodo
- cancelEditTodo

### 2-3) 토글 기능 (active <-> completed)

- [classnames](https://github.com/JedWatson/classnames)

- toggleTodo
- toggleAll
- deleteCompleted

### 2-4) 필터 기능

- selectFilter
- filterView


## 3. 라우터

- [react-router](https://github.com/ReactTraining/react-router/tree/master/docs)
- [route-matching](https://github.com/ReactTraining/react-router/blob/master/docs/guides/RouteMatching.md)
- [react-router 4.0](https://react-router-website-xvufzcovng.now.sh/)

```bash
> npm i -S react-router
```


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
