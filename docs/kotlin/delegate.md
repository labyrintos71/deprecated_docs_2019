---
layout: default
title: 위임(Delegation)
parent: Kotlin
nav_order: 7
---

# Delegation
Delegation은 구현해야 할 추상 메소드 또는 프로퍼티의 정의를 다른 객체에 떠넘기는 방법이다.  

코틀린에서는 자바와 달리 기본적으로 모든 클래스가 final 이다.
이는 상속에서 발생하는 종속성, 의존성 문제를 방지하기 위해 클래스 상속을 불가능하게 되어있다. 하지만 원한다면 상속될 클래스에 open을 명시해주면 가능하다.  

그래서 상속 기능이 필요할 때 데코레이터 패턴, 델리게이션 패턴을 이용하는데
코틀린에서 지원하는 Delegation Pattern을 알아보자.  

Delegation Pattern을 간단히 설명하면  
 한 객체가 모든 일을 수행 하는 것이 아니라 자신의 일중 일부를 다른 객체에 위임시켜 그 객체가 일을 처리하도록 하는것이다. 아래 예제를 보자.
```kotlin
interface printBase {
    val str:String
    fun printMessage()
    fun printMesageLine()
}
​// Delegate
class printer(val msg: String) : printBase {
    override val str: String
        get() = msg

    override fun printMessage() {
        print(msg)
    }

    override fun printMesageLine() {
        println(msg)
    }
}

​// Delegator
class reversePrint(val msg: String, impl: printBase) : printBase{
    ​// Delegation
    fun reversePrint(){
        print(str)
    }

    override val str: String
        get() = msg

    override fun printMessage() {
        print(msg)
    }

    override fun printMesageLine() {
        println(msg)
    }
}

fun main() {
    val printerclass = printer("test")
    val reverse = reversePrint("test",printerclass)
    reverse.reversePrint()
}
```

printBase를 구현한 printer 라는 객체를 상속 받고 싶을 떄, 상속이 안되므로 상위 인터페이스를 모두 재정의 해줘야 하는 번거로움이 있다. 이런 경우를 위해서 코틀린은 by 키워드를 지원한다.

## by
by 키워드는 구현되어야 할 추상 메소드들을 명시된 객체로 구현을 위임한다는 사실을 나타낸다. 아래 예제를 보면 이해가 빠를것이다.
```kotlin
interface printBase {
    val str:String
    fun printMessage()
    fun printMesageLine()
}

class printer(val msg: String) : printBase {
    override val str: String
        get() = msg

    override fun printMessage() {
        print(msg)
    }

    override fun printMesageLine() {
        println(msg)
    }
}

class reversePrint(impl: printBase) : printBase by impl{
    fun reversePrint(){
        print(str)
    }
    //멤버 변수를 오버라이딩 했을때 에는 무시당한다
    override val str: String
        get() = "abc"
    //위임으로 구현한 객체의 메소드 대신 ovverride 구현을 사용한다.    
    override fun printMessage() {
        print("abc")
    }
}

fun main() {
    val printerclass = printer("test")
    val reverse = reversePrint(printerclass)
    reverse.reversePrint()      
    reverse.print()            
    reverse.str                
}
```
반환값은 아래와 같다.
```
tset
abc
test
```

## 왜 이렇게 사용할까?
상속은 클래스의 변수와 메소드를 모두 받기 때문에 재구현할 필요가 없어서 편리하다. 하지만 올바르지 않은 상속은 많은 문제를 일으키는데 그중 하나가 객체의 유연성을 떨어트리는것 이다. 보통 부모와 자식 클래스가 밀접한 관계며 비슷한 상속을 받게 될때 유연하게 만들기 위해 사용한다.