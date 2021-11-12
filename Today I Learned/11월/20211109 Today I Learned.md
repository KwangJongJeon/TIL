# 20211109 Today I Learned

# 🎯 오늘의 목표

- [x]  스프링 시큐리티 공부
- [x]  프로젝트 로그인 구현

# 📖 오늘 무슨 일을 했나?

 스프링 시큐리티를 사용해 로그인 기능을 구현했다. 지금까지 느낀걸로는 세팅하는 부분이 대다수 인 것 같다. 아무래도 5시간짜리 인강 하나 들은 걸로는 좀 모자란 부분이 있는 것 같아 공식 문서나 다른 블로그들을 참고해서 공부를 더 해야 할 듯 하다.

# 🚀 오늘의 산출물

인가를 하는 방법은 크게 다음 세 가지로 나뉜다.

1. Basic Auth
2. Form Based Authentication
3. JWT Token Verify

이 중에서 위의 두 가지인 Basic Auth와 Form Based Authentication을 정리하려고 한다.

## 📗 Basic Auth

![Untitled 1](https://user-images.githubusercontent.com/19809346/140945578-9b252233-4595-478e-abd5-2339a5673ab0.png)

 다음과 같은 순서로 진행된다.

1. 클라이언트가 인가(authorization)를 받지 않고 서버에게 원하는 자원에 대해 GET request를 보낸다.
2. 서버는 클라이언트가 인가를 받지 않은 사용자이므로 401 Unauthorized Http 상태 메시지를 보낸다.
3. 클라이언트가 Basic64로 암호화한 username과 password를 'request header'에 포함해 서버에게 전송한다
4. 클라이언트가 보낸 정보가 올바른 정보라면 서버는 200 ok Http 상태 메시지를 보낸다.
5. 그 뒤에 클라이언트가 서버에 요청을 보낼 때 마다 Basic64로 암호화한 유저 정보를 Request Header에 넣어 보내게 된다.

 이 인증 방법은 가볍고 빠르지만 클라이언트가 요청을 보낼때마다 Request Header에 데이터를 전송해서 보내므로 logout을 시킬 방법이 없다는 단점이 있다.

Spring Security에서는 HttpSecurity.httpBasic()을 통해 활성화 시킬 수 있다.

## 📗 Form Based Auth

![Untitled](https://user-images.githubusercontent.com/19809346/140945583-bb4dac23-4891-4dc6-9c52-5f219b285b5c.png)
다음과 같은 순서로 진행된다.

1. 클라이언트가 자신의 정보를 POST 방식으로 보낸다.
2. 서버는 정보가 맞는지 확인하고 맞다면 OK 메시지와 세션 정보를 보낸다.
3. 클라이언트는 자신의 정보를 보낼때마다 SessionID를 함께 보낸다.
4. 서버는 세션 아이디를 검증하고 맞다면 200 OK 상태 메시지를 보낸다.

 가장 많이 쓰이는 인증 방법으로 Session을 활용하는 방법이다. 이름에서 알 수 있듯이 Form을 사용해서 인증을 하는 방식이며 서버측에서 가지고 있는 세션 정보를 지움으로써 로그아웃도 가능하다.

스프링 시큐리티에서는 HttpSecurity.authorizeRequests()를 통해 활성화 시킬 수 있다.

각각의 인증은 HTTPS를 사용하는것을 권장한다. 여유가 된다면 HTTPS를 사용하자.

## 📗CSRF(CROSS-SITE Request Forgery)

![csrf](https://user-images.githubusercontent.com/19809346/140945584-fbb08c64-833f-45d0-9752-b0d283017062.png)
웹 애플리케이션 취약점 중 하나로, 이용자가 의도하지 않은 요청을 통한 공격을 의미한다.

이건 좀 더 정리해야할듯 😅
