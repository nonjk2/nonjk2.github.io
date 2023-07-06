---
layout: post
title: "react-query"
category: frontend
description: >
  왜 지금까지 안쓰고 지냈을까 외면한걸까 그건아닌데 당장 기술해서 바로 사용하자
image:
  path: /assets/img/frontend/react/reactquery.png
tags: React TIL
permalink: frontend/React/reactquery
---

# react-query
{:.no_toc}

* this list will be replaced by the toc
{:toc}

## `React Query` 개요

> `React Query`는 JavaScript 라이브러리로, 주로 React에서 비동기 데이터를 가져오고 (fetch), 캐시하고 (cache), 동기화하고 (synchronize) 상태를 관리하는 (state management) 작업을 매우 쉽게 할수 있다.


<!--more-->
## 왜 `React Query`를 사용해야 하는가?

> `React Query`를 쓰면 여러 가지 힘들었던 상황을 한번에 해결 할수 있댜.

- **캐싱 및 데이터 동기화** : 가져온 데이터를 자동으로 캐시하고 데이터가 바뀌면 자동 업뎃한다.
- **백그라운드 업데이트**: 페이지가 포커스되있지 않아도 데이터를 자동업뎃
- **Stale-While-Revalidate**: 캐시된 데이터를 먼저 보여주고, 백그라운드에서 데이터를 업데이트하는 방식이다 !중요

- **쿼리 결과 공유 및 병합**: 같은 쿼리를 여러 곳에서 사용하더라도, `React Query`는 쿼리의 결과를 한 곳에서 관리해 중복요청을 피할 수 있다.

- **쿼리 사이드 이펙트 관리**: 쿼리의 데이터가 변경될 때마다 특정 동작을 할 수 있다.

## `React Query`의 장점

- **서버 상태에 대한 최적화** : `React Query`는 백엔드 API에 저장된 데이터의 클라이언트 측 캐시를 관리해 네트워크 요청과 백엔드의 로드를 줄인다.

- **성능 개선** : `React Query`는 캐싱, 데이터 동기화, 백그라운드 업데이트 등을 통해 애플리케이션의 성능을 크게 향상시킨다.

- **개발 시간 절약** : 복잡한 상태 관리 로직을 작성하는 시간에, `React Query`를 사용함으로서 비지니스 로직에 집중할 수 있다.

- **유연성**: REST, GraphQL, gRPC 등 어떤 종류의 API와도 잘 작동됨.

## `React Query` 설치 및 설정

자 그럼 설치과정과 직접 설정해보자

### 설치

```shell
npm install react-query
```

끝

### React Query 설정

: `QueryClient`와 `QueryClientProvider`
`React Query`는 `QueryClient` 객체를 사용하여 쿼리를 관리한다. 이 객체는 `QueryClientProvider` 컴포넌트를 통해 하위 컴포넌트에 전달된당.

```jsx
import { QueryClient, QueryClientProvider } from "react-query";

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <내부코드 />
    </QueryClientProvider>
  );
}

export default App;
```

### React Query Devtools

쿼리 상태를 실시간으로 확인하고 디버깅할 수 있는 React Query Devtools

> 개발모드에서만 로드되도록하는게 좋다

```shell
npm install react-query/devtools
```

설치하고

```jsx
import { ReactQueryDevtools } from "react-query/devtools";

<ReactQueryDevtools initialIsOpen={false} />; //내부코드안에넣자
```

## `Query` 와 `Mutation`

React Query에서는 `Query`와 `Mutation` 요것들을 이용해서 데이터를 관리한다.

### `Query` (Read)

`Query`는 데이터를 읽기위한 작업이다. 주로 GET 요청에 해당하지만, GET 외의 요청도 가능. 원격 데이터 가져오고, 자동으로 캐시하고, 데이터가 변경될 때마다 자동으로 동기화도,, 다해준다.

Query를 사용하는 방법은 `useQuery` 훅을 사용하면 된다.

코드를 보자

```jsx
import { useQuery } from "react-query";

function App() {
  const { isLoading, error, data } = useQuery("posts", fetchPosts);

  if (isLoading) return "로딩중...";
  if (error) return "에러발생 " + error.message;

  return (
    <div>
      {data.map((post) => (
        <p key={post.id}>{post.title}</p>
      ))}
    </div>
  );
}
```

중요하게 봐야댈것은 `useQuery`의 첫 인자인 **key**다

이 **key**는 refetching 하는데에도 쓰이고 캐싱을 처리하는데에도 쓰이고
앱 전체에서 이 쿼리를 공유하는 방법으로도 쓰인다.

- useQuery를 통해 얻은 결과물은 객체다.
- 안에 isLoding , errro ,data 그외 등등을 볼수있다.

### Mutation (Create , Update , Delete)

`Mutation` 데이터를 변경하기 위한 함수. 주로 POST, PUT, DELETE 등의 요청에 해당한다. `Mutation`을 이용해 원격데이터를 변경하고 , 변경 후에 원하는 동작 (예: 쿼리 데이터 재요청)을 할 수 있다.

`Mutation`을 사용하는 방법역시 `useMutation` 훅을 쓴다.

위와 똑같이 한번 봅세다

```jsx
import { useMutation, useQueryClient } from "react-query";

function App() {
  const queryClient = useQueryClient();
  const mutation = useMutation(addPost, {
    onSuccess: () => {
      queryClient.invalidateQueries("posts");
    },
  });

  return (
    <div>
      <button
        onClick={() => {
          mutation.mutate({ id: Date.now(), title: "New Post" });
        }}
      >
        Add Post
      </button>
    </div>
  );
}
```

`queryClient.invalidateQueries('posts')`는 `posts` 쿼리가 다시 실행되게 만들어준다. 이렇게해서 변경된 데이터를 즉시 반영할 수 있다.

## `React Query` 고오오오오급 기능

**React Query의 고급 기능**

리액트 쿼리는 위에서 설명한 기능들 , 데이터 관리 능력들 말고도 여러기능들이있다.

자 하나하나 살펴봅시다잉

### Prefetching

Prefetching은 미리 데이터를 가져와서 캐시에 저장하는 기능이다.

무언가를 하기전에 미리 데이터를 땡겨와서 수행하기 편하게 할 수 있다. UX증가를 기대해볼수 있다 ㅎㅎ

예를 들어, 사용자가 버튼을 클릭하여 새 페이지로 이동할 때, 그 페이지에서 필요한 데이터를 미리 가져와 페이지 로딩 시간을 크게 줄일 수 있다.

예제를 보자

```jsx
import { useQueryClient } from "react-query";

function SomeComponent() {
  const queryClient = useQueryClient();

  return (
    <button
      onMouseOver={() => {
        queryClient.prefetchQuery("todos", fetchTodos);
      }}
    >
      다음 버튼
    </button>
  );
}
```

되게 간단하다

로직은 마우스로 hover하기만해도 미리 데이터를 땡겨와서 다음 랜더를 위한 데이터를 준비한다

### Infinite Queries

Infinite Queries는 페이지네이션된 데이터를 무한히 가져올 수 있게 해주는 기능이다. 이 기능은 사용자가 스크롤을 내리면서 추가 데이터를 계속해서 가져올 수 있는 UI를 구현하는데 매우매우 좋다. 미리 알았다면,,,

`useInfiniteQuery` 훅을 사용해 이 기능을 사용할 수 있다.

```jsx
import { useInfiniteQuery } from "react-query";

async function fetchTodos(pageParam = 0) {
  const res = await fetch(`/api/todos?page=${pageParam}`);
  return res.json();
}

function Todos() {
  const { data, fetchNextPage, hasNextPage, isFetchingNextPage } = useInfiniteQuery(
    "todos",
    fetchTodos,
    {
      getNextPageParam: (lastPage, allPages) => lastPage.nextPage,
    }
  );

  return (
    <>
      {data.pages.map((page, index) => (
        <div key={index}>
          {page.todos.map((todo) => (
            <p key={todo.id}>{todo.title}</p>
          ))}
        </div>
      ))}
      <div>
        <button onClick={() => fetchNextPage()} disabled={!hasNextPage || isFetchingNextPage}>
          {isFetchingNextPage ? "로딩중..." : hasNextPage ? "더보기" : "마지막입니다"}
        </button>
      </div>
    </>
  );
}
```

`useInfiniteQuery`는 페이지네이션된 'todos' 데이터를 계속 가져온다. 각 페이지의 데이터는 `data.pages` 배열에 저장되며, `fetchNextPage` 함수를 호출해서 다음 페이지의 데이터를 가져온다. `hasNextPage`와 `isFetchingNextPage`는 다음 페이지의 데이터를 가져올 수 있는지, 그리고 다음 페이지의 데이터를 현재 가져오고 있는지를 boolean 값이다.

### Query Invalidation

`Query Invalidation`은 서버의 데이터가 변경되었을 때 그 데이터를 캐시에서 제거하거나 업데이트하는 기능이다. 항상 최신상태 유지를 위해 사용한다.

React Query는 쿼리를 최신상태로 할수있는 방법이 두가지 있다

- `invalidateQueries` 메서드를 이용하는 방법
- `mutate` 또는 `mutateAsync` 메서드의 `onSuccess` 콜백에서 `invalidateQueries`를 호출하는 방법

밑의 코드는 위에서 봤던 invalidateQueries다

```jsx
import { useMutation, useQueryClient } from "react-query";

function TodoAdder() {
  const queryClient = useQueryClient();
  const mutation = useMutation(addTodo, {
    onSuccess: () => {
      queryClient.invalidateQueries("todos");
    },
  });

  return <button onClick={() => mutation.mutate({ title: "dev choi" })}>생성생성</button>;
}
```

### Query Cancellation

얘는 진행 중인 쿼리를 취소하는 기능이다. 불필요한 네트워크 요청을 줄일 수 있고, 앱을 사용하는사람이 버튼 막 계속누를때 방지할 수 있는방법.

이 메서드는 특정 쿼리 키에 대한 쿼리를 취소하거나, 모든 쿼리를 취소할 수 있다.

```jsx
import { useQueryClient } from "react-query";

function CancelButton() {
  const queryClient = useQueryClient();

  return <button onClick={() => queryClient.cancelQueries("todos")}>쿼리 취소 빠튼</button>;
}
```

지금까지 react-query를 알아보았는데 vite를 생성할때 보이는 swr역시 이 react-query를 알면 사용하는데에 큰 어려움이 없다고 한다.

다음은 swr을 공부해서 기술해봐야겠다.
