##  1. 패키지
 패키지는 클래스와 소스 파일을 관리하기 위한 디렉토리 구조의 저장 공간.
 현재 클래스가 어떤 패키지(디렉토리)에 있는지 표시.
 디렉토리가 계층 구조로 만들어져 있으면 점(.)으로 구분해서 각 디렉토리를 모두 나열해 줌.
 
 package 메인 디렉토리.서브 디렉토리
 ```kotlin
 class 클래스 {
 }
 ```
 이 디렉토리 구조라면 윈도우의 파일 탐색기에서 보면 메인 디렉토리 아래에 서브 디렉토리가 있고, 서브 디렉토리 안에 실제 코드가 있는 클래스.kt 파일이 있음.
 <br>
 하나의 패키지에 여러 개의 파일을 생성할 수 있기 때문에 '서로 관계가 있는 파일을 동일한 패키지로' 만들어 두면 관리가 용이.

---

## 2. 추상화
프로그래밍을 하기 전 개념 설계를 하는 단계에서는 클래스의 이름과 클래스 안에 있음 직한 기능을 유추해서 메서드 이름으로 먼저 나열.<br>

이떄 명확한 코드는 설계 단계에서 메서드 블록 안에 직접 코드를 작성하는데, 그렇지 않는 경우에는 구현 단계에서 코드를 작성하도록
메서드의 이름만 작성.<br>

이 것을 추상화 Abstract 라고 하며 `abstract` 키워드를 사용해서 명시.

구현 단계에서 이 추상화된 클래스를 상속받아서 아직 구현되지 않은 부분을 마저 구현.

다음과 같이 추상화 된 Animal 클래스를 만들고 동물이 사용할 것 같은 기능중 walk()와 move()를 설게한다고 가정 +rest() 기능추가

#### 추상 클래스 생성
```kotlin
abstract class Animal {
    fun walk() {
        println("walking")
    }

    abstract fun move()

    abstract fun rest()
}
```
walk 는 명확하게 걸어가는 행위 이지만 move는 어떤 동물이냐에 따라서 달라진다고 가정.

어떻게 앞으로 상속받을 자식 클래스의 특징에 따라 코드가 결정될 가능성이 있다면 해당 기능도 abstract 키워드로 추상화.

그리고 ==실제 구현 클래스는 이 추상 클래스를 상속 받아서 아직 구현되지 않은 추상화되어 있는 기능을 모두 구현==해 줌.



> [!CAUTION]
> 추상 클래스는 독립적으로 인스턴스화 할 수 없기 떄문에 구현 단계가 고려되지 않는 다면 잘못된 설계가 될 수 있음.

#### 추상 클래스 구현

```kotlin
class Bird : Animal() {
    override fun move() {
        println("Bird move")
    }
    override fun rest() {
        println("Bird rest")
    }
}
```
---

## 3. 인터페이스
인터페이스 `interface` 는 실행 코드 없이 메서드 이름만 가진 추상 클래스라고 생각해도 됨.

> [!TIP]
> 누군가 설계해 놓은 개념 클래스 중에 실행 코드가 한 .줄이라도 있으면 추상 클래스, 
> 코드 없이 메서드 이름만 나열되어 있으면 인터페이스.

인터페이스는 안드로이드에서 주로 상속 관계의 설계보다는 외부 모듈에서 내가 만든 모듈을 사용할 수 있도록 메서드의 이름을 나열해둔 일종의 명세서로 제공.

인터페이스는 interface 예약어를 사용해서 정의할 수 있고 인터페이스에 정의된 메서드를 오버라이드해서 구현할 수 있음.

코틀린은 프로퍼티도 인터페이스 내부에 정의할 수 있고, 추상 클래스에 다르게 class 키워드는 사용되지 않음.

```kotlin
    interface 인터페이스명 {
        var 변수 : String
        fun 메서드1()
        fun 메서드2()
```

---

## 4. 인터페이스 만들기
`interface` 예약어로 인터페이스를 정의.

코틀린은 인터페이스 내부에 프로퍼티도 정의할 수 있음.

메서드는 코드 블록 없이 이름만 작성해둠. 인터페이스의 프로퍼티와 메서드는 abstract 키워드가 생략된 형태.

```kotlin
interface InterfaceKotlin {
    var variable: String
    fun get()
    fun set()
}
```

---

## 5. 클래스에서 구현하기
인터페이스를 클래스에서 구현할 때는 상속과 다르게 생성자를 호출하지 않고 인터페이스 이름만 지정해 주면 됨.

```kotlin
class KotlinImpl : InterfaceKotlin {
    override var variable: String = "Default"

    override fun get() {
    }

    override fun set() {
    }
}
```


인터페이스를 코틀린의 상속 형태가 아닌 소스 코드에서 직접 구현할 때도 있는데, object 키워드를 사용해서 구현해야 함.

`안드로이드 프로젝트에서 자주 사용하는 형태`

```kotlin
val kotlinImpl = object : InterfaceKotlin {
    override var variable: String = "Default"

    override fun get() {
    }

    override fun set() {
    }
}
```

---

### 인터페이스의 효율적인 사용
인터페이스는 외부의 다른 모듈을 위한 의사소통 방식을 지원하는 것.

혼자 개발 하거나 소수의 인원이 하나의 모듈 단위를 개발할 때는 인터페이스를 사용하지 않는 것이 좋음.

인터페이스를 남용하면 코드의 가독성과 구현 효율성이 떨어지기 떄문.


---
7일차 데이터클래스 접근제한자 참고

## 접근 제한자
코틀린에서 정의되는 클래스, 인터페이스, 메서드, 프로퍼티는 모두 접근 제한자 Visibility Modifiers를 가질 수 있음.

함수형 언어라는 특성 때문에 코틀린은 기본 객체지향에서 접근 제한자의 기준으로 삼았던 **패키지 대신에 모듈 개념이 도입**.

internal 접근 제한자로 모듈 간에 접근을 제한할 수 있음.

### 접근 제한자의 종류

접근 제한자는 서로 다른 파일에게 자신에 대한 접근 권한을 제공하는 것인데 각 변수나 클래스 이름 앞에 아무런 예약어를 붙이지
않았을 때는 **기본적으로 public** 접근 제한자가 적용

- `private` : 다른 파일에서 접근할 수 없음.
- `internal` : 같은 모듈에 있는 파일만 접근할 수 있음.
- `protected` : private와 같으나 상속 관계에서 자식 클래스가 접근할 수 있음.
- `public` : 제한 없이 모든 파일에서 접근할 수 있음.

---

### 코틀린에서 모듈이란?
코틀린에서 모듈이란 한 번에 같이 컴파일되는 모든 파일을 말함.
안드로이드를 예로 든다면 하나의 앱이 하나의 모듈이 될 수 있음. 또한 라이브러리도 하나의 모듈.

#### 접근 제한자의 적용
 접근 제한자를 붙이면 해당 클래스, 멤버 프로퍼티 또는 메서드에 대한 사용이 제한.

 접근 제한자가 어떻게 작용하는지 알아봄.
 다양한 접근 제한자를 가지는 부모 클래스를 하나 생성.

 ```kotlin
 open class Parent { // 부모 클래스
    private val privateVal = 1
    protected open val protectedVal = 2
    internal val internalVal = 3
    public val defaultVal = 4 // public은 적지 않아도 적용됨
}

class Child : Parent() { // 자식 클래스
    fun callVariables(){
//        println(privateVal) // 호출이 안됨
        println(protectedVal)   // 상속 관계이므로 접근할 수 있음.
        println(internalVal)    // 동일한 모듈이므로 접근할 수 있음.
        println(defaultVal)
    }
}
// 상속 관계가 아닌 외부 클래스에서 Parent 클래스를 생성하고 사용.
// 상속 관계가 아니기 떄문에 `public` 과 `internal` 에만 접근할 수 있음.
class Stranger {
    fun callVariables() {
        val parent = Parent()
//        println(parent.privateVal) // 호출 안됨
//        println(parent.protectedVal) // 호출 안됨
        println(parent.internalVal)
        println(parent.defaultVal)
    }
}
```