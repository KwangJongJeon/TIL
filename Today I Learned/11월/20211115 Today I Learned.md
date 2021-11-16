# 20211115 Today I Learned

# 📖 오늘 무슨 일을 했나?

해당 강의를 듣고 공부한 내용입니다.

https://www.inflearn.com/course/ORM-JPA-Basic/dashboard

## 📗 영속성 컨텍스트

 "엔티티를 영구 저장하는 환경" 논리적인 개념이며, JPA의 엔티티 매니저를 통해서 영속성 컨텍스트에 접근 할 수 있다. JPA의 엔티티 매니저와 영속성 컨텍스트는 다음과 같이 구성된다.

![엔티티매니저](https://user-images.githubusercontent.com/19809346/141780084-0fb536cd-292c-4036-a564-b0eaaa8b88bb.png)
[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

 JPA만 단독으로 사용하는 환경이면 엔티티매니저와 영속성 컨텍스트가 1:1로 매칭되고, J2EE나 스프링 프레임워크와 같은 컨테이너 환경일 경우 N:1로 관리된다.

엔티티의 생명주기는 다음과 같이 나뉜다.

- 비영속(new/transient): 영속성 컨텍스트와 전혀 관계가 없는 새로운 상태
- 영속(managed): 영속성 컨텍스트에 관리되는 상태
- 준영속(detached): 영속성 컨텍스트에 저장되었다가 분리된 상태
- 삭제(removed): 삭제 된 상태

 JPA Entity 객체를 생성 했을 때 비영속 상태로 시작하고, 엔티티 매니저의 persist() 함수를 사용하면 영속 상태로 변환된다.

```java
public class JpaMain {

    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

        EntityManager em = emf.createEntityManager();

        EntityTransaction tx = em.getTransaction();

				// 트랜잭션 시작
        tx.begin();
        try{
						//객체를 생성한 상태(비영속)
            Member member = new Member();
            member.setId("member1");
            member.setUsername(“회원1”);

            //객체를 저장한 상태(영속)
            em.persist(member);
            
						// 모든 변경 사항은 commit()혹은 flush()전에는 데이터베이스에 반영되지 않는다.
						// 단 ID의 strategy에 따라서 persist() 메소드에서
            // 데이터베이스에 쿼리가 날아갈 수도 있다.
            tx.commit();
        } catch (Exception e) {
            tx.rollback();
        } finally {
            em.close();
            emf.close();
        }
    }
}
```

 위에서 try / catch / finally 구문을 사용하는 것을 볼 수 있는데, 만약 commit이 실패하더라도 EntityManager와 EntityManagerFactory는 종료되어야하기 때문이다.

위의 em.persist() 이외에도 다음과 같은 것들이 있다.

```java
// 회원 엔티티를 영속성 컨텍스트에서 분리, 준영속 상태
em.detach(member);

// 객체를 삭제한 상태(삭제)
em.remove(member);
```

## 📗영속성 컨텍스트의 이점

- 1차 캐시
- 동일성(identity) 보장
- 트랜잭션을 지원하는 쓰기 지연(transacional write-behind)
- 변경 감지(Dirty Checking)
- 지연 로딩(Lazy Loading)

## 📙1차 캐시란

객체에서 

 객체를 영속화(persist() 사용)시키면 영속성 컨텍스트의 캐시테이블에 값이 올라간다.

![1차캐시](https://user-images.githubusercontent.com/19809346/141780088-d8d47856-7313-43a3-9c90-8cbddc991269.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

다음과 같은 코드가 있다고 해보자.

```java
Member member = new Member();
member.setId("member1");
member.setUsername("회원1");

//1차 캐시에 저장됨
em.persist(member);

//1차 캐시에서 조회
Member findMember = em.find(Member.class, "member1");
```

 위와 같은 코드가 있을 경우 em.persist에서 member가 영속 컨테이너에 올라가면서 영속 상태에 놓이게 된다. 이 때 em.find(...)을 하게 되면, 해당 내용을 DB에서 가져오는게 아니라 1차 캐시에 있는 값을 가져오게 된다. 

 만약 DB에만 저장되어있고 1차 캐시에 저장되어 있지 않은 값을 가져온다고 할 때 다음 그림과 같이 진행된다.

![1차캐시2](https://user-images.githubusercontent.com/19809346/141780075-fee20138-3de4-43df-bd0b-a38dc644fe79.png)
 영속성 컨텍스트는 영속 엔티티의 동일성 또한 보장한다.

```java
em.find(Member.class, "member1");
em.find(Member.class, "member2");

System.out.println(a == b); // 동일성 비교 true
```

## 📙 쓰기 지연

 영속성 컨테이너는 commit()이나 flush()전의 쿼리들을 모아 둔 다음 commit()등의 메소드 이후에 한번에 보낸다.

```java
EntityManager em = emf.createEntityManager();
EntityTransaction transaction = em.getTransaction();
//엔티티 매니저는 데이터 변경시 트랜잭션을 시작해야 한다.
transaction.begin(); // [트랜잭션] 시작
em.persist(memberA);
em.persist(memberB);
//여기까지 INSERT SQL을 데이터베이스에 보내지 않는다.
//커밋하는 순간 데이터베이스에 INSERT SQL을 보낸다.
transaction.commit(); // [트랜잭션] 커밋
```

![쓰기지연](https://user-images.githubusercontent.com/19809346/141780082-afb2da8f-1b16-4bf4-96a1-358a073b6c55.png)
쓰기지연 - [https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

 위의 그림에서 처럼 쓰기 지연 SQL 저장소에 SQL들을 모은 다음 commit()이 들어오면 한번에 DB에 SQL 구문을 보낸다.

## 📙 변경감지 (Dirty Checking)

 JPA에서는 변경을 진행하려 할 경우 em.update()등의 구문 없이 영속 상태의 엔티티 객체의 set메소드를 call 함으로써 변경을 진행 할 수 있다. 이런 일이 어떻게 일어 날 수 있을까? 

이는 Dirty Checking이라는 방법을 통해 진행된다. 

![변경감지](https://user-images.githubusercontent.com/19809346/141780079-c3d13b63-4a73-4069-aaae-a6db55a44ba3.png)
 위에 그림에서와 같이, 만약 영속화된 객체에 데이터 변경이 발생한다면 스냅샷에 변경된 객체를 집어넣는다. 그 뒤에 트랜잭션이 commit 될 때 스냅샷과 Enitity를 일일히 비교해서, 다른 부분이 있다면 JPA가 update 쿼리문을 SQL 저장소에 넣은 다음 commit할 때 일괄적으로 보내게 된다.

## 📙 플러시

플러시란 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영하는 것을 의미한다. 플러시는 다음과 같을 때 발생한다. 

- em.flush() - 직접 호출
- 트랜잭션 커밋 - 플러시 자동 호출
- JPQL 쿼리 실행 - 플러시 자동 호출

또한 JPQL를 실행 할 때 자동으로 flush가 일어난다.

```java
em.persist(memberA);
em.persist(memberB);
em.persist(memberC);
//중간에 JPQL 실행
query = em.createQuery("select m from Member m", Member.class); // flush 실행!
List<Member> members= query.getResultList();
```

### 플러시 모드

- [FlushModeType.AUTO](http://FlushModeType.AUTO) : 커밋이나 쿼리를 실행할 때 플러시 (기본값)
- FlushModeType.COMMIT : 커밋할 때만 플러시

```java
// 다음과 같이 적용이 가능하다.
em.setFlushMode(FlushMOdeType.COMMIT);
```

웬만하면 기본 값인 FlushModeType.AUTO를 사용하자.

** 플러시가 일어난다고 해서 영속성 컨텍스트를 비우지는 않고, 단순히 변경 내용만 데이터베이스에 동기화한다.

엔티티 매핑에 관해서도 배웠는데 이는 내일 정리해야겠다 ;P
