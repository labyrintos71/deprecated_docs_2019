---
layout: default
title: 위임 프로퍼티(Delegation-property)
parent: Kotlin
nav_order: 8
---

## Delegate
상속을 허용하지 않는 클래스에 새로운 동작을 추가해야 할 때 또는 메소드의 동작을 변경하고 싶을 때 데코레이터 패턴을 사용한다.  
위 예제에서 HouseBlend를 Espresso를 상속받아 구현하고 싶다고 하자. Espresso는 open 키워드가 없기 때문에 final 선언이므로 상속이 불가 하다.
```kotlin
class HouseBlend : Espresso() {      // 에러!
    val kind = "하우스 블렌드 커피"
}
```
따라서 상속하고싶은 클래스와 동일한 인터페이스를 구현하는 새로운 클래스를 만들고 상속하고 싶은 클래스는 내부에 멤버 변수로 가지는 방식으로 구현해야 한다.
```kotlin
class HouseBlend : Coffee {
    val kind = "하우스 블렌드 커피"
    override fun getDescription(): String = "에스프레소"
    override fun cost(): Double = 1.99
}
```
이 떄 by 키워드를 사용하면 간단하게 작업 가능하다.
```kotlin
class HouseBlend(val espresso: Coffee) : Coffee by espresso {
    val kind = "하우스 블렌드 커피"
}
```
#### Space

## Lazy
lazy()는 람다를 받아 지연 프로퍼티를 구현한 대리자인 Lazy<T>의 인스턴스를 반환한다.  
get()을 처음 호출하면 lazy()에 전달한 람다가 실행되고, 그 결과가 저장된다.  
이후에 get()을 호출하면 저장한 값을 반환한다.
```kotlin
val lazyValue: String by lazy {
    println("computed!")
    "Hello"
}

fun main(args: Array<String>) {
    println(lazyValue)
    println(lazyValue)
}
```
위 예제는
```
computed!
Hello
Hello
```
을 출력한다.