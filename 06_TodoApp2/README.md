# Ch 6. todo App 2 - ajax 통신

로컬 환경에서 DB를 구축하고, ajax를 통해 서버와 통신하면서 진짜 동작하는 app을 만들어 봅시다.

이하에서는 mongoDB와 deployd를 설치하는데, 여러분이 실제로 mongoDB를 조작할 일은 없습니다.
deployd가 mongoDB를 기반으로 동작하기 때문에 설치하는 것입니다.

## 1. 설치

### 1-1) mongoDB

#### mac

- http://brew.sh/index_ko.html 에서 'Homebrew 설치하기' 아래의 내용을 복사하여 터미널에 붙여넣기.
- brew install mongodb
- brew update

#### win

- install : https://www.mongodb.com/download-center#community
- 설치 후 세팅 필요 : [환경변수세팅](./mongodb_for_windows.md)



### 1-2) deployd

```bash
> [sudo] npm i -g deployd
```

http://deployd.com/


#### trouble shooting

```bash
> dpd -V
/* version check. 0.6.8 이상이면 okay. */
```
http://docs.deployd.com/docs/getting-started/installing-deployd.html


#### initialize

```bash
> dpd create [DB폴더명]
> cd [DB폴더명]
> dpd -d
```

or

```bash
> dpd create [DB폴더명]
> dpd [DB폴더명]/app.dpd -d
```

http://docs.deployd.com/docs/getting-started/your-first-api.html


### 1-3) axios

https://github.com/mzabriskie/axios

```bash
> npm i -S axios
```


## 2. 성능 최적화

### 2-1) shouldComponentUpdate와 불변객체

### 2-2) Immutabe Helper : `react-addons-update` => `immutability helper`

[불변성 헬퍼들](https://facebook.github.io/react/docs/update.html)

[Immutability Helper](https://github.com/kolodny/immutability-helper)

```bash
> npm i -S immutability-helper
```

### 2-3) 낙관적 업데이트

- 정상적으로 서버와 송수신이 이뤄질 것이라 믿고(긍정적으로!), 일단 업데이트.
- 만의 하나 송수신이 제대로 이뤄지지 않을 경우 원상복구.
