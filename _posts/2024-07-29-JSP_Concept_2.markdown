---
title: "{ CS } JSP_2"
date: 2024-07-29 18:00:00 +07:00
tags: [JSP, Servlet, setAttribute, getRequestDispatcher, sendRedirect]
---

# 주요 개념

### RequestDispatcher

**RequestDispatcher**는 서블릿 API의 인터페이스로, 요청을 서버 내의 다른 자원(예: 다른 서블릿, JSP 페이지, HTML 파일 등)으로 전달하여 추가 처리나 렌더링을 수행할 수 있게 해줍니다.

#### 사용 예시:

```java
RequestDispatcher dispatcher = request.getRequestDispatcher("/anotherPage.jsp");
dispatcher.forward(request, response);
```

**사용 시점**:

- 사용자가 요청한 페이지와 같은 서버 내의 다른 페이지로 요청을 전달하고자 할 때
- 요청과 응답 객체를 계속 유지하고 싶을 때

### sendRedirect

**sendRedirect**는 클라이언트에게 다른 URL로 리다이렉트하도록 지시하는 메소드입니다. 클라이언트의 브라우저가 새로운 URL로 요청을 보내게 됩니다.

#### 사용 예시:

```java
response.sendRedirect("/anotherPage.jsp");
```

**사용 시점**:

- 사용자가 요청한 페이지와는 다른 URL로 클라이언트를 이동시키고자 할 때
- 이전 요청의 상태나 데이터가 필요하지 않을 때

### 차이점 요약

- **RequestDispatcher**는 서버 내에서 요청을 전달하는 방식으로, 클라이언트의 브라우저는 요청의 변경을 알지 못합니다. 요청과 응답 객체는 포워딩된 자원에서도 계속 사용할 수 있습니다.
- **sendRedirect**는 클라이언트의 브라우저가 새로운 URL로 요청을 보내도록 지시하는 방식으로, 클라이언트의 브라우저는 URL의 변경을 인식합니다. 요청과 응답 객체는 새로 생성되며 이전 요청의 데이터는 유지되지 않습니다.

### 예제 코드 - POST 요청 처리 (getRequestDispatcher 사용)

```java
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	    request.setCharacterEncoding("UTF-8");

	    // 문자열을 Date 객체로 변환하기 위해 SimpleDateFormat 객체 생성
	    SimpleDateFormat tool = new SimpleDateFormat("yyyy-MM-dd");
	    Date birth = null;

	    try {
	        // 'birth' 파라미터를 Date 객체로 변환
	        birth = tool.parse(request.getParameter("birth"));
	    } catch(ParseException e) {
	        e.printStackTrace();
	    }

	    // 요청(request) 객체에 "birth"라는 이름으로 birth 값을 설정
	    request.setAttribute("birth", birth);

	    // 취미를 저장할 Map
	    Map<String, String> hobbyMap = new HashMap<>();
	    hobbyMap.put("1", "볼링");
	    hobbyMap.put("2", "펜타스톰");
	    hobbyMap.put("3", "철권");

	    // 선택된 hobby 데이터를 저장할 문자열 변수 초기화
	    String hobbyData = "";
	    // 요청 파라미터에서 'hobby' 값을 배열로 가져옴
	    String[] hobbies = request.getParameterValues("hobby");

	    // 'hobby' 파라미터가 존재하고 비어있지 않은 경우
	    if(hobbies != null && hobbies.length != 0) {
	        // 선택된 취미 키를 hobbyData에 추가
	        for(String key : hobbies) {
	            hobbyData += key + " ";
	        }
	    } else {
	        hobbyData += "선택없음.";
	    }

	    // 최종적으로 hobby 데이터를 요청 객체에 추가
	    request.setAttribute("hobbyData", hobbyData);

	    // area 목록을 저장할 Map 객체 생성 및 초기화
	    Map<String, String> areaMap = new HashMap<>();
	    areaMap.put("1", "서울");
	    areaMap.put("2", "경기");
	    areaMap.put("3", "인천");
	    areaMap.put("4", "강원");

	    // 요청 파라미터에서 'area' 값을 가져옴
	    String area = request.getParameter("area");
	    // 선택된 areaData 데이터를 요청 객체에 추가
	    request.setAttribute("areaData", areaMap.get(area));

	    // 입력 폼 결과 페이지로 포워드
	    request.getRequestDispatcher("/inputFormResult.jsp").forward(request, response);
	}
```

- 폼 데이터를 처리하고 이를 JSP 페이지로 포워딩 (getRequestDispatcher.forward) 하여 결과를 표시하는 데 사용됩니다.
- 각 단계에서 데이터를 변환하고, 요청 객체에 데이터를 설정하여 JSP 페이지에서 쉽게 접근할 수 있도록 합니다.
