## Chapter 11. DSL(Domain Specific Language)

### 도메인 특화 언어(DSL, Domain Specific Langauge)
> A Domain-Specific Language (DSL) is a computer language that's targeted to a particular kind of problem, rather than a general purpose language that's aimed at any kind of software problem. 
> Domain-specific languages have been talked about, and used for almost as long as computing has been done.
> > *martin-fowler : Domain-Specific Languages Guid(https://martinfowler.com/dsl.html)*

#### 도메인 특화 언어의 다양한 예시
* Java라는 언어로 SQL의 표현을 쓴다 -> [QueryDSL](http://querydsl.com/)
* Elastic Search에서 마치 Structured Query Language 표현과 비슷하게 JSON으로 표현하고 싶다 -> [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)
* Gradle 스크립트를 Groovy가 아닌 Kotlin으로 표현하고자 한다(```gradle.kts```)
  * [gradle.kts 예시](https://github.com/kotlin-serverside-study/kitchen-force/blob/develop/build.gradle.kts)

#### 제가 볼 땐, DSL을 직접 구현하는 케이스 보다는, 해석하거나 API를 그대로 사용하는 경우가 많을 것 같네요.
* 다만, 일부 유용하게 쓴다고 하면 아래와 같은 장점이 있습니다.
  * 반복되는 보일러 플레이트 코드를 줄이고, 
  * 코드를 직관적이고 깔끔하게 표현 가능
  * 프로그리밍 언어 보다는 **특정 도메인의 언어**에 가깝게 설계될 수록 비IT 전문가도 코드를 이해하기 쉬움.
* 예시 : https://toss.tech/article/kotlin-dsl-restdocs

### 경험상 유용하게 사용했던 DSL 라이브러리
* kotlin-test(https://codechacha.com/ko/unittest-with-kotlintest-in-kotlin/)
* mockk(https://mockk.io/)
* 가끔씩 SpringBoot의 Kotlin Support 기능으로 일부 스프링 기능들에 대해서 Kotlin DSL 확장 API를 제공하기도 합니다.
  * mockMvc with Kotlin DSL(https://www.baeldung.com/kotlin/mockmvc-kotlin-dsl)
    * 다만 이 경우 ```Spring Rest Doc```과 연동해서 사용하는 경우에 제약이 생기는 것 같습니다.
  * spring security dsl(https://www.baeldung.com/kotlin/spring-security-dsl)