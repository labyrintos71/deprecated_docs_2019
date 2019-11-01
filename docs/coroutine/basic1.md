---
layout: default
title: Coroutine 기초 - 1
parent: Coroutine
nav_order: 3
---
# Coroutine 
코틀린 공식 가이드에 있는 예제를 보도록 하자.
```kotlin
import kotlinx.coroutines.*

fun main() {
    GlobalScope.launch { // launch a new coroutine in background 
                         // and continue
        delay(1000L) // non-blocking delay for 1 second 
        println("World!") // print after delay
    }
    println("Hello,") // main thread continues while coroutine is delayed
    Thread.sleep(2000L) // block main thread for 2 seconds 
                        // to keep JVM alive
}
```
launch는 코루틴 빌더이며, 이를 이용해 코드를 CoroutineScope안에서 실행시킨다.  
위 예제는 GloablScope를 사용했는데
GlobalScope의 lifetime 은 전체 앱 프로세스의 의존하므로 앱이 종료하면 같이 끝난다. 그래서 main() 끝에 sleep 을 걸어야 launch 내부 동작을 실행 할 수 있다.
  
/*단, 위 코드를 안드로이드에서 실행하면 sleep을 걸지 않아도 "Hello world"가 출력됨을 알수 있다. Activity를 finish()하더라도 process 자체가 죽지 않기 때문에 sleep이 없더라도 coroutine 내부 코드는 그대로 실행된다.
*/  

delay는 suspend함수다. suspend란 잠시 중단한다는 의미이고, 잠시 중단한다면 언젠가는 다시 resume된다는 뜻이다. 위 코드에서는 delay라는 suspend가 끝이나면 그때 caller가 resume시켜 아랫줄 코드를 실행시킨다.

delay라는 함수는 현재 실행중인 thread를 block시키진 않지만 코루틴은 일시 중지시킨다. thread입장에서는 non-blocking이다.

#### Space
## runBlocking
위 코드는 non-blocking 함수인 delay()와 blocking 함수인 Thread.sleep() 함수를 혼용하고 있다. 이렇게 쓰면 헷갈리니 blocking 함수라고 명확하게 명시를 해보자.
```kotlin
fun main() {
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L)
        println("World!")
    }
    println("Hello,") // main thread continues here immediately
    runBlocking {     // but this expression blocks the main thread
        delay(2000L)  // ... while we delay for 2 seconds to keep JVM alive
    }
}
```
runBlocking{...} 에서 유추할수 있듯이 현재 쓰레드를 블록시키고 새로운 코루틴을 실행시킨다.  

언제까지 블록 시킬까? 블록안에 있는 코드가 끝날 떄 까지 블록시킨다.
runBlocking{...}안에 delay(2000)을 실행 시켰으니 2초동안 메인쓰레드가 블록된다. 물론 2초동안 launch했던 코루틴은 동작하고 2초가 지나면 main()은 종료된다.  

혹은 이렇게도 쓸 수 있다.
```kotlin
fun main() = runBlocking<Unit> { // start main coroutine
    GlobalScope.launch { // launch a new coroutine in background and continue
        delay(1000L)
        println("World!")
    }
    println("Hello,") // main coroutine continues here immediately
    delay(2000L)      // delaying for 2 seconds to keep JVM alive
}
```
runBlocking{...}을 메인메소드에 걸어줌으로써  
시작으로부터 메인스레드를 블록 시키고 top-level 코루틴을 시작한다.  
따라서 runBlocking{...}에서 가장 오래걸리는 delay(2000)이 끝날 때 까지 main 메소드가 살아있다.  

/*안드로이드 의 경우 runBlocking() 함수를 메인 스레드 (UI 스레드) 에서 호출하여 시간이 오래 걸리는 작업을 수행하는 경우 ANR 이 발생할 위험이 있으므로 주의해야합니다.*/
#### Space
## Job
하지만 delay(1000)인 World! 를 찍기 위해 2초를 기다리는건 비효율적이다.  
이는 Job의 join()을 통해서 대체할 수 있다.  
```kotlin
fun main() = runBlocking {
//sampleStart
    val job = GlobalScope.launch { // 새로운 코루틴을 실행하고 job 에 저장
        delay(1000L)
        println("World!")
    }
    println("Hello,")
    job.join() // wait until child coroutine completes
//sampleEnd
}
```
launch는 Job이라는 객체를 반환한다.이는 스케쥴 관리나 상태 등을 확인 할 떄 사용한다.  
job.join()이 job이 끝나기를 계속 기다린다. job이 끝나지 않으면 runBlocking{...}으로 생성한 코루틴은 안끝난다.  

#### Space

모든 코루틴빌더(runBlocking{},launch{}...)는 빌더로 인해 생성되는 코드블록안에 CouroutineScope객체를 추가한다. 위 코드에서는 runBlocking의 블록안에서 GlobalScope로 코루틴을 만들어 launch를 했지만 그러지 않고 그냥 launch를 호출하면 바로 바깥의 스코프와 동일한 스코프에 생긴다.  
게다가 바깥의 있는 코루틴은 안쪽에 있는 코루틴이 끝날때 까지 끝나지 않는 다는 성질을 이용해 코드를 좀 더 간결하게 만들 수 있다.
```kotlin
fun main() = runBlocking { // this: CoroutineScope
    launch { // launch a new coroutine in the scope of runBlocking
        delay(1000L)
        println("World!")
    }
    println("Hello,")
}
```
