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
``` 