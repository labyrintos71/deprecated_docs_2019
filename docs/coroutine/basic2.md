---
layout: default
title: Coroutine 기초 - 2
parent: Coroutine
nav_order: 4
---
## Structured concurrency

GlobalScope.launch 는 top-level coroutine을 만든다.  
GlobalScope는 process가 살아있는한 계속 유지 되기 때문에 GlaobalScope.launch만 사용하게 된다면 이를 관리하기 위해 Job을 모두 가지고 있으며 join으로 관리해야 한다. 그렇지 않으면 메모리나 관리면에서 에러를 유발시키기 쉽다.  

이런 문제점을 해결하기 위해 Structured concurrency를 사용할 수 있다.
특정한 Coroutine Scope안에서 새로운 빌더로 Coroutine Scope를 만들면
외부의 Scope는 내부의 coroutine들이 종료되기 전까지 종료되지 않으므로 join 없이 간단하게 만들 수 있다.

```kotlin
fun main() {
    println("start function")
    runBlocking {
        println("start Run BLock")
        val jobs = List(3) {
            launch {
                delay(1000L)
                println("count : $it")
            }
        }
        // join을 하면 End Run Block이 나중에 찍힌다.
        //  jobs.forEach { it.join() }
        println("End Run BLock")
    }
    println("End function")
}

```
실행결과
```
start function
start Run BLock
End Run BLock
aaa
aaa
aaa
End function
```
runBlocking은 내부의 coroutine인 list의 3번이 다 수행 될 때 까지 블록된다.

## Scope builder
순서를 보장하고 싶을 떄 위처럼 join을 걸어도 되지만  
내부에 CoroutineScope를 만들어서 보장할 수 있다.

```kotlin
fun main() = runBlocking {
    // this: CoroutineScope
    launch {
        delay(200L)
        println("Task from run Blocking    #2")
    }
    coroutineScope {
        // Creates a new coroutine scope
        launch {
            delay(500L)
            println("Task from nested launch   #3")
        }
        delay(100L)
        println("Task from coroutine scope #1")
        // This line will be printed before nested launch
    }
    // This line is not printed until nested launch completes
    println("Coroutine scope is over   #4")
}
```
실행결과
```
Task from coroutine scope #1
Task from run Blocking    #2
Task from nested launch   #3
Coroutine scope is over   #4
```
#### Space
## Extract function refactoring
만약 coroutine에서 사용하는 함수를 외부에서 정의하고 싶으면 suspend 키워드를 이용할수 있다.
```kotlin
fun main() = runBlocking {
    launch { doWorld() }
    println("Hello,")
} 
// this is your first suspending function 
suspend fun doWorld() {
    delay(1000L)
    println("World!")
}
```
다만 doWorld()는 일반함수에서 호출하지 못하고, suspend를 붙인다고 해서 다른 스레드에서 실행시키는게 아니기에 오래걸리는 작업은 다른 Dispatcher에서 관리하도록 해야한다.
#### Space
## Coroutines ARE light-weight
Essentially, coroutines are light-weight threads.  
코루틴 공식 가이드 라인에도 처음부터 나오듯이 코루틴은 경량 쓰레드다.
```kotlin
fun main() = runBlocking {
    repeat(100_000) {
        // launch a lot of coroutines 
        launch {
            delay(1000L)
            print(".")
        }
    }
}
```
만약 같은 코드를 스레드로 한다면 OOM을 일으킨다.

#### Space
## Global coroutines are like daemon threads
GlobalScope는 기초 1 문서에서도 말했듯이 앱의 생명주기와 같다.
```kotlin
fun main() = runBlocking {
    GlobalScope.launch {
        repeat(1000) { i ->
            println("I'm sleeping $i ...")
            delay(500L)
        }
    }
    delay(1300L) // just quit after delay
}
```
실행결과
```
I'm sleeping 0 ...
I'm sleeping 1 ...
I'm sleeping 2 ...
```
runBlocking은 내부에서 발생한 모든 자식 corouitne의 동작을 보장한다.  
내부에서 GlobalScope을 이용하여 launch 했기 때문에 runBlocking의 scope가 아니다.  
따라서 runBlock은 1.3초만 대기하고 종료하고, main함수가 종료되면서 application process 역시 종료 된다.  

//단 안드로이드에서 이 코드를 수행하면 죽지않고 계속 로그가 찍힌다.
