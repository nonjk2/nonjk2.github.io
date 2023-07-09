---
layout: post
title: "useContext 와 useReducer"
category: frontend
description: >
  react 유용한 hook useContext와 useReducer
image:
  path: /assets/img/frontend/react/WIL0709.png
tags: React WIL
permalink: WIL3
---
# 2주차 WIL
{:.no_toc}

* this list will be replaced by the toc
{:toc}

<!--more-->

## useContext와 useReducer

useContext와 useReducer를 짧게 정리해보려고 한다. 알고있는것들과 정리하면서 더욱 알기위해,,,

### useContext

`useContext`는 React의 context를 함수형 컴포넌트에서 사용하기 위한 hook. 얘는 리액트에서 state를 전역적으로 쓰고싶을때 사용한다.

그리고 특징으로는 context 객체를 인자로 받아 그 context의 현재 value를 반환한다.

코드코드

```tsx
import React, { createContext } from "react";

interface 인터페이스 {
  name: string;
  age: number;
}

const MyContext = createContext<인터페이스>({
  name: "Baby eun",
  age: 0,
});

export default MyContext;
```

컨텍스트 만들고

```tsx
import React, { useContext } from "react";
import MyContext from "./MyContext"; // 이전에 만든 Context를 import합니다.

const MyComponent: React.FC = () => {
  const myContext = useContext(MyContext);

  return (
    <div>
      <p>Name: {myContext.name}</p>
      <p>Age: {myContext.age}</p>
      {/* 나머지 필요한 데이터를 표시합니다. */}
    </div>
  );
};

export default MyComponent;
```

컨텍스트 사용

매우쉬움

그리고 Provider를 감싸줘야하는데

```tsx
import React from "react";
import MyContext from "./MyContext";
import MyComponent from "./MyComponent";

const ParentComponent: React.FC = () => {
  const contextValue = {
    name: "adult eun",
    age: 5,
    // 필요한 값들
  };

  return (
    <MyContext.Provider value={contextValue}>
      <MyComponent />
    </MyContext.Provider>
  );
};

export default ParentComponent;
```

요렇게 리덕스처럼 감싸주면 됌 좀더 응용을해서 파일을 하나빼서

```tsx
import React, { createContext, useContext, useMemo, useState } from "react";

interface UpContextType {
  counter: number;
  increaseCounter: () => void;
}

const UpContext = createContext<UpContextType | null>(null);

export const useUpContext = () => {
  const context = useContext(UpContext);

  return context;
};

export const UpProvider: React.FC = ({ children }) => {
  const [counter, setCounter] = useState<number>(0);

  const increaseCounter = () => {
    setCounter((prevCounter) => prevCounter + 1);
  };

  const value = useMemo<UpContextType>(() => {
    return {
      counter,
      increaseCounter,
    };
  }, [counter, increaseCounter]);

  return <UpContext.Provider value={value}>{children}</UpContext.Provider>;
};
```

이렇게 만들어 줄수도 있음

### useReducer

`useReducer`는 Redux와 비슷한 패턴을 구현하기 위한 훅이다. `useState`와 비슷하게 상태 관리를 할 수 있고 , 훨신더 복잡한 로직을 다룰 때 더 자주사용된다.

`useReducer`는 reducer 함수와 초기 상태를 인자로 받아, 현재 상태와 dispatch 함수를 같이 제공한다. reducer 함수는 현재 상태와 action 객체를 인자로 받아 새로운 상태를 반환하는 함수다. **리덕스랑 똑같음**

```tsx
interface State {
  counter: number;
}

type Action = { type: "INCREMENT" } | { type: "DECREMENT" };

function counterReducer(state: State, action: Action): State {
  switch (action.type) {
    case "INCREMENT":
      return { counter: state.counter + 1 };
    case "DECREMENT":
      return { counter: state.counter - 1 };
    default:
      return state;
  }
}
```

요로코롬 리듀서 함수 만들어 주고

```tsx
import React, { useReducer } from "react";

const CounterComponent: React.FC = () => {
  const [state, dispatch] = useReducer(counterReducer, { counter: 0 });

  const increment = () => {
    dispatch({ type: "INCREMENT" });
  };

  const decrement = () => {
    dispatch({ type: "DECREMENT" });
  };

  return (
    <div>
      <p>Count: {state.counter}</p>
      <button onClick={increment}>Increase</button>
      <button onClick={decrement}>Decrease</button>
    </div>
  );
};

export default CounterComponent;
```

요로코롬 쓰면 된다.

뭔가 react에서 점차점차 제공되는 hook들이 엄청 좋아지는거같다.
