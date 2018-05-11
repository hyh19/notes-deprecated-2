# 第 1 章 初识 Vue.js

```html
<!-- 自动识别最新稳定版本的 Vue.js -->
<script src="https://unpkg.com/vue/dist/vue.min.js"></script>

<!-- 指定某个具体版本的 Vue.js -->
<script src="https://unpkg.com/vue@2.1.6/dist/vue.min.js"></script>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <button v-if="showBtn" v-on:click="handleClick">Click me</button>
    </div>
</body>
<script type="text/javascript">
    new Vue({
        el: "#app",
        data: {
            showBtn: true
        },
        methods: {
            handleClick: function () {
                console.log('Clicked!')
            }
        }
    })
</script>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <script src="https://unpkg.com/vue@2.1.6/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <ul>
            <li v-for="book in books">{{ book.name }}</li>
        </ul>
    </div>
</body>
<script type="text/javascript">
    new Vue({
        el: "#app",
        data: {
            books: [
                { name: "《Vue.js 实战》" },
                { name: "《JavaScript 语言精粹》" },
                { name: "《JavaScript 高级程序设计》" }
            ]
        }
    })
</script>
</html>
```

