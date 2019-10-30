---
layout: default
title: Home
nav_order: 1
description: "Kotlin documentation"
permalink: /
---

## Kotlin

---
Kotlin은 2017년 구글I/O에서 공식언어로 지정했고, 2019년에도 다시한번 언급하면서 Kotlin 시대를 알리고 구글예제들 또한 Kotlin으로 대체된것을 알 수 있습니다. 아래는 realm의 post에서 발췌한 설명입니다.

Kotlin은 JVM, Android, 브라우저를 위한 정적 타입의 프로그래밍 언어입니다. 

정적으로 타입이 지정되므로 Java와 같은 동일한 타입 안정성을 갖습니다. 따라서 자동 완성 기능을 훌륭하게 제공할 수 있습니다. 정적으로 타입이 지정되므로 IDE에게 무엇이 가능한지 알려줄 수 있죠.

Java와 비교해보면, 타입의 유추도 가능합니다. 문자열인 것이 명확한 경우 문자열이라고 특정할 필요가 없습니다. 즉, Java에서는 String name = "Victoria";와 같이 꼭 문자열을 지정해줘야 하지만, Kotlin에서는 문자열임이 명확하므로 name = "Victoria"라고만 해도 충분합니다. 세미콜론도 필요 없고, val이라는 키워드가 Java의 final 선언과 같이 작용합니다.

Null 안정성도 타입 시스템에 내장됩니다. 따라서 언제 무엇이 null일 수 있을지 알 수 있으며 컴파일러가 이를 확인하도록 강제합니다. 따라서 지긋지긋한 NullPointerExceptions에서 해방될 수 있습니다. 또한, Kotlin은 기반 코드를 생성해주는 방식과 함수형 언어 기능 부분이 매력적입니다. Kotlin은 Java보다 훨씬 간결하면서도 좋은 방식을 사용합니다. 다른 언어들이 그렇듯 지나치게 간결해져서 가독성을 해치는 일도 없습니다.

또한, Kotlin은 Java와 함께 사용할 수 있고, JVM이 실행할 수 있는 바이트 코드로 컴파일됩니다.

요약하자면  

- 간결하고 실용적인 코드
- 자바와의 100% 상호운용
- Nullable/Null타입으로 NPE로부터 인한 안전성
- 함수형프로그래밍 지원  


등이 있습니다.
