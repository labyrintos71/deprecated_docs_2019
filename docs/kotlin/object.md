---
layout: default
title: 오브젝트
parent: Kotlin
nav_order: 6
---

# Object
객체지향에서 객체는 일반적으로 클래스를 정의하고 생성자를 통해 객체를 찍어낸다.  
하지만 object를 이용하면 그러지 않아도 생성할 수 있다.  
class 대신 object 키워드를 사용하면, 뒤에 정의되는 내용은  
클래스가 아니라 객체 그자체, 인스턴스가 된다.

```kotlin
//아래처럼 싱글톤으로 사용 가능하다
fun test(){
    var a = Foo     // Init !
    var b = Foo     //
    Foo.callFoo()   //Foo 2030562336
}

object Foo {
    init { println("Init !")}
    fun callFoo() = println("Foo " + this.hashCode())
    var foo = "foo"
}
```
#### Space
## Companion object, 동반자 객체
코틀린에는 static이 없는대신 동반자 객체가 있다.  
이를 이용하면 기존 static 처럼 사용할 수 있다. 
```kotlin
fun test() {
    val a = tester().testfun() //init  testfun875827115
    val b = tester().testfun() //init  testfun787387795
    tester.callFoo()           //Foo 2030562336
}

class tester {
    init {
        println("init")
    }
    fun testfun(){
        println("testfun" +this.hashCode())

    }
   companion object Foo {
        fun callFoo() = println("Foo "+this.hashCode())
        var foo = "foo"
    }
}
```
