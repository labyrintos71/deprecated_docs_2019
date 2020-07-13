---
layout: default
title: Vuejs - Event
parent: VueJS
nav_order: 3
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
        {{year}}<br>
        <button v-on:click="plus">plus</button><br> <!-- v-on 생략 불가능 -->
        <button v-on:click="minus">minus</button><br>
        <form v-on:submit.prevent="submit">
            <!-- submit은 기본적으로 리로딩이 됨 -->
            <input type="text" , v-on:keyup.enter="submit"><br>
            <button type="submit">Submit</button>
        </form>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                year: 2020
            },
            methods: {
                plus() {
                    this.year++;
                },
                minus() {
                    this.year--;
                },
                submit() {
                    alert('hello world!');
                    console.log('hello');

                }
            }
        })
    </script>
</body>

</html>
```
 vue 에 대한 이벤트 처리입니다.