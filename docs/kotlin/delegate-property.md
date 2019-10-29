---
layout: default
title: 위임 프로퍼티(Delegation-property)
parent: Kotlin
nav_order: 8
---
## 위임 프로퍼티(Delegation-property)
위임 프로퍼티란 프로퍼티의 접근자 게터와 세터를 다른객체로 위임하는 방식을 말한다.
프로퍼티의 값을 변경하거나 읽어올 때 작업이 필요할 떄 사용하는데 아래처럼 by 키워드를 사용해 이용가능하다.
```kotlin
class Delegator(var value: Int) {
    operator fun getValue(thisRef: Any?, property: KProperty<*>): Int {
            println("${property.name} get! $value")
            return value
    }
    operator fun setValue(thisRef: Any?, property: KProperty<*>, newValue: Int) {
        println("${property.name} set! $newValue")
        value = newValue
    }
}

class Person(val name: String, age: Int, salary: Int) {    
    var age: Int by Delegator(age)
    var salary: Int by Delegator(salary)
}

fun main(args: Array<String>?) {
    val p = Person("Monguse", 20, 2000)
    p.age = 21
    p.salary = 2100
    println("${p.name} - age: ${p.age}, salary: ${p.salary}")
}
```
```kotlin
age set! 21
salary set! 2100
age get! 21
salary get! 2100
Monguse - age: 21, salary: 2100
```
#### Space

## Lazy
lazy()는 람다를 받아 지연 프로퍼티를 구현한 대리자인 Lazy<T>의 인스턴스를 반환한다.  
get()을 처음 호출하면 lazy()에 전달한 람다가 실행되고, 그 결과가 저장된다.  
이후에 get()을 호출하면 저장한 값을 반환한다.  

간단하게 말하자면 지연초기화 이다. var 에서 사용했던 lateinit에 val 버젼이라고 생각하면된다.
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
출력은 아래와 같다.
```
computed!
Hello
Hello
```
#### Space

## Observable
코틀린에서는 프로퍼티에 값이 할당될 때 처리할 메서드를 등록할 수 있는 메서드를 제공한다. 바로 'Delegates.obsevable()' 메서드이다. observable() 메서드는 람다 식을 파라미터로 전달받아 setValue() 메서드가 구현된 객체를 반환한다. 그래서 위임 프로퍼티 예제와 같은 기능을 하는 코드를 'Delegator' 클래스의 구현 없이 아래와 같이 간단하게 구현할 수 있다.
```kotlin
import kotlin.properties.Delegates

class Person(val name: String, age: Int, salary: Int) {    
    var age: Int by Delegates.observable(age) {
        prop, old, new -> println("${prop.name} set! $old->$new")
    }
    var salary: Int by Delegates.observable(salary) {
        prop, old, new -> println("${prop.name} set! $old->$new")
    }
}

fun main(args: Array<String>) {
    val p = Person("Monguse", 20, 2000)
    p.age = 21
    p.salary = 2100
}
```
#### Space

## Map
Map 과 MutableMap 인터페이스는  getValue()와 setValue()에 대한 확장함수를 제공한다. 즉 위임이 가능하다.
```kotlin
class Person(val map: MutableMap<String, Any?>) {
    val name: String by map
    var age: Int? by map
    var salary: Int by map
}

fun main(args: Array<String>) {
    val p = Person(mutableMapOf("name" to "Monguse", "age" to null, "salary" to 2000))
    p.salary = 2100
    println("name: ${p.name}, age: ${p.age}, salary: ${p.salary}")
}
```