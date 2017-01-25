# Ch 6. todo App 2

node DB API로 실제 데이터를 적용해보자.

## 1. 설치

### 1-1) mongoDB

#### mac

- http://brew.sh/index_ko.html 에서 'Homebrew 설치하기' 아래의 내용을 복사하여 터미널에 붙여넣기.

```bash
$ brew install mongodb
$ brew update
```

#### win

- install : https://www.mongodb.com/download-center#community
- 설치 후 세팅 필요 : http://cyberx.tistory.com/76 `윈도우에 설치하기` 참조.



### 1-2) deployd

#### mac

```bash
> [sudo] npm i -g deployd
```

#### win

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

### 2-1) 낙관적 업데이트

- 정상적으로 서버와 송수신이 이뤄질 것이라 믿고(긍정적으로!), 일단 업데이트.
- 만의 하나 송수신이 제대로 이뤄지지 않을 경우 원상복구.


### 2-2) Immutabe Helper : `react-addons-update` => `immutability helper`

[불변성 헬퍼들](https://facebook.github.io/react/docs/update.html)

[Immutability Helper](https://github.com/kolodny/immutability-helper)

```bash
> npm i -S react-addons-update
```
