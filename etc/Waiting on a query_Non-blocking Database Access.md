# Waiting on a query: Non-blocking Database access
> 쿼리 기다리기: 데이터베이스 비차단 접근
> 2016 9월 28일 5:14 PM
> 원글 URL : https://www.voxxed.com/2016/09/non-blocking-database-access/

<iframe width="560" height="315" src="https://www.youtube.com/embed/onYjxRcToto" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
<br/><br/><br/>

지난 주 JavaOne에서 오라클 데이터베이스 JDBC 설계가인 Douglas Surber가 'JDBC의 다음 단계는 무엇인가'라는 주제로 발표를 했다. SQL 데이터베이스에 접근할 수 있는 새로운 Java 표준인 비차단 API(non-blocking API)를 미리 살펴보았다. JDBC를 대체하거나 확장한다기 보다 JDBC처럼 데이터베이스에 대한 접근을 제공하는 별도의 API이다.

Douglas Surber는 Voxxed와 이야기하며 이 새로운 표준이 어떤 모습일지 이것이 달성하고자 하는 목표는 무엇인지에 대해 몇가지 심도있는 통찰을 했다.

## JDBC 프로젝트
JDBC(Java Database Connectivity)는 Java로 만들어진 프로그램을 관계형 데이터베이스에 있는 데이터와 연결하기 위한 인터페이스를 제공하는 Java 표준이다. 요청은 SQL로 인코딩되어 데이터베이스를 관리하는 프로그램에 전달된다. 이는 ```java.sql``` 인터페이스를 통해 정의되고 구현된다.

기존에는 데이터베이스 동적 호출의 어려움을 극복하기 위해 더 많은 스레드가 생성되었다. 하지만 이로 인해 더 많은 스레드 스케줄링과 경쟁이 발생할 수 있고 시스템이 느려질 수가 있다. 처리량이 높은 어플리케이션은 스레드 차단과 지연시간이 길어지는 것을 피하기 위해 세심한 보정이 필요할 수도 있다.

## 비차단 API (Non-blocking API)
비차단 API 표준은 절대 어떠한 사용자 스레드도 차단하지 않는 것을 목표로 한다. 비차단 API는 기존 JDBC API를 확장하거나 대체하는 것이 아닌 데이터베이스 접근을 위한 일종의 대안 API로써 봐야 한다.

Surber에 따르면: "JavaOne 세션에서는 특정 구현이 아닌 표준으로써의 API에만 초첨을 맞추었다. 가능한 한 API 구현을 구체적으로 명시하지는 않았다. 어떤 면에서 이는 java.sql API보다 구현에 있어 명확한 기준이 없는 것이다. 오라클은 오라클 데이터베이스에 맞는 개념 구현 증명서를 보유하고 있다. Type 4 오라클 데이터베이스 JDBC 드라이버로부터 파생된 것이지만 크게 변경되어 완벽히 비차단적이다. Java NIO Selector에 기반하고 있어 기본적으로 비차단적이다. 그러나 단순히 개념에 대한 증명서일 뿐이다. Java 표준으로서 오라클은 최선을 다했지만 벤더사들이 이를 구현할 수 있었다."

### 다른 스레드를 생성하지 않는 이유는?
Surber에 따르면, 스레드는 너무 비용이 많이 들어 확장가능한 해결책이 아니다. 데이터베이스 접근에 사용하는 스레드 수를 줄이는 것이 목표다.

"Java 진영 경험상 스레드는 비용이 많이 든다. 유휴 상태의 스레드 조차도 자원을 소비한다. 스레드 스케줄링과 스레드 간 문맥 전환은 희소한 CPU 주기를 소비한다. Java 역사상 대체로 무언가 차단되었을 때 다른 스레드를 생성하는 것이 해결책이었지만 원하는 정도까지 확장할 수 없다는 것을 알게 되었다. 스레드의 수를 적게 유지하고 기기가 제공하는 하드웨어 스레드의 수를 동일한 규모로 유지하며, 모든 스레드를 항상 사용하도록 유지하는 것이 훨씬 낫다는 사실을 발견했다. 유휴한 스레드와 차단된 스레드는 자원을 낭비한다."

### 기존 JDBC API는 어떤가?
이미 여러 비차단 JDBC API들이 있다. 비차단 JDBC API 표준은 그러한 솔루션들(이를테면 Streams, NodeJS)이 제공하지 못하는 다른 어떠한 것을 제공할 수 있는지 물었다.

"NodeJS는 Java API가 아니다. NodeJS에서의 데이터베이스 비차단 접근은 NodeJS의 전체적인 인프라에 의존한다. 그러한 인프라는 Java SE에는 존재하지 않는다. 반면에 Streams API는 분명히 Java SE의 일부분이며, 비차단 JDBC API를 만들 수 있는 확실한 후보이다."

#### 스트림을 근본적으로 차단한다
"3년 전 이 프로젝트를 시작했을 때 API의 첫 번째 결과물은 Streams 기반이었다. Java Streams는 근본적으로 기본적인 레벨에서는 차단정책을 사용한다는 것을 재빠르게 깨달았다. Java Core 라이브러리 팀은 이러한 사실을 아주 명백히 했다. 또한 이것이 현재 Streams 모델의 근본이며 우리가 관심있는 기간 내에는 변화하지 않을 것이라는 것도 명확히 했다. 우리는 비차단 JDBC API 기반의 Streams를 만들었지만 이것은 유용하지 않았다. Streams 기반으로 만드는 것의 가치는 Streams 인프라를 활용할 수 있는 능력에 달려있다. Streams는 기본적으로 차단적이기 때문에 Streams 기반의 어떠한 비차단 JDBC API도 나머지 Streams 인프라와 연결되는 즉시 차단을 시작한다. 이는 우리 요구사항을 만족시키지 못했다."

#### API를 Java 클래스 라이브러리와 통합해야 한다
"조금이라도 Java와 연관있는 모든 데이터베이스 비차단 접근 API를 찾고 연구하는데 노력을 많이 했다. 모든 것을 다 살펴봤다고는 할 수 없지만 우리는 틀림없이 시도했다. 어떠한 것도 우리의 요구사항과 맞지는 않았다. 가장 큰 걸림돌은 Java 표준이 되기 위해서 API가 나머지 Java 클래스 라이브러리와 잘 통합할 수 있어야 한다는 것이었다. 대부분의 대안들은 Java 클래스 라이브러리의 일부분이지 않은 인프라에 의존적이다. 모나딕<sup>monadic</sup> 프로그래밍은 클래스 라이브러리 ```java.util.concurrent.CompletionStage```과 ```CompletableFuture```로 잘 표현된다. 그래서 JavaOne에서 발표한 API는 이것을 기반으로 만들었다."

### 실행 모델
For the new standard, everything will be an operation. Operations consist of SQL, parameter assignments, result handling, submission and CompletableFuture. The user thread creates the Operation and submits it. The implementation of the non-blocking API will execute the Operation asynchronously.

Operations can be grouped. Members are submitted to a group, and OperationGroup is submitted to a unit. It will have its own result handling and CompletableFuture. The members will be executed in the order they are submitted, or parallel in any order. The error response can be dependent, skipping remaining group members, or independent, leaving the other group members unaffected.

We asked Surber what the use case is for an asynchronous call to a database, and how the Operation object comes into play:

“Database access does not have to be synchronous so much as it needs to be ordered; each database access follows the preceding one in a well-defined order. Ordered database accesses does not have to be synchronous. The non-blocking JDBC API presented in the JavaOne session makes that very overt. Each database access is represented by an Operation object. Operations are executed in a well-defined order so, for example, a query will definitely precede an update if that’s what the programmer specifies. At the same time nothing blocks. Creating an Operation does not require any blocking. Creating a sequence of Operations, including ones that depend on prior ones, does not require any blocking. So the programmer can clearly specify a sequence of database accesses without blocking and without entering call-back hell, which is another critical goal of the project.”

### 코드 예제
이 예제들은 [Douglas Surber의 JavaOne 슬라이드](https://static.rainfocus.com/oracle/oow16/sess/1461693351182001EmRq/ppt/CONF1578%2020160916.pdf)에서 다시 볼 수 있다. 표준이라는 것이 어떠한 것인지 보여주는 예제이지만 변경될 수도 있다.

#### 단순 Insert
``` java
public void trivialInsert(DataSource ds) {
   String sql = "insert into tab values (<<i id>>, <<i name>>, <<i answer>>)";
   try (Connection conn = ds.getConnection()) {
      conn.countOperation(sql)
         .set("id", 1, JdbcType.NUMERIC)
         .set("name", "Deep Thought", JdbcType.VARCHAR)
         .set("answer", 42, JdbcType.NUMERIC)
         .submit();
   }
}
```

매개변수는 아직 예비 단계이다. 이 예제에서의 매개변수는 아래와 같다.

```
<<i in_parameter>>
<<o out_parameter>>
<<io inout_parameter>>
<<column_name>> same as column_name <<o column_name>>
```

예를 들어,

```sql
select <<id>>, concat(first_name, ‘ ‘, family_name) <<o name>>
from tab where id = <<i target>> /* <<not_a_marker>> */
```

Surber에 따르면: "매개변수 표시가 아닌 매개변수 표시 패턴은 (리터럴과 코멘트와 같이) 모두 제외해야 한다."
“Any occurrence of the parameter marker pattern that is not a parameter marker must be escaped, e.g. in literals or in comments”.

#### 단순 Select
```java
public void trivialSelect(DataSource ds, List<Integer> result) {
 String sql = "select <<id>>, <<name>>, <<answer>> " +
              "from tab where answer = <<i target>>";
 try (Connection conn = ds.getConnection()) {
    conn.<List<Integer>>rowOperation(sql)
      .set("target", 42, JdbcType.NUMERIC)
      .rowAggregator( (ignore, row) -> {
          result.add(row.get("id", Integer.class));
          return null;
       } )
       .submit();
 }
}
```

심화 예제(기본적인 Select, 대량 Insert, 병렬 Update 등)는 슬라이드에 있다.

### 다음 단계
새로운 표준을 기대할 수 있는 시점이므로 우리는 얼마간 기다려야 할지도 모른다.

"초창기 비차단 JDBC 프로젝트의 목표는 API가 Java 표준이 되는 것이었다. 몇몇 비표준 비차단 JDBC API들이 이미 있기 때문에 또 다른 비표준 비차단 API르르 만드는 것은 별로 이점이 없다. 오직 표준 API만이 이점이 될 수 있다. 그 어떤 표준 API도 "
“None of those non-standard APIs have achieved much penetration, not because they are technically deficient, but purely because they are non-standard.”

“The Java community is moving toward non-blocking APIs in many areas so there is a clear need for a standard non-blocking JDBC API. Oracle would like to have some kind of standard non-blocking JDBC in Java as soon as possible for our own needs. It certainly doesn’t have to be the API presented in the JavaOne session, though that’s what we will take to the Expert Group as a jumping off point. Exactly when a non-blocking JDBC API will appear in Java is a function of how fast the JDBC Expert Group and the JCP can reach consensus. Our target is Java 10, but that’s pretty aggressive.”

For now, the JDBC Expert Group will continue development, and it will be submitted through the Java Community Process.
