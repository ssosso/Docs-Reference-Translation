# 소개

## Slick이란 무엇인가
Slick(Scala Language-Integrated Connection Kit)은 관계형 데이터베이스로 작업하기 쉽도록 [TypeSafe](http://www.typesafe.com/)<sup>1)</sup>가 만든 Scala용 FRM(Functional Relational Mapping)이다. 데이터베이스에 저장된 데이터를 Scala Collection을 사용하는 것처럼 사용할 수 있고, 그와 동시에 데이터베이스 접근이 발생하는 시기와 전송하는 데이터를 완전히 제어할 수 있다. SQL을 직접 사용할 수도 있다. 데이터베이스 작업은 비동기로 실행되므로 Slick은 [Play](https://playframework.com/) 및 [Akka](http://akka.io/)를 기반으로 하는 반응형 어플리케이션에 가장 적합하다.

```Scala
val limit = 10.0

// 쿼리를 다음과 같이 표현할 수 있다
( for( c <- coffees; if c.price < limit ) yield c.name ).result

// 동일한 의미의 SQL: select COF_NAME from COFFEES where PRICE < 10.0
```

쿼리에 SQL 대신 Scala를 사용하면 컴파일 타임의 안정성과 구성성(Compositionality)에 좋다. Slick은 확장가능한 자체적인 쿼리 컴파일러를 사용하여 다른 백엔드 데이터베이스에 대한 쿼리를 생성할 수 있다.

TypeSafe [Activator](https://typesafe.com/activator)에서 [Hello Slick 템플릿](https://typesafe.com/activator/template/hello-slick-3.1)을 사용해서 몇 분만에 Slick 배울 수 있다. 또한 Slick이 코드를 생성할 수 있는 데이터베이스 시스템 지원 목록을 [여기](http://slick.lightbend.com/doc/3.1.1/supported-databases.html)에서 살펴볼 수 있다.

<sub>1) TypeSafe은 Scala와 Akka 등을 배포하는 회사이다. 최근에는 LightBend로 사명을 변경하였다(관련 글 [https://www.lightbend.com/blog/typesafe-changes-name-to-lightbend](https://www.lightbend.com/blog/typesafe-changes-name-to-lightbend)).</sub>

## Functional Relational mapping
