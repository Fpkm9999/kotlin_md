## 1. 지연 초기화 (Lazy Initialization)

코틀린에서 지연 초기화를 사용하면 클래스 코드에서 Nullable 처리가 과도하게 사용되는 것을 방지할 수 있습니다. 이는 `?.` (Safe Call)의 남용을 줄여 코드의 가독성을 높이는 데 도움이 됩니다.

#### 예제 코드: 기본 초기화 사용
```kotlin
class Person {
    var name: String? = null

    init {
        name = "Jane"
    }

    fun process() {
        name?.plus("Messi")
        println("이름의 길이= ${name?.length}")
        println("이름의 첫글자 = ${name?.substring(0, 1)}")
    }
}
```
위 예제에서 변수 `name`은 Nullable 타입으로 선언되어 있어 메서드나 프로퍼티 사용 시 `?.` (Safe Call)을 반복적으로 사용해야 합니다. 이는 코드의 가독성을 저하시키는 요인이 됩니다.

#### `lateinit` 사용: 늦은 초기화

`lateinit` 키워드를 사용하면 변수를 선언할 때 바로 객체를 할당하지 않아도 됩니다. 이를 통해 Safe Call의 사용을 줄이고 코드의 가독성을 개선할 수 있습니다.

##### 특징
- `lateinit`은 변수를 미리 선언하고 나중에 값을 할당합니다.
- 오직 `var` 변수에만 사용 가능합니다.
- 기본 자료형(Primitive Type)에는 사용할 수 없습니다 (Int, Boolean, Double, Char 등).
- getter와 setter 사용이 제한됩니다.
- `null` 값은 허용되지 않습니다.

##### 예제 코드: `lateinit` 사용
```kotlin
class Person2 {
    lateinit var name: String

    init {
        name = "Jane"
    }

    fun process() {
        println("이름의 길이= ${name.length}")
        println("이름의 첫글자 = ${name.substring(0, 1)}")
    }
}
```
`lateinit` 사용 시 주의할 점:
- `lateinit`은 변수를 미리 선언한 후 나중에 초기화하는 방식이므로, 초기화되지 않은 상태에서 해당 변수의 메서드나 프로퍼티를 참조하면 `NullPointerException`이 발생할 수 있습니다.
- 초기화되지 않은 상태에서 변수를 사용할 가능성이 있다면 Nullable 처리나 초기값 할당을 고려하는 것이 좋습니다.
---

## 2. `lazy` 지연 초기화

`lazy`는 코틀린에서 읽기 전용 변수인 `val`을 사용하여 지연 초기화를 구현할 때 사용됩니다. `lateinit`과 달리 `lazy`를 사용하면 한 번 초기화된 이후 그 값이 변경되지 않습니다.

#### 사용 방법
변수를 선언할 때 `val`로 선언하고 `by lazy` 키워드를 사용하여 초기화 값을 지정합니다. 초기화는 해당 변수가 처음 사용될 때 실행됩니다.

#### 예제 코드
```kotlin
class Company {
    // by lazy 를 사용하면 반환되는 값의 타입을 추론할 수 있기 때문에 변수의 타입을 생략할 수 있음.
    val person: Person by lazy { Person()}
    init {
        // lazy 는 선언 시에 초기화 하기 떄문에 초기화 과정이 필요 없음.
    }
    fun process() {
        println("person의 이름은 ${person.name}") // 최초 호출되는 시점에 초기화
    }
}
```
lazy로 선언된 변수가 최초 호출되는 시점에 by lazy{} 안에 넣은 값으로 초기화.

코드에서 Company 클래스가 초기화 되더라도 person에 바로 Person() 으로 초기화 되지 않고,

process() 메서드에서 person.name 이 호출되는 순간 초기화.

### `lazy`의 특징
1. **초기화 위치**: `lazy`로 선언된 변수는 최초로 호출될 때 초기화 코드가 실행됩니다. 따라서 초기화 시점을 조절할 수 있습니다.
2. **타입 추론**: `by lazy`를 사용하면 반환되는 값의 타입을 컴파일러가 추론할 수 있어, 변수 선언 시 타입을 명시하지 않아도 됩니다.



#### 주의 사항
- `lazy` 초기화는 처음 호출되는 시점에 초기화 작업이 이루어지기 때문에, 초기화에 많은 시간이나 자원이 필요한 경우 전체 처리 속도에 영향을 줄 수 있습니다. 예를 들어, `Person` 클래스의 생성자가 복잡하고 시간이 많이 소요되는 로직을 포함하고 있다면, 간단한 `person.name` 호출에도 시간이 많이 걸릴 수 있습니다.
- 복잡한 초기화 로직을 포함한 클래스의 경우, 지연 초기화보다는 미리 초기화하는 방법을 고려하는 것이 성능에 더 유리할 수 있습니다.

---

## 3. 람다식 (Lambda Expressions)

코틀린에서는 람다식을 익명 함수의 형태로 사용할 수 있습니다. 람다식을 간단히 정의하면, 이름 없이 정의되고 마치 값처럼 다룰 수 있는 익명 함수입니다.

#### 람다식의 기능
람다식은 함수의 인수로 전달되거나 함수에서 반환값으로 사용될 수 있습니다. 이를 통해 고차 함수에서의 유연성을 제공하고, 코드를 더 간결하게 만들 수 있습니다.

#### 1. 람다식 정의 및 사용 예

##### 기본 형태
```kotlin
fun main() {
    val sayHello = fun() { println("Hello world!") }
    sayHello()
    
    val squareNum: (Int) -> Int = { number -> number * number }
    println(squareNum(12)) // 출력: 144
}
```
- `sayHello`: 이름 없는 함수를 변수에 할당
- `squareNum`: 정수를 입력 받아 그 제곱을 반환하는 람다식
- `(Int)` : 람다식 인수의 자료형을 지정
- `Int` : 람다식의 반환 자료형을 지정. 이 경우에는 정수를 넣고 정수를 반환한다.
- `number` : 인수 목록을 나열. number의 자료형은 자료형에서 명시해주었으므로 형 추론이 되어 number는 Int 가 됨.
- `number * number` : 람다식에서 실행할 코드를 지정

##### 타입 추론을 활용한 형태
```kotlin
val squareNum2 = { number: Int -> number * number }
println(squareNum2(12)) // 출력: 144
```
- 자료형 추론을 사용하여 람다식을 더 간결하게 표현할 수 있습니다.

##### 단일 인수 람다식
```kotlin
val squareNum3: (Int) -> Int = { it * it }
println(squareNum3(12)) // 출력: 144
```
- 단일 인수인 경우 `it` 키워드를 사용하여 인수를 생략하고 직접 사용할 수 있다.

### 요약
코틀린의 람다식은 코드의 간결성을 향상시키고, 함수를 값처럼 취급하여 고차 함수의 파라미터나 반환 값으로 사용할 수 있다.


---

#### 코틀린에서 람다식의 다양한 사용법과 SAM 변환

코틀린에서 람다식은 "값처럼" 사용할 수 있는 익명 함수로, 함수의 인수로 전달되거나 반환값으로 사용될 수 있습니다. 이는 함수형 프로그래밍의 유연성을 증가시키며 코드의 간결성을 높여줍니다.

#### 2. 람다식 표현의 다양한 방법

```kotlin
// 람다식을 인수로 받는 함수
fun invokeLambda(lambda: (Int) -> Boolean): Boolean {
    return lambda(5)
}

// 람다식을 변수로 선언하고 함수에 전달
val paramLambda: (Int) -> Boolean = { num -> num == 10 }
println(invokeLambda(paramLambda)) // 출력: false

// 변수를 사용하지 않고도 바로 넣어줄 수도 있음.
// 다음의 람다식들은 모두 똑같이 작동
invokeLambda({ num -> num == 10 }) // 람다식 바로 넣어주기
invokeLambda({ it == 10 }) // 인수가 하나일 때 it으로 변경 가능
invokeLambda() { it == 10 }  // 만약 함수의 마지막 인수가 람다일 경우 밖으로 뺄 수 있음.
invokeLambda { it == 10 } // 그 외 인수가 없을 때 () 생략 가능
```

#### 3. SAM(Single Abstract Method) 변환

SAM 변환은 특정 조건을 만족하는 인터페이스에 대해 람다식을 사용하여 익명 객체를 생성할 수 있게 해줍니다. 주로 자바 인터페이스에서 사용되며, 코틀린에서도 자바와의 호환성을 위해 지원됩니다.

##### 조건:
1. 자바 인터페이스여야 합니다.
2. 인터페이스 내에 하나의 추상 메서드만 존재해야 합니다.

이를 통해 안드로이드 개발에서 자주 보는 `setOnClickListener` 같은 함수에서 람다식을 사용할 수 있습니다.

```kotlin
button.setOnClickListener {
    // 버튼이 클릭됐을 때 수행할 동작
}
// 함수의 인수가 하나이고 람다식인 경우에 () 을 생략하고 {} 에 코드를 작성할 수 있음.

```

##### OnClickListener 인터페이스 예시
```java
public interface OnClickListener {
    void onClick(View v);
}
```

##### setOnClickListener 메서드 구현
```java
public void setOnClickListener(@Nullable OnClickListener l) {
    if (!isClickable()) {
        setClickable(true);
    }
    getListenerInfo().mOnClickListener = l;
}
```

setOnClickListener 는 이와 같이 람다식이 아님에도 마치 람다식처럼 취급되고 있음.
이것이 가능한 이유가 바로 자바 8에서 소개된 SAM 변환.