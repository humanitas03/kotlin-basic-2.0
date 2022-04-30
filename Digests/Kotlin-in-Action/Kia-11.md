## Chapter 11. DSL(Domain Specific Language)

### 도메인 특화 언어(DSL, Domain Specific Langauge)

* Java라는 언어로 SQL의 표현을 쓴다 -> [QueryDSL](http://querydsl.com/)
* Elastic Search에서 마치 Structured Query Language 표현과 비슷하게 JSON으로 표현하고 싶다 -> [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)
* Gradle 스크립트를 Groovy가 아닌 Kotlin으로 표현하고자 한다(```gradle.kts```)


#### 제가 볼 땐, DSL을 직접 구현하는 케이스 보다는, 해석하거나 API를 그대로 사용하는 경우가 많을 것 같네요.
* 다만, 일부 유용하게 쓴다고 하면 반복되는 보일러 플레이트 코드를 줄이고, 표현을 엄청 직관적이고 깔끔하게 유지할 수 있다는 점에서 매력이 있다고 봅니다.
* 예시 : 

### 경험상 유용하게 사용했던 DSL 라이브러리
* kotlin-test(https://codechacha.com/ko/unittest-with-kotlintest-in-kotlin/)
* mockk(https://mockk.io/)
* 가끔씩 SpringBoot의 Kotlin Support 기능으로 일부 스프링 기능들에 대해서 Kotlin DSL 확장 API를 제공하기도 합니다.
  * mockMvc with Kotlin DSL(https://www.baeldung.com/kotlin/mockmvc-kotlin-dsl)
    * 다만 이 경우 ```Spring Rest Doc```과 연동해서 사용하는 경우에 제약이 생기는 것 같습니다.
  * spring security dsl(https://www.baeldung.com/kotlin/spring-security-dsl)