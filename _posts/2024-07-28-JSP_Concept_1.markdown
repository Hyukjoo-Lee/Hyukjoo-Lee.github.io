---
title: "{ CS } JSP_1"
date: 2024-07-28 22:00:00 +07:00
tags: [JSP, Servlet, Response, Request]
---

# JSP 와 Servlet

웹 애플리케이션 개발에 사용되는 자바 기반 기술입니다. 이 둘은 서버 측에서 실행되며, 웹 브라우저에 동적 콘텐츠를 제공하는 데 사용됩니다.

## 자바 서블릿 (Servlet)

### 개념

서블릿은 자바 클래스로, 웹 서버에서 실행되며 클라이언트의 요청(HTTP 요청)을 처리합니다.

### 역할

- 주로 HTTP 요청을 처리하고, 요청에 대한 응답을 생성하는 역할을 합니다. 자바의 장점은 플랫폼 독립성을 갖추고 있습니다.
- 플랫폼 독립성이란 소프트웨어에 포함된 코드가 운영 체제에 종속되지 않고 여러 환경에서 동일하게 동작할 수 있는 능력을 의미합니다.
- "Write Once, Run Anywhere"

### 사용 방법

- 클라이언트가 웹 브라우저를 통해 요청을 보내면, 웹 서버는 해당 요청을 서블릿으로 전달합니다.
- 서블릿은 요청을 처리한 후, HTML이나 JSON 등의 형태로 응답을 생성해 클라이언트에게 보냅니다.

### 코드 예제

```java
import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;

public class HelloWorldServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 응답의 유형을 HTML으로 명시
        response.setContentType("text/html;chatset=UTF-8");
        // 모든 print methods를 실행할 수 있는 클래스
        PrintWriter out = response.getWriter();
        out.print("<h1>Hello, World!</h1>");
    }
}
```

## JSP (Java Server Pages)

### 개념

JSP는 HTML 내에 자바 코드를 포함하여 웹 페이지를 동적으로 생성할 수 있도록 해주는 기술입니다.

### 사용 방법

- JSP 파일은 HTML 코드와 자바 코드를 혼합하여 작성됩니다.
- 클라이언트가 JSP 페이지를 요청하면, 웹 서버는 JSP 페이지를 서블릿으로 변환하여 실행하고, 결과를 클라이언트에게 전달합니다.

### 코드 예제

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <title>JSP</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <%
       int ran = (int)(Math.random()*9+1);
       오늘의 행운의 숫자는 <%=ran %>입니다.
    %>
</body>
</html>
```

## 주요 차이점

- **서블릿**은 자바 코드 안에 HTML이 포함된 형태로 작성되며, 주로 비즈니스 로직을 처리하는 데 적합합니다.
- **JSP**는 HTML 안에 자바 코드가 포함된 형태로 작성되며, 주로 프리젠테이션 로직을 처리하는 데 적합합니다.

## 결합 사용

일반적으로 서블릿과 JSP는 함께 사용됩니다. 서블릿은 요청을 받아 비즈니스 로직을 처리하고, 결과를 JSP로 전달하여 JSP가 HTML을 생성해 클라이언트에게 응답하는 방식으로 사용됩니다. 이 방식은 MVC(Model-View-Controller) 패턴을 구현하는 데 유용합니다.
