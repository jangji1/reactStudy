# Ch 1. 개발환경세팅

## 1. git

- [(gui) sourceTree 설치](https://www.sourcetreeapp.com/)
- [git 간편안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
- [직접 따라하며 git 브랜치 배우기](http://learnbranch.urigit.com/)
- [Git 실습교육](http://www.slideshare.net/flyskykr/github-46014813)


## 2. node / npm

- [nodejs](https://nodejs.org/ko/)

```bash
$ node -v
$ npm -v
$ [sudo] npm i -g npm webpack babel-core
```

- [yarn](https://yarnpkg.com/)

```bash
$ [sudo] npm i -g yarn
```


## 3. package.json

- [모두 알지만 모두 모르는 package.json](http://programmingsummaries.tistory.com/385)

```bash
$ mkdir [folderName] && cd [folderName]

$ npm init -y
// yarn init -y

$ npm i jquery       ( i  === install )

$ npm i -S jquery    ( -S === --save )
// yarn add jquery

$ npm i -D jquery    ( -D === --save-dev )
// yarn add jquery -D

$ npm un jquery      ( un === uninstall )

$ npm un -S jquery
$ npm un -D jquery

$ npm i -S react react-dom
$ npm i -D react react-dom babel-core

$ npm un -S react react-dom
$ npm un -D react react-dom babel-core

$ yarn add jquery
$ yarn add react react-dom
$ yarn add react react-dom babel-core -D
$ yarn remove react react-dom babel-core
```

```js
//------ package.json ------
{
  "name": "npmTest",    // 필수
  "version": "1.0.0",   // 필수
  "description": "",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-core": "6.13.0"
  }
}
```

| 표기                | 설명
| :---                | :---
| version             | version 과 일치.
| &gt;version         |
| &gt;=version        |
| &lt;version         |
| &lt;=version        |
| ~version            | version 과 근사한 버전.
| ^version            | version 과 호환되는 것.
| 1.2.x               | 1.2.0, 1.2.1, 등등. 1.3.0은 제외
| *                   | 모든 버전
| ""                  | * 와 같음
| version1 - version2 | &gt;= version1 &lt;= version2 과 같음.
| `range1 || range2`  | range1 또는 range2


```bash
> npm i
// yarn i

> npm update
// yarn upgrade
```

```js
"devDependencies": {
  "babel-core": "^6.13.0"
}
```

```bash
> npm update
```


```js
"devDependencies": {
  "babel-core": "^6.13.0",
  "babel-preset-es2015": "^6.14.0"
},
"dependencies": {
  "react": "^15.3.2",
  "react-dom": "^15.3.2"
}
```

```bash
> npm i
```


## 4. babel

[babeljs.io](https://babeljs.io/)

```js
//------ .babelrc ------
{
  "presets": ["es2015"]
}
```

> [plugin/preset ordering](http://babeljs.io/docs/plugins/)
> - 플러그인들이 프리셋보다 먼저 실행된다.
> - 플러그인 실행 순서 : 처음 -> 나중
> - 프리셋 실행 순서 : 나중 -> 처음

```js
{
  "plugins": [
    "transform-decorators-legacy"  // 1번째로 실행됨
  ],
  "presets": [
    "es2015", // 4번쨰로 실행됨
    "react",  // 3번째로 실행됨
    "stage-1" // 2번쨰로 실행됨
  ],
}
```

```js
//----- test.js -----
console.log([1,2,3].map(n => n + 1));
```

```bash
> [sudo] npm i -g babel-cli
> babel test.js
> babel test.js -d bundle
```

## 5. webpack

- [webpack module bundler](https://webpack.github.io/)
- [javascript 모듈화도구 webpack](http://d2.naver.com/helloworld/0239818)
- [초보자용 webpack 튜토리얼 part1](https://firejune.com/1798/%EC%B4%88%EB%B3%B4%EC%9E%90%EC%9A%A9+Webpack+%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC+%ED%8C%8C%ED%8A%B81+-+Webpack+%EC%9E%85%EB%AC%B8)
- [Webpack - The Confusing Parts (원본)](https://medium.com/@rajaraodv/webpack-the-confusing-parts-58712f8fcad9#.zgbts1puu)
- [Webpack의 혼란스런 사항들 (번역)](https://github.com/FEDevelopers/tech.description/wiki/Webpack%EC%9D%98-%ED%98%BC%EB%9E%80%EC%8A%A4%EB%9F%B0-%EC%82%AC%ED%95%AD%EB%93%A4)
- [webpack configurations](https://webpack.github.io/docs/configuration.html)
- [list of plugins](https://github.com/webpack/docs/wiki/list-of-plugins)
- [입문자를 위한 Webpack 튜토리얼](https://github.com/arahansa/WebpackTutorial/tree/master/ko-arahansa)

- [webpack dev tools](https://webpack.github.io/docs/configuration.html#devtool)

```bash
> [sudo] npm i -g webpack
> npm i -D webpack babel-loader
```

```js
//----- webpack.config.js -----
module.exports = {
  devtool: 'eval',                    // 개발용 디버깅 기능
  entry: './main.js',                 // 진입파일
  output: {                           // 결과파일
    path: './',
    filename: 'bundle.js'
  },
  module: {
    loaders: [                        // 모듈별 핸들링 정의
      {
        test: /\.js$/,                // 정규표현식(조건 설정)
        exclude: [ /node_modules/ ],  // 제외할 경로
        include: [ /src/ ],           // 포함할 경로
        loader: 'babel'               // 적용할 로더
      }
    ]
  }
}
```

```js
//----- main.js -----
const a = 1;
const b = 2;
console.log(`${a} + ${b} = ${a+b}`);
```

```bash
> webpack
```
