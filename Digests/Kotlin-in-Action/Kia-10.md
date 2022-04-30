## Chapter 10. 애노테이션과 리플렉션
* 애노테이션은 생략하겠습니다.


### Reflection : 실행 시점에 코틀린 객체 내부 관찰
* 리플렉션은 **실행 시점**에 (동적으로) 객체의 프로퍼티와 메소드에 접근할 수 있게 해주는 방법
* 코틀린의 특성상 리플렉션을 다루기 위해서는 두 가지 서로 다른 리플렉션 API를 다뤄야 합니다.
  * **```java.lang.reflect```**
    * 표준 리플렉션
    * 코틀린 클래스도 일반 자바 바이트코드로 컴파일되므로 자바 리플렉션 API도 완벽하게 지원 가능.
  * **```kotlin.reflect```**
    * 자바에는 없는 코틀린 고유 개념에 대한 리플렉션을 제공합니다.

### Kotlin Reflection
> 제가 참고하는 코틀린 인 액션 도서가 타겟을 두고 있는 버전이 1.3 입니다.
* https://kotlinlang.org/docs/reflection.html
* 코틀린 Reflection API를 사용하기 위해서는 ```kotlin-reflect``` 라이브러리가 필요 합니다.
  * 코틀린 리플렉션 API는 자바 리플렉션 API를 대체할 만큼 복잡하지가 않음.
  * 안드로이드와 같이 런타임 라이브러리 크기가 문제가 되는 플랫폼을 위해 별도의 리플렉션 api는 별도의 .jar 파일로 제공

#### KClass
* 클래스 안에 모든 선언을 확인 하고 접근이가능
* 상위 클래스를 얻는 등의 작업들이 가능
* ```Myclass::class``` 키워드를 통해 KClass의 인스턴스를 얻을 수 있음.
#### KCallable
* KCallable은 함수와 프로퍼티를 아우르는 공통 상위 인터페이스
* call 메서드를 사용하여 프로퍼티의 게터를 호출할 수도 있음.
  * call을 사용할 때는 함수 인자를 vararg 리스트로 전달한다.
#### KFunction
* 코틀린에서는 이중콜론(::) 심볼을 통해서 멤버 참조(member reference) 기능을 수행할 수 잇음.
  * 코틀린에서 함수도 멤버 참조 가능
  * 자바에서는 ```메서드 참조(Method Reference)``` 표현과 유사하다고 볼 수 있습니다.
* 이 때, 멤버 참조로 받은 타입은 ```KFunction<..>``` 타입
* KFunction 타입을 통해 함수를 호출하기 위해 더 구체적인 메서드를 사용할 수 있음.
  * ```KFunctionN```인터페이스를 통해 파라미터와 반환값 정보를 알아낼 수 있음.

#### KProperty
* 멤버 프로퍼티는 KProperty로 표현이 가능하다
* KProperty를 통해서 setter나 getter를 호출할 수 있다.

### 개인 경험
#### 코틀린 ```object``` 클래스의 Properties를 리플렉션 API를 통해서 얻어 오고 싶은데....
* 전역 공통 상수를 표현하기 위해 ```object``` 클래스를 이용함.
  * ```object```는 자바에서 지원하지 않는 코틀린 고유의 키워드
  * 컴파일이 되면 ```static class```의 정적 멤버 변수로 취급이 됨
    * 코틀린 ```object``` 클래스
```kotlin
object MyPocketMonsters {
    const val PIKACHU = "PIKA_PIKA"
    const val KKOBUGI = "KKOBUK_KKOBUK"
    const val PAIRI = "PI_PI"
}
```
    * 자바 코드
```java
public static class MyPocketMonsters {
    @NonNull
    public static String PIKACHU = "PIKA_PIKA";
    @NonNull
    public static String KKOBUGI = "KKOBUK_KKOBUK";
    @NonNull    
    public static String PAIRI = "PI_PI";
}
```

* 실행 시점에서 ```object``` 안에 프로퍼티의 값을 읽기 위해서 Kotlin Reflect API 이용(```decraredMemberProperties```)
  * declaredMemberProperties
```kotlin
val <T : Any> KClass<T>.declaredMemberProperties: Collection<KProperty1<T, *>>
```
  * 구현
```kotlin
fun getAllMyMonstersHowling() : List<String> =
  MyPocketMonsters::class.decraredMemberProperties.map{it.getter.call().toString}
```