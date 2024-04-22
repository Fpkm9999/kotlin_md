### 문제
코틀린 언어를 사용하여 주어진 문자열 리스트에서 다음 기준에 따라 문자열을 분류하고 결과를 출력하는 프로그램을 작성하십시오.

### 입력
`listOf("hello", " ", "", "world", "!", " \n ")`

### 요구 사항
isEmpty() 함수를 사용하여 문자열이 완전히 비어 있는 경우 ("")를 찾아 해당하는 문자열들의 개수를 출력하십시오.

isBlank() 함수를 사용하여 문자열이 비어 있거나 공백, 탭, 줄바꿈으로만 구성된 경우를 찾아 해당하는 문자열들의 개수를 출력하십시오.

위 두 조건에 해당하지 않는 문자열들의 개수를 출력하십시오.

### 출력
`isEmpty()`를 만족하는 문자열의 개수
`isBlank()`를 만족하지만 isEmpty()는 만족하지 않는 문자열의 개수
위 두 조건을 모두 만족하지 않는 문자열의 개수

예시
입력 `리스트: listOf("hello", " ", "", "world", "!", " \n ")`

출력:
isEmpty: 1
isBlank but not isEmpty: 2
Neither: 3

```kotlin

fun main() {

    val strings = listOf("hello", " ", "", "world", "!", "  \n  ")
    var emptyCount = 0
    var blankCount = 0
    var neitherCount = 0

    println("strings 의 크기 ${strings.size}") // 요소가 6개이므로 emptyCount, blankCount, neitherCount 의 합은 6이 나와야 함
    for (string in strings) {
        if (string.isEmpty()) {
            emptyCount++
        } else if (string.isBlank()) {
            blankCount++
        } else {
            neitherCount++
        }
    }

    println("isEmpty: $emptyCount")
    println("isBlank but not isEmpty: $blankCount")
    println("Neither: $neitherCount")


    //    isEmpty: 1
    //    isBlank but not isEmpty: 2
    //    Neither: 3
    /*
```


### 설명
`isEmpty()`가 참인 경우는 문자열이 완전히 비어있는 경우이므로 `""`만 해당된다. 그 결과는 1개 이다.

`isBlank()`는 문자열이 비어 있거나 공백, 탭, 줄바꿈 등으로만 구성된 경우를 체크합니다. 
`" "`, `" \n "`가 해당되고, 이 중 isEmpty()는 아닌 경우가 2개이다.

Neither: isEmpty()도 isBlank()도 아닌 경우는 "hello", "world", "!"로 총 3개이다.


추가 분석
```kotlin
    // isBlank 만 단독으로 검사 할 경우
    var isBlankCount = 0
    for (string in strings) {
        if (string.isBlank()) {
            isBlankCount += 1
        }
    }
    println(isBlankCount)

    val stock = mutableListOf("INTEL", "APPLE", "MICROSOFT", "", "\n", " ","    ","\t")
    for (string in stock) {

        println(string.isBlank())
    }
    
    println("isEmpty Test")
    for (string in stock) {
        println(string.isEmpty())
    }
}
```
```shell
출력
3
false
false
false
true
true
true
true
true
isEmpty Test
false
false
false
true
false
false
false
false
```