# Ch 3. React 기초 1

## 0. Intro

- [React and the Virtual DOM](https://youtu.be/BYbgopx44vo)



## 1. easy way

```html
<!-- 1_helloworld.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <div id="root"></div>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/15.3.1/react-dom.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-core/5.8.38/browser.min.js"></script>
  <script type="text/babel">
  ReactDOM.render(
    (<h1>Hello, World!</h1>),
    document.getElementById('root')
  );
  </script>
</body>
</html>
```



## 2. with babel & webpacks (boilerplate 적용)

```cmd
> git clone https://github.com/react-study/boilerplate.git [folder-name]
> cd [folder-name]
> npm i

// 이후 package.json의 name, discription 등 정보를 바꾸어준다.
```



## 3. Component

- class (React.Component를 상속하는 클래스)
- render 메소드를 실행한 결과값(virtual dom)을 반환하는 역할.
- render 메소드는 state, props가 바뀔 때마다 실행된다.

### 3-1. original type : `React.CreateClass`
```js
//----- Parent.js -----
import React from 'react';
import Child from './Child';
const Parent = React.createClass({
  render: function() {
    return (
      <Child />
    )
  }
});
export default Parent;

//----- Child.js -----
import React from 'react';
const Child = React.createClass({
  getInitialState: function(){
    return {
      isToggle: false
    }
  },
  handleClick: function() {
    this.setState({
      isToggle: !this.state.isToggle
    });
  },
  render: function() {
    const { isToggle } = this.state;
    return (
      <h1
        style={{ color: isToggle ? '#f00' : '#00f' }}
        onClick={this.handleClick}
      >
        Hello!!
      </h1>
    );
  }
});
export default Child;
```

### 3-2. es6 type : `class extends React.Component`

```js
//----- Parent.js -----
import React from 'react';
import Child from './Child';
class Parent extends React.Component {
  render() {
    return (
      <Child />
    )
  }
}
export default Parent;

//----- Child.js -----
import React from 'react';
class Child extends React.Component {
  constructor() {
    this.state = {
      isToggle: false
    }
  }
  handleClick() {
    this.setState({
      isToggle: !this.state.isToggle
    });
  }
  render () {
    const { isToggle } = this.state;
    return (
      <h1
        style={{ color: isToggle ? '#f00' : '#00f' }}
        onClick={this.handleClick.bind(this)}
      >
        Hello!!
      </h1>
    );
  }
}
export default Child;
```


## 4. `ReactDOM.render(VirtualDOM, targetElement)`

- VirtualDOM을 실제 html의 targetElement에 그려주는 역할.

```js
//----- main.js -----
import React from 'react';
import ReactDOM from 'react-dom';
import Parent from './Parent'
ReactDOM.render(
    (<Parent />),
    document.getElementById('root')
);
```


## 5. jsx 문법

- [JSX 깊이보기](https://facebook.github.io/react/docs/jsx-in-depth-ko-KR.html)
- nested Element : 최상단에는 반드시 하나의 엘리먼트만 존재해야 한다. 즉, 여러 형제요소들은 반드시 부모요소로 감싸야 한다.
- 모든 태그는 닫는 태그가 존재해야 한다.
- 모든 태그는 단일태그로도 표현이 가능하다. (ex. `<div></div>` === `<div />`)
- 리액트 컴포넌트를 태그처럼 사용할 수 있다. (ex. `<Parent> <Child /> </Parent>`)
- 리액트 컴포넌트는 html 태그와 구분하기 위해 관행적으로 첫글자를 대문자로 써준다.
- 주석은 `{/* ... */}` 식으로, container의 안쪽에서만 가능.
- 표현식 처리 : `{ }`으로 감싸준다. ('문'형태는 불가!)
- html의 hyphens -> **camelCase** 로 표기해야 한다.
- `class` => `className`


```js
// wrong!!
(
  <Header />
  <Contents />
  <Footer />
)

// correct!!
(
  <div>
    <Header />
    <Contents />
    <Footer />
  </div>
)
```

```js
const price = 2000;
const amount = 3;
const total = price * amount;

(
  {/* 이 위치에 주석이 오면 안됨. (주석도 하나의 Node이기 때문)*/}
  <div>
    {/* 여기부터는 주석 가능. */}

    <div>가격: {price}원</div> {/* 변수나 표현식은 { }로 묶어준다. */}
    <div>수량: {amount}개</div>
    <div>할인가: {total > 7000 ? total * 0.9 : total}</div>
    <label
      className="label" {/* class는 className으로 */}
      htmlFor="Input" {/* 'for'는 'htmlFor'로 */}
      style={{
        color: '#000',
        fontSize: '16px',
        marginTop: 20,
        lineHeight: 1.3
      }}
    > {/* 각 property들은 expand 형태로 작성.*/}
      <input
        id="Input"
        type="text"
        disabled={this.props.isFocused ? true : false}
        onClick={this.handleClickInput}
      />
      {/* 하위요소가 불필요한 경우 단일태그로 처리할 수 있음. */}
      {/* 이벤트 핸들러 등록은 위와같이. */}
    </label>
  </div>
)
```


## 6. props

컴포넌트 자신이 직접 변동에 관여하지 않고, 상위 컴포넌트 등에서 전송받아 활용하는 목적으로만 쓰이는 데이터. 어떤 값을 부모 컴포넌트에서 자식 컴포넌트에 전달하기 위한 수단.
- parent 컴포넌트에 의해 값이 변경됨.
- 컴포넌트 내부에서 값 변경 불가.


```js
//----- Parent.js -----
class Parent extends React.Component {
  render() {
    return (
      <div>
        <Child name="gomugom" gender="male" />
        <Child name="iu" gender="female" />
      </div>
    )
  }
}

//----- Child.js -----
class Child extends React.Component {
  render() {
    const { name, gender } = this.props;
    return (
      <div>
        <h2>{name}</h2>
        <strong>{gender}</strong>
      </div>
    )
  }
}
Child.defaultProps = {
  name: '이름없음',
  gender: '성별없음'
};
```

## 7. state

컴포넌트 내부에서 값을 변경, 활용하기 위한 데이터. 클릭에 따른 토글상태값 기억 등 국소범위만을 위한 데이터에 유용.
- parent 컴포넌트에 의해 값이 변경되지 않음.
- 컴포넌트 내부에서 값 변경 가능.

```js
//----- Parent.js -----
class Parent extends React.Component {
  constructor() {
    this.state = {
      isToggle: false
    };
  }
  handleClick() {
    this.setState({
      isToggle: !this.state.isToggle
    });
  }
  render () {
    const { isToggle } = this.state;
    return (
      <h1
        style={{ color: isToggle ? '#f00' : '#00f' }}
        onClick={this.handleClick.bind(this)}
      >
        Hello!!
      </h1>
    );
  }
}
```


## 8. 부모/자식 component간의 통신

React의 data flow는 위에서 아래로 흐르는 일방통행이 원칙!

- Parent => Child :
  - Parent의 state 또는 props 등을 Child의 props로 지정.

- Child => Parent :
  - Parent에 Child의 상태변화를 위한 메소드 정의.
  - 위에 정의한 메소드를 Child의 props로 지정.
  - Child의 상태변화시 앞서 지정된 메소드를 호출.

```js
//----- Parent.js -----
class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      people: [{
        name: 'gomugom',
        phone: '010-1111-2222',
        show: false
      }, {
        name: 'iu',
        phone: '010-2222-3333',
        show: false
      }, {
        name: 'akmu',
        phone: '010-1133-3245',
        show: false
      }]
    };
  }
  handleClick(i) {
    console.log(this.state);
    const newPeople = this.state.people;
    newPeople[i].show = !newPeople[i].show;
    this.setState({
      people: newPeople
    });
  }
  render() {
    const people = this.state.people;
    return (
      <ul>
        <Child
          name={ people[0].name }
          phone={ people[0].phone }
          show={ people[0].show }
          handleClick={this.handleClick.bind(this, 0)}
        />
        <Child
          name={ people[1].name }
          phone={ people[1].phone }
          show={ people[1].show }
          handleClick={this.handleClick.bind(this, 1)}
        />
        <Child
          name={ people[2].name }
          phone={ people[2].phone }
          show={ people[2].show }
          handleClick={this.handleClick.bind(this, 2)}
        />
      </ul>
    );
  }
}

//----- Child.js -----
class Child extends React.Component {
  render() {
    const { name, phone, show, handleClick } = this.props;
    return (
      <li onClick={handleClick}>
        <p>name: {name}</p>
        <p style={{
          display: show ? 'inline' : 'none'
        }}>
          {phone}
        </p>
      </li>
    );
  }
}
```

## 9. refs
- [refs에서 컴포넌트로](https://facebook.github.io/react/docs/more-about-refs-ko-KR.html)
- state나 props로는 수행할 수 없는 DOM 제어 등을 위한 추가수단.
- 가급적 '데이터흐름'과 무관한 경우에만 활용할 것.
- 기존에는 문자열로 지정하고 접근할 수 있었으나, 이제는 다음 방식만 유효함.
- stateless component에는 사용 불가.

```js
//----- Parent.js -----
class Parent extends React.Component {
  handleClick() {
    this._input.focus();
  }
  render() {
    return (
      <div>
        <input
          type="text"
          ref={ ref => { this._input = ref }}
        />
        <button
          onClick={()=> this.handleClick()}
        >
          클릭!
        </button>
      </div>
    )
  }
}
```

## 10. react devtool

[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi)
