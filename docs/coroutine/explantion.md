---
layout: default
title: Coroutine 이론
parent: Coroutine
nav_order: 1
---
# Coroutine 
코루틴은 먼저 3가지 키워드로 알아보려 한다.  
1. 협력형 멀티 태스킹
2. 동시성 프로그래밍 지원
3. 쉬운 비동기 처리  

## 협력성 멀티 태스킹
코루틴이란 쉽게 말하면 Co + routine이다. Co는 협력, 함께 라는 의미이며 routine 은 하나의 함수와 같다. 두단어를 붙이면 협력형 멀티태스킹 이라고 할 수 있다.  

코루틴 전에 먼저 routine에 대해서 알아보자. 루틴은 메인루틴과 서브루틴으로 나뉘어지는데 개념자체는 간단하다. 
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
위와 같이 main의 prtinln 보다 reverse의 println이 먼저 실행된다. 
어찌보면 익숙하고 당연한 이야기다.  

하지만 코루틴은 조금 다르다. 코루틴은 위와 다르게 진입점과 호출점이 여러개다.  
즉 코루틴은 return과 상관없이 언제든지 중간에 나갈수 있으며 다시 그자리로 진입이 가능하다.

```kotlin
fun main() {
    // 코루틴 블록 ////
    GlobalScope.launch {
        delay2()
        println("inscope!")
        delay3()
    }
    ////////////////////
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
코드의 자세한 설명은 놨두고 일단 GlobalScope.launch블록이 코루틴이라고 생각해보자.  
코루틴에서는 return 이 아닌 suspend를 만나면 잠시 멈추고 나갈수 있다. 출력 결과를 보자.  
```
mainroutine
delay2
inscope!
delay3
```
// GlobalScope.lauch{...} 는 thread{...}와 같은 역할, delay{...}는 Thread.sleep{...}과 같은 역할

일반적인 루틴이라면 delay2 가 먼저 출력이 되어야한다.  
하지만 코루틴에서는 suspend인 delay2()를 만나 코루틴을 잠시 나가서 mainroutine을 출력하게 된다.  
delay2()는 suspend를 만나 멈췄다가 2초가 지나면 caller가 불러 delay2를 출력하고 아래 구문을 진행한다.
#### Space

## 동시성 프로그래밍
코루틴의 이와같은 특성은 동시성 프로그래밍을 가능케 하는데  
동시성 프로그래밍의 뜻은 아래 병행성 프로그래밍과 비교한 예시를 보자.  

두개의 도화지에다가 그림을 그린다고 가정을 할 떄  
동시성은 한손으로 양쪽 도화지를 빠르게 번갈아가며 그리는거고  
병행성은 양손으로 양쪽에 동시에 그리는 거라고 할 수 있다.

코루틴에서 지원하는 동시성은 멀티스레드에서 지원하는 동시성보다 훨씬 더 좋은 효율을 낼 수 있다. cpu코어 1개에서 여러개의 쓰레드가 점유와 해제를 반복하는 비용이 1개의 쓰레드에서 여러 루틴을 왔다갔다 하는것 보다 더 들기 떄문이다.