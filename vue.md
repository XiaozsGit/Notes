# Vue框架

**采用MVVM模式（view   <=>   view-model   <=>   model）** 

使用 vue 要注意不要产生死循环



## 注意

- 因为在vue实例内部获取数据和方法都需要使用 **this. **  所以要特别注意 this 的指向，与箭头函数一起使用，更加灵活
- 注意在某些逻辑过程中是否对数据进行了改变，是否会自动改变视图而产生死循环
- 注意在某些逻辑中改变的数据是否是 data 中定义并绑定了的数据，有些方法会返回一个数据，而我们会对返回的数据进行操作，所以不会自动触发 视图更新









## vue 的生命周期

- 创建 => 渲染=> 数据更新 => 销毁
- 对应的事件方法
  - beforeCreate   created
  - beforeMount    mounted  
  - boforeUpdate updated 
  - beforeDestroy destroyed
- 在这些函数内部添加 事件处理逻辑，可以对接口数据初始化，等（在vue实例中定义了初始化函数后，没有直接的环境来调用该方法，vue实例中都是键值对，不能直接加括号删除，在外部使用 vm.方法 又有点不伦不类，所以可以在生命周期的方法中调用）





## 实例选项

**注意哪些加  - s -**

- el：确定当前vue 实例所管理的视图，注意不要让其直接管理 body 或 html ，只匹配满足条件的第一个视图结构
- data：响应式数据，数据变化=> 视图变化，
- methods：注意不能直接定义箭头函数，会影响this 指向
- filters：局部过滤器
- computed：计算属性
- watch：监听
- components：局部组件
- directives：局部自定义组件



## Vue内置对象

```js
// Vue 内置对象
Vue.$refs
// 操作dom元素，标签元素上要有 ref 属性

Vue.$router
// VueRouter的实例,相当于一个全局路由对象，许多路由属性和子对象 
// history 对象
// push go 

Vue.$route
// 相当于当前正在跳转的路由对象，是具体的哪一个路由，可以获取路径上的信息
// name,path,params,query

Vue.$emit 
// 抛出一个自定义类型的事件，而且是已经触发了的
// 触发了一个自定义类型的事件

Vue.$on
// 在代码中添加监听事件，可用在 EventBus 中

Vue.$slot
// 关于插槽的内容都在这里面

// ------------------------------------------------------------------------------
vm.$data 可以访问到原始数据对象 
vm.$data.a       等价于     vm.a  或者  this.a (前提是this的指向是正确的)


Vue.$nextTick()

```





## 全局注册

Vue.全局注册命令('注册名称'， 函数名称)





## 创建vue

- 先要引入vue.js ，在引入vue.js之后，在内存中将会存在一个Vue构造函数，可以使用其创建vue对象

``` js
	<div class="box">
    	{{ msg }} // 会显示数据 ，插值表达式
    </div>
    <div class="box">
    	{{ msg }} // 不会显示数据，直接显示 {{msg}} 因为vue只会绑定一个视图
    </div>
	var vm = new Vue({
        
        // 确定vue的工作区域，各种的选择器
        // 可以传入一个dom元素
        // 只会匹配一个，有多个html也不会显示
        el: '.box',  
        // el: document.getElementById('id');
        
        
        // 数据
        data: {
            // msg 为随意取得变量名，可以添加到工作区域中
        	// 有以下三种添加方法
            msg: '数据内容'  
        	msg2： '<h2>这是一个二级标题</h2>'
        },
        
        
        // 在methods中定义了当前vue实例可用的方法
      	methods:{
        	// 使用键值对的方式 key:value
            show : function() {
        		// 在此处调用data中的数据需要加 this 
        		this.msg = '获取数据内容';
    		}
    	}
    })
    
    
    // 获取数据 两种方式
    // 1 - $ 与内部的做一个区别
    vm.$data.msg

	// 2 - vm 中自动 代理 了 data 中的数据
	vm.mag


	// 调用方法
	//  vm 中自动 代理 了 methods 中的方法
	vm.show();
	// 插入到插值表达式中
	{{ show( ) }}
```



- **注意** ：因为 vue 的实例vm 中会自动代理 data 和 methods 中的数据和方法，所以在 data 中的数据 **不能** 与 methods 中的方法重名
- 箭头函数要慎用，直接在methods 中使用箭头函数时，this 的指向将不再是 vue 实例，但可以在其内部的函数中进行嵌套（如果需要的话）
- 注意数据的双向绑定，当数据发生变化时，将会重新执行视图更新，又会使数据发生变化，可能会产生死循环

```js
var vm = new Vue({
    el:'.box',
    data:{
		name:'123456789'
    },
    methods: {
        // fn1 不能是箭头函数，但其内部可以有，
		fn1 : function () {
    		console.log('1'+this);
            // 内部可以嵌套箭头函数
    		var jiantou = () => { console.log(this)};
    		jiantou();
		}
	}
})  
```





## 插值表达式

- {{  为所欲为  }}
- 可以在插值表达式内进行许多操作，逻辑判断，运算，调用方法，**三元运算 ？ ：** 
- 不能 （if else ）（循环） （定义 var ） 



**注意**：在插值表达式中的所使用到的数据一旦发生变化，视图就会跟着变化，一个 **单项绑定** 可以把 {{ }}作为单项绑定的关键词，只能使用在标签中，

​		属性的的数据绑定要使用 v-bind: 或 v-model 因为属性内不能使用 {{  }} 。



##  指令

- 指令使用 “  ”，其内部的变量直接写，字符串要使用‘’（相当于把“”当作了{ } ，作为其内部运行环境）

### 设置内容

- v-text          不解析标签
- v-html         可以解析标签
-  v-cloak       解决闪烁问题



```html
	<!-- 将数据放入工作区域中 -->
	<!-- 插值表达式 -->
	可以存放多个 {{  }}  {{  }}

	<!-- 方式一 插值表达式（闪烁）-->
	<!-- 缺陷：在js代码加载较慢时，页面中会先出现{{msg}} 字符串，在页面加载好之后才会变为数据内容 -->
	<!-- 解决方案：使用 v-cloak 指令 -->
	<!-- 不会覆盖其中添加的前后内容 ++++数据内容----  会这么显示 -->
	<style>
        [v-cloak] {
			display:none;
        }
	</style>	
	<div class="box"  v-cloak > +++ {{msg}} ---- </div>


	<!-- 方式二 -->
	<!-- 会覆盖其中添加的前后内容 数据内容  之会这么显示 +++++ ----- 将会被覆盖 -->
	<div class="box"  v-text ="msg">+++++ -----</div>



	<!-- 方式三 -->
	<!-- 只有 v-html 会解析html格式，以及其内部可能存在的js代码 一般不用，容易被攻击 -->
	<div class="box"  v-html ="msg2"></div>
```





### 条件渲染（显示/隐藏）

- v-if                           * 通过 增加/删除 元素来控制 显示/隐藏
- v-show                    * 通过使用display 来控制元素的 显示/隐藏
- **要求布尔值 true/false** 
- 在使用的同时注意一下两者使用效果，二者在效果上是一致的，因此该关注开销，`v-if` 有更高的**`切换开销`**，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。如果 切换频繁 `v-if` 开销更大  

- **注意：** v-show 只是设置组件的显示/隐藏，并不会销毁，数据还在，也就是说他内部的created钩子函数只会执行一次，当遇到组件要复用的时候可能不能刷新数据
- v-if 会销毁组件，再重新创建，每次服用都会执行created函数

```php+HTML
<!-- 但要对许多元素进行显示隐藏时，可以加一个父元素 -->
<!-- 方式一 -->
<div v-if="isshow">
    <p>{{msg}}</p>
	<p>{{msg1}}</p>
    <p>{{msg2}}</p>
    <p>{{msg3}}</p>
</div>
// 通过控制 isshow = true/false 来控制显示/隐藏

// 缺点：会增加一个多余的 div 标签（他只是为了可以整体设置而存在的）


<!-- 方式二 -->
// 使用 template 标签，在浏览器渲染之后并不会有 <template> 标签出现
<template v-if="isshow">
    <p>{{msg}}</p>
	<p>{{msg1}}</p>
    <p>{{msg2}}</p>
    <p>{{msg3}}</p>
</template>    
```







### 数据绑定（属性）

### * 单向绑定 v-bind：

- v-bind：属性名= “变量名”           数据的单向绑定  M  ->   V 数据影响视图	

- 缩写   ：属性名= “变量名”

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



- **注意：** `v-bind` 时，只有 `null`, `undefined`，和 `false` 被看作是假。`0` 和空字符串将被作为真值渲染



### * 双向绑定 v-model

- 只能在表单元素中使用（只有表单才可以改变视图）

- v-model 会忽略所有表单中的 value，checked，selected 属性，其初始值应该在v-model中设定

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

- 注意：**v-model只能运用在表单元素**中（所以其后不用跟属性名，没有冒号,（猜测：数据直接与value或checked或celected挂钩，看这个元素标签主要是什么在起作用））



- ## v-model 绑定表单元素

  - 表单元素:  input  textarea checkbox radio  select 

  - **注意**  checkbox在input标签中需要给定value值（单个/多个）(需要根据value来判断该复选框是否选中) 从数据操作视图的方向考虑
  - 所有表单元素一旦绑定了 v-model  就会忽略掉 原有的value值 checked值 selected值  需要从数据对象中取默认值

```html
    <div id="app">
        
      <!-- input -->
      <p>{{ name }}</p>
      <input v-model="name" type="text" />

      <!-- textarea -->
      <p>{{ nameTextArea }}</p>
      <textarea
        v-model="nameTextArea"
        name=""
        id=""
        cols="30"
        rows="10"
      ></textarea>
        
      <!-- checkbox 单个的checkbox -->
      <p>{{ boolCheckbox }}</p>
      <input type="checkbox" v-model="boolCheckbox" />食堂好吃吗
      <p>{{ arrCheckbox }}</p>
        
      <!-- 多个的checkbox  绑定的值是个 数组 数组里面是选项 -->
      <input type="checkbox" v-model="arrCheckbox" value="字节跳动" />字节跳动
      <input type="checkbox" v-model="arrCheckbox" value="华为" />华为
      <input type="checkbox" v-model="arrCheckbox" value="腾讯" />腾讯
      <input type="checkbox" v-model="arrCheckbox" value="阿里" />阿里
        
      <!-- radio  单选  男/女 -->
      <p>{{ radio }}</p>
      <input type="radio" v-model="radio" value="男" />男
      <input v-model="radio" type="radio" value="女" /> 女
        
      <!-- select -->
      <p>{{ select }}</p>
      <select name="" id="" v-model="select">
        <option value="京东">京东</option>
        <option value="字节跳动">字节跳动</option>
        <option value="美团">美团</option>
      </select>
    </div>


    <script>
      var vm = new Vue({
        el: "#app",
        data: {
          name: "狗剩儿",
          nameTextArea: "我textarea的内容",
          boolCheckbox: false,
          arrCheckbox: ["字节跳动"],
          radio: "女",
          select: "京东"
        },
        methods: {}
      });
    </script>

```



### 绑定事件 v-on：

- **v-on:事件名.修饰符 = '方法名'** 

  - 缩写     @
  - **this.** 
  - 可以有click， mouseover， mouseout 等 会自动传入 一个 event
- 匿名传参  =>当只写 方法名时 =>  方法中默认第一个参数就是event(事件参数)
  - 显示传参 => 当写  方法名() => 如果想要获取event,必须显示的用$event传到方法里
  
  ```js
  <input class='box' v-on:click =“show”  />
  <input class='box' @click =“show”  />   
  // 也可以是一个语句
  <input class='box' @click =“msg += ‘添加’ ”  />   
  
  var vue1 = new Vue({
    el:  '.box',
      
    data: {
  	msg : '双向数据绑定'
    }
      
    // 在methods中定义了当前vue实例可用的方法
    methods:{
    	show: function (event) {
        alert(123);
        alert(this.msg);
    	}
    }
    
  })
  ```



  ```js
  
- 要在方法中调用data内的变量时，使用**this.变量名**可以获取

- 要在方法中传入参数时直接加括号加参数


  // 方法 click 事件 
  // 匿名传参：只写方法名时，默认的第一个参数是 event
  
  // 显示传参：写方法名加括号时，可以使用 $event 来传入 event 若不写，则会覆盖，与顺序无关
  <input class='box' v-on:click =“show”  />
   
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

  





### 事件修饰符-5种

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





### 循环指令 v-for 

- for in 
- for of 
- 要循环谁，就在谁上加，并不是在其父元素上
- 在众多指令中，v-for 会出现多出来的变量  value item key index ，这几个变量在v-for 中定义了，那么 就可以被其他的指令使用
- 只要循环特定项时（如：从第二项开始循环）可以对数组进行一些操作，而且可以考虑是否使用操作原数组的方法，也可以使用返回一个新数组的方法

```js
// 使用方法
v-for="元素 in 容器(数组和对象)"
v-for="对象中的属性值 in data中的对象名"

// 数组
v-for = "(item, index) in items"

// 对象
v-for = "(value, key, index) in items"

```



```js
// for in
<li v-for="item in list">{{item}}</li>

// 从第二项开始
<li>{{第一项不想循环，写死的一个}}</li>
<li v-for="(item, index) in list.slice(1)">{{ item }}</li>

// for of
<li v-for="item of obj">{{item.name}}:{{item.age}}</li>

var vm = new Vue({
    el:'.box',
    data:{
		list:[1,2,3,4,5],
        obj:[
            {name:x,age:20},
            {name:z,age:21},
            {name:s,age:22}
        ]
    }
});

```

- v-for  key

```html
<!-- v-for 
  key属性: 值通常是一个唯一的标识
  key是一个可选属性
  养成好习惯:建议在写v-for时 设置:key="唯一值"
-->
<ul>
  <li v-for="(item,index) in list" :key="index">{{item}}---{{index}}</li>
</ul>
```

- template 标签

指挥在编译器中显示，渲染到浏览器中时会消失，但不想添加多余的结构时可以使用



### v-if 加 v-for

- 二者内存在优先级关系，否则在 v-if 中 使用 v-for 定义的变量 value 等会报错

```html
<p v-if="index>1" v-for="(item,index) in list"></p>
```

```js
// 了解v-if 和v-for的层级关系及使用

// v-for循环元素时,标签可使用item属性, 如果这个时候用v-if来进行操作 会产生什么效果?

// 以上代码执行: 会将数组中前两个元素忽略掉

// 说明一个问题: v-for 的优先级大于v-if ,所有v-if才能使用v-for的变量
```



### v-once

- 作用: 使得所在元素只渲染一次  
- 场景:静态化数据 



## 自定义指令

- 有时需要对一个普通的DOM元素进行操作，需要用到DOM元素
- Vue.directive("名称",{inserted:function(使用了指令的DOM ,  指令后的表达式 ){ } });
- directives:{}



### 全局自定义指令

- 指令名称要全小写，不加 v-，在调用的时候加 v- 调用

```js
Vue.directive("focus", {
    // dom 是使用了v-focus 该自定义指令的dom元素
    // expression 是 自定义指令 后跟的表达式
        inserted(dom,expression) {
          dom.focus();
        }
 });
```



### 局部自定义指令

```js
directives: {
    // 自定义指令，可以在这里定义多个
    focus: {
        inserted(dom,expression) {
        	dom.focus();
        }
	}，
    
    focus2: {
        inserted: function(dom,expression) {
        	dom.focus();
        }
	}
} // 局部自定义指令实现
```





## 过滤器

- 场景: data中的数据格式(日期格式/货币格式/大小写等)需要数据时  等
- 使用位置:{{}}和v-bind="表达式 | 过滤器名称"
- 具体用法:{{msg | 过滤器名字}}
- 传参：过滤器可以有多个参数，但是第一个参数一定是 | 前传进来的
- 串行使用：可以有多个  |  |  |  前一个处理后的结果传给下一个过滤器



### 全局过滤器

- 在new Vue **上面/之前**  Vue.filter('名称'，函数)

```js
// 语法： Vue.filter('该过滤器的名字', (要过滤的数据) => { return 对数据的处理结果 });

Vue.filter("toUpper", function(value) {
    
	return value.charAt(0).toUpperCase() + value.substr(1);
    
});// 过滤器核心代码
```

- 使用：在视图中通过 {{ 数据 | 过滤器的名字 }} 或者 v-bind 使用过滤器
  - {{ 数据 | 过滤器的名字 }} 
  - v-bind:属性="表达式 | 过滤器名称"

```js
<p>{{ text | toUpper }}</p>  // 常规用法

<p> {{ text.split("").reverse().join("") | toUpper }} </p>  // 将字符串翻转
```



### 局部过滤器

- 在Vue实例上 的选项上 filters：{名称： 函数}  （在实例选项中使用 key=>value 的方式）

```js
filters: {
    // 懒人写法1
    toUpper(value) {
      return value.charAt(0).toUpperCase() + value.substr(1);
    }
}
```





## Watch

Vue 实例选项

可以用于监听个别数据，数据一变化就会更新视图

- 合理利用，可以简化代码，数据发生变化可能因为多种方式，而如果要对变化后的数据进行同一类操作的话，不是用 watch 的话就要在每一种是数据变化的方式中添加操作代码，而可以直接监听这个数据，当他发生变化就进行操作，不关心是因为什么而引起的变化（分离思想无处不在）

```js
// watch 的使用方法
watch: {
	// 数据名称: function(newdata, olddata) {}
    
    data (newdata, olddata) {
        
    }
}
```





## 计算属性

- 比methods 中的效率高，计算属性会把数据缓存起来，如果数据没有变化，则会从缓存中取
- 当 **data** 中无法完成复杂逻辑时,通通可以在**计算属性**中实现，计算属性主要是作为数据的强化，使用methods有点得不偿失，所以使用计算属性，局限性在于一定要有返回值，返回给使用者
- 调用不需要加括号 （）
- 因为return是必须的，所以计算属性内**不应该存在**任何 **异步操作**
- computed:{ (key)计算属性名: **`带返回值`**的函数 }



```js
// 必须带有返回值
computed: {
    nameReverse() {
        return this.name
        .split("")
        .reverse()
        .join("");
    }
} // 定义计算属性
```







## ref 操作 DOM

- 先给元素定义 ref属性, 然后通过$refs.名称 来获取dom对象
- 注意**不要在 created **中使用，因为此时 dom 元素还没有渲染出来，找不到 dom

```js
// 先定义 ref
<input type="text" ref="myInput" /> // 定义ref
    
focus() {
    
    // 获取dom对象 聚焦
	this.$refs.myInput.focus();
}
```





## vue 样式class-style

- 方法一：使用 class 类名操作样式
- 方法二：使用 style 操作样式

- 绑定class对象

```js
// :class= "{ class名称": 布尔值 }"
// :class= " 对象变量 "

<p :class="{left: ifShow}">内容</p>
// 绑定class和原生class会进行合并 不会覆盖
```

- 绑定class数组

```js
// :class= "[class变量1,class变量2..]"
// :class= " 数组变量 "

<p :class="[activeClass,selectClass]" class="default">内容</p>
```

- 绑定style对象

```js
// :style="{css属性名: 变量}"
// :class= " 对象变量 "

<p :style="{fonSize:fontsize}" >内容</p>
// font-size要写成 fontSize  使用驼峰命名规则
```

- 绑定style数组

```js
// :style="[对象1,对象2...]
// :class= " 数组变量 "
// 数组内对象可以是多个属性的集合

<p :style="[{ },{ }]" class="default">内容</p>
```



## Axios 

**第三方插件，在所有的环境框架下都可以使用** 既可以在浏览器端又可以在node.js中使用的发送http请求的库，**`支持Promise`**，不支持jsonp，如果遇到jsonp请求, 可以使用插件 `jsonp` 实现



- 引入 axios 包，**返回一个promise 对象**，可以直接 .then() .catch()

- post:               * 新增                     成功 code： 201
- get:                 * 获取                     成功 code： 200
- put:                 * 修改                     成功 code： 200
- delete:            * 删除                     成功 code： 200



```js
// axios 使用方式 
axios.post('url', {data}).then(处理程序);

axios.get('url', {data}).then(处理程序)；

axios(
	data: 数据,
    method: 请求方式 -- post, get, put, delete ...
).then(
	// 处理函数 data 返回的数据
    function (data) {

    }
)
```

注意是否要使用箭头函数，在then中使用箭头函数可以跳一层，更加快速的访问到vue实例中的数据和方法 直接 this. 就可以了



## async-await

- 一种新的解决同异步操作的方案
- await 后必须是一个 promise对象，只有在对象中 resolve 了之后才会继续执行，如果是reject了，那么要在外部包一层try/catch 块，会进入到 catch中，默认有一个 error 对象

- 使用 async 包裹 await ，不会造成代码卡死

```js
// 源代码
function init () {
    axios({
        url: '/home',
        data: {name: '张三'}
    }).then(result = {
        axios({
            url: '/home',
            data: {name: '张三'}
        })
    }).then(result1 = {
		console.log(result1)
    })
}

// 使用 async-await 之后
async function init () {
    await axios({
        url: '/home',
        data: {name: '张三'}
    })
    let result1 = await axios({
        url: '/home',
        data: {name: '张三'}
    })
    console.log(result1)
}
```

- **注意**：不要滥用 await 

- 如果有一个异步操作，他的后续操作并不会影响到其他操作的时候，可以把它放到 await 之前去执行，把需要同步进行的操作以及与他有关联的操作放到最后





## 组件

- 组件特点: 组件是一个**`特殊的 Vue实例`** 
- 有：data/methods/computed/watch
- 没有：el（使用template结构替代）template属性(一个字符串，内部是一个html结构，只有**一个根节点**)
- data 是一个**`带返回值的函数`**，而不是对象，函数的返回值是对象，分割了各个组件，形成了独立的作用空间



- 全局组件：
  - Vue.component("组件名" ,   {各种组件属性});



- 局部组件：
  - components:{"组件名" ：{ } }



> 和实例相似之处:   data/methods/computed/watch  等一应俱全  
>
> - vue实例有**`el`**选项  组件实例没有el 但是**`templete`**页面结构
>
> **注意** 值得注意的是  data和Vue实例的区别为 组件中data为一个函数   没有el选项 
>
> template 代表其页面结构 (**`有且只要一个根元素`**)
>
> 每个组件都是**`独立`**的 运行作用域  **`数据 逻辑没有任何关联`**
>
> data () {  return {  数据属性 } }



### 全局组件

```js
// 全局组件的定义
Vue.component("组件名", {
    // template: `html标签`,
    // template 中只能有一个根节点，在这个例子中，ul 是必须的，否则就有三个li标签节点，不符合要求
    template : 
    `<ul>
		<li></li>
        <li></li>
        <li></li>
	</ul>`,
    
    // 数据
    data() {
        // 在返回的对象中存放数据
      	return {
            name: 123
        }  
    },
    
    // 方法
    methods: {
        
    },
    ...
});
```





### 局部组件

```js
// 局部组件的使用

new Vue({
    
    components: {
		"组件名": {
            // 模板
            template: 
            `<div>
           		{{count}}
            </div>`,
            
            // 数据
            data() {
                return {
              	 	count: 1
                };
            }
        }
    }
    
})

// 在省略双引号进行定义的时候，组件名使用 - 会报错
```



- ### scoped

  - 普通的样式会作用的子组件的根节点上（如果加上了 >>> 或 /deep/  则会加到子组件的内部,根据你的class，而且必须从根节点出发/组件 class 还是组件的根节点 class 都可以）
  - 注意：样式是属于HTML标签的，组件是不存在 class 属性的，所以在组件上添加的 class 会添加作用到组件的根节点上，效果是一样的

```html
<!-- 父组件 -->
<template>
    <!-- 组件 class abc -->
    <foo class="abc"></foo>
</template>
<style scoped>
    .abc {
        // 组件的类名
		// 自定义样式
        ...
    }
    .foo {
        // 组件中根节点的类名
		// 自定义样式
        ...
    }
    // 这两种写法的效果是一样的，可以作用到根节点
</style>

<!-- 子组件组件 foo -->
<template>
    <!-- 根节点 class foo -->
    <div class="foo">我是 foo 子组件</div>
</template>
```







## 组件嵌套

- 我们可以在new Vue()实例中使用自定义组件
- 也可以在注册自定义组件时,嵌套另一个自定义组件,也就是父子组件的关系

```js
var comA = {
	template: `<div>我是子组件</div>`
};

// 在另一个自定义的组件中嵌套组件
var parentA = {
    template: 
        `<div>
            我是父组件
            <com-a></com-a>
        </div>`,
    
    components: {
        'com-a': comA 
    }
};

var vm = new Vue({
    el: "#app",
    data: {},
    methods: {},
    
    components: {
    	'parent-a":parentA
    }
});
```





## 组件间传值

### 父 => 子

- 1 - 在子元素的标签上绑定数据
- 2 - 在子元素中使用 props 局部组件的实例属性接受，数组形式
- 向子组件中传递数据，可以通过 **自定义的子元素组件** 作为一个**中转** 

```js
// 父元素向子元素组件传值
<div id="app">
    // 以自定义的子组件作为一个中转
	<son :f_name = "name"></son>
</div>

Vue.component("son", {
    template: `<li>{{f_name}}</li>`,
    
    // 可以接受多个属性变量，也可以使用对象接收，数组接收起来简单些
    // 注意 f_name 的使用，直接 ** this.f_name ** 就可以使用
    // 是不是 ** 只读属性 ** 存疑
    props: ["f_name"]
});

var vm = new Vue({
    el: "#app",
    data: {
        name: "狗蛋",
        list: [1, 2, 3]
    },
    methods: {
    }
});
```



### 子 => 父

- 在子组件中的数据要传递出来，可以通过 **自定义的子元素组件** 作为一个**中转** 
- 使用 $emit 方法可以抛出一个自定义事件，并且已经触发了这个事件



> $emit 函数的解析
>
> $emit(自定义函数名，传出去的参数（可以多个）)
>
> 由于 $emit 是一个方法，只有执行了这个 $emit 才会抛出并触发自定义事件，但在子组件内没有直接的运行环境（子组件是一个对象，其内部都是键值对），只能嵌套在另一个函数体内去执行这个函数
>
> 另类的理解：调用这个方法就相当于鼠标点击了一下（或其他的事件触发了），只是触发的是一个自定义的事件类型，若有这种事件类型的监听，则触发该监听函数
>
> 监听这个事件若是



```xml
// 子元素给父元素传值
<div id="app">
	<son @zidingyi="getSon"> {{  }} </son> 
	<div>{{f_name}} + '132'</div>
</div>

Vue.component("son", {
   	template: `<div @click="tothrow"></div>`,
    
    data() {
        return {
        	name: "狗蛋儿"
        }
    },
    
    methods: {
        tothrow() {
			this.$emit('zidingyi',this.name);
        }
    }
});

var vm = new Vue({
    el: "#app",
    
    data: {
		f_name: ''
    },
    
    methods: {
		getSon(value) {
			this.f_name = value;
		}
	}
    
    
});
```





## 单页应应用 - SPA

- single page application 
- 速度快，首屏慢，
- 切换模块时不引起页面的刷新，
- 刷新时，依旧停留在当前模块
- hash值          锚链接                 #





## 路由

- vue-router
- 在页面内使用路由一共有六个步骤
  - 1 - 引 vue vue-router包
  - 2 - 设置html结构
  - 3 - 定义组件
  - 4 - 实例化路由对象，配置路由规则
  - 5 - 在vue实例中挂载vue-router 实例





## 嵌套路由

- 所谓的二级路由的要点在于提前放置一个路由视图，只点击进入一级路由时，会由后台js代码跳到二级路由中，这个二级路由对应的组件放在哪里呢，就放在提前放置的路由视图中



- 获取路由地址中的变量, $route.params(要查阅修改)
- 使用children属性设置二级路由表，二级路由表中 开头不带 /    ‘’ 表示默认的二级路由地址
- tag 控制 route-link 渲染成什么标签
- linkActiveClass: "active", // active为bootstrap中的 一个class样式



this.$route 可以拿到一些数据

编程路由导航， this.$router 有一些方法，与this.$route 区分开 一个 r 的区别

go

push

replace

```html
<!-- 路由会激活样式，可以自己设定样式表 -->
<a href="#/news" class="router-link-exact-active router-link-active"></a>
```







to 属性的赋值 '/path' '变量' name: 'path' path: '/path'



rediretc 重定向



动画，一定要有显示/隐藏



vue-cli 中统一引入 axios   main.js 下统一引入

import axios from 'axios'

Vue.prototype.$http =  axios;

// $http 为自定义名称 因为 vue组件中 的变量多用$ 所以加一个$



统一设置请求 地址头 baseURL（在之后的请求中不再写 http://localhost:3000）

axios.defaults.baseURL = "http://localhost:3000"





<slot></slot> 插槽，可以设置默认内容，一个标签代表一份









slot 标签使用 name 属性就变成了一个具名插槽，可以有后备内容

使用 slot 属性来确定是那个具名插槽，默认内容若是没有指定是那个插槽，那么会给匿名插槽

slot 是多对一的，可以有多个标签的 slot 属性是一样的，他们都会放到同一个具名插槽中

具名插槽是否可以有 



导航守卫 请求 判断是否有token 



请求拦截，统一注入 token 在请求到达后台之前



响应拦截





Vue.use（对象） 的原理：会执行对象内的 install 函数，然后这个函数内就会产生一个可以调用别的时间方法的环境，如果有东西需要同时使用到 Vue 和 某一对象，可以使用这种方式 然后只 使用 Vue.use(对象)



```js
// 通常
// main.js 文件中
import Axios default 'axios'
Vue.prototype.$axios = Axios;


// 变化
// main.js 文件中 
import Axios default '/axios.config'
Vue.use(Axios)

// axios.config.js 文件中
import Axios default 'axios'

export default {
	// install 中会自动传入一个 Vue 对象
	install(Vue) {
		Vue.prototype.$axios = Axios;
    }
}

```







# Element-UI



this.$message

栅格为 24 份

表单验证 rules prop medol

validator function (rule, value, callBack) {} // 这里的callBack有点不同

validate



访问令牌 替代了 session ，用户太多，服务器不再为用户保存身份信息，浏览器自行保存到缓存 token



flex 布局 

elementui 中设置了大量的属性以替代自己设置css样式，碰到需要修改默认样式的地方，先看看elementui 中是否为他设置了属性以达到一样的效果



导航（路由，index）



注意分离组件的时候，不要把elementui 定义的容器分出来了，把组件放到布局里，不要破坏了布局

```html
// 默认布局
<el-container class="main-container">
    <el-aside>侧边栏</el-aside>
    <el-container>
      <el-header >头部</el-header>
      <el-main >主页</el-main>
    </el-container>
  </el-container>
// 要做一个 侧边栏的组件，不要把 el-aside 拿出来放到组件中，要定义组件放到 el-aside 标签内
<el-aside>
    <!-- 把组件放到这 -->
    <zidingyi-aside></zidingyi-aside>
</el-aside>

<!-- 不建议这种方式 -->
<zidingyi-aside>
	<el-aside></el-aside>
</zidingyi-aside>

```



# Webpack 

统一打包工具

可以统一降级/转码处理

模块化开发（import）

对img图片进行字符串转换（base64）把图片文件转为字符串嵌入到html中，节省http请求开销







loadsh 函数库

- 防抖/节流