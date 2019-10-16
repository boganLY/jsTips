# 1. % 的用法

```javascript
  const users = [
  '付小小',
  '曲丽丽',
  '林东东',
  '周星星',
  '吴加好',
  ];
  for(var i = 0; i < 50; i++){
    let user = users[i % users.length];
    console.log(user); // 付小小 曲丽丽 林东东 周星星 吴加好 付小小....
  }
```
- `x % y` 得到的数字最大不会超过 `y`,可以循环输出数组里的内容。

# 2. Object.defineProperty()
> 问到vue都会脱口而出MVVM数据绑定，主要是数据变化更新视图。给视图添加一个监听事件，使用`Object.defineProperty()`方法定义属性，把`data`对象里的数据通过`getter`和`setter`进行读写，然后监听数据的变化，从而进行实时更新。

* 在vue中实现双向数据绑定的代码
```javascript
<div id="mvvm-app">
    <input type="text" v-model="word">
    <p>{{word}}</p>
    <button v-on:click="sayHi">change model</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  var vm = new Vue({
    el: '#mvvm-app',
    data: {
      word: 'Hello World!'
    },
    methods: {
      sayHi: function() {
        this.word = 'Hi, everybody!';
      }
    }
  });
</script>
```

* 手动实现一个简单的双向数据绑定

### `html`
```html
<div id="app">
  <input type="text" id="txt">
  <p id="show-txt"></p>
</div>
```
### `首先需要一个放数据的地方，先创建一个对象，获取修改和显示数据的元素`
```JavaScript
let obj = {};
let writedom = document.getElementById('txt');
let showdom = document.getElementById('show-txt');
```
### `接下来给input绑定一个监听事件`
```JavaScript
writedom.addEventListener('keyup', function(e) {
  obj.txt = e.target.value;
})
```
> 给`input`绑定一个`addEventListener()`方法，`addEventListener()`接收三个参数（event， function，useCapture），分别是`事件`，`触发时的函数`，`事件是否在捕获或冒泡阶段执行`，前两个是必须。`keyup`键盘弹起时触发函数，函数接收一个`Event对象`，获取到值赋值给`obj.txt`。
### `接下来使用Object.defineProperty()去修改obj的值`

```JavaScript
Object.defineProperty(obj, 'txt', {
  get: function() {
    return obj;
  },
  set: function(val) {
    showdom.innerHTML = val
  }
})
```
> `Object.defineProperty(obj, prop, descriptor)`，也接收三个参数分别是：`定义属性的对象`，`要定义或修改的属性的名称`，`将被定义或修改的属性描述符`。get()返回定义的属性对象，set()方法接收的形参是当前修改的值，把值在视图上显示出来就实现了一个简化版的数据绑定