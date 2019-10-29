---
layout: default
title: Decorator
parent: Design Pattern
nav_order: 3
---
# Decorator Pattern
데코레이터 패턴은 객체의 결합을 통해 기능을 동적으로 유연하게 확장 할 수 있게 해주는 패턴이다.  
즉, 기본 기능에 추가할 수있는 기능의 종류가 많은 경우에 각 추가 기능을 Decorator 클래스로 정의 한 후 필요한 Decorator 객체를 조합함으로 써 추가 기능의 조합을 설계하는 방식이다.
```kotlin
//음료의 상위클래스
abstract class Coffee {
    abstract fun getDescription(): String
    abstract fun cost(): Double
}

//하우스 블랜드 커피
class HouseBlend() : Coffee() {
    override fun getDescription(): String = "하우스 블렌드 커피"
    override fun cost(): Double = 0.89
}

// 에스프레소
class Espresso() : Coffee() {
    override fun getDescription(): String = "에스프레소"
    override fun cost(): Double = 1.99
}

// 첨가물 데코레이터
abstract class CondimentDecorator : Coffee()

class Mocha(val beverage: Coffee) : CondimentDecorator() {
    override fun getDescription(): String = beverage.getDescription() + " 모카"
    override fun cost(): Double = beverage.cost() + 0.20
}

class Shot(val beverage: Coffee) : CondimentDecorator() {
    override fun getDescription(): String = beverage.getDescription() + " 샷"
    override fun cost(): Double = beverage.cost() + 0.15
}

class Whip(val beverage: Coffee) : CondimentDecorator() {
    override fun getDescription(): String = beverage.getDescription() + " 휘핑"
    override fun cost(): Double = beverage.cost() + 0.10
}


fun main(args: Array<String>) {
    var beverage: Coffee = Espresso()
    beverage = Shot(beverage)
    beverage = Mocha(beverage)
    beverage = Whip(beverage)
    println("description : ${beverage.getDescription()} cost : ${beverage.cost()}")
}
```