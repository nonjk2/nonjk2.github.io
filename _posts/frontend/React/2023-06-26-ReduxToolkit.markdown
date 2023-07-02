---
layout: post
title: "Redux Toolkit"
category: frontend
description: >
  리덕스 버전업을 보자
image:
  path: /assets/img/frontend/react/reduxtoolkit.png
tags: React TIL
permalink: frontend/React/reduxtoolkit
---
<!--more-->
# State란
{:.no_toc}

* this list will be replaced by the toc
{:toc}

# Redux-toolkit

[redux](./2023-06-26-Redux.markdown){:.heading.flip-title}{:.read-more}에서 뭔가 내용을 많이 빠뜨리고 적은거같아 다시 정리해보려한다.

## 리덕스 데이터 흐름도

![리덕스데이터흐름](/assets/img/frontend/react/reduxImage.gif)

[출처 : redux-docs](https://ko.redux.js.org/)

- 리덕스는 일반적으로 리덕스와 리액트를 함께 통합하기위해 React-Redux 라이브러리와 함께 사용
- 리덕스 툴킷은 리덕스 로직을 작성하는데 권장하는 방법 -> **리덕스피셜 리덕스 버전2 선언**
- 자자,, `action`은 필드가있는 `type` 이고 뭔일이있는지 설명한다.
- `reducer`는 이전 `state` + `action`을 기반으로 새로운 `state`를 계산
- `store`는 `action`이 발송될때 마다 `Rootreducer`를 실행 한다.

## 리덕스는 단방향 데이터 흐름

- `state`는 특정 시점의 앱의 `state`를 설명하고 UI는 해당 `state` 기반으로 render
- 앱에서 어떤 일이 발생하면 UI가 작업을 전달 -> `store`는 `reducer`를 실행하고 발생한 상황에 따라 `State`가 업데이트된다.
- `store`는 `State`가 변경되었음을 UI에 알린다.
- 새로운 `State`에 따라 UI가 다시 render된다.

## reduce

리듀서의 **특별한 규칙**

- state 와 action인수를 기반으로 새 state 값만 나와야한다.
- 현재 state를 수정할 수 없다. 기존 state를 복사하고 변경하여 업데이트를 해야됌
- 비동기 수행이랑 딴 로직을 넣음 안됌

뭔가 기술할게 쌓여만 가는데 여기까지하고 나중에 deep 하게 기술할때 써야겠다.

## 정리

> - Redux 상태는 일반 JS 데이터로 구성.
> - action은 말그대로 무슨행동인지 설명하는함수.
> - reducer는 현재 state에서 작업을 수행하고 새 상태를 계산.
> - reducer는 불변성을 지키면서 업뎃하면서 상관없는 딴로직 넣지말기
> - createStore는 루트리듀서기능으로 redux스토어를 생성.
> - store는 enhancer , middleware를 사용하여 사용자 정의 가능
> - redux DevTools 확장을 통해 시간여행가능
> - useSelector 는 현재 state가져옴
> - useDispatch 는 말그대로 action을 할수있음
> - Provider로 앱을 감싸고 store를 인자로 넘기자
> - Reducer에서 부작용이있는 로직은 쓸수없다했는데 redux middleware에서는 로직작성 허용
> - middleware는 데이터 흐름에 추가 단계를 추가해서 비동기 로직을 활성화
> - thunk 함수는 비동기 로직을 작성할수있는 방법
> - action 객체 및 thunk를 캡슐화
> - 요청 상태는 enum값으로 추적
> - 정규화를 잘시키자

## Redux-toolkit을 알아보자

자 지금까지 원리들을 대충 머리속에 넣고 어떻게 로직이 향상이됬는지 알아보자

### 리덕스 툴킷이 생긴이유

별거없다 위에 만들어야되는게 겁나많다 action , reducer 등등 암튼 손으로 작성하기가 너무 귀찮고 리덕스의 논리를 작성하는 올바른 방법이 뭔지 확신하지 못하는 경우가 매우많아 개발자들이 요청요청요청 해서 프로세스를 줄이고줄이고줄여서 만든게 redux Tookit 되시겠다.

### 리덕스 툴킷 개선점

-`createSlice` 얘가 action이랑 reducer 기능에 해당하는 **action creator를 자동으로 생성** 하신단다.

- `createSlice` 기본 케이스에서 자동으로 현재 state를 반환 -> `initialState`
- `createSlice` state를 안전하게 변형 가능 -> 이것또한 좋다 Redux-toolkit에는 `createSlice` 내부에 `Immer`라는 라이브러리를 사용하는데 깊은복사를 다해주는거 같다.

`Proxy`Immer는 제공한 데이터를 래핑하기 위해 a라는 특수 JS 도구를 사용하며 래핑된 데이터를 "변형"하는 코드를 작성할 수 있다고한다.

자아아아 드디어 예제코드를 한번 보자꾸나

### 리덕스 기본 State

```jsx
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  status: "idle",
  entities: {},
};

const todosSlice = createSlice({
  name: "todos",
  initialState,
  reducers: {
    todoAdded(state, action) {
      const todo = action.payload;
      state.entities[todo.id] = todo;
    },
    todoToggled(state, action) {
      const todoId = action.payload;
      const todo = state.entities[todoId];
      todo.completed = !todo.completed;
    },
  },
});

export const { todoAdded, todoToggled } = todosSlice.actions;

export default todosSlice.reducer;
```

reducer 객체안에 action을 만들고 state와 action 인자를 가져와서 로직을 수행한다.

그리고 위에서 설명했던 Immer의 기능 새 배열을 만들어서 return 하는 것이 아닌 바로 값을 할당을 해버린다. 이걸 가능케하네

### 리덕스 API 떵크

이것또한 쓰기 쉬워짐

```jsx
// omit imports

export const fetchTodos = createAsyncThunk("todos/fetchTodos", async () => {
  const response = await client.get("/fakeApi/todos");
  return response.todos;
});

export const saveNewTodo = createAsyncThunk(
  "todos/saveNewTodo",
  async (text) => {
    const initialTodo = { text };
    const response = await client.post("/fakeApi/todos", { todo: initialTodo });
    return response.todo;
  }
);

const todosSlice = createSlice({
  name: "todos",
  initialState,
  reducers: {
    // omit case reducers
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchTodos.pending, (state, action) => {
        state.status = "loading";
      })
      .addCase(fetchTodos.fulfilled, (state, action) => {
        const newEntities = {};
        action.payload.forEach((todo) => {
          newEntities[todo.id] = todo;
        });
        state.entities = newEntities;
        state.status = "idle";
      })
      .addCase(saveNewTodo.fulfilled, (state, action) => {
        const todo = action.payload;
        state.entities[todo.id] = todo;
      });
  },
});

// omit exports and selectors
```

`createAsynThunk`를 사용하면 세 가지 작업 생성자와 작업 유형, 호출 시 해당 작업을 자동으로 전달하는 썽크 함수가 생성됨

- `fetchTodos.pending`: `todos/fetchTodos/pending`
- `fetchTodos.fulfilled`: `todos/fetchTodos/fulfilled`
- `fetchTodos.rejected`: `todos/fetchTodos/rejected`

같은 슬라이스 내에 다른 작업 유형을 받을수 있게하는 옵션도 있다 `builder`

안에서 3가지 유형을 나누고 보내고받는중 , 다받음 , 그리고 reject 당했을때 처리를 각각할수 있다.

### 정규화엔티티

`createEntityAdapter`

`addOne`/ `addMany`: 상태에 새 항목 추가
`upsertOne`/ `upsertMany`: 새 항목 추가 또는 기존 항목 업데이트
`updateOne`/ `updateMany`: 부분 값을 제공하여 기존 항목 업데이트
`removeOne`/ `removeMany`: ID를 기준으로 항목 제거
`setAll`: 기존 항목 모두 교체

대충 이런 메소드가 제공되는데 불변성 관리에도 쓸수있고 필요한것만 골라서 쓰는 메소드다. 알잘딱깔센 해서 쓰자

다음은 그냥 To Do list redux 올려야겠다.


### 그외에

`redux-thunk`
함수 형태의 액션을 디스패치할 수 있게 해주는 미들웨어. 이 함수 내에서는 비동기 작업을 수행하고, 작업이 끝나면 새로운 액션을 디스패치할 수 있다.

`redux-saga`
 제너레이터 함수를 이용하여 비동기 작업을 더 세밀하게 제어할 수 있게 해주는 미들웨어. 복잡한 비동기 작업 흐름을 관리하는데 유용하다.

`redux-promise`
 Promise를 반환하는 액션을 디스패치할 수 있게 해주는 미들웨어. Promise가 resolve되면, 해당 결과를 payload로 가지는 새로운 액션을 디스패치한다.

`redux-persist`

  Redux 스토어의 상태를 지속적으로 유지하기 위한 라이브러리. 웹 애플리케이션을 새로 고침하거나 재시작할 때, Redux 스토어의 상태가 초기화되는 것을 방지하기 위해 사용한다
## 요약 정리

> - `RTK` === `Redux Toolkit` , 떵크 = 대충 API 나 프로미스작업
> - RTK에는 대부분의 Redux 코드를 단순화하는 API가 포함되있다.
> - RTK는 리덕스안에서 다른 유용한 패키지를 포함한다.
> - `sliceReducer`를 자동으로 결합하여 `rootReducer`를 생성한다.
> - `Redux DevTools Extension` 및 디버깅 미들웨어를 자동으로 설정한다.
> - createSliceRedux 작업 및 reducer 작성이 단순화됨
> - slice/reducer 이름을 기반으로 action 생성자를 자동으로 생성한다.
> - createSlice.Reducer는 Immer를 사용하여 내부 상태를 "변경"할 수 있다 -> 내부 롸입뤄리.
> - createAsyncThunk비동기 호출에 대한 썽크 생성
> - 떵크 + pending/fulfilled/rejected액션 생성자를 자동으로 생성. 각각 설정가능
> - 썽크를 디스패치하면 페이로드 생성자가 실행되고 작업이 바로 디스패치됨.
> - 떵크 작업은 createSlice.extraReducers 에서 처리가능
> - 그외 기능

다음은 redux-persist에대해 알아보자