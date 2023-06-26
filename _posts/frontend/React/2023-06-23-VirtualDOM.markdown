---
layout: post
title: "React Virtual DOM"
subtitle: "just do it"
category: frontend
description: >
  React의 virtualDOM
image:
  path: /assets/img/frontend/react/virtual.png
tags: React TIL
---
<!--more-->

# React Virtual DOM
{:.no_toc}


리액트의 엄청난 장점중 하나인 **리액트 가상돔** 은 리액트가 넘버원이 되는데 일조한 가장 큰 장점중 하나다.
브라우저에 페이지를 그려주는 속도가 엄청나게 빠르기 때문이다.

가상돔에 대해 알아보고 이것의 장단점을 알아보자

일단 **React Virtual DOM**에서 DOM을 알아보자

## DOM

- "Document Object Model"의 약자로 웹페이지의 구조를 나타내는 인터페이스다.
트리구조 , 객체형식의 형태로 표현되고 자바스크립트같은 언어가 웹페이지의 요소에 접근을 가능케 한다.


웹 페이지는 HTML 태그를 통해 생성되고, 각 태그들은 DOM 트리 노드(node)로 표현된다.

### DOM의 구조

- Document
  - HTML
    -  HEAD
        - TITLE
    - BODY
        - H1
        - P

돔트리는 아래처럼 구성
```html
<!DOCTYPE html>
<html>
<head>
    <title>Page Title</title>
</head>
<body>
    <h1>My First Heading</h1>
    <p>My first paragraph.</p>
</body>
</html>

```

## 그렇다면 Virtual DOM은 무엇일까

DOM을 직접 조작하는 것은 비용이 많이 들고 느릴 수 있기 때문에, 가상의 DOM 트리를 두어 변경사항을 먼저 적용하고 그 다음 가상돔을 실제 DOM에 적용하는 방법을 사용한다. 

Virtual DOM 작동 원리 3단계.

1. **렌더링 단계** 
- 웹,앱 의 상태가 변경되면 전체 UI를 Virtual DOM에 렌더링합니다. <- 이과정은 실제 DOM에 적용하는것이 아니라 빠르게 단계가 지나간다.

2. **Diffing 단계**
- 이제 새로 렌더링된 Virtual DOM과 이전 Virtual DOM을 비교한다 이것을 한다고한다 .여기서 React는 두 트리의 차이점을 계산하여 그 결과를 찾는다.
  
  -  리액트는 항상 두개의 가상돔 객체(렌더링 이전 화면 구조를 나타내는 가상돔 , 렌더링 이후에 보이게 될 화면 구조를 나타내는 가상돔 ) 가 존재한다.
  -  리액트는 `state`가 변경되면 re-rendering이 발생하는데 이때마다 새로운 내용이 담긴 가상돔이 생긴다.
  -  이때에 생긴 가상돔 객체 두개를 비교해서 어떤 Elment가 달라졌는지 비교하는것이 **Diffing**
  -  디핑알고리즘은 두개에 IF가 있다.
     -  두 개의 서로 다른 타입의 요소는 서로 다른 트리를 만들 것.
     -  개발자는 key prop을 통해, 여러 렌더링 사이에서 어떤 자식 요소가 변경되지 않아야 하는지 표시할 수 있어야할 것.
  -  ### [디핑알고리즘](https://ko.legacy.reactjs.org/docs/reconciliation.html#the-diffing-algorithm)에 대해 짧게 요약하자면
  > **주요단계**
  >-  `Tree diff`: 두 트리의 루트 노드를 비교한다. 노드의 타입이 다르면, React는 이전 트리를 버리고 완전히 새로운 트리를 구성한다.
  >-  `Component diff`: 같은 타입의 사용자 정의 컴포넌트를 비교할 때, React는 이전 컴포넌트의 인스턴스를 유지하고, 새로운 컴포넌트의 render 메소드만 호출하여 결과를 비교한다.
  >-  `Element diff`: 노드의 타입이 같을 경우, React는 노드의 속성을 비교하여 변경된 속성만을 업뎃한다.

1. **패치 단계**
- 계산된 차이점을 실제 DOM에 적용한다. 
- 변경사항이 최소화되어 효율적이다.

>Virtual DOM을 이용하면 실제 DOM을 덜 조작하고, 변경사항을 효율적으로 관리하면서 애플리케이션의 성능을 최적화할 수 있다. 또한 이러한 장점을 이용해 리액트 기반 아키텍처와 결합하면 [스토리북](https://storybook.js.org/)처럼 UI의 각부분을 독립적으로 개발하고 관리할 수 있어 개발의 편의성이 상승한다 

## Virtual DOM이 사용되는과정

브라우저 워크플로우는 
HTML 파싱 -> CSS 파싱 -> 랜더 트리 생성 -> 레이아웃 -> 페인팅 과정을 거친다.

이과정에서 랜더 트리를 기반으로 웹페이지가 그려지는데 페이지의 상태가 달라지면 처음부터 파싱하고 생성해 기존 작업을 반복한다.

**Virtual DOM**에서는 페이지의 상태가 변경되면 이를 반영하기 위해 전체 UI를 다시 그리는 대신 먼저 Virtual DOM에 변경사항을 적용한다.

Virtual DOM에 변경사항이 적용되면, 새로운 Virtual DOM과 이전의 Virtual DOM을 비교하여 두 트리 간의 차이점을 계산하고,

이 차이점을 기반으로 실제 DOM을 업뎃한고.위 과정에서는 실제 DOM이 최소변경만 변경한다.

정리해보면 아얘 새로그리지않고 비교분석해 차이점만을 변경한다는 것 엄청난 장점이라 할 수 있다.


--- 
## 정리

- React의 가상 돔은 웹 페이지의 렌더링 속도를 크게 향상시키고 DOM의 비효율성 보완

- DOM은 웹 페이지의 구조를 나타내는 인터페이스로, 자바스크립트 등을 통해 웹 페이지의 요소에 접근할 수 있게 해준다. 하지만 DOM의 변경사항을 반영하는 것은 연산 비용이 많이 들고 느림.

- 이것때문에 나온게 가상 돔. 
  - 변경사항을 실제 DOM에 직접 반영하는 대신, 가상 돔에 반영하여 두 돔 사이의 차이점을 계산하고, 이를 실제 DOM에 적용하는 방식. 이 과정은 렌더링, Diffing, 패치 세단계가 존재.

- 브라우저의 전통적인 렌더링 프로세스는 HTML 파싱, CSS 파싱, 랜더 트리 생성, 레이아웃, 페인팅 등의 단계가있다. 가상 돔을 이용하면 실제 DOM의 최소한의 변경만으로 웹 페이지를 업데이트할 수 있다. 결론 리액트 짱짱맨.
