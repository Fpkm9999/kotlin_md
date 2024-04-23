# 코틀린의 제네릭, 널 안전성

## 1. 코틀린의 제네릭

코틀린의 제네릭스를 사용하면 클래스, 인터페이스 또는 메소드에서 타입을 매개변수로 사용함으로써 코드를 더욱 유연하게 만들 수 있습니다. 제네릭스를 사용하면 인터페이스를 변경하지 않고도 다양한 타입의 데이터를 저장할 수 있는 컬렉션을 생성할 수 있습니다.

결과적으로 `var list: Array<E>가 var list: Array<String>`으로 변경이 되는 것.


### 예시: MutableList 선언

```kotlin
public interface MutableList<E> : List<E>, MutableCollection<E>
```

여기서 `<E>`는 `String`과 같은 어떤 타입으로도 대체될 수 있으며, 클래스 내의 모든 `E` 인스턴스가 `String`으로 변환된다.

### 사용 예시:

```kotlin
fun testGenerics() {
    // String으로 제네릭을 사용했기 때문에 list 변수에는 문자열만 담을 수 있음.
    var list: MutableList<String> = mutableListOf()
    list.add("abc")
    list.add("def")
    list.add("ghi")
//    list.add(30) // 입력 오류가 발생

    // String 타입의 item 변수로 꺼내서 사용할 수 있음.
    for (item in list) {
        println("리스트에 저장된 값은 ${item}입니다")
    }
}
```

## 2. 코틀린의 널 안전성

코틀린의 널 안전성은 `NullPointerException`을 제거하기 위해 설계된 언어의 핵심 기능입니다. 코틀린은 널 값을 가질 수 있는 변수를 명시적으로 처리하도록 요구합니다.

코틀린에서는 변수가 `널(null)`을 가질 수 있는지 명시적으로 표시해야 합니다.

### 핵심 개념:

1. **널이 허용되는 타입 선언 (`?`)**: 타입 뒤에 `?`를 추가하여 널 저장을 허용합니다.
2. **안전 호출 연산자 (`?.`)**: 객체가 널이 아닐 때만 속성이나 메서드에 접근합니다.
3. **엘비스 연산자 (`?:`)**: 왼쪽 표현식이 널이 아니면 그 결과를 사용하고, 널이면 오른쪽 표현식의 결과를 사용합니다.
4. **강제 호출 연산자 (`!!`)**: 널이 아닌 값을 강제하며, 값이 널일 경우 예외`(NullPointerException)`를 발생시킵니다.

### 널 안전성 예시:

```kotlin
fun main() {
    val name: String? = "Hello, Kotlin!"  // 널이 허용되는 String 타입 변수

    // 안전 호출 연산자 사용
    val length: Int? = name?.length  // name이 널이 아니면 length를, 널이면 null을 반환

    // 엘비스 연산자 사용
    val safeLength = length ?: 0  // length가 널이면 0을 사용

    println("The length of the string is $safeLength.")

    // 강제 호출 연산자 사용
    val nonNullName: String = name!!  // name이 널이 아님을 보증하고 싶을 때 사용
    println("Name is $nonNullName.")
}
```
이 코드에서 `name` 변수는 널일 수 있는 `String?` 타입입니다. `?.` 연산자를 사용하여 `name`이 널이 아닐 때만 `.length` 속성에 접근합니다. `length` 또한 널이 될 수 있기 때문에, `?:` 연산자를 사용해 널일 경우 대체값으로 `0`을 제공합니다. `!!` 연산자는 변수가 널이 아님을 강제하므로 사용에 주의가 필요합니다.

코틀린은 null 값 처리에 많은 공을 들인 언어.

`null` 은 프로그래밍하면서 항상 이슈의 중심에 있는데 null 로 인해 프로그램 전체, 혹은 앱 전체가 멈출 수 있기 때문에 프로그램이 멈출 수 있는 상황을 코드로 만들어 봄.

1개의 메서드를 가진 클래스를 만듬.

### `널 오류 예시`

```kotlin
class One {
    fun printMe() {
        print("null safety")
    }
}

fun main() {
    var one: One
    if ( 1 > 2) {
        one = One()

    }
    one.printMe()
    // 에러뜸 : Kotlin: Variable 'one' must be initialized
}
```

참조변수가 객체 생성한 적도 없는데 아무런값이 없는데 호출하면 에러

이 코드에서 조건이 false 이기 때문에 one 변수는 아무것도 없는 null 상태가 됨.

print 메소드를 호출하면 null 포인터 예외가 발생하면서 프로그램이 다운.
물론 IDE 에서 오류를 발생시켜 컴파일 되지 않도록 막아줌.

하지만 코드양이 많아 지면 이런 상황이 언제든지 발생할 수 있는데, 코틀린은 이런 상황을 방지하기 위해서 안전장치를 마련해둠.
그 결과물이 `Null safety`

## 3. 코틀린에서 널 처리하기
 코틀린에서 지정하는 기본 변수는 모두 null이 입력되지 않음.

 null 값을 입력하기 위해서는 변수를 선언할때 타입 뒤에 `? (Nullable, 물음표)`를 입력.

`var variable : String?`

### 1) 변수에 null 허용하기
    
변수의 타입 뒤에 물음표를 붙이지 않으면 null 값을 입력할 수 없음.

null 예외를 발생시키고 싶지 않다면 기본형으로 선언.

```kotlin
fun main() {
    var nullable: String? // 타입 다음에 물음표를 붙여서 null 값을 입력할 수 있음.
    nullable = null

    println(nullable) // null
    
    var notNullable: String
//    notNullable = null // ERROR // 일반 변수에는 null 을 입력할 수 없음.
     
//    println(notNullable) // ERROR
}
```
---

### 2) 함수 파라미터에 null 허용 설정하기


함수의 파라미터에도 `null` 허용 여부를 설정할 수 있음.

함수의 파라미터가 null 을 허용하려면 해당 파라미터에 대해서 null 체크를 먼저 해야만 사용할 수 있음.

```kotlin
fun nullParameter(str : String?){
    var length = str.length // ERROR Null이 들어갈 가능성이 있는데 str.length 를 알 수 없음
}
```

해결
```kotlin
fun nullParameter(str: String?) {
    // 파라미터 str에 null 이 허용되었기 때문에 함수 내부에서 null 체크를 하기 전에는 str을 사용할 수 없음.
    if (str != null) {
        var length = str.length
    }
}
```

위의 코드처럼 str 파라미터를 조건문에서 null 인지 아닌지를 체크해야지만 사용할 수 있음.


### 3) 함수의 리턴 타입에 null 허용 설정하기
함수의 리턴타입에도 물음표를 붙여서 null 허용 여부를 설정할 수 있음.
    
```kotlin
fun nullReturn(): String? {
    return null
}
```
// 함수의 리턴 타입이 Nullable 이 지정되어 있지 않으면 null 값을 리턴할 수 없음.

```kotlin
fun nullParameter(str: String?) {
    // 파라미터 str에 null 이 허용되었기 때문에 함수 내부에서 null 체크를 하기 전에는 str을 사용할 수 없음.
    if (str != null) {
        var length = str.length
    }
}
```

위랑 아래랑 같다. 아래는 if 조건문이 없는 형태. 더 간결함 `!!` 을 사용
```kotlin
fun nullParameter(str: String?) {
    // 파라미터 str에 null 이 허용되었기 때문에 함수 내부에서 null 체크를 하기 전에는 str을 사용할 수 없음.
        var length = str!!.length

}
```

## 2. 안전한 호출 : ?.
변수를 `Nullable` 로 만들기 위해서 `물음표`를 사용.<br>
`?.` (Safe Call, 물음표와 점) 을 사용하면 null 체크를 간결하게 할 수 있음.

`Nullable` 인 변수 다음에 `?.` 을 사용하면 해당 변수가 null 일 경우 ?. 다음의 메서드나 프로퍼티를 호출하지 않음.

### 안전 호출:
예시

오류
```kotlin
fun testSafeCall(str: String?):Int? {
    var result: Int = str.length
    return result
}
```

- 해결
1단계
```kotlin
fun testSafeCall(str: String?):Int? {
    var result: Int = str?.length
    return result
}
```
2단계(완)
```kotlin
fun testSafeCall(str: String?):Int? {
    var result: Int? = str?.length
    return result
}
```
- 문자열 길이를 반환하는 length 프로퍼티를 호출헀는데 str 변수 자체가 null 일 경우에는 length 프로퍼티를 호출하지 않고
- null 을 반환.

null 말고 기본값을 설정하고 싶다면?

## 3. Null 값 대체하기 : `?:` 
`?:` (Elvis Operator, 물음표와 콜론)을 사용하면 원본 변수가 null 일 때 넘겨줄 기본값을 설정할 수 있음.

다음 코드에서 Safe Call 다음에 호출되는 프로퍼티 뒤에 다시 ?: 을 붙이고 0이라는 값을 표시.

이렇게 호출하면 str 변수가 null 일 경우 가장 뒤에 표시한 0을 반환.

```kotlin
fun testElvis(str: String?): Int {
    // length 오른쪽에 ?: 을 사용하면 null 일 경우 ?: 오른쪽의 값이 반환.
    var resultNonNull: Int = str?.length ?: 0
    return resultNonNull
}
```

---

물음표의 위치와 형태에 따라서 Nullable, Safe Call, Elvis Operator 가 구분.

#### Nullable
- 표기법 : 선언하는 변수의 타입 다음에 `?` 표기.
- 사용 목적 : null 을 입력 받기 위해 사용.
- 사용 예 : var nullable: String?

#### Safe Call
- 표기법 : 선언된 변수의 이름 다음에 `?.` 표기
- 사용목적 : null 일 때 ?. 다음에 나오는 속성이나 명령어를 처리하지 않기 위해 사용.
- 사용 예 : var result = 변수?.length

#### Elvis Operator
- 표기법 : 선언된 변수의 이름 다음에 `?:` 표기.
- 사용 목적 : null 일 때 ?: 다음에 나오는 값을 기본 값으로 사용.
- 사용 예 : var result = 변수 ?: 0

```kotlin
fun main() {
    println(testElvis(null)) // 0
}
```



## 요약

코틀린에서 `?`, `?.`, `?:` 연산자는 널을 처리하는 데 있어 핵심적인 역할을 하며, 코드를 `NullPointerException`으로부터 안전하게 보호합니다. 
