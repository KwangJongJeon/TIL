# 20211102 Today I Learned

# 🚀 오늘의 공부 내용

## 📗 웹서버

- HTTP 기반으로 동작한다.
- 정적 리소스 및 기타 부가 기능 제공
- 정적 파일 (HTML, CSS, JS, 이미지, 영상)등 제공
- NGINX, APARCH등이 있다.

## 📗 웹 애플리케이션 서버(WAS: Web application Server)

- HTTP 기반으로 동작
- 기존 웹서버가 하는 기능 모두 지원
- 프로그램 '코드'를 실행해서 애플리케이션 로직 수행후 동적(변화하는) 컨텐츠 제공 ( 동적 HTML, HTTP API (JSON등))
- 톰캣(Tomcat), Jetty, Undertow등이 있다.

## ⚒️ WEB SERVER VS WAS

- 웹 서버는 정적 리소스를 제공하는데 반해 WAS는 애플리케이션 로직 제공
- 둘의 용어도 경계도 모호함
- 하지만 보통 자바 '서블릿 컨테이너' 기능을 제공하면 WAS라고 한다.
- WAS는 애플리케이션 코드를 실행하는데 특화되어있다.

## 💡 일반적인 서버의 구성도

![서버구현](https://user-images.githubusercontent.com/19809346/139853216-4503841d-8b65-42ca-ba6c-51cdd32e70b1.png)
from - inflearn 김영한 - 스프링 MVC 1편

### 위와 같은 구성도일 경우의 장점

 WAS만을 사용하면 HTML, CSS등의 정적 코드와 애플리케이션 로직 둘 다 제공 할 수 있는데 그렇게 하지 않는 이유는 '정적 이미지'만을 제공하는 기능을 잘 죽지 않는 애플리케이션 비즈니스 로직같은 경우는 정말 잘 죽기 때문이다. 만약 WEB SERVER만 살아있다면, WAS등이 죽어도 예외 처리 페이지등으로 이동 시킬 수 있어, UX 방면으로도 장점이 있다.

![WAS](https://user-images.githubusercontent.com/19809346/139853226-b0f91b49-c797-4247-b2ac-e1b7ae84d7e1.png)

from - inflearn 김영한 - 스프링 MVC 1편

 웹 시스템 리소스의 효율적인 관리가 가능하다. 정적인 소스가 늘어나면 Web server만 증설하면 되고, 애플리케이션 리소스가 많이 사용되면 WAS만 늘리면 된다.

# 📗Servlet

![서블릿1](https://user-images.githubusercontent.com/19809346/139853225-2eca1bb1-7f7f-44d2-b109-7ea6655ba30f.png)

from - inflearn 김영한 - 스프링 MVC 1편

 

 서블릿이란 웹상에서 HTTP 메시지를 간편하게 보내 줄 수 있도록 해주는 라이브러리라 볼 수 있다. 예를 들면 어떤 웹 상에 데이터를 요청하기 위해서는 헤더 등의 규격을 HTTP 메시지를 전부 작성해서 보내야한다. 받을 때도 메시지를 전부 파싱해야 할텐데 이런 작업들은 손이 정말 많이가는 작업들이다. 

 **이런 손이 많이 가는 작업들을 대신해 주는 것이 서블릿**이라 볼 수 있다. 서블릿을 통해 개발자는 자신이 개발해야하는 비즈니스 로직에만 집중해서 개발 할 수 있게 된다.

## 📗 서블릿컨테이너

 

 스프링은 이 서블릿을 '서블릿 컨테이너'라는 형태로 제공한다. 스프링 컨테이너와 마찬가지로 서블릿 컨테이너 안에 싱글톤 서블릿 객체들을 저장하는 방식이다. 

 다음과 같이 스프링에서 서블릿을 사용 할 수 있다.

```java
@WebServlet(name = "requestHeaderServlet", urlPatterns = "/request-header")
public class RequestHeaderServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
    }
```

 

HttpServlet을 상속받고, @WebServlet 애노테이션을 선언하면 된다. 각각의 요소는 다음과 같다.

- @WebServlet(name = 서블릿객체를을식별할수있는이름, urlPatterns = 서블릿이 적용되는 url)

 service의 request는 request되어진 http 메시지를 의미하고, response는 response할 http 메시지를 의미한다. public으로 선언된 service가 아닌 **protected**로 선언된 service를 상속해야 하는 것에 유의하자. request, response 각각의 객체에 있는 메소드들을 통해 http 메시지를 편리하게 파싱 할 수 있다.

 * 동시 요청을 위한 멀티 쓰레드 처리도 지원한다

⚠️ 서블릿 객체 또한 싱글턴으로 생성되니 **'공유변수'**를 주의하자!

### Reference
https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard
