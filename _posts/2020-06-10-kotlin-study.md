---
title: "Kotlin 기본 문법 공부"
categories: Kotlin
toc: true
toc_sticky: true
toc_label: "Kotlin 문법"
comments: true
---

# Kotlin 공부

## 변수 선언

val	: 변경되지 않는 변수

var	: 변경될 수 있는 변수 

```kotlin
val count: Int = 10
// count = 15 compile error
```

```kotlin
var count: Int = 10
count = 15
```



type을 명시하지 않아도 Kotlin 컴파일러가 languageName의 type을 결정해준다.

```kotlin
val languageName = "Kotiln"
```



## Null Safety

코틀린에서는 변수에 __null__ 값을 선언하기 위해선 ?을 붙여야 된다.

ex ) String -> String?, Int -> Int?

```kotlin
val languageName: String = null // comfile error

val languageName: String? = null // ok
```

__Null Safety__ 하기 때문에 _NullPointerException_이 발생할 가능성을 낮출 수 있다.



### Java와의 상호운용성

null safety 문법은 Kotlin에서 새롭게 나온 것이기 때문에 처리를 잘해줘야 한다.

```java
public class Account implements Parcelable {
    public final String name;
    public final String type;
    private final @Nullable String accessId;

    ...
}
```

Kotlin에서 위의 코드의 Account 클래스에서 name 멤버를 참조하면 Kotlin에서는 String이 String인지 String? 인지 알지 못한다(모호하다).  이러한 모호성은 __String!__ 을 통해 표시할 수 있다.



또 Java 코드를 작성할 때 nullability annotaiton을 사용하는게 좋다.

위의 코드에 accessId 멤버 변수는 @Nullable로 annotation되어 있어 null값을 보유할 수 있다고 명시한다. 

그렇기에 Kotlin에서는 accessId를 String? type 변수로 취급한다.

```java
public class Account implements Parcelable {
    public final @NonNull String name;
    ...
}
```

반면에 위의 name은 @NonNull annotation에 의해 Kotlin에서 String type 변수로 간주된다.



만약 자바 유형을 잘 모르는 경우(nullability annotation이 없는 경우)엔 null을 허용하는 것으로 간주해야 한다.

### Null을 허용하는 변수의 처리

- !! 연산자

```kotlin
val account = Account("name", "type")
val accountName = account.name!!.trim()
```

!! 연산자는 모든 결과가 null이 아닌 것으로 취급한다. 만약 account.name의 값이 null 이면 _NullPointerException_이 발생한다.



- ? 연산자

```kotlin
val account = Account("name", "type")
val accountName = account.name?.trim()
```

? 연산자는 name이 null이 아닌 경우 trim()을 실행한다. 만약 name이  null이면 name?.trim()의 값은  null이 된다. 따라서 _NullPointerExceptiion_을 발생시키지 않는다.

? 연산자는 _NullPointerException_을 방지하지만 accountName에는 null값을 전달한다. 이럴 때  __?:__ 연산자를 사용해 null 값을 처리할 수 있다.

```kotlin
val account = Account("name", "type")
val accountName = account.name?.trim() ?: "Default name"
```



- 조건문

```kotlin
val account = Account("name", "type")
val accountName = if(account.name != null){
    account.name.trim();
}
```



## Conditionals(조건문)

- if else
- when

```kotlin
// if else 사용
val answerString = if(count == 42) {
    "I have the answer."
} else if (count > 45) {
    "The answer is close."
} else{
    "The answer eludes me."
}

// when 사용
val answerString = when {
    count == 42 -> "I have the answer."
    count > 35 -> "The answer is close."
    else -> "The answer eludes me."
}

println(answerString)
```



## 함수

Kotlin 함수 기본 구성

``` kotlin
fun functionName(parmeters, ...): return type {
    return type
}
```



같은 함수 generateAnswerString() 의 다른 작성법

```kotlin
// #1
fun generateAnswerString(countThreshold: Int): String {
    val answerString = if (count > countThreshold) {
        "I have the answer."
    } else {
        "The answer eludes me."
    }
    
    return answerString
}

// #2
fun generateAnswerString(countThreshold: Int): String {
    return if (count > countThreshold) {
        "I have the answer."
    } else {
        "The answer eludes me."
    }
}

// #3
fun generateAnswerString(countThreshold: Int): String = if (count > countThreshold) {
    "I have the answer."
} else {
    "The answer eludes me."
}
```



### Anonymous function

```kotlin
val stringLengthFunc: (String) -> Int = { input -> input.length }

val stringLength: Int = stringLengthFunc("Android")
```



### Higher-order function

```kotlin
fun stringMapper(str: String, mapper: (String) -> Int): Int {
    return mapper(str)
}

stringMapper("Android", {input -> input.length})

stringMapper("Android") {input -> input.length}
```

## 참고

- [Kotlin 언어 알아보기](https://developer.android.com/kotlin/learn)

- [Android에서 일반적인 Kotlin 패턴 사용](https://developer.android.com/kotlin/common-patterns)

- [kotlinlang.org](https://kotlinlang.org/docs/reference)
