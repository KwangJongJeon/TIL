# 20211019 Today I Learned

# ๐ฏ ์ค๋์ ๋ชฉํ

- [x]  Mock ์กฐ์ฌํ ๋ด์ฉ ๋ธ๋ก๊ทธ์ ์ ๋ฆฌํด์ ์ฌ๋ฆฌ๊ธฐ
- [x]  Mockito ์กฐ์ฌํ ๋ด์ฉ ๋ธ๋ก๊ทธ์ ์ ๋ฆฌํด์ ์ฌ๋ฆฌ๊ธฐ
- [x]  IP, TCP, UDP ๋ฑ๋ฑ ๋ด์ฉ ๋ธ๋ก๊ทธ์ ์ ๋ฆฌํด์ ์ฌ๋ฆฌ๊ธฐ
- [x]  ๊น์ํ๋ JPA ์ธํ๋ฐ ๊ฐ์ ๋ฃ๊ธฐ

# ๐ ์ค๋ ๋ฌด์จ ์ผ์ ํ๋?

1. ์ด์  Mock๊ณผ Mockito์ ๋ํด ์กฐ์ฌํ ๋ด์ฉ์ ๋ธ๋ก๊ทธ์ ์ฌ๋ ธ๋ค.  [https://kj97.tistory.com/category/ํ์คํธ](https://kj97.tistory.com/category/%ED%85%8C%EC%8A%A4%ED%8A%B8)
2. IP, TCP, UDP๋ฑ์ ๋ํ ๋ด์ฉ์ ์ ๋ฆฌํด์ ์ฌ๋ ธ๋ค.
3. JPA ๊ฐ์๋ฅผ ํ๋ ๋ค์๋ค. 

# ๐์ค๋ ๋ฐฐ์ด ๊ฒ ์ ๋ฆฌ

์ธ์ ๋ ์ ์ธ์ง๊ณ ์๋ ๊น์ํ๋์ ์ธ๊ฐ์ ๋ณด๊ณ  ์ ๋ฆฌํ๋ค. 

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

## ๊ธฐ์กด SQL์ ๋ฌธ์ ์ 

1. ๋น์ทํ ๊ธฐ๋ฅ์ ํ๋ ์ฟผ๋ฆฌ๋ฅผ ๊ณ์ํด์ ๋ฐ๋ณตํด ์ง์ผ ํ๋ค. ex) CRUD์๋ฌด
2. SQL์ ์์กดํด์ ๊ฐ๋ฐ์ ํด์ผํ๋ค.
3. ํจ๋ฌ๋ค์์ด ๋ถ์ผ์นํ๋ค. Java ๊ฐ์ ๊ฒฝ์ฐ ๊ฐ์ฒด์งํฅ ํ๋ก๊ทธ๋๋ฐ ํ์์ ๋ฐ๋ฅด๋๋ฐ ๋ฐํด DB๋ ๊ด๊ณํ DB์ด๊ธฐ ๋๋ฌธ์ ๋๊ฐ์ ํจ๋ฌ๋ค์์ผ๋ก ์ง์คํด์ ๊ฐ๋ฐ ํ  ์ ์๋ค. 

![์ํฐํฐ_์ ๋ขฐ๋ฌธ์ ](https://user-images.githubusercontent.com/19809346/137914744-f963154e-5139-4cc5-9157-c5d61182de88.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

1. ์ํฐํฐ์ ์ ๋ขฐ ๋ฌธ์ ๊ฐ ๋ฐ์ ํ  ์ ์๋ค. ์์ ๊ทธ๋ฆผ๊ณผ ๊ฐ์ด ๋๊ตฐ๊ฐ๊ฐ ๋ง๋ค์ด๋ memverDAO๊ฐ ์๋ค๊ณ  ํ  ๋ .getTeam()๊ณผ ๊ฐ์ getter ๋ฉ์๋๋ฅผ ํธ์ถํ  ๊ฒฝ์ฐ ์ด๊ฒ ์ด๋ค ํ์์ผ๋ก ๋ง๋ค์ด ์ง ๊ฒ์ธ์ง ์ ์ ์๊ณ , ์ค์ ๋ก ์ด๊ฒ ๋์๋๋์ง๋ ํ์  ํ  ์ ์๋ค. ์คํ๋ง์์ ์ฌ์ฉํ๋ Layered Architecture์์๋ ์ด์  ๊ณ์ธต์ ๋ํ ์ ๋ขฐ๊ฐ ํ์์ธ๋ฐ, ์ด๋ ๊ฒ ๊ฐ๋ฐํ  ๊ฒฝ์ฐ ๋ผ๋ฆฌ์ ์ผ๋ก ๊ฐ๊ฒฐํฉ์ด ์ผ์ด๋ ์ด์  ๊ณ์ธต์ ์ ๋ขฐํ  ์ ์๊ฒ ๋๋ค. ์ฆ ์ํคํ์ณ์ ๊ณ์ธต ๋ถํ ์ด ์ด๋ ค์์ง๋ค.

# JPA์ ๋ฑ์ฅ

## JPA๋

- Java Persistence API
- ์๋ฐ ์ง์์ ORM(Object Relational Mapping) ๊ธฐ์  ํ์ค
- ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๊ฐ์ฒด์งํฅ ์ธ์ด๊ฐ ํจ๋ฌ๋ค์์ ๋ถ์ผ์น๋ฅผ ํด๊ฒฐํ๋ค.
- ์ ํ๋ฆฌ์ผ์ด์๊ณผ JDBC ์ฌ์ด์์ ๋์ํ๋ค.
- ์๋ฐ ๋ฌธ๋ฒ๋๋ก ๊ตฌํํ๋ฉด ๋๋ฏ๋ก ์ ์ง๋ณด์์ฑ๊ณผ ์์ฐ์ฑ์ด ๋ฐ์ด๋๋ค.
- ๋ฐ์ ๊ทธ๋ฆผ๊ณผ ๊ฐ์ DB์ ์ง์  ํต์ ํ๋ ๊ท์ฐฎ์ ์ผ๋ค์ ์ ๋ถ ๋์ ํด์ค๋ค.
- ๊ตฌํ ๊ฐ์ฒด๋ก ํ์ด๋ฒ๋ค์ดํธ, EclipseLink, DataNucleus๋ฑ์ด ์๋ค.

![JPA_์ ์ฅ](https://user-images.githubusercontent.com/19809346/137914751-f7a36b03-2752-4b48-8be9-386c36d44c3d.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

![JPA_์กฐํ](https://user-images.githubusercontent.com/19809346/137914753-8f49ad7a-b88c-48b5-82e8-94db51392361.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)

### ORM์ ์ด๋ค ์์ผ๋ก ์คํ๋๋๊ฐ?

๊ฐ์ฒด๋ ๊ฐ์ฒด๋๋ก ์ค๊ณํ๊ณ , ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค๋๋ก ์ค๊ณํ ๋ค์์ ORM์ด ์ค๊ฐ์์ ๋งคํํด์ฃผ๋ ํ์์ผ๋ก ์คํ๋๋ค.

## ๋ฌธ๋ฒ

- ์ ์ฅ: **jpa.persist**(member);
- ์กฐํ: Member member = **jpa.find(memberId)**;
- ์์ : **member.setName**("๋ณ๊ฒฝํ  ์ด๋ฆ");
- ์ญ์ : **jpa.remove**(member);

## JPA์ ์ฑ๋ฅ ์ต์ ํ ๊ธฐ๋ฅ

1. 1์ฐจ ์์น์ ๋์ผ์ฑ(identity) ๋ณด์ฅ
2. ํธ๋์ญ์์ ์ง์ํ๋ ์ฐ๊ธฐ ์ง์ฐ
3. ์ง์ฐ ๋ก๋ฉ(Lazy Loading)

## ์ง์ฐ๋ก๋ฉ๊ณผ ์ฆ์๋ก๋ฉ

- ์ง์ฐ ๋ก๋ฉ: ๊ฐ์ฒด๊ฐ ์ค์  ์ฌ์ฉ๋  ๋ ๋ก๋ฉ
- ์ฆ์ ๋ก๋ฉ: JOIN SQL๋ก ํ๋ฒ์ ์ฐ๊ด๋ ๊ฐ์ฒด๊น์ง ๋ฏธ๋ฆฌ ์กฐํ

![์ง์ฐ์ฆ์](https://user-images.githubusercontent.com/19809346/137914747-b2eec621-0cf9-4f5c-b7ee-a723d6ca61ce.png)

[https://www.inflearn.com/course/ORM-JPA-Basic/dashboard](https://www.inflearn.com/course/ORM-JPA-Basic/dashboard)
