# JS高级(ES5)

## 构造函数

- 在ES5 中的 创建 对象有三种方法

  - 使用字面量直接创建
  - 使用 new 
  - 自定义一个构造函数

  ```js
  // 第一种
  var obj1 = {
      name : '张三丰',
      age : 26 
  };
  
  // 第二种
  var obj2 = new Object();
  obj2.name = '张三丰';
  obj2.age = 26;
  
  // 第三种
  function Hero (name, age) {
      this.name = name;
      this.age = age
      return {
  		name,
          age
      }
  }
  
  var obj3 = Hero('张三丰', 26);
  ```



## 静态、实例

- 静态成员

  - 构造函数本身也是一个对象，所以它也可以使用 obj.xxx = xxx  来定义其对象属性，这种称为 静态成员（属性与方法）属于构造方法本身的，new的对象不可以访问
- 实例成员

  - 在构造函数内使用this.xxx 创建的，实际上是new了一个实例对象并且对 实例对象.xxx 的操作，该属性，方法是存在于new 出来的实例对象中的，构造函数不可以访问
- **注意这两个对象不可以混淆的调用，只能各调各的**





```js
    function Gouzao (name) {

        this.name = name;
        this.say = function() {
            console.log('我是实例成员!');
        }	
    }

    // 静态成员
    Gouzao.name = '这是静态成员';

    // 实例成员
    var obj = new Gouzao('实例成员');
    obj.name;
    obj.say();
```





## 原型与原型对象

- 原型                实例对象的属性     fn.__ proto__
- 原型对象         构造函数的属性     Fn.prototype
  - 注意两个调用不可以混淆了

- constructor      指向构造函数，由__ proto__ 或者 prototype 调用，
  - __ proto__.constructor
  - prototype.constructor
  - 其实 constructor 是 prototype 中的属性，但是因为 __ proto__ 可以指向 prototype，所以 __ proto__ 也可以调用



### 原型对象 prototype

- 这是构造函数的一个属性，该属性值为一个对象 所以可称为是 原型对象
- 每个构造函数都有一个prototype 属性





- 原型对象的作用：可以节省内存空间，但一个方法或变量在所有的同类型对象中都要 使用 或 者拥有 时，可以将其放在其原型对象中，这样只需要创建一个函数就好，但实例对象需要使用方法，而其自身又没有时，就会去他的原型对象中查找，如果有就可以执行了，如果没有就继续往上找，直到找到了或者到达顶部（Object）却依旧没有而返回 is not defined

### 原型 __ proto__

- 每一个对象都拥有原型 __ proto__
- 对象可以调用其原型对象中的方法就是因为在每一个对象中都有一个 原型 __ proto__ 其指向 prototype 
- 该属性 只读 不可赋值 设置
- __ proto__ 对象原型和原型对象prototype 是等价的
- __ proto__ 对象原型的意义就在于为对象的查找机制提供一个方向，或者说一条路线，但是它是一个非标准属性，因此实际开发中，不可以使用这个属性，它只是内部指向原型对象prototype



## constructor

- 原型（__proto__）和构造函数（prototype）原型对象里面都有一个属性constructor属性，constructor 我们称为构造函数，因为它指回构造函数本身。constructor 主要用于记录该对象引用于哪个构造函数，它可以让原型对象重新指向原来的构造函数。一般情况下，对象的方法都在构造函数的原型对象中设置。如果有多个对象的方法，我们可以给原型对象采取对象形式赋值，但是这样就会覆盖构造函数原型对象原来的内容，这样修改后的原型对象constructor  就不再指向当前构造函数了。此时，我们可以在修改后的原型对象中，添加一个constructor 指向原来的构造函数。

## ES5中继承

- 第一种（有bug）

  ````js
  function Father () {
  
  }	
  Father.	say : function() {
  	console.log(我姓萧);
  }
  
  function Son () {
  	
  }
  
  // 现在 Son 想继承 Father 的say 方法
  Son.prototype = Father.prototype
  
  // 把父亲的原型赋值给子 可以实现子元素继承父元素的方法，但是，修改了子元素的原型后，父元素的原型也会跟着改变， 引用传值，
  ````



- 第二种（改良）

  ```js
  // 由于父元素的实例对象也可以访问父元素原型内的所有方法，所以，可以把父元素的实例对象作为子元素的原型
  function Father () {
  
  }	
  Father.	say : function() {
  		console.log(我姓萧);
      }
  
  
  function Son () {
  	
  }
  // 把父元素的实例对象赋值给子元素的原型
  Son.prototype = new Father();
  
  // 修改子元素的构造函数
  Son.prototype.constructor = Son;
  ```

  





# 改变this

- 改变内部函数的 this 的指向是一个很常见的情况，可以用于有一个对象调用另一个对象中的方法且该过程中对 this 有运用，需要时刻保持 this 的指向

  ## 三种方式

- 这三种方式都是由 要执行的函数调用的，可以理解为正常的函数调用过程，只是因为要改变this指向，所以额外调用了一个方法

- **.call()**

  ```js
  fun.call(thisArg, arg1, arg2, ...)
  
  // thisArg：在fun 函数运行时指定的this 值
  
  // arg1，arg2：传递的其他参数
  var obj1 = {
  	fn : function(name) {
      	console.log(this + name)
  	}
  }
  var obj2 = {
  	name : this,
      
      fn2 : obj1.fn(this, this.name)
  }
  obj1.fn.call(obj2, obj2.name)
  
  ```

- **.apply()**

  ```js
  fun.apply(thisArg, [arg1, arg2, arg3 ....])
  // 必须以数组方式传入参数，其他的与 call一样
  
  Math.max.apply(this,[1,2,3,4,5]);
  ```

  

- **.bind()**

  bind() 只会改变this的指向，不会调用，要想调用，需要赋值给一个变量再使用该变量进行调用

  ```js
  fun.bind(thisArg, arg1, arg2, arg3 ...)
  
  var btn = document.querySelector('input');
  
  	btn.onclick = function () {
  		this.disabled = true;
          
  		window.setTimeout(function () {
  			this.disabled = false;
  		}.bind(btn) , 2000);
  }
  ```







# ES6

- 类（class）继承更加方便
- let 和 const

- 箭头函数
- 模板字符串





## 类-构造函数

```js
// 使用ES6 建立构造函数
class Father {
    // 构造函数不带括号 传参传入 constructor 中
    // 两个函数之间不能使用逗号分隔，可以用分号 一般不用
    
    // constructor 构造器
    // 只要调用了构造函数，这个构造器就会自动执行
    constructor (uname, uage) {
        // 主要存放一些公共属性
        this.uname = uname;
        this.uage = uage;
    }
    
    // 方法 不需要带 function 
    fn () {
		console.log(123);
    }
    
    sing() {
		console.log(666);
    }
}
```



## 继承

- extends

  ```js
  // 在ES6中，实现对象的继承可以直接调用 extends 方法，还有super 方法
  
  ```


## let / const

- let/const 可以产生块级作用域(只要是含有一对大括号内的区域)
- let/const 不允许重复定义（即便是 let a ；var a；）



- 使用 let 将不存在变量提升
- let 会有暂时性死区
  - 在外部有一个全局变量a，在一个if块中的 第5行 使用 let 也定义了一个 a ，那么即便是前四行也不可以使用全局变量a ，在let在快内定义了a 的那一刻，不论a在哪定义的，这个区域都将属于a



- const 不能只声明不定义，使用const时必须赋值而且一旦定义，就不可改变，相当于一个**静态变量**



## 箭头函数

- () => { } 

- 箭头函数都是匿名函数，没有具名函数，但是它可以赋值给一个变量从而获得名称

  ```js
  // 匿名函数
  function () {
  }
  () => {
  }
  	// **匿名函数可以**赋值**给一个变量**
  	var fn1 = function () {
  	}
      var fn2 = () => {
  	}
      
      
  // 具名函数
  function fn3(){
  }
  ```

  

  ```js
  (参数1，参数2,...,参数N) => {函数声明}
  
  //当函数声明只有一个return语句时，可省略{}和return关键字
  (参数1，参数2,...,参数N) => {return expression;}
  (参数1，参数2,...,参数N) => expression;
  
  //当函数只有一个参数时，参数()是可选的
  (单一参数) => {函数声明}
  单一参数 => {函数声明}
  
  //没有参数的箭头函数应写成如下的形式
  () => {函数声明}
  ```

- 箭头函数本身没有自己的 this，arguments，super 或 new.target，this 是从其所处的环境中继承而来，可以理解为判断箭头函数的 this 的指向时要跳出自己这一层箭头函数，看他所处的环境的 this 指向；

  ```js
  // 箭头函数
  ()=>{
  	console.log(this);  // window
  }
  ```

  

## 模板字符串

- 两个反引号 ``  其内部元素的结构会保留，可换行，可以嵌入变量，使用 ${变量名}

  ```js
  // 模板字符串
  var str = `
  			<li>${value}</li>
  			<li>${item}</li> `
  ```

  





# Ajax

- ajax是一门技术，用于从后台服务器中获取数据
- 其中有两种获取方式 get 和 pos

## readyState属性

- Ajax 请求/响应过程的阶段 （使用 readyState 属性的值来表征）

- 0 - 未初始化：尚未调用 open 方法

- 1 - 启动：已调用 open 方法，还未 发送 send

- 2 - 发送：已经发送 send 还未接收到响应

- 3 - 接收：开始接收，接收到了一部分数据，不全

- 4 - 完成：已经完全接收到了全部的响应数据，可以使用了

  

- **正是因为 ajax 的这几个阶段，所以在开始进行数据处理的时候要清楚 xhr.response 内是否有数据，是否已经接收到了数据，要加入时间的观念**

  

- 1 -  readystatechange  每次 readystate 值变化都会触发该函数

  ```js
  xhr.onreadystatechange = function () {
  	console.log(xhr.readystate);
  }
  ```

- 2 -  readystatechange 方法在 3 阶段时，若是**接收的数据很大时，接收的数据不同，也会触发该事件**，不仅仅只有 readystate 变化才会触发该事件


## 状态代码 status

- status： 响应的 http 状态

- statusText： http状态的说明

  ```js
  // 响应已经成功返回
  xhr.status = 200;
  // 请求的资源并没有修改，可以直接使用浏览器中缓存的版本
  xhr.status = 304;
  ```



## GET

```js
// Ajax的使用方式 - get
// 创建一个 请求对象，用于承接一些信息
var xhr = new XMLHttpRequest();

// 调用 open 方法，传入 请求类型（不区分大小写） 和 请求地址 和 **是否异步** true 异步 或 false 同步
// get 方式 若有数据直接加到url后面，使用？连接，之后的键值对使用 & 连接
// ‘url?name=zhansan&age=24&color=red’
xhr.open('GET', 'URL?数据');

// 发送请求,  其中的null可以省略，最好要带着，因为在有的浏览器中没有该参数会报错
xhr.send(null);

// 取得响应内容, 两种方法都可以使用
xhr.responseText
xhr.response            // ES5 中的新方法，推荐


// 之后的步骤就是对数据 xhr.response 进行操作，根据其返回的数据进行不同的操作
// 注意事项，xhr.response 的取得是需要一定的时间的，由于程序直接由上到下顺序执行，有可能在 xhr.response 还为空时，程序已经结束而产生 有误的结果，所以要判断请求执行到了那个阶段
// onreadystatechange 在每次请求阶段变化时都会调用
xhr.onreadystatechange = function () {
    if (xhr.readyState == 1){
        // 请求的第一阶段 
    } else if (xhr.readyState == 2) {
        // 请求的第二阶段
    } else if (xhr.readyState == 3) {
        // 请求的第三阶段
    } else if (xhr.readyState == 4) {
        // 请求的第四阶段
        // 一般关注这一阶段
    }
}
```



##  POST

- post 比 get 多一步，设置请求头
- send 的数据不同

```js
// Ajax的使用方式 - get
// 创建一个 请求对象，用于承接一些信息
var xhr = new XMLHttpRequest();

// 设置请求头 该步骤必须在 open 之前，不然已经发送出去了再设置没有意义
xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');   // 最常用的方式

// 调用 open 方法，传入 请求类型（不区分大小写） 和 请求地址 和 **是否异步** true 异步 或 false 同步
// post 方式不在 url 上填数据，在 send 中设置
xhr.open('GET', 'URL');

// 发送请求,  一个字符串，其中有多个键值对时使用 & 连接，键名称要与后台对应'name=zhansan&age=24&color=red'
xhr.send('符合后台要求的字符串');

// 取得响应内容, 两种方法都可以使用
xhr.responseText
xhr.response            // ES5 中的新方法，推荐


// 之后的步骤就是对数据 xhr.response 进行操作，根据其返回的数据进行不同的操作

```

- **注意使用同步请求之后，接收数据的方法是否已注册**

  ```js
      var xhr = new XMLHttpRequest();
  
  	// 使用同步请求，会阻塞事件
      xhr.open('GET', '/getXML', false);
  	
  	// 发送同步请求，只有响应完成才会执行后续代码
  	xhr.send(null);
  	// 响应完成，开始调用load事件，未发现load事件，也没发现对 readystate 的监听，没有任何事情发生，继续向下执行
  
  
  	// 开始注册事件进行对 readystate 进行监听，但是此时响应已经完成了，一直为4，不再变化，失去了他的意义
      xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
          console.log(xhr.response);
        }
      }
  ```

  ```js
      // ** 变化 **
  
  	var xhr = new XMLHttpRequest();
  
  	// 使用同步请求，会阻塞事件
      xhr.open('GET', '/getXML', false);
  
  
  	// 注册事件 进行对 readystate 进行监听
      xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
          console.log(xhr.response);
        }
      }
  
  
  	// 发送同步请求，只有响应完成才会执行后续代码
  	xhr.send(null);
  	// 响应完成，开始调用load事件，未发现load事件，发现了 对 readystate 的监听 调用事件	
  ```







## 其他方法

- onload                --  当readyState等于4的时候触发。只有请求成功了才触发。
- onprogress         --  当readyState等于3的时候触发（数据正在返回途中的时候触发）
- onloadstart()       --  当开始发送请求的时候触发，要放到send之前
- onloadend()        --  当请求响应过程结束的时候触发。无论成功还是失败都会触发。

## responseType

- 预期服务器返回的数据的类型，当设置了该属性后，通过 `response` 接收数据的时候，会根据该属性的值来自动处理结果为JS能够识别的数据。
  - “”  -- 空，表示文本，和text一样。空为默认值
  - text -- 文本
  - json -- JSON格式数据
  - document -- 文档对象。当服务器返回的结果是XML类型的时候，需要指定为document

## jQuery-Ajax

## $.ajax()

- $.ajax( { 各种相关属性值 } )       内部参数为 对象

```js
    <script src="./assets/jquery.js"></script>
    <script>
        
        // $.ajax(JS对象);
        $.ajax({
        // 属性: 值
        type: 'GET', // 请求方式

        url: '/query-get',

        // 发送给接口的数据，可以写成对象，jQuery内部会自动将对象转成字符串
        data: 'id=111&age=222&name=zs',
        // data: {id: 333, age: 666, name: 'zs'},

        // 表示不让jQuery将对象形式的data处理成字符串，一般在文件类型数据中使用
        // processData: false,

        //表示不让jquery自动设置 content-type为 'application/x-www-form-urlencoded'，一般在文件类型数据中使用
        // contentType: false,

        dataType: 'json', // 如同 responseType。

        // 回调函数
        success: function (res) {
        	console.log(res);
        }
        
        error: function (xhr) {
        	console.log(xhr)
        },
            
        complete: function (xhr) {
        	console.log('request completed')
        }

        beforeSend: function (xhr) {
        	console.log('before send')
        }
    });
    </script>
```

​	

```js
    // cache: 设置ie浏览器的缓存问题， cache: false 不缓存
    
    // url：请求地址
    
    // type：请求方法，默认为 `get`
    
    // dataType：预期服务端响应数据类型
    
    // contentType：请求体内容类型，如果是POST请求，默认 `application/x-www-form-urlencoded`
    
    // data：（object|string）传递到服务端的数据

    // timeout：请求超时时间
    
    // beforeSend：请求发起之前触发
    
    // complete：请求完成触发（不管成功与否）
    
    // success：请求成功之后触发（响应状态码 200）
    
    // error：请求失败触发
    
    // processData：是否让jQuery帮我们将发送给服务器的数据进行处理（默认：true表示将对象处理成字符串）


```

## get() / post()

- 两个方法的使用方法一样，可以单个传参，也可以传对象
  - $.get(url, [data], [callback], [dataType])       或    $.get({settings})
  - $.post(url, [data], [callback], [dataType])     或    $.post({settings})
- **四个参数，接口，数据，回调函数，数据类型** 

```js
  
  <script src="./jquery.js"></script>
  <script>
      // $.get(请求的接口, 发送到服务器的数据, 用于处理服务器返回结果的函数, 预期服务器返回数据的类型);
  
      /* $.get('/time', function (result) {
              console.log(result);
          }); */
  
      /* $.get('/query-get', {id: 123, age: 345}, function (result) {
              console.log(result);
          }, 'json'); */
  
      $.post('/query-post', {id: 123, age: 345}, function (result) {
          console.log(result);
      }, 'json');
  
  </script>
```

  

## 全局处理事件

- $.ajaxSetup({事件: 处理函数, 事件:处理函数, ... });
  - 每次Ajax请求都需要的事件，比如给一个请求响应过程进度提示，可以使用全局事件处理。反过来说，通过全局事件处理的事件，**后续**的每个ajax请求都会触发。

```js
    
	// 设置全局事件处理
    $.ajaxSetup({
        // 设置发送请求前的事件
        beforeSend: function () {
            // 这里可以提示，玩命加载中...
        },
        // 设置完全接收响应数据后的事件
        complete: function () {
            // 这里可以去掉“玩命加载中...”
        }
    });
```





- 扩展（加载进度条）

```js
	// 要先引包，css 和 js    

	$(document).on('turbolinks:click', function() {
        NProgress.start();
    });

    $(document).on('turbolinks:render', function() {
      NProgress.done();
      NProgress.remove();
    });
```



$(selector).load()

$.getJSON()

$.getScript()





# XML

- xml 的数据很严格，很完善，可以看作是html结构，当通过响应接收到之后，可以通过获取html节点的方式获取其内部数据

```js
// 第一行必不可少，之后的标签可以自定义 不区分大小写
<?xml version="1.0" encoding="UTF-8" ?>
<students>
    // 属性值要加引号 
	<stu id="1">
    	<name>张三</name>
        <age>18</age>
        <sex>男</sex>
        <other height="175cm" weight="65kg" />
    	// 单标签必须闭合 />
    </stu>
    <stu id="2">
    	<name>李四</name>
        <age>20</age>
        <sex>女</sex>
        <other height="170cm" weight="60kg" />
    </stu>
</students>
```







# 模板引擎

## 使用步骤

- 加载 js 文件
- 设置模板 html 加 数据的格式 {{ }}
- 调用 template 合成数据，合成的结果是一个字符串，需要使用 innerHTML 添加才会在页面中显示



```js
    <!-- ----------------------- 使用模板引擎-1.加载js文件 -->
    <script src="./assets/template-web.js"></script>

    <!-- ----------------------- 使用模板引擎-2.设置模板 -->
    // 一定好指定 id 和 type（text/html）
    <script id="test" type="text/html">
        <h1>{{title}}</h1>
    </script>

    <script>
        <!-- -------------------- 使用模板引擎-3.调用template函数 -->
        // var 模板和数据组合好的结果 = template(模板id, 模板中使用的数据必须是js对象类型);
        var data = {
            title: '这是模板引擎的例子'
        };

        // ** 调用 template(模板id,  Object)  两个参数，一个 模板id ，一个数据 data 该值必须为一个js **对象**
        var html = template('test', data);

        // 得到一个字符串，可打印，可设置
        console.log(html);
        document.body.innerHTML = html;

	</script>
```







## 模板内可用的语法

- 输出普通数据（字符串、数值等）

    ```js
        // 模板写法，直接使用传入对象的 键值
        {{ str }}
    
        // template函数写法
        var html = template('id', {
            str: 'hello world'
        });
    ```




- 条件语句

    ```js
        // 模板写法
        {{if age > 18}}
            大于18
        {{else}}
            小于18
        {{/if}}
    
    ```



- 循环语句

    ```js
    	// 在循环内的数据要加 $ 
        {{each arr}}
            {{$index}} -- 数组的下标
            {{$value}} -- 数组的值
        {{/each}}
    
        // template函数写法
        var html = template('id', {
            arr: ['apple', 'banana', 'orange']
        });
    ```



- 实例,调用

```html
	<script src="./assets/template-web.js"></script>

    <!-- 1. 定义模板 -->
    <script id="abc" type="text/html">
        <h1>{{name}}</h1>
        <p>我是{{nickname}}，我有一辆{{car}}，我今年{{age}}岁了</p>
        
        {{if age >= 18}}
            <p>欢迎~</p>
        {{else}}
            <p>禁止进入</p>
        {{/if}}
        
        <p>我有好几个朋友，分别是：</p>
        <ul>
        
            {{each girls}}
            <li>{{$index}} -- {{$value}}</li>
            {{/each}}
            
        </ul>
    </script>


    <script>
        
        // 2. 调用template函数
        var str = template('abc', {
            name: '狗哥',
            nickname: '北狗最光阴',
            car: '宝马',
            age: 31,
            girls: ['王婆', '金莲', '西门大官人', '李师师', '赛金花']
        });

        console.log(str);
        document.body.innerHTML = str;
    </script>
```





# FormData对象(H5)

- FormData是H5中新增的一个内置对象可以将数据编译成键值对方式，以前的ajax只能提交字符串，现在可以提交 二进制 数据

  new FormData（[表单对象]），可以传入一个表单对象，也可以不传，必须是DOM对象，JQ对象需要转换

  

可以在两种情况下工作



- 第一种情况，（有form标签）

```html
    
	<form id="fm">
        // name 属性是必须的
        <input type="text" name="user"><br>
        <input type="password" name="pwd"><br>
        <input type="radio" name="sex" value="男" checked>男
        <input type="radio" name="sex" value="女">女<br>
        <input type="file" name="pic"><br/>
        <input type="button" id="btn" value="提交">
    </form>
```

​	

```js
	// 当点击提交按钮的时候，需要把表单各项的值，提交给fd接口。
    document.getElementById('btn').onclick = function () {
  
        // FormData 专门用于收集表单各项值 **必须要表单有name属性**
        // 1. 有表单，找到表单
        var form = document.getElementById('fm');
        // 2. 实例化FormData，将表单的DOM对象传入即可
        var fd = new FormData(form); // fd对象中包含了表单所有的值

        // 将各项值发送给 '/fd' 接口
        var xhr = new XMLHttpRequest();
        xhr.open('POST', '/fd');
        
        // 不能设置这个请求头，会解析成字符串而产生错误
        // xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
        
        xhr.responseType = 'json';
        xhr.send(fd);
        xhr.onload = function () {
            console.log(this.response);
        }
    }

```



- 第二种情况，（没有form标签）

```php+HTML
<input type="text" id="user"><br>
<input type="password" id="pwd"><br>
<input type="file" id="pic"><br/>
<input type="button" id="btn" value="提交">

<script>
    // 点击提交按钮的时候，把数据发送给fd接口
        document.getElementById('btn').onclick = function () {
            // 收集表单数据
            // 1. 先实例化FormData
            var fd = new FormData();.
            
            // 没有表单就一个一个的往 fd 中传值
            // 2. 调用FormData内置的方法append，向fd对象中，添加值
            // fd.append(key, value);
            // key 是 name 属性
            fd.append('username', document.getElementById('user').value);
            fd.append('pwd', document.getElementById('pwd').value);
            
            // 如果是文件的话，必须使用文件对象
            var file = document.getElementById('pic');
            // file 中还包含有许多其他的东西，不能直接用
            // console.dir(file);
            var fileObj = file.files[0];
            // fd.append('myfile', 文件对象);
            fd.append('myfile', fileObj);

            var xhr = new XMLHttpRequest();
            xhr.open('POST', '/fd');
            xhr.responseType = 'json';
            xhr.send(fd);
            xhr.onload = function () {
                console.log(this.response);
            }
        }
</script>
```



- 在jQuery中使用FormData 对象

  - FormData 中必须是DOM对象，$对象要转换
  - $.ajax({ });            由于文件数据的存在，要考虑 请求头 和 返回数据类型 的不同

  

```php+HTML
	<form id="fm">
        <input type="text" name="user"><br>
        <input type="password" name="pwd"><br>
        <input type="radio" name="sex" value="男" checked>男
        <input type="radio" name="sex" value="女">女<br>
        <input type="file" name="pic"><br />
        <input type="button" id="btn" value="提交">
    </form>
    <script src="/jquery.js"></script>
    <script>

        $('#btn').click(function () {
            var fm = $('#fm');
            var fd = new FormData(fm[0]); // 这里fm必须是DOM对象
            console.log(fd);

            $.ajax({
                type: 'post',
                url: '/fd',

                // 如果data使用的是对象，ajax方法会把对象转成字符串，
                // 即把{name: 'zs', age: 18}转成name=zs&age=18
                // data: {name: 'zs', age: 18}, 
                data: fd,
                
                // processData: false, 表示不让jQuery把fd对象转成字符串，而是直接发送fd对象
                processData: false,
                
                // contentType：false，表示不让jQuery去设置content-type，让FormData去处理
                contentType: false,
                success: function (res) {
                    console.log(res);
                }
            });
        });

        // xhr.send('name=zs&age=18');
    </script>
```







- 当页面中有 form 时，若要获取的数据只是文本值而没有文件，则可以使用，$('form').serialize() 方法，该方法与 Form Data 一样，也需要使用name属性，只是其获取到的结果会自动转换为字符串

  ```js
  // 获取到一个字符串， 类似于 “name=xzs&age=23” 这种格式，使用方便
  $('form').serialize();
  ```

  













