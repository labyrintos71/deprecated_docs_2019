---
layout: default
title: Singletone
parent: Design Pattern
nav_order: 2
---
# Singletone Pattern
Singletone Pattern은 전역변수를 사용하지 않고 객체를 하나만 생성 하도록 하며 어디에서든지 참조 할 수 있도록 하는 패턴
코틀린에서는 Java에서의 싱글톤 객체를 만들 필요가 없다.
```kotlin
object Elvis {
	fun foo(){ }
	fun bar(){ }
}
```
object 선언을 통해 싱글톤 객체를 손쉽게 만들 수 있다.