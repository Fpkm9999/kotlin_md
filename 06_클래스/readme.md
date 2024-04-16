# 코틀린에서 사용되는 클래스의 기본 구조

클래스는 변수와 함수의 모음입니다. 이를 통해 관련된 데이터와 함수를 그룹화하고 이름을 붙여 쉽게 관리할 수 있습니다.

```kotlin
fun main() {
    class 클래스이름 {
        var 변수: 타입
        fun 함수() {
            // 코드
        }
    }
}
```

## 클래스 코드 작성하기

클래스를 정의하기 위해서는 `class` 키워드를 사용하고, 클래스 이름 다음에 중괄호 `{}`를 사용해 클래스 스코프를 정의합니다.

```kotlin
class 클래스이름 {
    // 클래스 스코프 (Class Scope)
    // '몇몇 예외'는 있지만 대부분의 코드는 클래스 스코프 안에 작성.
}
```

클래스를 사용하기 위해서는 생성자가 필요합니다. 코틀린은 프라이머리(primary)와 세컨더리(secondary) 두 종류의 생성자를 제공합니다.

### 프라이머리 생성자

프라이머리 생성자는 클래스의 헤더처럼 사용할 수 있으며, 필요에 따라 `constructor` 키워드를 생략할 수 있습니다.

```kotlin
   class Person 프라이머리 생성자(){

    }
```

```kotlin
class Person constructor(value: String){
    // 코드
}
```

```kotlin
class Person(val name: String) {
    init {
        println("생성자로부터 전달받은 값은 $name입니다.")
    }
}

val person = Person("John")
```

```kotlin
    class Person1 constructor (str: String){
        init {
            println("생성자로부터 전달받은 값은 ${str}입니다")
        }
    }
    
    var person1 = Person1("1") // 생성자로부터 전달받은 값은 1입니다
```

클래스의 생성자가 호출되면 init 블록의 코드가 실행되고,<br>
init 블록에서는 생성자를 통해 넘어온 파라미터에 접근할 수 있음.

하지만 init 초기화 작업이 필요하지 않다면 init 블록을 작성하지 않아도 됨.

대신 파라미터로 전달된 값을 사용하기 위해서는 파라미터 앞에 변수 키워드인 val을 붙여주면 클래스 스코프 전체에서 해당 파라미터를 사용할 수 있음.

```kotlin
class Person2 (val str: String){
    fun process(){
        println("생성자로 생성된 값은 ${str}입니다")
    }
}
val person2 = Person2("2")
person2.process() // 생성자로 생성된 값은 2입니다
```

자바로 작성한다면 생성자를 통해서 멤버변수를 만들고 거기에 값을 넣어야하는데 코틀린은 간편하게 작성할 수 있다.

`(val str: String)` 파라미터 이 자리가 여러가지 역할을 포함(대체)하고있다

1. 생성자를 통한 값 초기화 2. 멤버변수 선언 3. 멤버변수 초기화<br>
생성자 파라미터 앞에 var 도 사용할 수는 있으나, 읽기 전용인 val 을 사용하는 것을 권장.

### 세컨더리 생성자

세컨더리 생성자는 클래스 내부에 `constructor` 키워드를 사용하여 정의하며, 다양한 초기화 요구사항에 대응할 수 있습니다.
    `constructor` 키워드를 함수처럼 클래스 스코프 안에서 직접 작성가능함

```kotlin
    class Person3 {
        constructor(str: String) {
            println("생정자로부터 전달받은 값은 ${str}입니다.")
        }
    }
```

```kotlin
class Sample {
    constructor(str: String) {
        println("생성자로부터 전달받은 값은 $str입니다.")
    }

    constructor(value: Int) {
        println("생성자로부터 전달받은 값은 $value입니다.")
    }
}

val sample1 = Sample("Hello")
val sample2 = Sample(123)
```
세컨더리 생성자는 파라미터의 개수, 또는 파라미터의 타입이 다르면 (오버로딩) 여러 개를 중복해서 만들 수 있음.

```kotlin
    class Sample {
        constructor(str: String) {
            println("생성자로부터 전달받은 값은 ${str}입니다.")
        }

        constructor(value: Int) {
            println("생성자로부터 전달받은 값은 ${value}입니다.")
        }

        constructor(value1: Int, value2: String) {
            println("생성자로부터 전달 받은 값은 ${value1}, ${value2}입니다.")
        }

    }
    val sample = Sample(1,"2") // 생성자로부터 전달 받은 값은 1, 2입니다.
 
    val sample2 = Sample("2") // 생성자로부터 전달받은 값은 2입니다.

```
자바와 다른점

```kotlin
   class Sample {
        constructor(str: String) {
            println("생성자로부터 전달받은 값은 ${str}입니다.")
        }

        constructor(value: String) {
            println("생성자로부터 전달받은 값은 ${value}입니다.")
        }

        constructor(value1: Int, value2: String) {
            println("생성자로부터 전달 받은 값은 ${value1}, ${value2}입니다.")
        }

    }
    val sample = Sample(1,"2") // 생성자로부터 전달 받은 값은 1, 2입니다.

    val sample2 = Sample("2") // 생성자로부터 전달받은 값은 2입니다.
```
코드 작성시점에선 오류가 없으나
호출할때 에러가 뜬다

### Default 생성자

클래스에 생성자를 명시적으로 작성하지 않으면, 파라미터가 없는 기본 생성자가 자동으로 제공됩니다.

```kotlin
    class Student { // 생성자를 작성하지 않아도 기본 생성자가 동작.
        init {
            // 기본 생성자가 없더라도 초기화가 필요하면 여기에 코드를 작성.
        }
    }
```

```kotlin
class Student {
    init {
        println("Student 클래스가 초기화됩니다.")
    }
}
```

## 클래스의 사용

클래스 인스턴스는 생성자 호출을 통해 생성됩니다. 코틀린에서는 `new` 키워드 없이 클래스 이름과 괄호를 사용하여 생성자를 호출할 수 있습니다.

```kotlin
val car = Car("blue")
println(car.color) // blue
```
아무런 파라미터 없이 클래스명에 괄호를 붙여주면 생성자가 호출되면서 init 블록 안의 코드가 자동으로 실행.
세컨더리 생성자의 경우 init 블록이 먼저 실행되고 constructor 블록 안의 코드가 실행.

코틀린에서는 자바보다 클래스를 좀 더 편하게 정의할 수 있음.

자바의 보일러 플레이트 코드가 삭제되고 필요한 코드들만 작성하면 되도록 바뀜.
* 보일러 플레이트 코드 : 상용구 코드. 변형이 거의 없는 혹은 전혀 없이 여러 위치에서 반복되는 코드 문구.

kotlin 으로 작성
```kotlin
    class Car(val color: String)
```

객체를 생성. 자바에서는 new 키워드를 사용했지만, 코틀린에서는 필요가 없음
```kotlin
   // 객체 생성
   var car = Car("blue")
   // 객체 호출
    println(car.color) // blue
```

프라이머리 생성자가 나온 이유가 이거임.
-> `class Car(val color: String)`


## 오브젝트

`object` 키워드를 사용하면 인스턴스를 생성하지 않고 클래스의 프로퍼티와 메서드에 접근할 수 있습니다. 이는 자바의 `static`과 유사합니다.

```kotlin
object Cat {
    var name: String = "pinky"

    fun printName() {
        println("Cat의 이름은 $name입니다.")
    }
}

fun main() {
    Cat.name = "minky"
    Cat.printName() // Cat의 이름은 minky입니다.
}
```

object 코드 블록 안의 프로퍼티와 메서드는 클래스명에 점 연산자를 붙여서 생성자 없이 직접 호출할 수 있음.
주의할 점은 클래스명을 그대로 사용하기 떄문에 호출하는 클래스명의 첫 글자가 대문자.