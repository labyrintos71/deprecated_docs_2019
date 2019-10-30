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
## launch, async
launch, async는 CouroutineScope의 확장함수 이며, 넘겨받은 코드블록으로 코루틴을 만들고 실행해주는 빌더다.  

launch는 Job, async는 Defferd 객체를 반환하며, 이 객체를 사용해 수행 결과를 받거나 스케쥴 관리가 가능하다.