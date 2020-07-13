---
layout: default
title: Vuejs - v-if & v-show
parent: VueJS
nav_order: 6
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
        <button @click="onToggle">toggle</button><br>
        <template v-if="show"> <!-- number === 3 이런 식도 대입 가능하다-->
            <div>1</div>
            <div>2</div>
            <div>3</div>
        </template>
        <div v-else-if="show">No</div>
        <div v-else="show">No</div>

        <div v-show="show">Hi</div>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                show: true
            },
            methods: {
                onToggle() {
                    this.show = !this.show;
                }
            }
        })
    </script>
</body>

</html>
```

v-if는 조건부 렌더링 이여서 초기 렌더링 비용이 높다.
하지만 v-show는 무조건 초기 렌더링을 하고 display none으로 보여주기 때문에 초기 렌더링 비용이 저렴하다