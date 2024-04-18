  ## 클래스 상속과 확장
코틀린은 클래스의 재사용을 위해서 상속을 지원

상속을 지원하는 예를 들면 `Activity` 라는 클래스가 미리 만들어지고, 이 Activity 클래스 내부에는
글자를 쓰는 기능, 그림을 그리는 기능, 화면에 새로운 창을 보여주는 기능이 미리 정의되어 있음.
상속이 있기에 이런 기능을 직접 구현하지 않고 Activity 클래스를 상속받아 약간의 코드만 추가하면 앱에 필요한 기능을 추가할 수 있음.

상속은 코드를 재활용하는 측면도 있지만, 코드를 체계적으로 관리할 수 있기 때문에 규모가 큰 프로젝트로 효과적으로 설계할 수 있음.

### 1. 클래스 상속

코틀린에서는 클래스가 다른 클래스를 상속받기 위해서는 `open` 키워드로 선언되어야 합니다. 이는 기본적으로 코틀린의 모든 클래스가 `final`이기 때문입니다. 상속받은 클래스는 부모 클래스의 생성자를 호출해야 합니다.
호출은 괄호를 입력해서 꼭 부모의 생성자를 호출해야 함.

```kotlin
open class ParentClass { // 부모 클래스는 open 키워드로 선언
    // 부모 클래스의 코드
}

class ChildClass : ParentClass() { // 부모 클래스의 생성자 호출
    // 자식 클래스의 코드
}
```

#### 생성자가 있는 클래스의 상속
부모 클래스에 파라미터가 있는 생성자가 있을 경우, 자식 클래스에서는 해당 파라미터를 전달해야 함

```kotlin
    open class 상속될 부모 클래스(value : String){
        // 코드
    }
    class 자식 클래스(value : String) : 부모 클래스(value){
        // 코드
    }
    부모 클래스에 세컨더리 생성자가 있다면, 역시 자식 클래스의 세컨더리 생성자에서 super 키워드로 부모 클래스에 전달할 수 있음.
    부모 클래스의 세컨더리 생성자를 이용하는 경우에는 부모 클래스명 다음에 오는 괄호를 생략.
```
```kotlin
open class ParentClass(value: String) {
    // 코드
}

class ChildClass(value: String) : ParentClass(value) {
    // 코드
}
```

```kotlin
fun main (){
      open class Parent {
        var hello: String = "안녕하세요"
        fun sayHello(){
            println(hello)
        }
    }
    class Child: Parent() {
        fun myHello() {
            hello = "Hello"
            sayHello()
        }
    }
    val child = Child()
    child.myHello()
}
```

### 2. 메서드와 프로퍼티 오버라이드 ()=재정의)
자식 클래스에서 부모 클래스의 메서드나 프로퍼티를 다르게 사용하고 싶을 때는 `override` 키워드를 사용하여 재정의할 수 있다.<br> 

상속받은 부모 클래스의 프로퍼티와 메서드 중에 자식 클래스에서는 다른 용도로 사용해야 하는 경우가 있음.
동일한 이름의 메서드나 프로퍼티를 사용할 필요가 있을 경우에 `override` 키워드를 사용해서 재정의 할 수 있음.
오버라이드 할 때는 프로퍼티나 메서드도 클래스 처럼 앞에 `open` 을 붙여서 상속할 준비가 되어 있어야 함.

#### 메서드 오버라이드
상속할 메서드 앞에 open 키워드를 붙이면 오버라이드 할 수 있지만, open 키워드가 없는 메서드는 오버라이드 할 수 없음.

```kotlin
open class BaseClass {
    open fun opened() {
        // 기본 구현
    }

    fun notOpened() {
        // 오버라이드할 수 없는 메서드
    }
}

class ChildClass : BaseClass() {
    override fun opened() {
        super.opened() // 부모 클래스의 메서드 호출
        // 추가 구현
    }
}
```

#### 프로퍼티 오버라이드
메서드 오버라이드처럼 프로퍼티 역시 open 으로 열려 있어야만 오버라이드를 할 수 있음.

상속받은 클래스에서 프로퍼티 역시 동일한 이름을 사용할려면 오버라이드 해야 한다.


```kotlin    
open class BaseClass2 {
        open var opened: String = "I am"
    }
    class ChildClass2 : BaseClass2() {
         override var opened: String = "You are"
    }
```

### 3. 익스텐션
코틀린은 기존 클래스에 새로운 메서드나 프로퍼티를 추가할 수 있는 익스텐션 기능을 제공

이미 만들어져 있는 클래스에 다음과 같은 형태로 추가할 수 있음.
```kotlin
fun 클래스.확장할 메서드() {
// 코드
}
```


// 자바의 경우 새로운 클래스를 만들고 원본 클래스를 상속받아 메서드 2개를 추가해야 했는데 코틀린은 쉽다.

```kotlin
  fun String.addText(word: String): String {
        return this + word
    }
    fun testStringExtension() {
        val original = "Hello"
        val added = "Guys~"

        print("added를 더한 값은 ${original.addText(added)}입니다.")

    }
        testStringExtension(); // added를 더한 값은 HelloGuys~입니다.
```