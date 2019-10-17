# vue实例的创建

vue控制的这个元素区域就是MVVM中的 v（view）
``` HTML
<div id = 'app' >{{ msg }}</div>
```
vm接收new出来的vue实例是MVVM中的VM调度者

里面 data，就是MVVM的 M，用来保存数据 
```JavaScript
var vm = new Vue({
  el:'#app',
  data:{
    msg:'vue study note day one'
  }
})
``` ## vue实例的创建

vue控制的这个元素区域就是MVVM中的 v（view）
``` HTML
<div id = 'app' >{{ msg }}</div>
```
vm接收new出来的vue实例是MVVM中的VM调度者

里面 data，就是MVVM的 M，用来保存数据 
```JavaScript
var vm = new Vue({
  el:'#app',
  data:{
    msg:'vue study note day one'
  }
})
``` 

## v-cloak
解决了插值表达式的闪烁问题
```HTML
<div id = 'app' >
  <p v-cloak > {{ msg }} </p>
</div>
<!--v-cloak 保持在元素（<p>）上，直到关联实例结束编译  -->
```

```JavaScript
var vm = new Vue({
  el:'#app',
  data:{msg:123}})
```
和css的display none配合使用，可以隐藏未编译的 Mustache（{{}} 就是插值表达式） 标签直到实例准备完毕
```CSS
[v-cloak]{display:none;}
/* 属性选择器 */
```

## v-text
插值表达式值渲染自己的占位部分<br>

v-text没有闪烁问题<br>
v-text 作为属性存在，直接进行内容覆盖
```HTML
<div id = 'app' >
  <p> ++++++{{ msg }}</p>
  <h4 v-text = 'msg'>+++++</h4>
</div>
<!-- 
++++++123

123 
-->
```

## v-html
### 更新元素的 innerHTML，用v-html
```JavaScript
var vm = new Vue({
  el:'#app',
  data:{
  msg:123,
  msg2:<h4>这是一个h4</h4>
  }})
```
```HTML
<div id = 'app' >
  <p v-html = 'msg2'></p>
</div>
```
## v-bind
### 用于绑定属性
```HTML
<div id = 'app' >
  <input type="button" value='put mouse on it' v-bind:title='mytitle'>
</div>
```
效果：鼠标移致button，显示title“test”。
```JavaScript
var vm = new Vue({
  el:'#app',
  data:{ mytitle:'test'}})
```
v-bind简写为英文冒号':
```
v-bind:title='mytitle' => :title='mytitle'
```

而且v-bind里面可以写js表达式，如下
```HTML
<div id = 'app' >
  <input type="button" value='put mouse on it' 
  v-bind:title='mytitle '  + '可以添加语句'>
</div>
```

## v-on
### 用于事件处理


```HTML
<div id = 'app' >
  <input type="button" value='click me' 
  v-bind:title='mytitle' v-on:click ='show'>
</div>
```

```JavaScript
var vm = new Vue({
  el:'#app',
  data:{
    mytitle:'test'
  },
  
  methods:{show:function() {alert('show up')} 
  }})
```
v-on可以简写为@
```
  v-on:click ='show' => @click ='show'
```
v-on可以绑定所有的[DOM原生事件](https://developer.mozilla.org/zh-CN/docs/Web/Events)  

