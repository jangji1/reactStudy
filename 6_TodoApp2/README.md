# Ch 6. todo App 2

node DB API로 실제 데이터를 적용해보자.

## 1. deployd & ajax

### 1-1) install
install MongoDB : https://www.mongodb.com/download-center#community

#### mac
```bash
> [sudo] npm i -g deployd
```

#### win
install deployd : http://deployd.com/

#### trouble shooting
```bash
> dpd -V
/* version check. 0.6.8 이상이면 okay. */
```
http://docs.deployd.com/docs/getting-started/installing-deployd.html


### 1-2) initialize
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

### 1-3) 프로퍼티 설정 및 초기 데이터 추가하기

### 1-4) axios
https://github.com/mzabriskie/axios

```bash
> npm i -S axios
```


## 2. 성능 최적화

### 2-1) 낙관적 업데이트

- 정상적으로 서버와 송수신이 이뤄질 것이라 믿고(긍정적으로!), 일단 업데이트.
- 만의 하나 송수신이 제대로 이뤄지지 않을 경우 원상복구.

### 2-2) Immutabe Helper : `react-addons-update` => `immutability helper`

[불변성 헬퍼들](https://facebook.github.io/react/docs/update.html)

[Immutability Helper](https://github.com/kolodny/immutability-helper)

```bash
> npm i -S react-addons-update
```
