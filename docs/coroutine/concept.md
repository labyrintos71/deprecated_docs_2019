---
layout: default
title: Coroutine 키워드
parent: Coroutine
nav_order: 2
---
##  Coroutine 키워드
코루틴을 배워보기 전에, 알고가면 좋은 키워드다.
- CoroutineScope, GlobalScope
- CoroutineContext
- Dispatcher
- launch, async

## CoroutineScope, GlobalScope
CoroutineScope는 말 그대로 코루틴을 묶음으로 제어할수 있는 단위다.  

GloablScope는 CoroutineScope의 한 종류이며 프로그램 전반에 걸쳐 백그라운드에서 실행된다.  

#### Space
## CoroutineContext 
CoroutineContext는 코루틴을 어떻게 처리할것인지 에 대한 레퍼런스이며  
주요 요소로는 Job, dispatcher가 있다.  

#### Space
## Dispatcher
Dispatcher는 CoroutineContext의 주요 요소다.
CoroutineContext 을 상속받아 어떤 스레드를 이용해서 어떻게 동작할것인지를 미리 정의되어있다.

- Dispatchers.Default : CPU 사용량이 많은 작업에 사용, 주 스레드에서 작업하기에는 너무 긴 작업들에게 적합.
- Dispatchers.IO : 네트워크, 디스크 사용 할때 사용, 파일 읽고, 쓰고, 소켓을 읽고, 쓰고 작업을 멈추는것에 최적화.
- Dispatchers.Main : 안드로이드의 경우 UI 스레드를 사용.
- Dispatchers.Unconfined  : 특정 스레드, 스레드 풀을 지정하지 않으며 일반적으로 사용 안함.

#### Space
# launch, async
launch, async는 CouroutineScope의 확장함수 이며, 넘겨받은 코드블록으로 코루틴을 만들고 실행해주는 빌더다.  

launch는 Job, async는 Defferd 객체를 반환하며, 이 객체를 사용해 수행 결과를 받거나 스케쥴 관리가 가능하다.

## launch 
launch()로 정의된 코루틴 블록은 Job을 반환한다.
```kotlin
val job : Job = launch {
    ...
}
```
받아온 Job객체는 해당 코루틴 블록의 스케쥴 관리나 상태 등을 확인 할 떄 사용한다.  
Job.join()을 이용하면 해당 코루틴 블록이 완료 될 떄 까지 대기 할 수 있다.
```kotlin
fun main() = runBlocking {
    val job1: Job = launch {
        var i = 0
        while (i < 10) {
            delay(500)
            i++
        }
    }
    //job1 완료 대기
    job1.join()
    //job1.cancel() 취소
}
```
job객체가 여러개라면 일일이 join()을 호출하지 않고 joinAll()을 사용할 수 있다.
```kotlin
joinAll(job1,job2)
```
launch()로 정의된 코루틴 블록은 즉시 수행되며 Job 객체로는 블록 제어는 가능하지만 블록의 결과를 받아 올 수없다. 이를 위해서는 async()를 사용하면 된다.  
#### Space
## async
async()로 정의된 코루틴 블록은 Defferd을 반환한다.
```kotlin
val deferred : Deferred<T> = async {
    ...
    T // 결과값
}
```
받아온 Defferd객체를 이용해 제어가 가능하며 await()를 이용해 결과값을 받아올 수 있다.
```kotlin
fun main() = runBlocking {
    val deferred: Deferred<String> = async {
        var i = 0
        while (i < 10) {
            delay(500)
            i++
        }

        "result"
    }

    val msg = deferred.await()
    println(msg) // result 출력
}
```
여러개의 async 코루틴 블록을 실행할 경우 각각의 Deferred 객체에 대해서 await() 함수로 코루틴 블록이 완료 될때까지 다음 코드 수행을 대기할수 있다. await() 함수는 코루틴 블록이 완료되면 결과를 반환한다.
```kotlin
fun main() = runBlocking {
    val deferred1 = async {
        var i = 0
        while (i < 10) {
            delay(500)
            i++
        }

        "result1"
    }

    val deferred2 = async {
        var i = 0
        while (i < 10) {
            delay(1000)
            i++
        }

        "result2"
    }

    val result1 = deferred1.await()
    val result2 = deferred2.await()

    println("$result1 , $result2") // result1 , result 2 출력
}
```
물론 join()과 마찬가지로 await()도 한번에 호출할 수 있다.
```kotlin
var result : List<Any> = awaitAll(deferred1,deferred2)
```
Any 타입으로 결과를 받기 때문에 반환형태가 달라도 상관없다.
```kotlin
 fun main() = runBlocking {
    val deferred1 = async {
        var i = 0
        while (i < 10) {
            delay(50)
            i++
        }

        "result1"
    }

    val deferred2 = async {
        var i = 0
        while (i < 10) {
            delay(10)
            i++
        }

        1234
    }

    val result1 = deferred1.await()
    val result2 = deferred2.await()
    awaitAll(deferred1,deferred2).forEach{
        println(it)
    }
}
```

## 지연실행
launch() 와 async() 모두 즉시 실행되는데 만약 처리 시점을 뒤로 미루고 싶을 경우 CoroutineStart.LAZY를 이용해 지연 시킬 수 있다.

```kotlin
val job = launch (start = CoroutineStart.LAZY) {
    ...
}
또는
val deferred = async (start = CoroutineStart.LAZY) {
    ...
}
```
launch 코루틴 블록을 지연 실행 시킬 경우 Job 클래스 의 start() 함수 를 호출하거나 join() 함수를 호출하는 시점에 launch 코드 블록이 수행된다.
```
job.start()
또는
job.join()
```
async 코루틴 블록을 지연 실행 시킬 경우 Deferred 클래스 의 start() 함수 를 호출하거나 await() 함수를 호출하는 시점에 async 코드 블록이 수행된다.
```
deferred.start()
또는
deferred.await()
```
launch, async 블록 모두 start()로 실행시킬 경우 수행결과를 반환하지 않는다.
```kotlin
fun main() = runBlocking {
    println("start")

    val deferred = async(start = CoroutineStart.LAZY) {
        var i = 0
        while (i < 5) {
            delay(500)
            println("lazy async $i")
            i++
        }
    }

    deferred.start()

    println("end")
}
```
위에처럼 start()로 실행 할 경우
```
start
end
lazy async 0
lazy async 1
lazy async 2
lazy async 3
lazy async 4
```
를 출력하게 된다.