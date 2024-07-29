---
title: "{ CS } JSP_3"
date: 2024-07-30 01:00:00 +07:00
tags: [Session]
---

# Session

- 세션(Session)은 웹 애플리케이션에서 사용자의 상태를 유지하고 데이터를 저장하는 데 유용한 방법입니다.
- 로그인 페이지에서 세션을 사용하는 것은 매우 일반적인 작업입니다.
- 사용자가 로그인할 때 세션을 통해 로그인 상태를 유지하고, 인증된 사용자에게만 특정 자원에 접근할 수 있게 할 수 있습니다.
- session 으로 받지 않고 단순히 request.getAttribute("id") 로 받는다면 url로 재접속 시에 null 로 표시됩니다.

### 코드 예시

#### 로그인 서블릿 (로그인 검증 및 세션 설정)

```java
@WebServlet("/Login.do")
public class LoginServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 아이디와 비밀번호를 요청에서 가져옴
        String id = request.getParameter("id");
        String password = request.getParameter("password");

        // 간단한 로그인 검증
        if ("bigie2".equals(id) && "0000".equals(password)) {
            // 세션을 생성하거나 기존 세션을 가져옴
            HttpSession session = request.getSession();
            // 사용자 아이디를 세션에 저장
            session.setAttribute("id", id);
            // 대시보드 페이지로 리다이렉트
            response.sendRedirect("/myPage.jsp");
        } else {
            // 로그인 실패 메시지를 설정하고 로그인 페이지로 포워드
            request.setAttribute("message", "로그인에 실패하였습니다.");
            request.getRequestDispatcher("/login.jsp").forward(request, response);
        }
    }
}
```

#### 로그인 후 페이지 (성공 시)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>대시보드</title>
</head>
<body>
    <%
    // 세션에서 사용자 아이디를 가져옴
    String userId = (String) session.getAttribute("id");

    if (userId == null) {
        // 세션에 아이디가 없으면 로그인 페이지로 리다이렉트
        response.sendRedirect("/login.jsp");
    } else {
    %>
        <h1>안녕하세요, <%= userId %>님!</h1>
        <a href="Logout.do">로그아웃</a>
    <%
    }
    %>
</body>
</html>
```

## 요약

- 세션 확인: session.getAttribute("id")로 세션에 저장된 사용자 아이디를 확인합니다. 확인 되는 id가 없으면 로그인 페이지(/login.jsp)로 리다이렉트하여 로그인하도록 유도합니다.

#### 로그인 후 페이지 (실패 시)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>로그인 페이지</title>
</head>
<body>
    <!-- 로그인 폼 -->
    <form action="/Login.do" method="post">
        아이디 : <input type="text" name="id"><br>
        비밀번호 : <input type="password" name="password"><br>
        <input type="submit" value="로그인">
    </form>

    <!-- 로그인 처리 메시지 출력 -->
    <%= request.getAttribute("message") == null ? "" : request.getAttribute("message") %>
</body>
</html>
```

## 요약

- 이 JSP 는 사용자 로그인 정보를 입력받기 위한 폼을 제공합니다.
  사용자가 아이디와 비밀번호를 입력하고 제출하면 `/Login.do` 서블릿으로 POST 요청이 전송됩니다.
  서블릿에서 로그인 처리 결과로 설정한 메시지가 있으면(로그인이 실패하면) 이 페이지에서 표시됩니다.
