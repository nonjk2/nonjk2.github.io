---
layout: post
title: "React Router Dom"
category: frontend
description: >
  React Router Dom 을 알아봅시다잉
image:
  path: /assets/img/frontend/react/ReactrouterDom.png
tags: React TIL
permalink: frontend/React/ReactrouterDom
---
<!--more-->

# React - Router - Dom
{:.no_toc}

* this list will be replaced by the toc
{:toc .large-only}

요즘 Next.js 를 기본으로 다들 써가지고 CRA를 쓰지 않는것 같지만 나같이 이제 리액트를 시작하는 사람에겐 혹은 CRA를 쓰는 사람들에겐 라우터, 라우팅은 필수다

React Router DOM을 사용하면 웹 앱에서 동적으로다가 라우팅을 구현할수있다.

## Single Page Application (SPA)

---

리액트는 SPA이기 때문에 하나의 index.html 템플릿 파일을 가지고 있다. 이 하나의 템플릿에 자바스크립트를 이용해서 다른 컴포넌트를 이 index.html 템플릿에 넣어가지고 페이지를 바꿔준다. 이 때 React Router Dom 라이브러리가 새 컴포넌트로 라우팅/탐색을 하고 렌더링하는데 쓰인다.

![Untitled](assets/img/frontend/react/route1.png)

## React Router Dom 설치하기

```bash
npm install react-router-dom --save

yarn add react-router-dom
```

![Untitled](assets/img/frontend/react/route2.png)

## BrowserRouter

BrowserRouter 는 HTML5의 History API를 사용하여 페이지를 새로고침하지 않고도 주소를 변경할 수 있도록 해준다.

이 history 객체로 말할꺼같으면

```js
{
  length: number,   // 브라우저 기록의 항목 수
  action: string, // 현재 액션 (PUSH, REPLACE, POP)
  location: {
    pathname: string,  // 현재 경로
    search: string,    // 쿼리 문자열
    hash: string,      // 해시 값
    state: any,        // 상태 객체
  },
  push(path: string, [state: any]): void,      // 새로운 URL로 이동하고 항목 추가
  replace(path: string, [state: any]): void,   // 현재 URL을 대체하고 기록 수정
  go(n: number): void,                         // 현재 페이지에서 n개의 페이지 전/후로 이동
  goBack(): void, // 이전 페이지로 이동
  goForward(): void,  // 다음 페이지로 이동
}
```

요렇게 있단다

`push()`, `replace()`, `go()`, `goBack()`, `goForward()`와 같은 메소드를 사용하여 탐색(navigation) 하고 조작할수 있다.

[History API - Web API | MDN](https://developer.mozilla.org/ko/docs/Web/API/History_API)

![Untitled](assets/img/frontend/react/route3.png)

`Routes` : 앱에서 생성될 모든 개별 경로에 대한 컨테이너/상위 역할을 한다.

`Route` : `Route는` 단일 경로를 만드는데 쓴다.두가지 속성이 있음

- `path` : 원하는 컴포넌트의 URL 경로를 지정 이경로 이름을 원하는대로 정할 수 있다 . 위에서 첫 번째 경로 이름이 /(백슬래시) 임을 알 수 있다. `/` 가 경로인 컴포넌트는 앱이 처음 로드될 때에 가장먼저 랜더링 된다.
- `element` : 경로에 맞게 랜더링 되어야할 컴포넌트를 넣어주면 됨

## **React-Route-Dom API**

### **중첩 라우팅(nested routing)**

![Untitled](assets/img/frontend/react/route4.png)

이것은 **React Router**의 가장 좋은 기능중 하나다. 대부분의 레이아웃은 URL의 세그먼트에 연결된다.

### **Outlet**

![Untitled](assets/img/frontend/react/route5.png)

v6 로 업그레드 되면서 나온 `Outlet`

- 자식 경로 요소를 렌더링하려면 부모 경로 요소에서 <Outlet>을 사용하면 된다. 이렇게 하면 하위 경로가 렌더링될 때 중첩된 UI를 보여줄수 있다.

- 만약 부모 라우트가 정확 하지않으면 index 라우트를 랜더링 하거나 아무것도 없으면 아무것도 랜더링 하지않는다

`react-router-dom`에서 임포트.

### **useNavigate**

![Untitled](assets/img/frontend/react/route6.png)

navigate에

replace: true를 사용하면 navigate에 적힌 주소로 넘어간 후 뒤로 가기를 하더라도 방금의 페이지로 돌아오지 않고 메인 페이지 ("/")로 돌아오게 한다.

false가 디폴트고 뒤로 가기 가능.

![Untitled](assets/img/frontend/react/route7.png)

- 넷플릭스에서 검색하기를 누르고 뒤로가기를 하면 바로 뒷페이지로 가는게 아니고 검색하기 창으로 넘어감.

- 이렇게 구현하기 위해서는 relace 옵션을 사용.

위 예제는 history를 쌓을 때 첫 번째 글자일 때만 쌓고 그다음 글자부터는 안 쌓게 한다.

### **useParams**

`useParams Hook`는 <Route path>와 일치하는 현재 URL에서 동적 매개변수의 키/값 쌍 객체를 반환한다.

:style 문법을 path 경로에 사용해도 `useParams()`로 읽을 수 있음!.

yeah )

![Untitled](assets/img/frontend/react/route8.png)

### **useLocation**

`useLocation hook`을 이용해서 현재 URL 정보를 가져올 수 있다.

> `pathname`은 현재 URL의 경로를 나타냄
> `search`는 쿼리 매개변수를 포함한 검색 부분을 나타냄
> `hash`는 URL의 해시 부분을 나타낸다
> `state`는 React Router의 history 객체를 통해 전달된 상태를 나타낸다.

이녀석은 현재 위치 객체를 반환한다 현재 위치가 변경될 때마다 일부 side effect를 수행하려는 경우에 유용할 수 있다.

![Untitled](assets/img/frontend/react/route9.png)

### **useRoutes**

`useRoutes Hooks`는 `Routes` ,`Route` 와 기능적으로 비슷함 대신 요소 대신 JavaScript 객체를 사용하여 경로를 정의.

![Untitled](assets/img/frontend/react/route10.png)
도움된사이트[https://itchallenger.tistory.com/566](https://itchallenger.tistory.com/566)
