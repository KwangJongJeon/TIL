# 20211103 Today I Learned

# 🚀 오늘의 공부 내용

김영한님의 스프링 MVC - 1편을 듣고 작성했습니다.

[스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술 - 인프런 | 강의](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

## 💡 쓰레드

- 자바 메인 메서드를 처음 실행하면 main이라는 이름의 쓰레드가 실행됨
- 쓰레드는 한번에 하나의 코드 라인만 수행
- 동시 처리가 필요하다면 쓰레드를 추가로 생성

## 🧩 멀티쓰레드

![다중쓰레드](https://user-images.githubusercontent.com/19809346/140077446-fa315c24-ec88-4781-9072-3c3b3428bcc1.png)

from - [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

 

쓰레드가 하나고 요청이 두개라면 쓰레드가 요청 1번을 처리하는동안 요청 2번은 쓰레드가 사용 가능해질 때 까지 대기해야 한다. 만약 요청 1과 요청 2를 동시에 처리하기를 원한다면 쓰레드를 추가로 생성해야한다.

![요청마다_쓰레드](https://user-images.githubusercontent.com/19809346/140077444-f8339475-a8b1-432a-a066-1d2ae9d75d8a.png)

from - [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

 하지만 요청마다 쓰레드를 생성 할 경우 문제가 있는데 만약 쓰레드가 계속해서 증가해 애플리케이션이 모든 자원을 소모한다면 애플리케이션은 결국 outOfBoundException등으로 종료되게 될 것이다. 또한 쓰레드의 수가 굉장히 많기 때문에 컨텍스트 스위칭(Context switching) 비용도 많이 들 것이다. 이를 해결하기 위해서는 쓰레드의 개수에 **'상한'**을 거는 것이 중요하다.

![쓰레드풀](https://user-images.githubusercontent.com/19809346/140077438-96483261-c778-4e14-bf1a-e27a88116c4e.png)
from - [https://www.inflearn.com/course/스프링-mvc-1/dashboard](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)

이를 '쓰레드 풀'이라는 것으로 해결할 수 있다. 생성할 쓰레드를 미리 생성해두고 요청이 필요 할 때 쓰레드 풀에 쓰레드를 요청하게 한다. 만약 쓰레드 풀에 아직 사용하지 않은 쓰레드가 남아있다면 쓰레드를 사용하고, 작업이 들어왔는데, 쓰레드가 전부 사용중이라면 작업을 대기시킨 뒤 쓰레드가 반납되면 해당 작업을 다시 진행하면 된다. 

 쓰레드 풀은 적절한 크기가 중요하다. 만약 쓰레드 풀이 너무 작다면 멀티쓰레딩의 효율이 떨어지고, 쓰레드 풀이 너무 크다면, CPU, 메모리 리소스의 임계점이 초과되어 프로세스가 종료될 수 있다. 쓰레드 풀의 크기는 튜닝에 정말 많이 쓰이니, 꼭 신경써서 설정하자

 멀티쓰레드에 대한 부분은 WAS가 대부분 지원하여 개발자는 마치 싱글쓰레드 프로그래밍을 하듯이 개발 할 수 있다.

## 📗서버사이드 렌더링(SSR)

HTML 최종 결과를 서버에서 만들어서 웹 브라우저에 전달하는 방식, 주로 정적인 화면에 사용하며 JSP, 타임리프 등이 여기에 속한다.

## 📗클라이언트 사이드 렌더링(CSR)

 HTML 결과를 자바스크립트를 이용해 웹 브라우저에서 동적으로 생성해서 적용 할 수 있다. 주로 동적인 화면에서 사용하며 웹 환경을 마치 앱처럼 필요한 부분부분 변경 할 수 있다. 리액트, Vue.js등을 이용해 구현 할 수 있다.

## 📗HTTP API

HTTP가 데이터를 전송하는 방식 중 하나로, HTML이나 이미지등의 파일이 아닌 JSON등의 데이터를 전달한다. 서버간의 인터페이스를 담당한다.
