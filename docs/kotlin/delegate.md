---
layout: default
title: 위임(Delegation)
parent: Kotlin
nav_order: 7
---

# Delegation
Delegation은 구현해야 할 추상 메소드 또는 프로퍼티의 정의를 다른 객체에 떠넘기는 방법이다.  

코틀린에서는 자바와 달리 기본적으로 모든 클래스가 final 이다.
이는 상속에서 발생하는 종속성, 의존성 문제를 방지하기 위해 클래스 상속을 불가능하게 되어있다. (원한다면 상속될 클래스에 open을 명시해주면 가능하다. ) 

Delegation은 주로 상속을 허용하지 않는 클래스에 새로운 동작을 추가해야 할 때 또는 메소드의 동작을 변경하고 싶을 때 사용한다.  

위 예제에서 HouseBlend를 Espresso를 상속받아 구현하고 싶다고 하자. Espresso는 open 키워드가 없기 때문에 final 선언이므로 상속이 불가 하다.
```kotlin
abstract class Coffee {
    abstract fun getDescription(): String
    abstract fun cost(): Double
}

class Espresso() : Coffee() {
    override fun getDescription(): String = "에스프레소"
    override fun cost(): Double = 1.99
}

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
# by
by 키워드는 구현되어야 할 추상 메소드들을 명시된 객체로 구현을 위임한다는 사실을 나타낸다. by 키워드를 사용하면 위의 코드도 간단하게 작업 가능하다.
```kotlin
class HouseBlend(val espresso: Coffee) : Coffee by espresso {
    val kind = "하우스 블렌드 커피"
}
```
#### space

## 왜 이렇게 사용할까?
상속은 클래스의 변수와 메소드를 모두 받기 때문에 재구현할 필요가 없어서 편리하다. 하지만 올바르지 않은 상속은 많은 문제를 일으키는데 그중 하나가 객체의 유연성을 떨어트리는것 이다. 보통 부모와 자식 클래스가 밀접한 관계며 비슷한 상속을 받게 될때 유연하게 만들기 위해 사용한다.
이 외에도 상속받을수 없는 클래스를 상속받고자 할 떄 사용한다.