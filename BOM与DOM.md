# BOM

- BOM： browser object model，是把**浏览器看成是一个对象**，就是学习浏览器对象的各种方法和属性；
- 浏览器对象：**window对象**；

## window对象

- 所有window对象的属性和方法，**都可以直接省略  window.  而直接使用**

- **顶级对象**：页面中所有的东西都是依赖于这个对象存在的；

- document也是window的一个对象（因为其内容较多而人为的独立出来）

- 变量与函数：

  - **所有的全局变量和全局函数都是window对象的属性和方法**；
  - 在js代码里面，不使用var声明的变量，都是隐式全局变量，不推荐，因为如果不使用var声明，是**不会变量提升**的；

  ```js
  // 1.所有window对象的属性和方法，直接省略  `window.`
  document.getElementById('xx');
  
  // 2.顶级对象
  console.log(window.document == document);
  
  // 3.全局变量和函数 都是window上的挂载
  var a = 10;
  console.log(window.a);
  function fn() {
      console.log(1);
  }
  window.fn();
  
  // 4.隐式变量：定义变量，变量赋值；
  b = 2;
  console.log(window.b);
  
  // 访问变量：无赋值，就是访问变量；报错，无定义；
  c;
  console.log(window.c);
  ```

## 方法：window.onload

- 作用：页面加载完毕的时候执行
- 页面加载完毕：**页面所需的静态资源**全部加载完毕；
- 静态资源： **html文件、css文件、js文件、图片...**
- 这个方法调用一般是用window.onload 不省略window;

```javascript
window.onload = function(){
    // 想要获取图片的宽高，就需要等待图片加载完成后才执行后面的函数；
}
```

## 方法：定时器

- setTimout：一次性定时器；set - 设置；Timeout - 超时

```javascript
// 作用： 延迟一定的毫秒之后，调用函数一次;
// 返回值： 是该定时器的id，id可以用于停止这个定时器
var timer = setTimout(函数,延迟的毫秒数);

// 停止一次性的定时器：清除后，就不会执行这个定时器；
clearTimeout(timer);
```

- setInterval：永久性的定时器；interval - 间隔

```js
// 作用：阶段的时间执行函数；
// 返回值：就是该定时器的id
var timer = setInterval(函数,间隔);

// 清除永久定时器
clearInterval(timer);
```

- 上面两个方法都是window对象下的方法；执行定时器，**都要先等待**
- **即便只是函数内只有一条语句，也要使用函数包裹放入定时器内，不可以直接放入**

## 属性：location

- location：负责管理浏览器地址栏相关的行为和信息的对象；
- 属性：  location.href 属性；该属性就是浏览器的地址栏里面的内容，
  - 获取：当前浏览器的地址；
  - 重新设置，页面就会发生跳转；

```javascript
	// 如果想要使用js进行跳转，只需要 location.href = 新的地址;
	location.href = 'http://www.jd.com';
```

- 方法：location.search();获取地址栏中？后的数据，包含？





## localStorage

- 本地储存：本地指浏览器，储存指浏览器可以储存数据；
- 数据本地化：把数据进行**本地储存**，数据存在于浏览器中，即使再次刷新时，源码中没有给数据，浏览器还是会显示原先已经存储好的数据（以前：我们在页面上操作数据进行添加，一刷新之后数据不保存，会消失）

```js
// 存储： 后面的值 前面可以放入任何数据类型，保存后为字符串；
// 注意： 如果存储的是对象之类的复杂类型，需要先把复杂类型转换为的字符串，再存进去；
// 多次对一个键进行赋值，会把前面的值**覆盖**；
localStorage.setItem(键,值);

// 读取 数据
// 返回：我们存入的的数据的值，返回的是字符串；
localStorage.getItem(键);

// 删除键的值
localStorage.removeItem(键);　　

// 全部清空
localStorage.clear();　
```





## JSON

- 以前：我们想要存储**具有各种意义的数据**到字符串里面，一般会使用这样的格式：
  - 李狗蛋|12|男&翠花|13|女&铁柱|14|男
  - 该方式，不适合复杂的数据描述
- JSON格式：**[] - 表示数组；{} - 对象；和JS学习的对象，数组特别的像；**
  - JSON是**有一定格式的字符串**
    - 所有的键必须使用双引号包起来
    - 字符串也必须是双引号
    - 只有数字和字符串两种类型
    - 只是记录数据；
- JSON方法：js提供的JSON方法，封装了很多跟json操作相关的方法

```js
// 将对象转换为json格式的字符串
// 返回值：一个满足json格式的 字符串
JSON.stringify(对象);

// 将json格式的字符串转换为对象
// 返回值：依赖于你的json格式字符串，可能返回数组，或者是对象....
JSON.parse(json格式字符串);
```

- JSON：
  - 字符串，约定的数据格式的字符串；
  - BOM的一个方法：将对象类型转为JSON字符串；

- 数据类型

```js
// 各种数据在 json 中怎么表示
// null 类型 直接写
  null

// number 类型 不带引号
  2048

// boolean 类型 不带引号
  true

// string 类型 带双引号
  "hello"

// array 类型 []
  ["zhangsan", "lisi", "wangwu"]

// object 类型 {}
  {
    "name": "zce",
    "age": 18,
    "gender": true,
    "girl_friend": null,
    "arr": []
  }

```

- **注意**

```js
  1. JSON 中属性名称 必须用 双引号 包裹
  2. JSON 中表述字符串（值）必须使用双引号
  3. JSON 中不能有单行或多行注释
  4. **JSON 没有 `undefined` 这个值**
  5. 一个完整的JSON，不能有其他内容掺杂，必须是一个完整的 “数组” 或完整的 “对象”  或 完整的 字符串 或 ..........
```

- **数据转换**

  ```js
  // JSON.parse();
  // JSON.stringfy();
  
  // 定义一些JS格式的数据
  var a = ['张悦', '假冰冰', '老狗', '苏老湿'];
  var b = true;
  var o = {id: 1, name: '刘老湿', nickname: '北狗最光阴'};
  
  // 把JS数据转成JSON数据  （JSON.stringify()）
  var jsonA = JSON.stringify(a);
  var jsonB = JSON.stringify(b);
  var jsonO = JSON.stringify(o);
  console.log(jsonA)
  console.log(jsonB)
  console.log(jsonO)
  
  // 把JSON数据转成JS数据 （JSON.parse()）
  console.log(JSON.parse(jsonA));
  console.log(JSON.parse(jsonB));
  console.log(JSON.parse(jsonO));
  ```

  



# 获取DOM节点

设置变量名时注意顶层容器的 top（只读，不可修改，存疑，在子元素中定义的时候不指向顶层容器） 和 name属性

window 中是有 top（只读）和name属性的

## 获取节点

- document.getElementById（）                  id属性-获取单个元素，拥有指定id的第一个元素，是操作一个特定元素的最有效的方法
- document.getElementsByName（）         name属性-返回拥有指定name属性的一个伪数组，既为数组，就可以使用[0]调用
- getElementsByTagName（）                    标签名-大小写不敏感，指定标签名的伪数组
- getElementsByTagNameNS（） 
- getElementsByClassName（）                 class属性-返回拥有指定class属性的一个伪数组
- **querySelector（css选择器）**                   只获取第一个元素
- querySelectorAll（）                                  获取全部元素

## 子元素

注意子元素与子元素节点的区别

父元素.**children[]**,      获取子元素，注意其获取到的都是数组，哪怕只有一个

childnodes[],子元素节点（包含**文本**节点，）

## 兄弟元素

```js
元素.nextElementSibling  -  得到下一个兄弟元素
元素.previousElementSibling - 得到上一个兄弟元素
```

## 父元素

```js
元素.parentNode
```

## 获取节点样式

```js
// Computed：计算后的样式
// 返回值： 当前作用在这个元素身上的所有样式的集合对象  BOM的方法；
// 属性：具体的属性 无论是行内的还是样式设置的，都可以获取到；字符串
var res = window.getComputedStyle(元素对象)；
res.width 

// 只能操作行内属性；
var dom = document.getElementById('xx');
dom.style.color；
```

## 获取元素高度

```js
// 返回值：数值；

// 只能进行获取；
// 元素的实际宽度
元素.offsetWidth 
// 元素的实际高度
元素.offsetHeight

// 获取和设置
dom.style.width；
```











# 增删改.DOM节点

## 添加DOM节点

- **document.write()**

  - 会对原有的HTML结构进行覆盖；
  - 可以解析HTML结构，且多次写多次输出；
- **document.createElement（‘标签名’）**

  - 根据指定的标签名，创建一个新的元素
  - **注意：**此方法只创建一个元素，并不会自动添加到文档中，需要**手动添加**

- **添加方法**

  - 父元素.appendChild（子元素）         

    **作为父元素最后一个子元素添加到父元素中**，加到最后

  - 父元素.insertBefore（要添加的子元素1， 原有的子元素2）     把元素1放到元素2前面

    **添加到指定的子元素前面** 

## 修改DOM节点

- 元素.innerHTML = “  .  .  .”：
  - 获取和设置元素的内容，可以解析HTML结构
  - 会覆盖，可用  += 添加

- 元素.innerText = “. . . ”
  - 获取和设置元素对象（DOM节点）的文本信息,不可解析HTML，会把它作为字符串输出

## 删除DOM节点

- 父元素.removeChild(要删除的子元素);











# 事件

## 三个阶段

- 三个传播阶段：**捕获、到达目标、冒泡**

- 捕获：从根节点开始向着目标DOM节点一层一层的找，捕获是用户点击了那个DOM节点；

- 冒泡：从目标节点到根节点；

- 事件执行：**事件默认是在冒泡阶段执行；**当我们目标DOM节点注册了事件，冒泡往上的DOM节点也注册了同样的事件话，也会执行；

- **设置事件执行阶段**

  ```js
    // 捕获阶段执行 只有这一种方式
    yeye.addEventListener('click', function() {
      console.log('我是你爷爷');
    }, true);
    baba.addEventListener('click', function() {
      console.log('我是你爸爸');
    }, true);
    erzi.addEventListener('click', function() {
      console.log('我是你儿子');
    }, true);
  
    // 冒泡阶段执行
    yeye.addEventListener('click', function() {
      console.log('我是你爷爷');
    }, false);
    baba.addEventListener('click', function() {
      console.log('我是你爸爸');
    }, false);
    erzi.addEventListener('click', function() {
      console.log('我是你儿子');
    }, false);
  ```

## 阻止冒泡

- 你希望在哪里阻止，就在哪个事件处理程序里面调用即可；**在函数里面的前后位置无所谓**；

- e.stopPropagation();

  - 阻止冒泡不是阻止自己执行；
  - 会阻止事件向上传播，不论他是不是自己所触发的，若

  ```js
    var outer = document.querySelector('.outer');
    var inner = document.getElementById('inner');
    var box = document.getElementById('box');
  
    outer.onclick = function (e) {
      console.log('这是定义在outer的事件');
    }
    inner.onclick = function (e) {
      console.log('这是定义在inner的事件');
      // 在此处阻止冒泡，则由box冒泡而来的事件会执行，但不会在冒泡到 outer 上
      e.stopPropagation();
    }
    // inner.removeAttribute('id');
    box.onclick = function (e) {
      console.log('这是定义在box的事件');
    }
  ```

## 事件委托

- 为一个父元素设置事件监听，利用事件冒泡对子元素的点击做出响应
- **关键点：获取点击的子元素**
  - **e.target**                                        谁点击就是谁
  - **e.currentTarget**                           谁注册监听就是谁
  - 元素.nodeName                           获取元素的标签名，大写如 LI  UL   DIV     

## 事件类型

- click，mouseout, mouseover, mousedown, mouseup，blur, focus, **contextmenu(右键菜单)** 	 

### 鼠标事件

- mouseover    mouseout  

- e.button   (0 左  1  中  2  右)      可判断左右键点击

- **click**                                          鼠标点击事件

  - 配合e.target/ e.target.nodeName/ e.target.parentNode/ ... 可以获取到有关点击对象的周边对象进行操作
  - 注意其**无法响应右键**点击事件，原因**浏览器中有右键的默认显示菜单的事件**，不会对click做出响应，若要对右键的鼠标事件添加监听，可以使用 contextmenu 事件类型(自行决定是否取消其默认行为)

- **contextmenu**(右键菜单)

  - 可以使用 e.preventDefault(); 取消默认右键菜单，不论前后，函数体都会执行，菜单都会取消。若不取消，先执行函数体，然后显示右键菜单
  - 可以在这个事件中先取消事件的默认行为，再添加右键的事件实现鼠标的右键操作（扫雷中可以用到）

  ```js
  	// 实现右键点击时，显示弹窗	
  	document.oncontextmenu = function (e) {
  		alert('您点击了右键');
  		e.preventDefault();
  	}
  ```

- **auxclick**   （**非主键**按钮事件, **不响应左键**）现阶段还有一些兼容问题，在有的浏览器中使用中键和右键都行，在有的浏览器中只支持中键

  ```js
  	document.onauxclick = function (e) {
          if (e.button == 1) {
  			console.log('点击了中键！');
  		}
  		if (e.button == 2) {
  			console.log('点击了右键！');
  		}
  	}
  ```

### 键盘事件

- **注意是否内嵌了iframe，并且焦点是否在iframe上**

- keydown                 键盘按下，不松开

- keyup                      键盘抬起

  - 配合 e.keyCode/   e.key/   e.ctrlKey/  e.altKey/  e.shiftKey/   组合使用，知道是那个按键被按下
  - 可以为 html 添加**自定义属性**，其实与按键的 e.keyCode或 e.key相等，再使用 **属性选择器**快速获取对象进行操作
  - 组合键   与ctrl shift alt 的组合， 使用条件表达式&& 可以实现效果

  ```js
  text.onkeydown = function(e) {
      console.log(e.keyCode, e.ctrlKey);
      
      // 判断是否同时按下ctrl和回车
      if (e.ctrlKey && e.keyCode === 13) {
      }
  }
  ```

  - 三键组合（存疑）

### 动画结束事件

- transitionend：元素的过渡动画结束的时候触发；

  ```js
  var box = document.querySelector('.box');
  box.addEventListener('transitionend',function(){
    console.log(123);
  });
  ```

- animationend：会在帧动画结束的时候触发

  ```js
  var box = document.querySelector('.box');
  box.addEventListener('animationend',function(){
    console.log(123);
  })
  ```

- **注意：**
  - 不能使用on的方式注册，只有addEventListener才可以成功注册
  - 如果帧动画是**无限次**的，不会触发该事件animationend

## 多个按键同时按下！

**当有多个按键同时按下时，使用传统方法的话，先按下的按键在后面按键按下后将覆盖，不再执行，直到另一个按键按下**

该解决方案可以运用在网页游戏中，在同一时间内会有多个按键同时按下而且不一定会松开（wasd方向    jkui 打）

```js
	var key_pressed = {};
	// 也可以使用多维数组，记录更多的数据，注意记录那个键按下了，keyCode，以及true或false，
	// 若要实现更加复杂的逻辑并且需要用到 event 对象，可以记录 event
	document.addEventListener("keyup",function (e) { 
		// 记录某个按键的状态，假定谈起为false 按下为true，用于在后面判断定时器里的函数是否执行
		// 一般来说，定时器里的函数要通过数据判断与按键相匹配
		key_pressed[e.keyCode] = false;
	});
	document.addEventListener("keydown",function (e) {
		key_pressed[e.keyCode] = true;
	});

	setInterval(function() {

		for(var key in key_pressed) {
			if (key_pressed[key]) {
				// 代表 key 键按下了，没弹起，
                // **可以在此处书写代码更加复杂的代码逻辑**
				// 键盘按下的事件被**交由此处执行**，可做if判断是那个键来进行不同的操作
				console.log(key + " is down");
			}
		}
	},50);
	// 要注意这个时间，不要太大，不然在函数体等待期，按下或者松开了按键会无法被执行到，造成失灵

```

## 注册事件

- **添加事件监听**

  对象.addEventListener('事件类型'， function() { ... });

  此种方式添加的事件监听**不会覆盖**，在多处进行的注册监听都会生效

- **直接调用**

  对象.onclick = function () { ... }

  其中，click只是一种对象类型还有许多其他类型，mouseout, mouseover, mousedown, mouseup 等

  此种方式添加的事件监听**会覆盖**，在多处进行的注册监听只有在最后位置的监听才有效

## 注销事件

​	对应两种注册方式，注销也有两种方式（两种方式都要注意事件类型要与注册时的一致）

- 对象.**remove**EventListener('事件类型'，函数名称);    函数名称后不用加（）

  此种方法进行解绑事件时，要求添加事件的函数不能为匿名函数

- 对象.onclick = null;

  使用一个空的函数即可，因为该方式注册的事件会覆盖







# 移动端事件

## 触摸事件

- 移动端一般不使用click：
  - 在移动端可以双击，当我们点一下的时候，移动端不会立刻执行点击事件，而是会稍微等待一下（等待是否第二次点击），因为移动端需要在**点下的瞬间需要知道到底是单击还是双击**，执行的事件不同



- **触摸事件**
  - touchstart        会在手指触摸到屏幕的时候触发
  - touchmove      会在手指触摸到屏幕，移动的过程中触发
  - touchend         会在手指离开屏幕的瞬间触发
  - **注意** 
    - 触摸事件在pc端不会触发，只能在移动端触发
    - 推荐使用addEventListener的方式注册：有很多移动端的事件，都是后面h5或者c3才出现的，on的方式没有对应的属性

- 事件对象属性：触摸点

  - 事件对象.touches                                  在屏幕上面的触摸点
  - 事件对象.targetTouches                        在元素上面的触摸点
  - 事件对象.changedTouches                   变化的触摸点(移动端可以多指操作，触摸点减少或增多时)

  

- 用一个手指触摸屏幕内的元素时
  - **刚触摸时touchstart**：touches、targetTouches、changedTouches，有一个值；都是同一个值；
  - **在元素上触摸移动时touchmove**：touches、targetTouches、changedTouches，有一个值；都是同一个值；
  - 离开屏幕：touches、targetTouches没有值；changedTouches有最后离开屏幕的值；

## 封装 tap 手势

- **手势：**单击、双击、三击、放大、旋转、滑动....（相当于类型PC端的各种事件类型，PC端相对简单）
- 以上这些手势，**都没有原生的事件对应的**；
- 比如类似PC的点击click，在移动叫tap事件，我们只能自己利用js里面提供的touch事件自己封装。（封装的难度较大）
- **tap要求：模拟PC端的点击**  
  - 点下和松开的**位置要接近**
  - 点下和松开的**时间不能太长**
- 封装思路：
  - 在touchstart的时候，**记录开始位置**，在touchend的时候**记录结束位置**，相减，判断这个差不能大于某个值；
  - 在touchstart的时候，**记录开始时间**，在touchend的时候**记录结束时间**，相减，判断这个差不能大于某个值；
- 目标：我们自己要封装一个类似PC的点击，叫tap事件；
- 实现：
  - 获取元素：随便获取个元素对象；
  - 注册事件：touchstart、touchend
  - touch之后：
    - touchstart：
      - 判断点击touches的对象数量，若为2，则不是一个手指在点击；返回false;
      - 若为1个对象：**记录开始位置和开始时间；**
    - touchend：
      - 判断点击changedTouches的对象数量，若为2，则不是一个手指在点击；返回false;
      - 若为1个对象：
        - 记录结束位置和结束时间
        - 把位置和时间相减，再判断是否小于我们规定的值；
        - 从而判断是否为单个手指的“点击”效果；
        - 最后执行回调函数；

```js
// 封装：现阶段，封装对于我们来说，函数比较容易实现
// 函数，就需要参数，所以需要明确函数的参数的作用；
/**
 * 自己封装的tap事件
 * @param {object} element 事件源
 * @param {function} callback 事件处理程序
 * @param {number} offset 手指抖动的最大偏移量,默认是50
 * @param {number} timeSpan 点下的最大毫秒数，默认是300
 */
function tap(element, callback, offset, timeSpan) {
    offset = offset || 50;
    timeSpan = timeSpan || 300;
    var startX, startY, startTime,endTime;

    element.addEventListener('touchstart', function (e) {
      // 判断是否是一个手指按下
      if (e.touches.length != 1) {
        console.log('已经不是一个手指的操作了');
        return;
      }
      // 记录开始位置
      startX = e.touches[0].pageX;
      startY = e.touches[0].pageY;
        
      // 记录开始的时间
      startTime = Date.now();
    })

    element.addEventListener('touchend', function (e) {
      // 判断是否只有一个手指
      if (e.changedTouches.length != 1) {
        console.log('不是单击操作');
        return;
      }
        
      // 记录结束位置
      var endX = e.changedTouches[0].pageX;
      var endY = e.changedTouches[0].pageY;
      // 记录结束时间
      endTime = Date.now();
        
        
      // 计算，判断
      if (endTime - startTime > 300) {
        // 不是单击，是长按
        console.log('时间长按了');
        return;
      }
      
      // 希望开始和结束之间的距离不要超过50 - 要计算一下绝对值，因为我们滑动的方向可能不确定
      if (Math.abs(endX - startX) >= 50 || Math.abs(endY - startY) >= 50) {
        console.log('滑动了太多的位置');
        return;
      }

      // 如果是一个单击tap事件，就应该调用一个函数
      callback && callback();
    })
  }
```

- 自己封装一个是比较麻烦，一般在开发里面，会使用别人**封装好的类库；**



# 属性

- 元素.getAttribute(属性名)                                          获取需要的属性值

- 元素.setAttribute(属性名,属性值)                              设置某个属性的值

- 元素.removeAttribute(属性名)                                   移除某个属性值

## 自定义属性

- 设置

  - 行内式设置：

    ```html
    <input class='box' data-ziding = '自定义属性值'/>
    ```

  - js设置：

    ```js
    box.dataset.ziding = '自定义属性值'；
    ```

- 获取

  ```js
  box.dataset.ziding;
  
  <input class='box' ziding = '自定义属性值' />
  // 用此方式获取自定义属性，自定义属性不需要使用特定格式（data-）  直接ziding = '自定义属性值'即可
  box.getAttribute('ziding')   
  ```







# 大小/ 坐标！！！

## 三大系列

- offset 系列
- client系列
- scroll系列



- 元素.offsetParent               得到一个距离我最近的**定位的前代元素**，如果我的前代元素都没有定位，得到body或者html

## 大小

- 元素.offsetWidth             元素.offsetHeight             *       盒子的实际高度（包含盒子边框  content+padding+border；）
- 元素.clientWidth             元素.clientHeight              *       可视区域的宽高（不包含边框，包含padding）
- 元素.scrollwidth              元素.scrollHeight              *       只读，包括overflow样式属性导致的视图中不可见内容，没有垂直滚动条的情况下，scrollHeight值与元素视图填充所有内容所需要的最小值clientHeight相同。包括元素的padding，但不包括元素的margin.



## 坐标

- e.pageX                           e.pageY                    *       鼠标在文档中的位置（被翻上去而不显示的也计算位置，大）
- e.clientX                           e.clientY                   *       鼠标在当前窗口的位置,（不计算被翻上去而隐藏的位置，小）
- e.offsetX                           e.offsetY                           鼠标在事件对象中的位置
- 元素.offsetLeft                  元素.offsetTop          *       
  - 找到一个有定位的父亲元素进行参考(offsetParent)，如果亲生父亲没有定位，会一直往上找，直到找打有定位的父亲，或者body；

