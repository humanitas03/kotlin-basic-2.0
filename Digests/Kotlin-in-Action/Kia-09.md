## 9. Generics

#### 제너릭(Generics)이란...
* 꼭 JVM언어 계열에만 국한되는 개념이라기 보다는...(C++에서 ```template class``` 라는 개념이 있고, Golang(1.18)에서도 Generics 라고 하는 개념이 있음.)
* 좀 더 일반적인 개념으로 **런타임시 동적으로 다양한 타입들을 처리하여 코드의 재사용성을 높일 수 있는 프로그래밍 기법** 이라 정의할 수 있을듯 합니다. 
  * [관련 위키 참고](https://ko.wikipedia.org/wiki/%EC%A0%9C%EB%84%A4%EB%A6%AD_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

#### 실체화한 타입 파라미터(Reified Type Parameter)
* 실체화된(Reified) + 타입 파라미터(Type Parameter)
* 타입 패러미터란 어떤 변수의 타입을 정하는 것도 패러미터(Parameter)로 넘긴다 라는 컨셉
  * Type-Safety 관점 -> Java와 Kotlin은 정적 타입 언어(Static Typed Language)이므로, 컴파일 시점에 동적으로 올 수있는 타입 패러미터의 안정성을 체크하는 것을 지향
* 상충되는 개념으로 **소거 된 타입 패러미터** 가 있음.
  * 자바 제너릭의 특성
* 코틀린의 경우 **런타임 까지 실체화된 타입 패러미터 표현을 남길 수 있는 키워드와 기능**을 언어 차원에서 제공함. 

#### 변성(Varience)
* 성질이 변한다
* **리스코프 치환 원칙(Liskov Substitution Principle)**


### 9.1 제너릭 타입 파라미터
* 코틀린 컴파일러는 제너릭 타입에 대해 어디까지 추론할 수 있는가?

```kotlin
val authors = listOf("Dmitry", "Svetlana") // authors라는 변수의 타입이 List<String> 이라고 인지할 수 있음.
```

* 제너릭 함수에 대한 시그니처

```kotlin
fun <T> List<T>.slice(indices: IntRange) : List<T>
```
* 제너릭 클래스/인터페이스에 대한 시그니처
```kotlin
interface List<T> {
    operator fun get(index: Int) : T
}

class ArrayList<T> : List<T> {
    override fun get(index: Int) : T = ...
}
```

* 타입 패러미터 제약
  * 타입 인자를 특정 타입으로 제한
```kotlin
fun <T: Number> List<T>.sum() : T
```

### 9.2 실행 시 제너릭스의 동작 : 소거된 타입 파라미터와 실체화된 타입 파라미터
* 자바와 마찬가지로 코틀린 제너릭 타입 인자 정보는 런타임 때 지워진다.
  * 이 개념을 ```타입 소거(Type Eraiser)```
* 예를 들어.. ```List<Int>```와 ```List<String>``` 두 가지 List 컬렉션 타입이 있는경우
  * 실행 시점에 두 리스트는 정수형 리스트 인지, 문자열 리스트 인지 알 수 는 없음.(Type 소거)
  * 다만 컴파일러가 타입 인자를 알고 올바른 값의 타입만 각 리스트에 넣을 수 잇도록 보장하고 있음.
* 코틀린에서는 ```reified``` 키워드를 통해서 실행 시점에 지정한 타입의 인스턴스 인지 검사할 수 있음.
  * ```reified``` 키워드를 사용하면 타입 패러미터가 실행 시점까지 유지가 됨. 
  * 다만, ```inline``` 함수 에서만 제한된 키워드

```kotlin
inline fun <reified T>
    Iterable<*>.filterIsInstance() : List<T> {
    val destination = mutableListOf<T>()
    for (element in this) {
        if (element is T) {
          destination.add(element)   
        }
    }
}
```

> ### 인라인 함수에서만 실체화한 타입 인자를 쓸 수 있는 이유
> 컴파일러는 인라인 함수의 본문을 구현한 바이트코드를 그 함수가 호출되는 모든 지점에 삽입한다.
> 컴파일러는 실체화한 타입 인자를 사용해 인라인 함수를 호출하는 각 부분의 정확한 타입 인자를 알 수 있다.
> 따라서 (코틀린) 컴파일러는 **구체적인 클래스**를 참조하는 형태의 바이트코드를 생성하게 되는 것임.

#### 클래스 참조 어댑터
* 리플렉션과 관련된 개념
* 코틀린에서는 ```::class.java``` 구문을 통해 ```java.lang.Class``` 참조를 얻을 수 있음
* 가끔씩 어떤 함수에서는 패러미터 타입을 ```java.lang.Class``` 타입을 요구하는 경우가 있는데, 코틀린이라면 ```T::class.java``` 키워드를 사용하면 됨.

#### 실체화환 타입 패러미터 제약
* 다음과 같은 경우 실체화한 타입 파라미터 사용이 가능
  * 타입 검사와 캐스팅(is, !is, as, as?)
  * 리플렉션
  * ```java.lang.Class```
  * 다른 함수 호출 시 타입 인자 사용
* 아래와 같은 작업은 할 수 없음
  * 타입 파라미터 클래스의 인스턴스 생성
  * 타입 패러미터의 동반 객체 메소드 호출하기
  * 클래스, 프로퍼티, 인라인 함수가 아닌 함수의 타입 파라미터를 ```reified```로 지정하기

### 9.3 변성
> ### 변성
> 기저 타입(Base Type)이 같고 타입 인자(Type Argument)가 다른 두 타입의 관계가 어떤 관계가 있는지 설명하는 개념이다.
> 그 관계에 따라 세부적으로 공변성(Covariance), 반공변성(ContraVariance), 무공변성(invariance) 등으로 나누어짐.
> 자바, 코틀린은 기본적으로 객체지향 5원칙(SOLID)의 **리스코프 치환 원칙**의 제약을 받음
> > 예) List<String>, List<Int> 가 있다고 하면...
>>* 기저타입 : List
>>* 타입인자 : String, Int


#### 공변성 : 하위 타입 관계를 유지
* 어떤 타입 A, B가 있을때, B가 A의 하위타입인 경우 Generic<A>와 Generic<B>도 같은 관계를 가지도록 하는 개념
```kotlin
interface Producer<out T> {
    fun produce() : T
}
```

* 타입의 안정성을 보장하기 위해 공변적 패러미터는 아웃(out) 위치에 있어야 한다.
  * 클래스가 T 타입의 값을 생산할 수는 있지만 소비할 수 없다는 의미.(```Supplier```)

#### 반공변성 : 뒤집힌 하위 타입 관계
* 어떤 타입 A, B가 있을때, B가 A의 하위타입인 경우 Generic<A>와 Generic<B>에서는 그 관계가 뒤집힌 개념

```kotlin
interface Comparator<in T> {
    fun compare(e1: T, e2: T) : Int {
        // ...
    }
}
```

* ```in```이라는 키워드가 붙은 타입은 클래스 메소드 안으로 직접 전달되어 메소드에 의해 소비된다고 봐야함.

#### 선언 지점 변성과 사용자 지점 변성
* 코틀린은 클래스를 선언하면서 변성을 지정하면, 그 클래스를 사용하는 모든 장소에 변성 지정자가 영향을 끼친다.
* 자바의 경우 타입 패러미터가 있는 타입을 사용할 때마다 해당 타입 패러미터를 상위 타입이나 하위 타입중 어떤 타입으로 대치해야 되는지 명시해야 한다.
  * 이를 **와일드카드** 라고도 합니다.
```java
public interface Stream {
    <R> Stream <R> map(Function<? super T, ? extends R> mapper);
}
```

* 코틀린의 사용 지점 변성 선언은 자바의 한정 와일드 카드와 같습니다.
  * 코틀린의 MutableList<out T>는 자바 MutableList<? extends T>와 같고,
  * 코틀린 MutableList<in T>는 자바 MutableList<? super T>와 같다.

#### 스타 프로젝션 : 타입 인자 대신 * 사용
* 제너릭 타입 인자 정보가 없음을 표현하기 위해 **스타 프로젝션** 사용
  * 제약 조건 : 제너릭 타입을 알 필요가 없을 때, 값을 만들어 내는 메소드만 호출 가능.
  * ```MutableList<*>```와 ```MutableList<Any?>```는 다르다.
```kotlin
fun printFirst(list: List<*>) {
    if(list.isNotEmpty()) {
        println(list.first())
    }
}
```