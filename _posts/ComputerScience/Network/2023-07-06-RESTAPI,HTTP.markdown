---
layout: post
title: "REST API"
category: Study
description: >
  REST API에 대해 알아보자
image:
  path: /assets/img/network/restapi.png
tags: ComputerScience
permalink: Study/ComputerScience/restapi
---



# HTTP 메서드와 REST API
{:.no_toc}

* this list will be replaced by the toc
{:toc}

## HTTP 메서드

- GET ,HEAD ,POST ,PUT, DELETE , CONNECT , OPTIONS , TRACE
- 상황에 맞쳐서 HTTP 메서드를 사용
- 프론트엔드 개발자가 사용시 백엔드 개발자나 서버에서 미리 약속된 메서드로 등록

## REST API

- 위에 명시된 메서드들을 아름답게 만드는것을 API (~~아름답지않음~~)
- 예제 https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch
- PUT -> 전체 수정 , PATCH -> 부분 수정
  > **프론트와 백엔드 간에 약속이라 정해진 틀이없음 전부 PUT을 써도 되고 PATCH를 써도되고 전부 GET을 써도 됨 하지만 가장 중요한것은 상황마다 필요한 메서드를 사용하는것이 베스트**
- ~~REST API 에는 정해진 룰이 있는거같다 피곤하게~~
- HEAD 메서드는 라우터가 존재 하는지 안하는지확인하는 메서드 -> 응답 body를 보내주지않음
- OPTIONS : CORS (**\*Cross-Origin Resource Sharing)**설정 -> 다른사이트에 요청을 보낼때에 옵션설정을 함\*
- TRACE 메소드는 프록시 서버 추척 + 최종 서버가 받은 메세지를 알려줘서 프록시 서버가 어떤 헤더를 바꿨는지 알 수 있게 함

> TMI : GET 보다 POST 가 보안이 뛰어나다? 보안쪽은 **HTTPS 유무** 어차피 와이어샤크 하나면 다털린다

- 안전한 메서드 : 서버한테 상태를 바꾸지 않는 메서드
  - GET,HEAD,OPTIONS,TRACE
  - GET을 예로 들면 서버로부터 데이터를 가져오는것이기때문에 서버가 달라지는것이없음
- 멱등성 메서드 : 위 메서드를 포함한 PUT ,DELETE 한번만 실행되므로 비교적 안전한 메서드
  - 멱등성 :**연산을 여러 번 하더라도 결과가 달라지지 않는 성질**
- 위에 따른 이유로 안전한 메서드인 GET ,HEAD 캐싱을 지원한다.
- PUT , PATCH를 구별할때 멱등성을 가지냐 안가지냐에 따라 구별가능

## 상태 코드(1XX, 2XX)

---

- 브라우저가 서버한테 요청 서버가 브라우저한테 응답 서버로부터 실패나 에러 처리를 상태 코드로 응답
- 200~300 번대는 주로 요청응답 성공
- 400~500 번대는 에러 400번대는 주로 클라이언트 에러이며 500번대 에러는 주로 서버에러이다
  - 400번대에러는 프론트쪽 문제 500번대는 백엔드문제
- 100 번 = 스트리밍이나 계속 해라 ~
- 102 번 = 프로토콜을 전환하러는 요청 등
- **200 번** = 요청 성공 응답 개시 ~
- **201 번** = 메서드별 성공 상태코드 암튼 성공함 대충 위랑같음
- 204 번 = 요청을 보냇으나 바디는 비어있을때

> 💡 101 번 206번 등 잘안쓰는 상태 코드는 안건드리는게 가능하면 좋음 -> 서로 swagger 나 등 기존 약속과 틀릴수있기

## 상태 코드(3XX )

---

- Disable cache : 캐시를 유효화 하는 옵션 -> 매번 새로운 캐시를 불러오도록
- preserve log : 새로고침을 해도 남아있도록 하는 옵션
- [localhost](http://localhost/) 로 보낸것은 와이어샤크에서는 따로 특별한 설정을 하지않는이상 뜨지않음
- 300번 코드 : 요청에 대한 응답이 애매할때 300번 코드를 보냄
- **301번 코드 :** 서버에서 리다이렉트 처리를 했을때 브라우저가 알아서 그 주소로 이동 -> 주소는 Location header에 담겨있음 -> 도메인이 바뀌었을때!
- **302번 코드 :** 페이지가 수정중일때 다른곳에 계시다 오세요 ~ 할 때 보내주는 코드

## 에러 상태 코드(4XX, 5XX)

---

### 400에러

- 401 코드 : 로그인 해야지만 들어갈수 있는 페이지 => 토큰이없다
- 403 코드 : 권한이나 등급같은것이 없을때 접근할수 없는 페이지 -> 생각보다 요즘 안씀 -> 해커입장에서 승부욕을 자극시킬 수 있기때문에 생각 보다 요즘 안씀ㅎ 차라리 404코드를 대신씀
- 404 코드 : 서버에 그에 해당하는 주소가 없을때
- 405 코드 : 메서드 오류
- 406 코드 : 컨텐트 협상 실패
- 408 코드 : 요청 시간 오버
- 413 코드 : 서버 데이터 업로드시 용량이 너무 클때
- 414 코드 : 주소 길이 너무 길때에
- 418 코드 : 나는 주전자입니다 -> 개발자식 유머 노잼
- 451 코드 : 법적인 이유로 불가

### 500에러

- 500 코드 : 뭐 할지 애매하다
- 501 코드 : 아직 api 못만들었음
- 502 코드 : 서버앞 프록시에서 에러 AWS nginx 설정 등
- 503 코드 : 의도적일수있고 아닐 수도있음 예) 서버 점검 , 서버터짐 등
- 504 코드 : 502랑 비슷

## 컨텐츠 협상과 MIME Type

---

- HTTP (Hyper Text Transfer Protocol)
- HTTP 는 header와 , body ( 본문 , content , payload )로 구성 -> 요즘은 본문 , content 말고 payload라 많이 말함
- Header 는 key와 value 로 이루어짐
  GET /users , Host: [naver.com](http://naver.com/) ,HTTP/1.1
- **헤더는 한글이 안됌! 쿠키를 넣을때 보통 실수를 많이함 Cookie : name = '은석' 이렇게 넣는데 인코딩 해서 넣어야댐 ex) encodeURLComponent('은석')**

## 컨텐츠 협상

### To . 서버

- Accept : 프론트가 무엇을 원하는지 볼수 있음 -> MIME TYPE 형식은 대분류/확장자 예를 들어 image/png , image/ jpeg , video/mp4 application/xml
- 위에 분류를 보면 뒤에 q가붙는데 우선순위 높을수록 선호
- Accept-Language : 문서 언어 \* 문서언어코드는 (ISO Language Code Table)가서 보기
- Accept-Encoding : ex) gzip text다 보니까 용량이 너무 뻥튀기 됌 -> header body가 text 왓다갓다 하기 부담스러워 압축방식을 설정-> 브라우저가 알아서 풀어줌
- 서버 주도 협상 -> 클라이언트에서는 받고싶은 형태를 여러개 보내면 서버가 그중 하나를 골라 그런 형태로 다시 보내줌
- 클라이언트 주도 협상 -> 잘안함 2번 왓다갓다 해야되므로
- Accept-Charset : utf-8 , ascii , euc-kr 문자 방식

### FROM .서버

- Content-Type : text/html , Content-Language : ko, Content-Encoding : br 보통 이렇게 응답을 같이줌
- content-length : 2357 (byte)
- Charset 같은경우 content-type에 붙어있음
- Connetion : Keep-Alive -> HTTP/1.0 를 위해 존재 요청하나 보내면 3way handshake 가 일어나는데 자주 존재하는 이러한 로직때매 생김 keep-Alive 가 기본값 timeout = 5 (초) => 5초안에 3way handshake가 열렸던것을 재사용
- Date : 서버 시간 알려줌 ! 티케팅 꿀팁 ㅋ 인가 ? 대신 생성된 시간 기준으로 작성됌
- Transfer-Encoding : Content-Encoding이랑 같이 보면 좋음-> 예를들면 chunked 일 경우에 짤라서 보낸다는 뜻
- **Authorization**
- 값에 barear ,basic , Digest 등이 붙으며 뒤에 토큰같은걸 붙여서 넘김
- 기타 헤더 : 기타헤더에 요청 방식에따라 헤더에 붙는것이 다름 301 , 503 등
- referer(오타아님 제작자 스펠링실수임) -> 이전페이지가 어디인지 알려줌 = 마케팅 전략 등 유효
- 커스텀 헤더 : mdn에 없는 것이 header 들어있으면 커스텀 헤더 ex) 'X-머시기머시기' 암묵적인 룰
