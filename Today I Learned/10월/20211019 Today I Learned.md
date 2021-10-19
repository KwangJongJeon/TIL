# 20211019 Today I Learned

# 🎯 오늘의 목표

- [x]  Mock 조사한 내용 블로그에 정리해서 올리기
- [x]  Mockito 조사한 내용 블로그에 정리해서 올리기
- [x]  IP, TCP, UDP 등등 내용 블로그에 정리해서 올리기
- [x]  김영한님 JPA 인프런 강의 듣기

# 📖 오늘 무슨 일을 했나?

1. 어제 Mock과 Mockito에 대해 조사한 내용을 블로그에 올렸다.  [https://kj97.tistory.com/category/테스트](https://kj97.tistory.com/category/%ED%85%8C%EC%8A%A4%ED%8A%B8)
2. IP, TCP, UDP등에 대한 내용을 정리해서 올렸다.
3. JPA 강의를 하나 들었다. 

# 📗오늘 배운 것 정리

언제나 신세지고있는 김영한님의 인강을 보고 정리했다. 

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

## 기존 SQL의 문제점

1. 비슷한 기능을 하는 쿼리를 계속해서 반복해 짜야 한다. ex) CRUD업무
2. SQL에 의존해서 개발을 해야한다.
3. 패러다임이 불일치한다. Java 같은 경우 객체지향 프로그래밍 형식을 따르는데 반해 DB는 관계형 DB이기 때문에 똑같은 패러다임으로 집중해서 개발 할 수 없다. 

![엔티티 신뢰문제.PNG](20211019%20Today%20I%20Learned%207817df1f141446d2bd04a91400c9415c/%EC%97%94%ED%8B%B0%ED%8B%B0_%EC%8B%A0%EB%A2%B0%EB%AC%B8%EC%A0%9C.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

1. 엔티티의 신뢰 문제가 발생 할 수 있다. 위의 그림과 같이 누군가가 만들어둔 memverDAO가 있다고 할 때 .getTeam()과 같은 getter 메소드를 호출할 경우 이게 어떤 타입으로 만들어 진 것인지 알 수 없고, 실제로 이게 동작되는지도 확신 할 수 없다. 스프링에서 사용하는 Layered Architecture에서는 이전 계층에 대한 신뢰가 필수인데, 이렇게 개발할 경우 논리적으로 강결합이 일어나 이전 계층을 신뢰할 수 없게 된다. 즉 아키텍쳐의 계층 분할이 어려워진다.

# JPA의 등장

## JPA란

- Java Persistence API
- 자바 진영의 ORM(Object Relational Mapping) 기술 표준
- 관계형 데이터베이스와 객체지향 언어간 패러다임의 불일치를 해결한다.
- 애플리케이션과 JDBC 사이에서 동작한다.
- 자바 문법대로 구현하면 되므로 유지보수성과 생산성이 뛰어나다.
- 밑의 그림과 같은 DB와 직접 통신하는 귀찮은 일들을 전부 대신해준다.
- 구현 객체로 하이버네이트, EclipseLink, DataNucleus등이 있다.

![JPA 저장.PNG](20211019%20Today%20I%20Learned%207817df1f141446d2bd04a91400c9415c/JPA_%EC%A0%80%EC%9E%A5.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

![JPA 조회.PNG](20211019%20Today%20I%20Learned%207817df1f141446d2bd04a91400c9415c/JPA_%EC%A1%B0%ED%9A%8C.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

### ORM은 어떤 식으로 실행되는가?

객체는 객체대로 설계하고, 관계형 데이터베이스는 관계형 데이터베이스대로 설계한 다음에 ORM이 중간에서 매핑해주는 형식으로 실행된다.

## 문법

- 저장: **jpa.persist**(member);
- 조회: Member member = **jpa.find(memberId)**;
- 수정: **member.setName**("변경할 이름");
- 삭제: **jpa.remove**(member);

## JPA의 성능 최적화 기능

1. 1차 새치와 동일성(identity) 보장
2. 트랜잭션을 지원하는 쓰기 지연
3. 지연 로딩(Lazy Loading)

## 지연로딩과 즉시로딩

- 지연 로딩: 객체가 실제 사용될 때 로딩
- 즉시 로딩: JOIN SQL로 한번에 연관된 객체까지 미리 조회

![지연즉시.PNG](20211019%20Today%20I%20Learned%207817df1f141446d2bd04a91400c9415c/%EC%A7%80%EC%97%B0%EC%A6%89%EC%8B%9C.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)