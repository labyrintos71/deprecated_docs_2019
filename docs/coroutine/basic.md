---
layout: default
title: Coroutine 기초
parent: Coroutine
nav_order: 1
---
# Coroutine
코루틴이란 쉽게 말하면 Co + routine과 같다. Co는 협력, 함께 라는 의미이며 routine 은 하나의 함수와 같다. 이어서 붙이면 협력형 멀티태스킹 이라고 할 수 있다.  

먼저 routine에 대해서 알아보자. 루틴은 메인루틴과 서브루틴으로 나뉘어지는데 개념자체는 간단하다. 
```kotlin
fun main() {
    var str = reverse("asdf")
    println(str)
}

fun reverse(str: String): String{
    println("in reverse()")
    return str.reversed()
}
```
위 코드에서 main이 메인루틴이 되고 메인루틴에서 호출하는 reverse()가 서브루틴이 되는것이다.
여기서 주목해야 할 부분은 진입점인 reverse() 호출시,탈출점인 return을 만나 빠져나오기 전까지 쓰레드는 block 되어있다. 따라서 실행하게 되면
```
in reverse()
fdsa
```
아래와 같이 main의 prtinln 보다 reverse의 println이 먼저 실행된다. 
어찌보면 익숙하고 당연한 이야기다.  

하지만 코루틴은 조금 다르다. 코루틴은 위와 다르게 진입점과 호출점이 여러개다.
즉 코루틴은 return과 상관없이 언제든지 중간에 나갈수 있으며 다시 그자리로 진입이 가능하다.

```kotlin
fun main() {
    GlobalScope.launch {
        delay2()
        println("inscope!")
        delay3()
    }
    println("mainroutine")
    Thread.sleep(6000)
}

suspend fun delay2(){
    delay(2000)
    println("delay2")
}

suspend fun delay3(){
    delay(3000)
    println("delay3")
}
```
자세한 코드의 설명은 추후 문서에 하겠지만 GlobalScope.launch블록이 코루틴이라고 볼수 있다. 코루틴에서는 return 이 아닌 suspend를 만나면 잠시 멈추고 나갈수 있다. 출력 결과를 보자.  
```
mainroutine
delay2
inscope!
delay3
```
일반적인 루틴이라면 (delay()는 suspend 함수에서만 사용 가능한 Thread.sleep이라고 보면 된다.) delay2 가 먼저 출력이 되어야한다.  
하지만 코루틴에서는 suspend인 delay2()를 만나 코루틴을 잠시 나가서 mainroutine을 출력하게 된다.(그 중에도 delay2는 block된게 아니라 코루틴에서 돌아가고 있다.) 