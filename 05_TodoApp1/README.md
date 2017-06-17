# Ch 5. Todo App 1 - 로컬 앱 제작

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


### 2-1-1) callback 호출과 this 바인딩

##### (1) constructor에서 바인딩

##### (2) jsx 내부에서 바인딩

##### (3) jsx 내부에서 arrow function 활용

##### (4) class property 기능(es proposal) - arrow function 사용



### 2-2) 수정 기능

- editTodo
- saveTodo
- cancelEditTodo


### 2-3) 토글 기능 (active <-> completed)

- toggleTodo
- toggleAll
- deleteCompleted

### 2-4) 필터 기능

- [classnames](https://github.com/JedWatson/classnames)
- selectFilter
- filterView
