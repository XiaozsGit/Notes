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