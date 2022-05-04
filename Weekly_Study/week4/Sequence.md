
## Sequence
* Java의 ```Iterabele```에서 제공하는 기능과 유사함.
* 다층적인 컬렉션 처리(multi-step collection processing) 기능이 추가되어 있다는 점에서 일반적인 iterable과의 차이가 있음.
  * 시퀀스는 선행 연산 - 후행 연산 의 구조에서 요소 하나 하나마다 선/후행의 연산을 수행을 하게됨 
  * 일반적인 Collection 연산은 모든 요소가 선행 연산 이후 후행 연산을 수행하는 처리 흐름을 가짐
  * 이러한 시퀀스의 컨셉을 지연 평가(lazy evaluation) 라고 
* **Java의 Stream API의 메커니즘과 매우 유사한 형태** 입니다.
* 자바 Stream API에 대한 설명은 따로 **[이 문서](./Stream_api.md)**에 정리하였습니다.

* 예제를 보시면....
```kotlin
val words = "The quick brown fox jumps over the lazy dog".split(" ")
val lengthsList = words
    .filter { println("filter: $it"); it.length > 3 }  // 1번 연산 단어 수가 3글자 초과
    .map { println("length: ${it.length}"); it.length } // 2번 연산 글자수로 인스턴스 변환 string -> int 타입으로 변경
    .take(4) // 3번 연산 : 4개만 추출한다.

println("Lengths of first 4 words longer than 3 chars:")
println(lengthsList)
```

![](https://kotlinlang.org/docs/images/list-processing.png)
### Java의 Iterable 고찰
* Java에서 ```Iterable``` 인터페이스를 구현한 컬렉션(JCF)은 반복자(Iterator)로 컬랙션 데이터를 탐색할 수 있는 API를 제공한다.
* 1.8 이후에서는 Iterable에 정의된 default 메서드로 인해, 컬렉션 요소 접근시 ```for ~ each``` 문을 활용 할 수 있다.


### Collection 연산
* 자바랑 달리 코틀린에서는 Stream이나 Sequence 변환 없이도 Collection 전체에 대해 Stream API와 동일한 기능을 수행하는 함수들을 제공합니다.
* 이런 함수 구현체가 확장함수(Extension Function) 방식으로 기본 구현되서 제공되고 있습니다.
  * 기본 제공해주는 것 외에 개발자가 필요한 요구사항에 맞게 자체적으로 추가 구현도 가능합니다. 
  * 확장 함수에 대해서는 차주 함수 단원에서 더 깊이 학습해 보도록 하겠습니다.
* 코틀린은 한 술 더 떠 기존에 Java에서 지원하고 있던 stream api(map, filter, reduce 등등)에 대해서 보다 확장된 기능의 API도 제공해줍니다.
  * ex
    * [mapNotNull](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-not-null.html)
    * [mapIndexed](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/map-indexed.html)
    * [firstNotNullOfOrNull](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections/first-not-null-of-or-null.html)
* 기본 제공하는 것들이 
* API 문서 참고 : https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.collections

### Collection 연산과 Sequence 연산의 차이

* Collection 연산
```kotlin

```

* Sequence 연산
```kotlin
 
```

#### 참고 
* https://docs.oracle.com/javase/8/docs/api/java/lang/Iterable.html

### Reference
* https://kotlinlang.org/docs/sequences.html