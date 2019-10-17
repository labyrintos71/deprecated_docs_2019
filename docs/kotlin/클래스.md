---
layout: default
title: 클래스
parent: Kotlin
nav_order: 5
---

# 클래스

## 생성자
코틀린에서는 훨씬 적은 코드로 초기화를 할 수 있는 문법을 제공한다.  
클래스의 본문도 작성할 필요도 없다. 클래스 초기화는 다음과 같다.
```kotlin
// constructor는 생략 가능하다.
class Student constructor(var roll_number: Int, var name: String = "sheldon")

//name 처럼 생성자의 기본값도 설정할 수 있다.
class Student(var roll_number: Int, var name: String = "sheldon")

//기본값이 명시된 생성자는 생략할 수 있다.
fun test(){
    var amy = Student(3) // name엔 sheldon 자동으로 들어간다.
}

class Student(var roll_number: Int, var name: String = "sheldon"){

    //기존 생성자에서 처리하던 일은 init 메소드에서 처리하면 된다.
    init{
        //해야할 일
    }

    //this 키워드를 이용하면 다른생성자를 호출할 수 있다.
    constructor(var roll_number: Int, var name: String = "sheldon", var classinfo: Int) : this(roll_number, name){

    }
}
```
#### Space

## data class
이름 그대로 data를 보유하기만 하는 class 이다.
생성은 아래와 같이 한다.
```kotlin
data class constest(a: Int = 0, b: Double = 0.0, c: String = "default")
```
데이터 클래스는 기본적으로 다음과 같은 기능들을 제공하고 있다.  
equals()  
hashCode()  
copy()  
toString()  
componentsN()   
#### Space

## data class destructuring
destructuring은 data class에서 제공해주는 기능중 하나이다.  
```kotlin
fun desttest() {
    var user = Test("labyrintos71", 21)
    var (name, age) = user
    println("user name : $name, user age : $age")
}

data class Test(var name: String, var age:Int) {

}
```
이 중 아래와 같은 부분을 destructuring 이다.
```kotlin
var (name, age) = user

// 이렇게도 사용 가능하다.
var name = user.component1()
var age = user.component2()

// for문에도 사용 가능하다.
for( (a, b) in collection ) { ... }
```

## 인자값 전달
기본값이 명시되있는 경우 그 값은 생략 할 수 있다. 아래 예제를 보도록 하자. 
```kotlin
fun example() {
    //한번 변수를 명해주면 뒤에는 전부 명명해줘야된다
    constest(1, b = 0.1)
    constest(a = 1, b = 0.1)
    constest(a = 1, 0.1)      //에러
    constest(1, "123")        //에러
}

data class constest(a: Int = 0, b: Double = 0.0, c: String = "default")
```