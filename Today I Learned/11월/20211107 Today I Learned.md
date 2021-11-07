# 20211107 Today I Learned

# 🚀 오늘의 공부 내용

김영한님의 스프링 MVC - 1편을 듣고 작성했습니다.

[스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술 - 인프런 | 강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

## 서블릿을 통한 HTML Response의 문제점

 서블릿을 통해 HTML Reponse를 해야 할 경우 가장 큰 문제는 다음과 같이 response의 writer 객체에 하나하나 값들을 전부 써넣어야 한다는 것이다. 

```java
@WebServlet(name = "mamberSaveServlet", urlPatterns = "/servlet/members/save")
public class MemberSaveServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MemberSaveServlet.service");

        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        PrintWriter w = response.getWriter();

        w.write("<html>\n" +
                "<head>\n" +
                " <meta charset=\"UTF-8\">\n" +
                "</head>\n" +
                "<body>\n" +
                "성공\n" +
                "<ul>\n" +
                " <li>id="+member.getId()+"</li>\n" +
                " <li>username="+member.getUsername()+"</li>\n" +
                " <li>age="+member.getAge()+"</li>\n" +
                "</ul>\n" +
                "<a href=\"/index.html\">메인</a>\n" +
                "</body>\n" +
                "</html>");
    }
}
```

 단순한 Form을 출력해주는 HTML임에도 불구하고, w.write(...)부분을 보면 굉장히 복잡하고 길게 HTML문을 써야하는 것을 볼 수 있다.

 이를 해결하기 위해 나온 것이 Template 엔진들이고 그 중에서 자주 쓰였던 것이 JSP 템플릿 엔진이다.  템플릿 엔진은 HTML과 JAVA 코드를 혼용해서 사용함으로써 HTML 리턴의 복잡함을 해결했다. 

JSP는 다음과 같이 활용한다.

```java
<%@ page import="hello.servlet.member.Member" %>
<%@ page import="hello.servlet.member.MemberRepository" %>

<%--
  Created by IntelliJ IDEA.
  User: Jeon
  Date: 2021-11-07
  Time: 오후 2:33
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    // request, response 사용 가능
    MemberRepository memberRepository = MemberRepository.getInstance();

    System.out.println("MemberSaveServlet.service");
    String username = request.getParameter("username");
    int age = Integer.parseInt(request.getParameter("age"));

    Member member = new Member(username, age);
    memberRepository.save(member);
%>
<html>
<head>
    <title>Title</title>
</head>
<body>
성공
<ul>
    <li>id=<%=member.getId()%></li>
    <li>usename=<%=member.getUsername()%></li>
    <li>age=<%=member.getAge()%></li>
</ul>
<a href = "/index.html">메인</a>
</body>
</html>
```

 webapp 폴더 안의 JSP를 사용하면 위와 같이 자바 코드를 <% ... %> 안에 넣음으로써 HTML 안에서도 자바 코드를 동작 시킬 수 있게 되어 동적인 HTML 코드를 서블릿만 활용하는 것에 비하면 비교적 간편하게 만들 수 있다.

 하지만 위의 코드의 문제점은 명확하게 보인다. 바로 자바코드와 HTML코드가 섞여있어 유지보수를 진행할 때 작은 작업이여도 굉장히 많은 부분을 수정해야 할 것이다. 예를들어 UI를 담당하는 HTML을 수정하려고 하면 HTML 코드가 JSP코드 안에 있기 때문에 엮여있는 JAVA 코드까지도 확인을 해가며 유지보수를 진행해야 할 것이다. 이는 생산성 저하를 일으킨다. 

이를 해결한 방법은 무엇일까? 바로 MVC 패턴을 사용하여 JSP를 활용한 것이다.

## 📗MVC 패턴을 사용한 JSP

 MVC(Model, View, Controller) 패턴을 사용하여 각각의 기능을 분리한다. MVC 패턴이란 기능을 모델, 뷰, 컨트롤러의 큰 부분으로 나눠서 처리하는 것을 의미한다. 

![from - [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)](20211107%20Today%20I%20Learned%209615e552b1c942faa0a354b7dc2356e3/MVC.png)

from - [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

 위의 이미지는 MVC 패턴을 표현한 것이다. 클라이언트가 컨트롤러에 요청을 하면, 컨트롤러가 필요한 로직을 수행 한 후, 수행한 결과를 model에 담는다. 그 뒤에 '뷰' 계층이 모델에서 적절하게 처리된 데이터를 가져와서 동적으로 HTML을 생성하는 방식으로 진행된다.

 한번 구현해보자. 구현하려면 두 가지가 필요하다. model에 데이터를 저장하는 방법과 비즈니스 로직에서 뷰 로직으로 제어권을 넘기는 과정이 필요하다. 우선 model에 데이터를 저장하기 위해서, request에 있는 저장소를 활용하자. 이를 사용하기 위해서는 request.setAttribute(),request.getAttribute()를 사용하면 된다. 그리고 Servlet에서 뷰 로직으로 request, response 데이터와 함께 제어권을 넘기기 위해서 Servlet의 response 객체에 있는 dispatcher를 활용하자.  request.getRequestDespatcher(jspPath)와 같은 방식으로 dispatcher를 꺼내 올 수 있다.

```java
@WebServlet(name = "mvcMemberSaveServlet", urlPatterns = "/servlet-mvc/members/save")
public class MvcMemberSaveServlet extends HttpServlet {

    MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        // Model에 데이터를 보관한다.
        request.setAttribute("member", member);

        String viewPath = "/WEB-INF/views/save-result.jsp";
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
    }
}
```

```java
<%@ page import="hello.servlet.member.Member" %><%--
  Created by IntelliJ IDEA.
  User: Jeon
  Date: 2021-11-07
  Time: 오후 3:38
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
성공
<ul>
    <li>id=${member.id}
    <li>usename=${member.username}
    <li>age=${member.age}
</ul>
<a href = "/index.html">메인</a>
</body>
</html>
```

### 💡WEB-INF 폴더

 WEB-INF 폴더를 사용하면 해당 JSP 템플릿을 HTML을 통해 접근하는 것을 막아주고, 컨트롤러를 통해서만 접근을 할 수 있게 해준다.

### 💡Redirect vs forward

 리다이렉트는 실제 클라이언트에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다. 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다. 반면에 포워드 서버는 서버 내부에서 일어나는 호출이기 때문에 클라이언트가 전혀 인지하지 못한다

## 📗 MVC 패턴의 한계

- 공통 처리가 어렵다
- 중복되는 코드가 있다(dispatch 등)