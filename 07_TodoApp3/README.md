# Ch 7. Todo App - 라우터

- [react-router v4](https://reacttraining.com/react-router/web/guides/quick-start)

```bash
> npm i -S react-router-dom
```


### 1) `hashHistory` vs. `browserHistory`
- `hashHistory`
  - url 중 hash 영역(`#`의 뒷부분)의 변화를 감지하여 routing.
  - server-side setting 불필요
  - IE10 미만에서도 동작
- `browserHistory` (pushState, replaceState)
  - [브라우저 히스토리 조작하기](https://developer.mozilla.org/ko/docs/Web/API/History_API)
  - `pushState(stateObj, title, URL)`
  - server-side setting 필요
  - IE10 미만 미지원




### 2) 기본 구조

```js
/* main.js */
import ReactDOM from 'react-dom';
import {
  BrowserRouter as Router, // HashRouter, BrowserRouter 중 하나 사용.
  Route,
  Redirect,
  Switch,
  Link
} from 'react-router-dom';
import { Home, About, Name, Portfolio } from './Components';

render (
  <Router>
    <div> {/* Router 컴포넌트의 자식요소에는 오직 하나의 컴포넌트만 올 수 있다. */}
      <header>
        <ul>
          <li><Link to="/">Home</Link></li>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/about/name">About - Name</Link></li>
          <li><Link to="/about/redirect">About - RedirectTo: Portfolio #1</Link></li>
          <li><Link to="/portfolio">Portfolio - All</Link></li>
          <li><Link to="/portfolio/0">Portfoilo - #0</Link></li>
          <li><Link to="/portfolio/1">Portfoilo - #1</Link></li>
        </ul>
      </header>

      <Route exact path="/" component={Home} /> {/* exact는 정확히 해당 경로와 일치할 경우에만 렌더링 */}
      <Route path="/about" component={About} />
      <Route path="/about/name" component={Name} /> {/* exact가 없으므로 About과 Name 컴포턴트가 모두 렌더링된다. */}
      <Switch>
        <Redirect from="/about/redirect" to="/portfolio/1" /> {/* Redirect는 Switch 컴포넌트로 감싸주어야 정상 동작한다. */}
        <Route exact path="/portfolio" component={Portfolio} /> {/* exact에 유의. */}
        <Route path="/portfolio/:id" component={Portfolio} /> {/* id값 등 동적으로 할당되는 주소값에는 `:` 표기. */}
        {/* id값은 해당 컴포넌트에 `props.match.params.id`로 전달된다. */}
      </Switch>
    </div>
  </Router>
  , document.getElementById('root')
);
```


### 3) TodoApp에 적용해봅시다.


---
---

## * 과제 *

리액트로 금전출납부를 만들어보세요.
인풋창에 숫자를 입력하고 '입금' '출금'버튼을 누르면 각각 입/출금 현황 및 잔액이 표시되는 테이블을 구성하시면 되겠습니다.
디자인 필요없이 기능만 구현되면 되고, 서버에 저장하실 필요는 없습니다.

---

> 초기상태

| | | |
| --- | --- | --- |
| [ 숫자를 입력하세요 ] | [입금] | [출금] |

| 입금 | 출금 | 잔액 |
| --- | --- | --- |
| | | |

---

> 5000원 '입금'시

| 입금 | 출금 | 잔액 |
| --- | --- | --- |
| 5000 | | 5000 |

---

> 3000원 '출금'시

| 입금 | 출금 | 잔액 |
| --- | --- | --- |
| 5000 | | 5000 |
| | 3000 | 2000 |

---

### 구조

```
- main
- App
  - InputBox     : input, buttons
  - AccountBook  : data table
```
