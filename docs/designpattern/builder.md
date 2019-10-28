---
layout: default
title: Builder
parent: Design Pattern
nav_order: 1
---
# Builder Pattern
Builder Pattern은 클래스를 정의하면서 동시에 default 파라미터를 설정할 수 있다.
```kotlin
data class User(
    val name: String,
    val age: Int = 0,
    val address: String,
    val phone: Int
)

val user = User("Jeonggyu", 28, phone = 86343623)
```
위와 같이 선택적으로 필요한 필드에 필드 네임을 지정하여 값을 초기화할 수 있다.