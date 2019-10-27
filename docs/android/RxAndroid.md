---
layout: default
title: RxAndroid
parent: Andrroid
nav_order: 1
---
# RxAndroid
RxAndroid 는 RxJava에 최소한의 클래스를 추가하여 안드로이드 앱에서 리액티브 구성요소를 쉽고 간편하게 사용하게 만드는 라이브러리 입니다.  
그래들에 다음과 같이 추가하면 됩니다.
```gradle
implementation "io.reactivex.rxjava2:rxjava:2.1.1"
implementation "io.reactivex.rxjava2:rxandroid:2.1.1"
```
RxJava는 간단히 말해서 발행, 구독 입니다. 예를들어  
누군가는 데이터의 강에 데이터를 흘려보내고  
누군가는 데이터의 강에서 데이터를 줍습니다.
이때 흘려보내는걸 발행, 줍는걸 구독이라고 합니다.  

예를들어 데이터를 받아와서 ui에 업데이트 하는 앱이라면  
발행에서 데이터를 가져오고 구독에서 ui에 업데이트 하면 됩니다.  

코드는 subscribeOn, observerOn, subscribe 3개로 나뉘게 됩니다.  

먼저 subscribeOn은 데이터를 강에 흘려보내는 스레드(스케쥴러)를 지정하는 작업입니다.  
정확하게는 데이터를 발행(연산)하는 스케줄러를 지정하는 것이라고 할 수 있습니다.  

그 후에 obsereverOn으로 데이터를 줍는 스레드(스케쥴러)를 지정합니다.
데이터를 구독하는 스케쥴러를 지정하는 것입니다.

마지막으로 subscribe는 데이터를 주워서 처리하는 역할 입니다.  
인자값으로 람다식 두개를 전달하며 첫번째 람다는 성공했을 떄, 두번째 람다는 실패했을 때 실행됩니다.

아래는 Retrofit 예제입니다.
```kotlin
 //아래에서 설명
 compositeDisposable = CompositeDisposable()

 compositeDisposable.add(
            RetrofitCreator
                .create(GithubService::class.java)
                .getRepoList("discord")
                .subscribeOn(Schedulers.io())               // 네트워크 입출력이기 떄문에 io를 구독
                .observeOn(AndroidSchedulers.mainThread())  // UI수정을 위해서 mainthread를 사용  
                .subscribe({
                    // 정상적으로 잘 받아왔을경우 처리
                    text.text=it.items[0].full_name
                },{
                    // 오류가 생겼을 경우 처리
                    Log.d("MainActivity","ERROR message : ${it.message}")
                }))
```

## CompositeDisposable
만약 구독자가 텍스트뷰를 참조하고 있을 때, 액티비티가 비정상 종료 되더라도  텍스트뷰가 참조하는 액티비티는 GC 대상이 아니기 때문에 메모리 누수가 발생합니다.  
이를 해결하기 위해서는 CompositeDisposable 를 사용하면 됩니다.