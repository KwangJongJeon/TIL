# 20211107 Today I Learned

# ๐ ์ค๋์ ๊ณต๋ถ ๋ด์ฉ

๊น์ํ๋์ ์คํ๋ง MVC - 1ํธ์ ๋ฃ๊ณ  ์์ฑํ์ต๋๋ค.

[์คํ๋ง MVC 1ํธ - ๋ฐฑ์๋ ์น ๊ฐ๋ฐ ํต์ฌ ๊ธฐ์  - ์ธํ๋ฐ | ๊ฐ์](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

## ์๋ธ๋ฆฟ์ ํตํ HTML Response์ ๋ฌธ์ ์ 

 ์๋ธ๋ฆฟ์ ํตํด HTML Reponse๋ฅผ ํด์ผ ํ  ๊ฒฝ์ฐ ๊ฐ์ฅ ํฐ ๋ฌธ์ ๋ ๋ค์๊ณผ ๊ฐ์ด response์ writer ๊ฐ์ฒด์ ํ๋ํ๋ ๊ฐ๋ค์ ์ ๋ถ ์จ๋ฃ์ด์ผ ํ๋ค๋ ๊ฒ์ด๋ค. 

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
                "์ฑ๊ณต\n" +
                "<ul>\n" +
                " <li>id="+member.getId()+"</li>\n" +
                " <li>username="+member.getUsername()+"</li>\n" +
                " <li>age="+member.getAge()+"</li>\n" +
                "</ul>\n" +
                "<a href=\"/index.html\">๋ฉ์ธ</a>\n" +
                "</body>\n" +
                "</html>");
    }
}
```

 ๋จ์ํ Form์ ์ถ๋ ฅํด์ฃผ๋ HTML์์๋ ๋ถ๊ตฌํ๊ณ , w.write(...)๋ถ๋ถ์ ๋ณด๋ฉด ๊ต์ฅํ ๋ณต์กํ๊ณ  ๊ธธ๊ฒ HTML๋ฌธ์ ์จ์ผํ๋ ๊ฒ์ ๋ณผ ์ ์๋ค.

 ์ด๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด ๋์จ ๊ฒ์ด Template ์์ง๋ค์ด๊ณ  ๊ทธ ์ค์์ ์์ฃผ ์ฐ์๋ ๊ฒ์ด JSP ํํ๋ฆฟ ์์ง์ด๋ค.  ํํ๋ฆฟ ์์ง์ HTML๊ณผ JAVA ์ฝ๋๋ฅผ ํผ์ฉํด์ ์ฌ์ฉํจ์ผ๋ก์จ HTML ๋ฆฌํด์ ๋ณต์กํจ์ ํด๊ฒฐํ๋ค. 

JSP๋ ๋ค์๊ณผ ๊ฐ์ด ํ์ฉํ๋ค.

```java
<%@ page import="hello.servlet.member.Member" %>
<%@ page import="hello.servlet.member.MemberRepository" %>

<%--
  Created by IntelliJ IDEA.
  User: Jeon
  Date: 2021-11-07
  Time: ์คํ 2:33
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    // request, response ์ฌ์ฉ ๊ฐ๋ฅ
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
์ฑ๊ณต
<ul>
    <li>id=<%=member.getId()%></li>
    <li>usename=<%=member.getUsername()%></li>
    <li>age=<%=member.getAge()%></li>
</ul>
<a href = "/index.html">๋ฉ์ธ</a>
</body>
</html>
```

 webapp ํด๋ ์์ JSP๋ฅผ ์ฌ์ฉํ๋ฉด ์์ ๊ฐ์ด ์๋ฐ ์ฝ๋๋ฅผ <% ... %> ์์ ๋ฃ์์ผ๋ก์จ HTML ์์์๋ ์๋ฐ ์ฝ๋๋ฅผ ๋์ ์ํฌ ์ ์๊ฒ ๋์ด ๋์ ์ธ HTML ์ฝ๋๋ฅผ ์๋ธ๋ฆฟ๋ง ํ์ฉํ๋ ๊ฒ์ ๋นํ๋ฉด ๋น๊ต์  ๊ฐํธํ๊ฒ ๋ง๋ค ์ ์๋ค.

 ํ์ง๋ง ์์ ์ฝ๋์ ๋ฌธ์ ์ ์ ๋ชํํ๊ฒ ๋ณด์ธ๋ค. ๋ฐ๋ก ์๋ฐ์ฝ๋์ HTML์ฝ๋๊ฐ ์์ฌ์์ด ์ ์ง๋ณด์๋ฅผ ์งํํ  ๋ ์์ ์์์ด์ฌ๋ ๊ต์ฅํ ๋ง์ ๋ถ๋ถ์ ์์ ํด์ผ ํ  ๊ฒ์ด๋ค. ์๋ฅผ๋ค์ด UI๋ฅผ ๋ด๋นํ๋ HTML์ ์์ ํ๋ ค๊ณ  ํ๋ฉด HTML ์ฝ๋๊ฐ JSP์ฝ๋ ์์ ์๊ธฐ ๋๋ฌธ์ ์ฎ์ฌ์๋ JAVA ์ฝ๋๊น์ง๋ ํ์ธ์ ํด๊ฐ๋ฉฐ ์ ์ง๋ณด์๋ฅผ ์งํํด์ผ ํ  ๊ฒ์ด๋ค. ์ด๋ ์์ฐ์ฑ ์ ํ๋ฅผ ์ผ์ผํจ๋ค. 

์ด๋ฅผ ํด๊ฒฐํ ๋ฐฉ๋ฒ์ ๋ฌด์์ผ๊น? ๋ฐ๋ก MVC ํจํด์ ์ฌ์ฉํ์ฌ JSP๋ฅผ ํ์ฉํ ๊ฒ์ด๋ค.

## ๐MVC ํจํด์ ์ฌ์ฉํ JSP

 MVC(Model, View, Controller) ํจํด์ ์ฌ์ฉํ์ฌ ๊ฐ๊ฐ์ ๊ธฐ๋ฅ์ ๋ถ๋ฆฌํ๋ค. MVC ํจํด์ด๋ ๊ธฐ๋ฅ์ ๋ชจ๋ธ, ๋ทฐ, ์ปจํธ๋กค๋ฌ์ ํฐ ๋ถ๋ถ์ผ๋ก ๋๋ ์ ์ฒ๋ฆฌํ๋ ๊ฒ์ ์๋ฏธํ๋ค. 

![MVC](https://user-images.githubusercontent.com/19809346/140649352-cb73ddb2-0034-4d92-97e2-fac359905a8d.png)


from - [https://www.inflearn.com/course/์คํ๋ง-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

 ์์ ์ด๋ฏธ์ง๋ MVC ํจํด์ ํํํ ๊ฒ์ด๋ค. ํด๋ผ์ด์ธํธ๊ฐ ์ปจํธ๋กค๋ฌ์ ์์ฒญ์ ํ๋ฉด, ์ปจํธ๋กค๋ฌ๊ฐ ํ์ํ ๋ก์ง์ ์ํ ํ ํ, ์ํํ ๊ฒฐ๊ณผ๋ฅผ model์ ๋ด๋๋ค. ๊ทธ ๋ค์ '๋ทฐ' ๊ณ์ธต์ด ๋ชจ๋ธ์์ ์ ์ ํ๊ฒ ์ฒ๋ฆฌ๋ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์์ ๋์ ์ผ๋ก HTML์ ์์ฑํ๋ ๋ฐฉ์์ผ๋ก ์งํ๋๋ค.

 ํ๋ฒ ๊ตฌํํด๋ณด์. ๊ตฌํํ๋ ค๋ฉด ๋ ๊ฐ์ง๊ฐ ํ์ํ๋ค. model์ ๋ฐ์ดํฐ๋ฅผ ์ ์ฅํ๋ ๋ฐฉ๋ฒ๊ณผ ๋น์ฆ๋์ค ๋ก์ง์์ ๋ทฐ ๋ก์ง์ผ๋ก ์ ์ด๊ถ์ ๋๊ธฐ๋ ๊ณผ์ ์ด ํ์ํ๋ค. ์ฐ์  model์ ๋ฐ์ดํฐ๋ฅผ ์ ์ฅํ๊ธฐ ์ํด์, request์ ์๋ ์ ์ฅ์๋ฅผ ํ์ฉํ์. ์ด๋ฅผ ์ฌ์ฉํ๊ธฐ ์ํด์๋ request.setAttribute(),request.getAttribute()๋ฅผ ์ฌ์ฉํ๋ฉด ๋๋ค. ๊ทธ๋ฆฌ๊ณ  Servlet์์ ๋ทฐ ๋ก์ง์ผ๋ก request, response ๋ฐ์ดํฐ์ ํจ๊ป ์ ์ด๊ถ์ ๋๊ธฐ๊ธฐ ์ํด์ Servlet์ response ๊ฐ์ฒด์ ์๋ dispatcher๋ฅผ ํ์ฉํ์.  request.getRequestDespatcher(jspPath)์ ๊ฐ์ ๋ฐฉ์์ผ๋ก dispatcher๋ฅผ ๊บผ๋ด ์ฌ ์ ์๋ค.

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

        // Model์ ๋ฐ์ดํฐ๋ฅผ ๋ณด๊ดํ๋ค.
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
  Time: ์คํ 3:38
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
์ฑ๊ณต
<ul>
    <li>id=${member.id}
    <li>usename=${member.username}
    <li>age=${member.age}
</ul>
<a href = "/index.html">๋ฉ์ธ</a>
</body>
</html>
```

### ๐กWEB-INF ํด๋

 WEB-INF ํด๋๋ฅผ ์ฌ์ฉํ๋ฉด ํด๋น JSP ํํ๋ฆฟ์ HTML์ ํตํด ์ ๊ทผํ๋ ๊ฒ์ ๋ง์์ฃผ๊ณ , ์ปจํธ๋กค๋ฌ๋ฅผ ํตํด์๋ง ์ ๊ทผ์ ํ  ์ ์๊ฒ ํด์ค๋ค.

### ๐กRedirect vs forward

 ๋ฆฌ๋ค์ด๋ ํธ๋ ์ค์  ํด๋ผ์ด์ธํธ์ ์๋ต์ด ๋๊ฐ๋ค๊ฐ, ํด๋ผ์ด์ธํธ๊ฐ redirect ๊ฒฝ๋ก๋ก ๋ค์ ์์ฒญํ๋ค. ๋ฐ๋ผ์ ํด๋ผ์ด์ธํธ๊ฐ ์ธ์งํ  ์ ์๊ณ , URL ๊ฒฝ๋ก๋ ์ค์ ๋ก ๋ณ๊ฒฝ๋๋ค. ๋ฐ๋ฉด์ ํฌ์๋ ์๋ฒ๋ ์๋ฒ ๋ด๋ถ์์ ์ผ์ด๋๋ ํธ์ถ์ด๊ธฐ ๋๋ฌธ์ ํด๋ผ์ด์ธํธ๊ฐ ์ ํ ์ธ์งํ์ง ๋ชปํ๋ค

## ๐ MVC ํจํด์ ํ๊ณ

- ๊ณตํต ์ฒ๋ฆฌ๊ฐ ์ด๋ ต๋ค
- ์ค๋ณต๋๋ ์ฝ๋๊ฐ ์๋ค(dispatch ๋ฑ)
