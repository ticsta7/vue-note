## vue实例的创建

vue控制的这个元素区域就是MVVM中的 v（view）
``` HTML
<div id = 'app' >{{ msg }}</div>
```
vm接收new出来的vue实例是MVVM中的VM调度者
```JavaScript
var vm = new Vue({
  el:'#app',
  data:{
    msg:'vue study note day one'
  }
})
```
里面 data，就是MVVM的 M，用来保存数据 

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

```HTML
<div id = 'app' >
  <p> ++++++{{ msg }}</p>
  <h4 v-text = 'msg'>+++++</h4>
</div>
<!-- 页面显示：
++++++123
123 
-->
```
>插值表达式和v-text的区别<br><br>
1 插值表达式值只渲染自己的占位部分<br>
2 v-text没有闪烁问题<br>
3 v-text 作为属性存在，直接进行内容覆盖

## v-html
> 用于更新元素的 innerHTML
```JavaScript
var vm = new Vue({
  el:'#app',
  data:{
  msg:<h4>这是一个h4</h4>
  }})
```
```HTML
<div id = 'app' >
  <p v-html = 'msg'></p>
</div>
```
## v-bind
>用于绑定属性
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
>用于事件处理


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

### 事件修饰符

#### 1.默认的冒泡机制

```HTML
<div id='app'>
<div class='back' @click='divClick' >
  <input type="button" value='click me' @click='btnClick'>
</div>
</div>
```

```CSS
.back{
  height:150px;
  background-color:darkcyan;}
```

```JavaScript
var vm = new Vue({
  el:'#app',
  data:{},
  methods:{
    divClick(){console.log('触发divClick事件')},
    btnClick(){console.log('触发btnClick事件')}  
  }});
```

![冒泡机制](https://user-gold-cdn.xitu.io/2019/10/18/16ddc00c38d37101?w=468&h=196&f=png&s=9757)
点击btn时，console


>触发btnClick事件
<br>
>触发divClick事件

单独点击div（避开btn）

>触发divClick事件

#### 2. 阻止事件继续传播（冒泡） '.stop'
把上面的代码修改一下
```HTML
<input type="button" value='click me' @click='btnClick'>
//修改成↓↓↓↓↓
<input type="button" value='click me' @click.stop='btnClick'>
```
此时再点击btn，console
>触发btnClick事件

#### 3.阻止默认行为（.prevent）
例：可以阻止`<a>`标签的默认行为（跳转到指定url）

```
  <a href="http://www.github.com/ticsta7" 
  @click.prevent='linkClick'>github</a>
  
  methods:{ linkClick(){console.log('触发linkClick事件')}}
```
效果，只是在console显示消息，但是不跳转

#### 4.捕获（.capture）
>添加事件监听器时使用事件捕获模式 

```HTML
<div id='app'>
<div class='back' @click.capture='divClick' >
<input type="button" value='click me' @click='btnClick'>
</div>
</div>
```
结果与冒泡截然相反
>触发divClick事件
<br>触发btnClick事件

#### 5.无视捕获/冒泡 (.self)
>只在元素自身时触发处理函数
```HTML
<div id='app'>
<div class='back' @click.self='divClick' >
<input type="button" value='click me' @click='btnClick'>
</div>
</div>
```
#### 6. 串联修饰符
例：
`<form v-on:submit.prevent></form>`
<br>而且顺序也很重要

```
v-on:click.prevent.self //会阻止所有的点击
v-on:click.self.prevent // 只会阻止对元素自身的点击。
```

## 双向数据绑定
v-model,在表单 `<input>、<textarea> `及` <select>` `元素上创建双向数据绑定
<BR>M层的能渲染到V层，V层的也能渲染到M层
< BR>(model后面没有冒号)
```HTML
<input type="text" v-model='msg'>
```

##  class的绑定

### 对象语法

```HTML
<div>
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
</div>
```

```JavaScript
data: {  isActive: true,
         hasError: false}
```
结果为`<div class="static active"></div>`

格式-class：Boolean

当然也可以直接把obj写进data里面

```HTML
<div>
  class="static"
  v-bind:class="classObj"
</div>
```
```JavaScript
data: {classObj:{ active: isActive, 'text-danger': hasError }}
```

也可以利用计算属性（[vue官方文档](https://cn.vuejs.org/v2/guide/class-and-style.html)）

`<div v-bind:class="classObject"></div>`

```JavaScript
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### 数组语法
`<div v-bind:class="[activeClass, errorClass]"></div>`

```JavaScript
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
渲染为`<div class="active text-danger"></div>`

还可以直接用三元表达式（想不到吧？！）

`<div v-bind:class="[isActive ? activeClass : '', anotherClass]"></div>`

但是三元表达式可以用对象代替

`<div v-bind:class="[{ active: isActive }, anotherClass]"></div>`

## style的绑定（内联）
1.对象语法
<br>
可用驼峰式  或短横线分隔 (but记得用引号括起来，或者单引号 ) 来命名<br>
`<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
`

```JS
data: {activeColor: 'red',fontSize: 30}
```
当然了，这个obj可以直接写进data里面<br>
<br>`<div v-bind:style="styleObject"></div>`

```JS
data: {
  styleObject: {color: 'red',
               fontSize: '13px'}}
```

2.数组语法
<br>
同时绑定多个样式，baseStyles，overridingStyles，可以是obj

`<div v-bind:style="[baseStyles, overridingStyles]"></div>
`