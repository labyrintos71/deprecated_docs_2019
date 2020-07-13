---
layout: default
title: Vuejs - Computed & Watch
parent: VueJS
nav_order: 4
---
## VueJS
---
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>안녕! 나는 타이틀!</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
    <div id="app">
        {{printmsg}}<br>
        <button v-on:click="change">CLickMe</button><br>
        {{upd}}<br>
        {{Date.now()}}
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                msg: '야할룽',
                upd: " "
            },
            methods: {
                change() {
                    if (this.msg === "야할룽") this.msg = "123";
                    else this.msg = "야할룽";
                }
            },
            computed: {
                printmsg() {
                    return this.msg.substring(0, 2);
                }
            },
            watch: {
                msg(newVal, oldVal) {
                    console.log(newVal, oldVal);
                    this.upd = newVal;
                }
            }
        })
    </script>
</body>

</html>
```
computed 속성은 종속 대상을 따라 저장(캐싱)된다는 것 입니다. computed 속성은 해당 속성이 종속된 대상이 변경될 때만 함수를 실행합니다.  
즉 message가 변경되지 않는 한, computed 속성인 reversedMessage를 여러 번 요청해도 계산을 다시 하지 않고 계산되어 있던 결과를 즉시 반환합니다.

또한 Date.now()처럼 아무 곳에도 의존하지 않는 computed 속성의 경우 절대로 업데이트되지 않는다는 뜻입니다.  
Vue 인스턴스의 데이터 변경을 관찰하고 이에 반응하는 보다 일반적인 watch 속성도 있다.  
요컨대, computed는 대상이 변경될때만 호출되고 watch는 상시 감시한다고 보면 된다.