# Ch 4. ReactJS 기초 2

## 1. 컴포넌트 반복

### 1-1) forEach

- `array.forEach(function(val, key){ ... })`
- return `undefined`

```js
const arr = [1, 2, 3, 4, 5];
arr.forEach((v, i) => {
  console.log(v);
});
```

### 1-2) map

- `array.map(function(val, key){ ... })`
- return `[new array]`

```js
const arr1 = [1, 2, 3, 4, 5];
const arr2 = [
  { name : 'a', id: 0 },
  { name : 'b', id: 1 },
  { name : 'c', id: 2 }
];

const newArr1 = arr1.map((v,i) => v + 1);

const newArr2 = arr2.map((v,i) => {
  return { nameWithId : v.name + v.id }
});

console.log(newArr1, newArr2);
```

### 1-3) reduce

- `array.reduce(function(previousValue, currentValue, currentIndex, originalArray){ }, startValue)`
- return `[whatever]`
- startValue : 최초 previousValue에 할당할 값. 미지정시 첫번째 요소.

```js
const arr = [1, 2, 3, 4, 5];

const res1 = arr.reduce((p, c) => p + c);

const res2 = arr.reduce((p, c) => p + ' ' + c);

const res3 = arr.reduce((p, c) => {
  p.push(c + 1);
  return p;
}, []);

const res4 = arr.reduce((p, c) => {
  p[c] = c + 1;
  return p;
}, {});

console.log(res1, res2, res3, res4);
```

### 1-4) React에 적용

```js
//----- Parent.js -----
const people = this.state.people.map((v,i) => (
  <Child
    key         = {`person#${i}`}
    name        = {v.name}
    phone       = {v.phone}
    show        = {v.show}
    handleClick = {() => this.handleClick(i)}
  />
));
```

### 1-5) 더 간단하게

- destructuring

```js
//----- Parent.js -----
const people = this.state.people.map((v,i) => (
  <Child
    key         = {`person#${i}`}
    handleClick = {() => this.handleClick(i)}
    { ...v }
  />
));
```

- rest parameter

```js
//----- Parent.js -----
const people = this.state.people.map(({ name, ...a },i) => (
  <Child
    key         = {`person#${i}`}
    name        = {name}
    handleClick = {() => this.handleClick(i)}
    { ...a }
  />
));
```


## 2. component specs & lifecycle

### 2-1) 컴포넌트 명세
[컴포넌트 명세](https://facebook.github.io/react/docs/component-specs-ko-KR.html#컴포넌트-명세)

#### 2-1-1. `render`: 필수요소.

#### 2-1-2. `getInitialState`
- original : `getInitialState()` 메소드.

  ```js
  const Comp = React.createClass({
    getInitialState() {
      return {
        people : []
      }
    },
    ...
  })
  ```

- es6 : `constructor` 내에서 `this.state = {}`으로 직접 설정

  ```js
  class Comp extends React.Component {
    constructor() {
      super();
      this.state = {
        people : []
      };
    }
    ...
  }
  ```

#### 2-1-3. `getDefaultProps`

- original : `getDefaultProps()` 메소드.

  ```js
  const Comp = React.createClass({
    getDefaultProps() {
      return {
        name : 'no name'
      };
    },
    ...
  })
  ```

- es6 : `static defaultProps = {}`으로 직접 설정
  constructor보다 위에 위치시킬 것을 추천. ([babel-preset-stage-2](http://babeljs.io/docs/plugins/transform-class-properties/) 이하 설치 필요)

  ```js
  class Comp extends React.Component {
    static defaultProps = {
      name : 'no name'
    }
    constructor(){ ... }
    ...
  }
  ```

- es6 : 또는 class 정의를 마친 다음에 별도로 설정 가능

  ```js
  class Comp extends React.Component {
    ...
  }
  Comp.defaultProps = {
    name: 'no name'
  };
  ```

#### 2-1-4. `propTypes`

[prop 검증](https://facebook.github.io/react/docs/reusable-components-ko-KR.html#prop-검증)

- original : `propTypes: {}` 프로퍼티로 직접 지정

  ```js
  const Comp = React.createClass({
    propTypes: {
      name : React.PropTypes.string.isRequired
    }
    ...
  })
  ```

- es6 : `static propTypes: {}`으로 직접 지정

  ```js
  class Comp extends React.Component {
    static propTypes = {
      name : React.PropTypes.string
    }
    constructor(){ ... }
    ...
  }
  ```

- es6 : 또는 class 정의를 마친 다음에 별도로 설정 가능

  ```js
  class Comp extends React.Component {
    ...
  }
  Comp.propTypes = {
    name : React.PropTypes.string
  }
  ```

### 2-2) Lifecycle Methods

[생명주기 메소드](https://facebook.github.io/react/docs/component-specs-ko-KR.html#컴포넌트-명세)

1. `componentWillMount()`
2. `componentDidMount()`
3. `componentWillReceiveProps(nextProps)`
4. `shouldComponentUpdate(nextProps, nextState)`
5. `componentWillUpdate(nextProps, nextState)`
6. `componentDidUpdate(prevProps, prevState)`
7. `componentWillUnmount()`

```js
//----- Child.js -----
class Child extends React.Component {
  constructor() {
    super();
    this.state = { toggleColor: false };
  }
  componentWillMount() {
    console.log('1. 컴포넌트가 마운트될 예정입니다.');
  }
  componentDidMount() {
    console.log('2. 컴포넌트가 마운트되었습니다.');
  }
  componentWillReceiveProps(nextProps) {
    console.log('3. 컴포넌트가 새로운 props를 받을 예정입니다 : ', nextProps);
  }
  shouldComponentUpdate(nextProps, nextState) {
    console.log('4. 컴포넌트를 업데이트 해야할지 말지를 판단합니다 : ', nextProps, nextState);
    const shouldUpdate = confirm('업데이트 할까요?');
    return !!shouldUpdate;
  }
  componentWillUpdate(nextProps, nextState) {
    console.log('5. 컴포넌트가 업데이트될 예정입니다 : ', nextProps, nextState);
  }
  componentDidUpdate(prevProps, prevState) {
    console.log('6. 컴포넌트가 업데이트되었습니다 : ', prevProps, prevState);
  }
  componentWillUnmount() {
    console.log('7. 컴포넌트가 언마운트될 예정입니다.');
  }
  bgToggle() {
    this.setState({
      toggleColor: !this.state.toggleColor
    });
  }
  render() {
    const toggleColor = this.state.toggleColor;
    const list = this.props.list.map((v, i) => <li key={i}>{v}</li>);
    return (
      <div>
        <ul style={{
          backgroundColor: toggleColor ? '#acf' : '#fca'
        }}>{list}</ul>
        <button onClick={()=> this.bgToggle()}>색상변경</button>
      </div>
    );
  }
}

//----- Parent.js -----
class Parent extends React.Component {
  constructor() {
    super();
    this.state = {
      list: [0]
    }
    this.addChild = this.addChild.bind(this);
    this.removeChild = this.removeChild.bind(this);
  }
  addChild() {
    const nextList = [...this.state.list];
    nextList.push(nextList.length);
    this.setState({ list: nextList });
  }
  removeChild() {
    const nextList = [...this.state.list];
    nextList.pop();
    this.setState({ list: nextList });
  }
  render() {
    if(!this.state.list.length) return (
      <button onClick={this.addChild}>자식 추가</button>
    );
    return (
      <div>
        <Child list={this.state.list} />
        <button onClick={this.addChild}>자식 추가</button>
        <button onClick={this.removeChild}>자식 삭제</button>
      </div>
    );
  }
}
```

## 3. event handling

[Event Symstem](https://facebook.github.io/react/docs/events-ko-KR.html)

### 3-1) event attributes

event | data Type
:---  | :---
bubbles | boolean
cancelable | boolean
currentTarget | DOM Event Target
defaultPrevented | boolean
eventPhase | number
isTrusted | boolean
nativeEvent | DOM Event
preventDefault() | void
isDefaultPrevented() | boolean
stopPropagation() | void
isPropagationStopped() | boolean
target | DOM Event Target
timeStamp | number
type | string

### 3-2) event test

```js
class Comp extends React.Component {
  handleClick(e) {
    console.dir(e);
    console.dir(e.nativeEvent);
    console.log('bubbles :', e.bubbles);
    console.log('cancelable :', e.cancelable);
    console.log('defaultPrevented :', e.defaultPrevented);
    console.log('isPropagationStopped :', e.isPropagationStopped());
    console.log('eventPhase :', e.eventPhase);
    console.log('isTrusted :', e.isTrusted);
    console.log('target :', e.target);
    console.log('timeStamp :', e.timeStamp);
    console.log('type :', e.type);
    e.preventDefault();
    e.stopPropagation();
    console.log('defaultPrevented :', e.defaultPrevented);
    console.log('isPropagationStopped :', e.isPropagationStopped());
  }
  render() {
    <div onClick={this.handleClick.bind(this)} />
  }
}
```

### 3-3) Supported Events

[지원되는 이벤트](https://facebook.github.io/react/docs/events-ko-KR.html#지원되는-이벤트)

category | eventName
:--- | :---
clipboard | `onCopy` `onCut` `onPaste`
composition | `onCompositionEnd` `onCompositionStart` `onCompositionUpdate`
keyboard | `onKeyDown` `onKeyPress` `onKeyUp`
focus | `onFocus` `onBlur`
form | `onChange` `onInput` `onSubmit`
mouse | `onClick` `onContextMenu` `onDoubleClick` `onDrag` `onDragEnd` `onDragEnter` `onDragExit` `onDragLeave` `onDragOver` `onDragStart` `onDrop` `onMouseDown` `onMouseEnter` `onMouseLeave` `onMouseMove` `onMouseOut` `onMouseOver` `onMouseUp`
selection | `onSelect`
touch | `onTouchCancel` `onTouchEnd` `onTouchMove` `onTouchStart`
scroll | `onScroll`
wheel | `onWheel`
media | `onAbort` `onCanPlay` `onCanPlayThrough` `onDurationChange` `onEmptied` `onEncrypted` `onEnded` `onError` `onLoadedData` `onLoadedMetadata` `onLoadStart` `onPause` `onPlay` `onPlaying` `onProgress` `onRateChange` `onSeeked` `onSeeking` `onStalled` `onSuspend` `onTimeUpdate` `onVolumeChange` `onWaiting`
image | `onLoad` `onError`


## 4. stateless component

[상태를 가지지 않는 함수](https://facebook.github.io/react/docs/reusable-components-ko-KR.html#상태를-가지지-않는-함수)

```js
//----- Child.js -----
const Child = ({name, phone}) => (
  <li>{name} {phone}</li>
);
```
