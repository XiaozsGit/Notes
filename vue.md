# vue框架

**采用MVVM模式（M V VM）**

## 创建vue

- el,  data,  methods,

- 先要引入vue.js ，在引入vue.js之后，在内存中将会存在一个Vue构造函数，可以使用其创建vue对象

``` js
    var v = new Vue({
        el:'.box',  // 确定vue的工作区域
        // 数据
        data: {
            // msg 为随意取得变量名，可以添加到工作区域中
        	// 有以下三种添加方法
            msg: '数据内容'  
        	msg2： '<h2>这是一个二级标题</h2>'
        }
        // 在methods中定义了当前vue实例可用的方法
      	methods:{
            show:function() {...}
    	}
    })
```

##  将data中的数据传入工作区域中的**html**里

  - v-text             v-html                     {{ }}          （v-cloak）

```html
	<!-- 将数据放入工作区域中 -->
	<!-- 方式一 插值表达式闪烁-->
	<!-- 缺陷：在js代码加载较慢时，页面中会先出现{{msg}}，在页面加载好之后才会变为数据内容 -->
	<!-- 解决方案：使用 v-cloak 指令 -->
	<!-- 不会覆盖其中添加的前后内容 ++++数据内容----  会这么显示-->
	<div class="box"  v-cloak >+++ {{msg}} ----</div>

	<!-- 方式二 -->
	<!-- 会覆盖其中添加的前后内容 数据内容  之会这么显示 +++++ -----将会被覆盖 -->
	<div class="box"  v-text ="msg">+++++ -----</div>

	<!-- 方式三 -->
	<!-- 只有 v-html 会解析html格式 -->
	<div class="box"  v-html ="msg2"></div>
```

## 用data中的数据设置html内的属性值

  - v-bind：数据的单向绑定  M  ->   V
  - 缩写   :

  ```js
   var v = new Vue({
      el:'.box',  // 确定vue的工作区域
      // 数据
      data: {
          // msg 为随意取得变量名，可以添加到工作区域中
          // 有以下两种添加方法
       	mytitle: '这是我自定义的title属性'
      }
  })
  ```

  ```html
  <!-- v-bind 绑定  mytitle 可以作为一个变量进行字符串拼接 或者其他的js合法表达式-->
  <img v-bind:title = 'mytitle'/>
  <!-- v-bind 可以简写为一个冒号  ： -->
  <img :title = 'mytitle'/>
  ```

  

## 事件绑定机制 v-on：

  - 缩写     @
  - **this.**
  - 可以有click， mouseover， mouseout 等

  ```html
  <input class='box' v-on:click = show  />
  ```

  ```js
  var vue1 = new Vue({
      el:'.box',
      data: {
		msg:'双向数据绑定'
      }
    	// 在methods中定义了当前vue实例可用的方法
      methods:{
          show: function () {
              alert(123);
      		  alert(this.msg);
          }
      }
  })
  ```
- 要在方法中调用data内的变量时，使用**this.变量名**可以获取

## v-on的事件修饰符

  - .stop                阻止事件冒泡

  - .prevent          阻止默认行为

  - .capture          使用捕获机制

  - .self                只有是自己触发的事件才会执行，冒泡/捕获是不会触发

  - .once             只触发一次

    

  ```html
  <input class='box'  @click.stop = show  />
  <input class='box'  @click.prevent = show  />
  <input class='box'  @click.capture = show  />
  <input class='box'  @click.self = show  />
  <input class='box'  @click.once = show  />
  
  <!-- 阻止事件的默认行为，但因为.once的存在，其只会阻止一次 -->
  <input class='box'  @click.prevent.once= show  />
  ```

## 数据双向绑定v-model

- **唯一的**一个**双向**数据绑定指令

  ```js
  <input class='box'  v-model= 'msg'  />
   var vue1 = new Vue({
       el:'.box',
       data: {
  		msg:'双向数据绑定'
       }
      // 在methods中定义了当前vue实例可用的方法
       methods:{
         show: function () {
            alert(123);
          }
       }
    })
  ```

- v-model 后直接跟数据，没有冒号，可以实现**表单元素**和model中数据的双向绑定；
- 注意：**v-model只能运用在表单元素**中（所以其后不用跟属性名，没有冒号,（猜测：数据直接与value挂钩））

## vue中使用样式

- 方法一：使用 class 类名操作样式











