# 20211105 Today I Learned

## 📖 오늘 무슨 일을 했나?

 프로그래머스 실력 테스트를 봤다. 레벨 2는 어찌 통과했는데 레벨 3은 한 문제도 못 풀고 떨어졌다... 😢 그래도 옛날엔 이걸 대체 어찌 푸나 감도 안잡혔는데, 이론 공부를 좀 하고 들어가서 그런가 어떤 방식으로 문제 풀이를 접근해야할지 감이 잡히긴 하더라. 역시 뭐든지간에 이론 공부를 하고 나서 활용을 해야 잘 풀 수 있는 것 같다.

 인프런에서 인강을 들으면서 Servlet에 대한 학습을 했다. 공부를 하기 전엔 이게 대체 어떤식으로 돌아가는지 전혀 감이 안잡혔는데 Request, Response라는 서블릿 객체에 헤더를 설정하고 데이터를 쓰는 방식을 배우니 HTTP 통신에 대한 자신감이 생겼다. 앞으로의 프로젝트는 좀 더 명확하게 개발을 진행 할 수 있을 듯 하다!

## 🚀 오늘의 공부 내용

## 📗Request Servlet

 스프링의 Request Servlet을 사용하면 Http를 통해 Request되어진 데이터를 받아 올 수 있고, 'HttpServletRequest' 객체를 사용해서 요청되어진 헤더나 데이터등을 편리하게 파싱 할 수 있는 메소드들을 사용 할 수 있다. 

 HTTP에서 메시지가 Request되는 방식은 3가지로 나뉜다.

- GET 쿼리 파라미터를 통한 요청
- POST HTML Form을 통한 요청
- HTTP API(JSON)을 통한 데이터 요청

 이중에서 GET 쿼리 파라미터를 통한 요청과 POST HTML Form을 통한 요청은 '**request.getParameter()**'메소드를 통해 데이터를 받아 올 수 있다.

```java
/**
 * 1. 파라미터 전송 기능
 * http://localhost:8080/request-param?username=hello&age=20
 */
@WebServlet(name = "requestParamServlet", urlPatterns = "/request-param")
public class RequestParamServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        System.out.println("[전체 파라미터 조회] - start");

        request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> System.out.println(paramName + "=" + request.getParameter(paramName)));

        System.out.println("[전체 파라미터 조회] - end");
        System.out.println();

        System.out.println("[단일 파라미터 조회] - start");
        String username = request.getParameter("username");
        String age = request.getParameter("age");

        System.out.println("username = " + username);
        System.out.println("age = " + age);
        System.out.println("[단일 파라미터 조회] - end");
        System.out.println();

        System.out.println("[이름이 같은 복수 파라미터 조회]");
        String[] usernames = request.getParameterValues("username");
        for (String name : usernames) {
            System.out.println("username = " + name);
        }

        response.getWriter().write("ok");
    }
}
```

 쿼리 파라미터를 다루는 방식과 Form Data를 다루는 방식이 같기에, 위의 코드에 username과 age라는 이름으로 들어오는 Form Data가 들어 올 경우에도 코드의 변경 없이 처리가 가능하다. 

### JSON 요청 처리

 JSON 요청 같은 경우 '**request.getInputStream()**'을 사용해 입력을 바이트 스트림화 시킨 다음에 데이터를 객체에 저장하는 방식으로 요청을 처리하면 된다.

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

데이터 스트림을 객체에 매핑하는건 스프링에 내장되어있는 Jackson 라이브러리를 사용하면 간편하게 처리 할 수 있다. 위의 코드에서 objectMapper가 Jackson 라이브러리를 사용하는 부분이다. 

## 📗Response Servlet

Response 메세지에는 HTTP 헤더를 세팅하거나 BODY에 유저가 요청한 데이터나 HTML등을 실어서 보내 줄 수 있는 기능이 있다.

Response 메세지에 Header를 세팅하는건 **'Response.setHeader()'** 메소드를 사용해 헤더를 세팅 할 수 있다.

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

        // [Header 편의 메소드]
        content(response);
        cookie(response);
//      redirect(response);

        PrintWriter writer = response.getWriter();
        writer.println("안녕하세요");
    }

	private void content(HttpServletResponse response) {
	        //Content-Type: text/plain;charset=utf-8
        //Content-Length: 2
        //response.setHeader("Content-Type", "text/plain;charset=utf-8");
        response.setContentType("text/plain");
        response.setCharacterEncoding("utf-8");
        //response.setContentLength(2); //(생략시 자동 생성)
    }

    private void cookie(HttpServletResponse response) {
        //Set-Cookie: myCookie=good; Max-Age=600;
        //response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");
        Cookie cookie = new Cookie("myCookie", "good");
        cookie.setMaxAge(600); //600초
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

HTTP에서 메세지가 Response되는 방식은 크게 3가지로 나뉜다.

- 순수 텍스트 Response
- HTML Response
- JSON Response(HTTP API)

### 순수 텍스트 Response, HTML Response

response.getWriter() 메소드를 통해 Writer를 얻어낸 다음 원하는 메세지나 HTML을 적으면 된다.

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
        writer.println("<div>안녕?</div>");
        writer.println("</body>");
        writer.println("</html>");
    }
}
```

### JSON Response

JSON Response는 JSON Request에서 사용했던 것처럼 Jackson 라이브러리의 objectMapper를 사용하면 간편하게 데이터를 보낼 수 있다. 

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
