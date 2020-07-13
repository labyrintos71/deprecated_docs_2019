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
    <style>
        .red {
            color: red;
        }

        .font-bold {
            font-weight: bold;
        }
    </style>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
    <div id="app">
        <div :style="{color: red, fontSize: size}">Hello</div>
        <div :class="{ red: isRed, 'font-bold' : isBold }">
            World
        </div><br>
        <button @click="update">하이</button>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                isRed: false,
                isBold: false,
                red: 'red',
                size: '30px'
            },
            methods: {
                update() {
                    this.isRed = !this.isRed;
                    this.isBold = !this.isBold;
                }
            }
        })
    </script>
</body>

</html>
```
font-bold 처럼 -가 들어간건 사용할때 ' 로 감싸줘야 한다.