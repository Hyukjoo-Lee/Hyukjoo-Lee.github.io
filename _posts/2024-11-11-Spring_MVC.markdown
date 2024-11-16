---
title: "{ Spring } Spring MVC Model"
date: 2024-11-11 03:00:00 KST
tags: [Spring, MVC]
---

# MVC 패턴

- MVC (Model-View-Controller)

- JSTL + EL 문법 (Model 2)
- Model1 vs Model2

  - jsp 페이지가 단독으로 요청/응답
  - model2: mapping 주소 url 패턴으로 <-> 라우팅주소
  - servlet 포인트로 view page 로 응답을 해줌
    view page (jsp page)

  model1 에서는 form 에서 submit 을 클릭시 jsp 가 받지만
  model2 에서는 servlet (controller) 가 받음

- EL(Expression Language)
  jsp 에서 스크립트릿 (<% %>) 을 대체할 수 있는 표현식
  좀 더 자연스러운 형태 (액션태그)의 속성 값을 지정하고 객체의 메소드에 지정함으로서 코드가 훨씬 간결해짐

객체명.메소드
<%aCustomer.getAddress()> => ${aCustomer.address}
${num}

- <%= %> 값을 출력 표현하는데 jsp 의 스크립트 요소를 보완하는 역할을 한다. (간결/가독성 좋아짐)
- empty 연산자
-

## JSTL

xml 기반임

```java
// prefix = c: core 의 약자
<c:if test="empty $c.getEmail">
</c:if>

<c:catch>

// switch statement
<c:choose>
<c:when ~>
</c:choose>

<c:out value="출력할 값">
```

클라이언트 요청을 컨트롤러(servlet) 가 받고 컴트롤러가 view(jsp page) 로 응답을 반환함
controller (servlet) <-> model (javabean) <-> db
오청처리 데이터 접근 비즈니스 로직을 포함하고 있는 컨트롤러와 뷰는 엄격히 구분된다

xml 파일에서 db 로 연결하는 방식 - 보안의 이유

### 서블릿(servlet)



## DBCP API 를 이용한 커넥션 풀 사용

- tomcat server version 9 이후는 내장되어 있지만 아래로는 수동으로 import 해와야 한다.
- DBCP 커넥션 풀이란
- 자바 어플리케이션과 DBMS 사이에 JDBC 를 연결하기 위한 커넥션 풀이다

## 방법

- 웹 브라우처 요청 => 할당된 커넥션 객체가 있는지 확인 => 커넥션 풀에 할당된 커넥션 객체가 있으면 가져와서 객체 사용 => 객체 변환에서 return

- 접속 유저가 많은 트래픽의 경우에 확실히 선호된다
- 
