---
layout: default
title: Vuejs 시작하기
parent: VueJS
nav_order: 2
---
## VueJS
---
먼저 HTML 코드를 작성해야하는데 일일이 칠 필요 없이 ! 를 타이핑 한후 tab키를 누르면  
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>

</body>

</html>
```
위와 같은 HTML 보일러 플레이트가 나온다.


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
        {{ person.name}} <!-- 변수 받아오기 -->
        <br>
        {{ nextYear('야할룽') }}  <!-- 함수 결과값 받아오기 -->
        <br>
        <input v-bind:type="type" :value=inputdata> <!-- value는 initvalue, v-bind:value 는 해당 데이터 바인딩, v-bind 는 생략 가능-->
        <br>
        <a :href="getLink('/channel/UCcvLSRIWJIAGFDyWtzkbiHA')">유튜브로 이동하기!</a> <!--함수 리턴값도 바인딩 가능하다.-->
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                person: {
                    name: '수현냥은',
                    age: 21
                },
                inputdata: 'hello',
                type: 'text',
                link: "https://www.youtube.com"
            },
            methods: {
                getLink(channel) { return this.link + channel },
                nextYear(greeting) {
                    return greeting + '! ' + this.person.name + '는 내년에' + (this.person.age + 1) + '살 입니다';
                },
                otherMethod: function () {
                    return nextYear();
                }
            }
        })
    </script>
</body>

</html>
```
Vue 생성자 함수로 뷰모델을 만드는데 이 Vue 인스턴스를 스크립트 칸에 만들어줍니다.  
el 은 div와 연결해줄 키입니다.  
data 부분엔 사용할 데이터를, methods 에는 사용하고자 하는 함수를 구현해주면 됩니다.