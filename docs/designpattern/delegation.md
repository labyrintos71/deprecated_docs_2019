---
layout: default
title: Delegation
parent: Design Pattern
nav_order: 4
---
# Delegation pattern
Delegate pattern은 어떤 기능을 자신이 처리하지 않고 다른 객체에 위임을 시켜 그 객체가 일을 처리하도록 하는 것이다.  
상속은 클래스의 변수와 메소드를 모두 받기 때문에 재구현할 필요가 없어서 편리하다. 하지만 올바르지 않은 상속은 많은 문제를 일으키는데  
그중 하나가 객체의 유연성을 떨어트리는 것이다. 이런 해결방법으로 Composition관계로 구현하는 것을 권장하는데 Delegate Pattern은 그중 일반적인 패턴이다.
아래는 wiki에 작성되어있는 예제이다.
```kotlin
class Rectangle(val width: Int, val height: Int) {
    fun area() = width * height
}

class Window(val bounds: Rectangle) {
    // Delegation
    fun area() = bounds.area()
}
```
Composition 객체의 함수가 많아지면 보일러플레이트(형식적인 코드)가 많아 지는데 이를 위해 by라는 기능을 지원한다.
```kotlin
interface IWindow {
    fun getWidth() : Int
    fun getHeight() : Int
}

open class TransparentWindow() : IWindow {
    override fun getWidth(): Int {
        return 100
    }

    override fun getHeight() : Int{
        return 150
    }
}
// by가 IWindow 인터페이스 전달받은 window 객체를 이용해 Delegate Pattern 코드를 자동으로 작성한다.
class UI(window: IWindow) : IWindow by window {
    fun printInfo(){
        println("height = ${getHeight()}, width = ${getWidth()}")
    }
}

fun main(args: Array<String>) {
    val window: IWindow = TransparentWindow()
    val ui = UI(window)
    ui.printInfo()
}
```