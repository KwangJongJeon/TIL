# 20211105 Today I Learned

## ๐ ์ค๋ ๋ฌด์จ ์ผ์ ํ๋?

 ํ๋ก๊ทธ๋๋จธ์ค ์ค๋ ฅ ํ์คํธ๋ฅผ ๋ดค๋ค. ๋ ๋ฒจ 2๋ ์ด์ฐ ํต๊ณผํ๋๋ฐ ๋ ๋ฒจ 3์ ํ ๋ฌธ์ ๋ ๋ชป ํ๊ณ  ๋จ์ด์ก๋ค... ๐ข ๊ทธ๋๋ ์๋ ์ ์ด๊ฑธ ๋์ฒด ์ด์ฐ ํธ๋ ๊ฐ๋ ์์กํ๋๋ฐ, ์ด๋ก  ๊ณต๋ถ๋ฅผ ์ข ํ๊ณ  ๋ค์ด๊ฐ์ ๊ทธ๋ฐ๊ฐ ์ด๋ค ๋ฐฉ์์ผ๋ก ๋ฌธ์  ํ์ด๋ฅผ ์ ๊ทผํด์ผํ ์ง ๊ฐ์ด ์กํ๊ธด ํ๋๋ผ. ์ญ์ ๋ญ๋ ์ง๊ฐ์ ์ด๋ก  ๊ณต๋ถ๋ฅผ ํ๊ณ  ๋์ ํ์ฉ์ ํด์ผ ์ ํ ์ ์๋ ๊ฒ ๊ฐ๋ค.

 ์ธํ๋ฐ์์ ์ธ๊ฐ์ ๋ค์ผ๋ฉด์ Servlet์ ๋ํ ํ์ต์ ํ๋ค. ๊ณต๋ถ๋ฅผ ํ๊ธฐ ์ ์ ์ด๊ฒ ๋์ฒด ์ด๋ค์์ผ๋ก ๋์๊ฐ๋์ง ์ ํ ๊ฐ์ด ์์กํ๋๋ฐ Request, Response๋ผ๋ ์๋ธ๋ฆฟ ๊ฐ์ฒด์ ํค๋๋ฅผ ์ค์ ํ๊ณ  ๋ฐ์ดํฐ๋ฅผ ์ฐ๋ ๋ฐฉ์์ ๋ฐฐ์ฐ๋ HTTP ํต์ ์ ๋ํ ์์ ๊ฐ์ด ์๊ฒผ๋ค. ์์ผ๋ก์ ํ๋ก์ ํธ๋ ์ข ๋ ๋ชํํ๊ฒ ๊ฐ๋ฐ์ ์งํ ํ  ์ ์์ ๋ฏ ํ๋ค!

## ๐ ์ค๋์ ๊ณต๋ถ ๋ด์ฉ

## ๐Request Servlet

 ์คํ๋ง์ Request Servlet์ ์ฌ์ฉํ๋ฉด Http๋ฅผ ํตํด Request๋์ด์ง ๋ฐ์ดํฐ๋ฅผ ๋ฐ์ ์ฌ ์ ์๊ณ , 'HttpServletRequest' ๊ฐ์ฒด๋ฅผ ์ฌ์ฉํด์ ์์ฒญ๋์ด์ง ํค๋๋ ๋ฐ์ดํฐ๋ฑ์ ํธ๋ฆฌํ๊ฒ ํ์ฑ ํ  ์ ์๋ ๋ฉ์๋๋ค์ ์ฌ์ฉ ํ  ์ ์๋ค. 

 HTTP์์ ๋ฉ์์ง๊ฐ Request๋๋ ๋ฐฉ์์ 3๊ฐ์ง๋ก ๋๋๋ค.

- GET ์ฟผ๋ฆฌ ํ๋ผ๋ฏธํฐ๋ฅผ ํตํ ์์ฒญ
- POST HTML Form์ ํตํ ์์ฒญ
- HTTP API(JSON)์ ํตํ ๋ฐ์ดํฐ ์์ฒญ

 ์ด์ค์์ GET ์ฟผ๋ฆฌ ํ๋ผ๋ฏธํฐ๋ฅผ ํตํ ์์ฒญ๊ณผ POST HTML Form์ ํตํ ์์ฒญ์ '**request.getParameter()**'๋ฉ์๋๋ฅผ ํตํด ๋ฐ์ดํฐ๋ฅผ ๋ฐ์ ์ฌ ์ ์๋ค.

```java
/**
 * 1. ํ๋ผ๋ฏธํฐ ์ ์ก ๊ธฐ๋ฅ
 * http://localhost:8080/request-param?username=hello&age=20
 */
@WebServlet(name = "requestParamServlet", urlPatterns = "/request-param")
public class RequestParamServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        System.out.println("[์ ์ฒด ํ๋ผ๋ฏธํฐ ์กฐํ] - start");

        request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> System.out.println(paramName + "=" + request.getParameter(paramName)));

        System.out.println("[์ ์ฒด ํ๋ผ๋ฏธํฐ ์กฐํ] - end");
        System.out.println();

        System.out.println("[๋จ์ผ ํ๋ผ๋ฏธํฐ ์กฐํ] - start");
        String username = request.getParameter("username");
        String age = request.getParameter("age");

        System.out.println("username = " + username);
        System.out.println("age = " + age);
        System.out.println("[๋จ์ผ ํ๋ผ๋ฏธํฐ ์กฐํ] - end");
        System.out.println();

        System.out.println("[์ด๋ฆ์ด ๊ฐ์ ๋ณต์ ํ๋ผ๋ฏธํฐ ์กฐํ]");
        String[] usernames = request.getParameterValues("username");
        for (String name : usernames) {
            System.out.println("username = " + name);
        }

        response.getWriter().write("ok");
    }
}
```

 ์ฟผ๋ฆฌ ํ๋ผ๋ฏธํฐ๋ฅผ ๋ค๋ฃจ๋ ๋ฐฉ์๊ณผ Form Data๋ฅผ ๋ค๋ฃจ๋ ๋ฐฉ์์ด ๊ฐ๊ธฐ์, ์์ ์ฝ๋์ username๊ณผ age๋ผ๋ ์ด๋ฆ์ผ๋ก ๋ค์ด์ค๋ Form Data๊ฐ ๋ค์ด ์ฌ ๊ฒฝ์ฐ์๋ ์ฝ๋์ ๋ณ๊ฒฝ ์์ด ์ฒ๋ฆฌ๊ฐ ๊ฐ๋ฅํ๋ค. 

### JSON ์์ฒญ ์ฒ๋ฆฌ

 JSON ์์ฒญ ๊ฐ์ ๊ฒฝ์ฐ '**request.getInputStream()**'์ ์ฌ์ฉํด ์๋ ฅ์ ๋ฐ์ดํธ ์คํธ๋ฆผํ ์ํจ ๋ค์์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ฒด์ ์ ์ฅํ๋ ๋ฐฉ์์ผ๋ก ์์ฒญ์ ์ฒ๋ฆฌํ๋ฉด ๋๋ค.

```java
@Getter @Setter
public class HelloData {
    private String username;
    private int age;
}
```

```java
@WebServlet(name = "requestBodyJsonServlet", urlPatterns = "/request-body-string")
public class RequestBodyJsonServlet extends HttpServlet {

    private ObjectMapper objectMapper = new ObjectMapper();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        ServletInputStream inputStream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

        System.out.println("messageBody = " + messageBody);

        HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);

        System.out.println("helloData.getUsername() = " + helloData.getUsername());
        System.out.println("helloData.getAge() = " + helloData.getAge());

        response.getWriter().write("OK");
    }
}
```

๋ฐ์ดํฐ ์คํธ๋ฆผ์ ๊ฐ์ฒด์ ๋งคํํ๋๊ฑด ์คํ๋ง์ ๋ด์ฅ๋์ด์๋ Jackson ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํ๋ฉด ๊ฐํธํ๊ฒ ์ฒ๋ฆฌ ํ  ์ ์๋ค. ์์ ์ฝ๋์์ objectMapper๊ฐ Jackson ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํ๋ ๋ถ๋ถ์ด๋ค. 

## ๐Response Servlet

Response ๋ฉ์ธ์ง์๋ HTTP ํค๋๋ฅผ ์ธํํ๊ฑฐ๋ BODY์ ์ ์ ๊ฐ ์์ฒญํ ๋ฐ์ดํฐ๋ HTML๋ฑ์ ์ค์ด์ ๋ณด๋ด ์ค ์ ์๋ ๊ธฐ๋ฅ์ด ์๋ค.

Response ๋ฉ์ธ์ง์ Header๋ฅผ ์ธํํ๋๊ฑด **'Response.setHeader()'** ๋ฉ์๋๋ฅผ ์ฌ์ฉํด ํค๋๋ฅผ ์ธํ ํ  ์ ์๋ค.

```java
@WebServlet(name = "responseHeaderServlet", urlPatterns = "/response-header")
public class ResponseHeaderServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // [status-line]
        response.setStatus(HttpServletResponse.SC_OK);

        // [response-header]
//      response.setHeader("Content-Type", "text/plain;charset=utf-8");
        response.setHeader("Cache-Control", "no-cache, no-store, must-revalidate");
        response.setHeader("Pragma", "no-cache");
        response.setHeader("my-header", "hello");

        // [Header ํธ์ ๋ฉ์๋]
        content(response);
        cookie(response);
//      redirect(response);

        PrintWriter writer = response.getWriter();
        writer.println("์๋ํ์ธ์");
    }

	private void content(HttpServletResponse response) {
	        //Content-Type: text/plain;charset=utf-8
        //Content-Length: 2
        //response.setHeader("Content-Type", "text/plain;charset=utf-8");
        response.setContentType("text/plain");
        response.setCharacterEncoding("utf-8");
        //response.setContentLength(2); //(์๋ต์ ์๋ ์์ฑ)
    }

    private void cookie(HttpServletResponse response) {
        //Set-Cookie: myCookie=good; Max-Age=600;
        //response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");
        Cookie cookie = new Cookie("myCookie", "good");
        cookie.setMaxAge(600); //600์ด
        response.addCookie(cookie);
    }

    private void redirect(HttpServletResponse response) throws IOException {
        //Status Code 302
        //Location: /basic/hello-form.html
        //response.setStatus(HttpServletResponse.SC_FOUND); //302
        //response.setHeader("Location", "/basic/hello-form.html");
        response.sendRedirect("/basic/hello-form.html");
    }
}
```

HTTP์์ ๋ฉ์ธ์ง๊ฐ Response๋๋ ๋ฐฉ์์ ํฌ๊ฒ 3๊ฐ์ง๋ก ๋๋๋ค.

- ์์ ํ์คํธ Response
- HTML Response
- JSON Response(HTTP API)

### ์์ ํ์คํธ Response, HTML Response

response.getWriter() ๋ฉ์๋๋ฅผ ํตํด Writer๋ฅผ ์ป์ด๋ธ ๋ค์ ์ํ๋ ๋ฉ์ธ์ง๋ HTML์ ์ ์ผ๋ฉด ๋๋ค.

```java
@WebServlet(name = "responseHtmlServlet", urlPatterns = "/response-html")
public class ResponseHtmlServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Content-Type : text/html; charset=utf-8
        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");

        PrintWriter writer = response.getWriter();
        writer.println("<html>");
        writer.println("<body>");
        writer.println("<div>์๋?</div>");
        writer.println("</body>");
        writer.println("</html>");
    }
}
```

### JSON Response

JSON Response๋ JSON Request์์ ์ฌ์ฉํ๋ ๊ฒ์ฒ๋ผ Jackson ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ objectMapper๋ฅผ ์ฌ์ฉํ๋ฉด ๊ฐํธํ๊ฒ ๋ฐ์ดํฐ๋ฅผ ๋ณด๋ผ ์ ์๋ค. 

```java
@WebServlet(name="ResponseJsonServlet", urlPatterns = "/response-json")
public class ResponseJsonServlet extends HttpServlet {

    private ObjectMapper objectMapper = new ObjectMapper();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Content-Type: application/json
        response.setContentType("application/json");
        response.setCharacterEncoding("utf-8");

        HelloData helloData = new HelloData();
        helloData.setUsername("Jeon");
        helloData.setAge(25);

        // {"username": "Jeon", "age": 25}
        String result = objectMapper.writeValueAsString(helloData);
        response.getWriter().write(result);
    }
}
```
