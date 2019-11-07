---
layout: default
title: 변수 - Non-Null/Nullable
parent: Kotlin
nav_order: 3
---
# 변수 - Non-Null/Nullable
---

## Non-Null/Nullable
null을 값으로 가질 수 있으면 Nullable, null을 값으로 가질 수 없으면 Non-null타입이다.
```kotlin
// 에러     
var a:String = null
```
String 은 Non-Null타입이기 떄문에 null 을 가질 수 없고, 초기화도 안할 수 없다.  
만약 선언만 하고싶은 상황이라면 이는 추후에 설명할 lateinit 으로 해결 할 수 있다.

Nullable로 선언하고싶으면 변수 자료형 뒤에 ? 붙이면 사용 가능하다.
```kotlin
var a:String? = null
println(a?.length) //null 출력됨    
```  
####  Space

## Safe Calls, ?. 
코틀린에서는 ?. 연산자를 이용해 null값에 대해 안전하게 변수를 지정하고 불러 올 수 있다.
```kotlin
class Call {
     var name: String? = null

}
fun safeCallsTest() {
        val call: Call? = Call()
        val str: String = call?.name ?: "Null text"
        println(str) // Null text
}
```
####  Space

## Not null, !!
Null값이 안들어온다는 보증을 해주는 연산자이다.
Null이 아닌값만 들어올 때 사용해야 하며 Null이 들어오게 될경우 NPE를 발생시킨다. 최대한 지양하는게 좋다.
```kotlin
var str:String? = "TEST"
var nnstr:String = str!!
```
####  Space
