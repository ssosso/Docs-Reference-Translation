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
함수형 프로그래머는 오랫동안 관계형 데이터베이스를 연결할 때 **객체와 관계형(Object-Relational)** 또는 **객체와 수학(Object-Math)** 사이의 임피던스 불일치<sup>2)</sup> 문제를 겪고 있다. Slick의 새로운 FRM(Functional Relational Mapping) 패러다임은 느슨한 커플링, 최소한의 설정 등 관계형 데이터베이스와의 연결에 방해가 되는 것을 해결하여 매핑을 보다 쉽게 완료할 수 있도록 한다.

관계형 모델과 맞서려고 하지 않고 함수형 패러다임을 통해 받아들이고자 했다. 객체 모델과 데이터베이스 모델 사이의 격차를 줄이는 대신 개발자가 SQL코드를 작성할 필요가 없도록 데이터베이트 모델을 Scala에 가지고 왔다.

```Scala
class Coffees(tag: Tag) extends Table[(String, Double)](tag, "COFFEES") {
  def name = column[String]("COF_NAME", O.PrimaryKey)
  def price = column[Double]("PRICE")
  def * = (name, price)
}
val coffees = TableQuery[Coffees]
```

Slick은 데이터베이스를 Scala에 직접 통합하여 일반적인 Scala 클래스와 컬렉션을 사용하여 저장된 원격 데이터를 메모리 내 데이터와 동일한 방식으로 쿼리하고 처리할 수 있도록 한다.

```Scala
// 오직 "name" 칼럼만 리턴하는 쿼리
// 동일한 의미의 SQL: select NAME from COFFEES
coffees.map(_.name)

// price < 10.0 한 데이터만 리턴하는 쿼리
// 동일한 의미의 SQL: select * from COFFEES where PRICE < 10.0
coffees.filter(_.price < 10.0)
```

Slick은 데이터베이스 접근 시점과 전송할 데이터를 완전히 제어할 수 있게 한다. Slick의 FRM에 있는 언어 통합 쿼리 모델을 Microsoft의 LINQ 프로젝트에서 영감을 얻었으며, Ericsson의 Mnesia 초기 작업까지 거슬러 올라간 개념을 활용한다.

함수형 프로그래밍을 위한 Slick의 FRM 접근법의 주요 이점은 다음과 같다.

### 사전 최적화를 통한 효율성

FRM은 보다 효율적인 방식으로 연결한다. ORM과는 다르게 데이터베이스와의 통신을 사전 최적화하는 과정이 있다. ORM보다 FRM을 사용하면 어플리케이션을 보다 빠르게 구동할 수 있다.

### 타입 안정성을 통한 지루한 이슈해결 문제 해결

FRM은 데이터베이스 쿼리를 만들 때 타입 안정성을 가진다. 컴파일러가 타입이 지정되지 않은 문자열에서 에러를 찾는 데 필요한 지루한 타입이슈를 자동 타입찾기를 통해 해결하기 때문에 개발자들은 더욱 생산적일 수 있게 된다.

```Scala
// 안전한 타입의 칼럼 정의로 인해서
// "select PRICE from COFFEES" 쿼리의 결과는 Double 타입의 Seq이다.
val coffeeNames: Future[Seq[Double]] = db.run(
  coffees.map(_.price).result
)

// 쿼리 생성기는 타입 안정성을 가진다.
coffees.filter(_.price < 10.0)
// filter에 하나의 문자열만 사용하면 컴파일 에러가 발생한다.
```

price 칼럼명을 잘못 입력하면 어떻게 될까? 컴파일러는 아래와 같은 오류를 낼 것이다.

```Scala
GettingStartedOverview.scala:89: value prices is not a member of com.typesafe.slick.docs.GettingStartedOverview.Coffees
        coffees.map(_.prices).result
                      ^
```

타입 에러에 대해서도 동일하게 오류를 낸다.

```Scala
GettingStartedOverview.scala:89: type mismatch;
 found   : slick.driver.H2Driver.StreamingDriverAction[Seq[String],String,slick.dbio.Effect.Read]
    (which expands to)  slick.profile.FixedSqlStreamingAction[Seq[String],String,slick.dbio.Effect.Read]
 required: slick.dbio.DBIOAction[Seq[Double],slick.dbio.NoStream,Nothing]
        coffees.map(_.name).result
                            ^
```

### 쿼리 작성을 위한 더 생산적이고 구성가능한 모델

FRM은 쿼리를 생성하기 위한 구성가능한 모델을 제공한다. 이 모델은 아주 자연스러운 모델이다. 조각들을 모아 하나의 쿼리를 생성하고 코드 전반에 걸쳐 이 조각들을 재사용한다.

```Scala
// 10보다 가격이 싼 커피의 이름을 정렬하여 조회하는 쿼리를 생성한다.
coffees.filter(_.price < 10.0).sortBy(_.name).map(_.name)
// 생성된 SQL은 아래와 동일한 의미이다.
// select name from COFFEES where PRICE < 10.0 order by NAME
```

<sub>2) 임피던스(impedance)는 어떤 물체가 전류의 흐름을 방해하는 양을 측정한 것이다. 임피던스가 불일치한다는 것은 건전지를 예로 들어 설명할 수 있다. 광 에너지를 생산하는 배터리를 손전등에 연결하면 광에너지를 생산할 수 있음에도 불구하고 임피던스가 맞지 않아 효율을 낼 수 없고 반대로 AAA 건전지를 광 에너지를 쏘는 스포트라이트에 연결해도 임피던스가 맞지 않아 낮은 출력만을 내게 된다. 각각 임피던스가 맞는 광에너지 건전지-스포트라이트, AAA건전지-손전등 끼리 연결하면 최대 효율을 내게되는데 이것은 임피던스가 일치한다고 볼 수 있다. 임피던스가 불일치/일치한다는 것을 이러한 예에 비춰 이해할 수 있다. 객체와 관계형 간의 임피던스가 맞지 않아 개발자들은 ORM, FRM와 같은 것을 사용하게 되므로 '임피던스 불일치'는 해당 문단에서 빠질 수 없는 이야기이다. (관련 글 [https://haacked.com/archive/2004/06/15/impedance-mismatch.aspx/](https://haacked.com/archive/2004/06/15/impedance-mismatch.aspx/))</sub>


## 반응형 어플리케이션
Slick은 비동기, 비차단 어플리케이션 설계를 하기 쉽게 한다. 그리고 [반응형 메니페스토<sup>Reactive Manifesto</sup>](http://www.reactivemanifesto.org/)에 따라 어플리케이션을 만드는 것을 지원한다.

## 일반 SQL 문자열 지원

```Scala
val limit = 10.0

sql"select COF_NAME from COFFEES where PRICE < $limit".as[String]

// Automatically using a bind variable to be safe from SQL injection:
// select COF_NAME from COFFEES where PRICE < ?
```

## 라이센스
Slick은 BSD-Style 무료 오픈소스 소프트웨어 라이센스로 배포된다. 대형 상용 데이터베이스 시스템에 대한 Slick 드라이버 라이센스 상세내용은 Slick 상용 확장 애드온 패키지 챕터를 살펴보면 된다.

## 다음 단계
