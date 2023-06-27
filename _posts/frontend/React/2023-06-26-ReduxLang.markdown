---
layout: post
title: "redux(2)"
category: frontend
description: >
  리덕스 용어에 대해 알아보자
image:
  path: /assets/img/frontend/react/redux.png
tags: React TIL
permalink: frontend/React/reduxtwo
---
<!--more-->



# Redux 언어알아보자
{:.no_toc}

* this list will be replaced by the toc
{:toc .large-only}


리덕스에는 주로 사용되는 용어들이있다 함수, state , 리듀서 등 그걸 한번알아보자

## state

state는 어떻게보면 넓은 의미의 단어다. useState에 의해 관리도 되고 또한 변수에 어떤값을 할당시켜서 어떤식으로든 보관시키면 그걸 state라 지칭해 보관할수 있다. redux에서 state는 보통 저장소에의해 관리되고 `getState()`에 의해 반환되는 하나의 상태값을 말한다.

## action

`action`은 위 state를 변화시켜려는 의도를 표현하는 평범한 객체다. 저장소에 데이터를 넣는 유일한 방법!

`action`은 어떠한 행동을 가질지에대한 `Type`이 있어야함 ->( 나중에 switch로 돌려 어떤 로직으로 return)

그외에 비동기 `action` 도 있다.

## reducer

리듀서는 누적값과 값을 받아서 새로운 누적값을 반환하는 함수 -> 어디서 많이 본 메서드쥬 ? 이름과 비슷하게 `reduce()` 메서드에서 네이밍을 따왔다고 한다.

reduce메서드도 첫번째인자가 누적값인 만큼 Redux에서도 누적값은 상태 객체이고
누적될값은 `action` 그리고 리듀스는 무조건 **순수함수** 여야한다 대충 설명하면 ~~이함수안에는 {}쓰지마~~ 같은 입력이 있으면 같은 출력을 반환해야만 한다. 다른로직 쓰지말란 얘기 , **만약 쓰게된다면 어떠한 에러가 기다릴지 모름**

- 여기다 API 넣으면 안되는곳임

## dispatch

`dispatch` 함수는 `action` 이나 비동기 `action` 을 받는 함수 받은 다음 하나ㅏ 여러개의 `action`을 `store`에 보내지 않을수 있다.

[redux Docs](https://ko.redux.js.org/)에서는 `dispatch`함수는 **반드시** 동기적으로 저장소의 리듀서에 액션을 보내야 한다고 한다. 음 생각해보면 당연한일 리듀서도 새롭게 반환한 값을 알아야하니깐

## ActionCreator

이건 그냥 액션을 만드는 함수

## 비동기 action

비동기 액션은 `dispatch()` 함수로 전달하기전에 [미들웨어](#미들웨어)를 통해 액션으로 바껴야한다.

Promise나 thunk와 같은 비동기 기본형으로, reducer에게 직접 전달하진 않지만 작업이 완료되면 액션을 보낸다

## 미들웨어

```ts
type MiddlewareAPI = { dispatch: Dispatch; getState: () => State };
type Middleware = (api: MiddlewareAPI) => (next: Dispatch) => Dispatch;
```

이 녀석으로 말할거같으면 `dispatch`함수를 결합해서 새 디스패치 함수를 반환하는 고차함수 되시겠다. 비동기 액션도 액션으로 전환

## store

저장소 되시겠다.

```ts
type Store = {
  dispatch: Dispatch;
  getState: () => State;
  subscribe: (listener: () => void) => () => void;
  replaceReducer: (reducer: Reducer) => void;
};
```

애플리케이션의 상태 트리를 가지고 있는 객체 . 리듀서 수준에서 결합이 일어나기 때문에 , **redux 앱에는 단 하나의 저장소만 존재**

`getState()` -> 현재상태반환
`subscribe(listener)` -> 상태가 바뀔 때 호출될 함수를 등록.
`replaceReducer`는 우리가 사용할일이 없대요

## StoreCreator

얘는 Redux store를 만드는 함수 `createStore`랑 구분 필요

## StoreEnhancer

저장소 인핸서는 저장소 생산자를 결합하여 강화된 새 저장소 생산자를 반환하는 고차함수. 보통 DevTools 가 제공하는 enhancer로 사용

음 되게많이 끄적였는데 오늘은 용어만 알아보자
