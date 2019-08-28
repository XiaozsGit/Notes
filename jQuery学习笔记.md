# jQuery

- write less, do more

- 链式编程 ，隐式迭代

- 链式编程，要注意连起来之后的当前元素是哪一个，而我们要对那个元素进行操作，不能搞乱了

  ```js
  $(this).css('color', 'red').sibling().css('color', '');  
  // 并不一直是 $(this) 为当前对象，在获取了 .sibling()之后，则兄弟对象才是后面链起来的当前对象 若再有一个 .chileren(),获取的是 sibling() 的子代元素，而不是 $(this) 的
  ```





## 对象（相互转换）

- $  jQuery 二者等同

- DOM对象与jQuery对象的相互转换

  ```js
  var box = document.getElementById('box');
  var $box = $('#box');
  box.index();         // 获取元素下标，（伪数组）
    
  // DOM ==> jQuery 对象   一种 使用$()包起来就可以
  $(box); 
  $(this)；
  // $(document).ready(function () {})
  
  // jQuery ==> DOM 对象   两种
  $box[0]         // $box[index] 
  $box.get(0)     //$box.get(index)
  // jQuery ==> DOM 对象的思想，使用jQuery获取到的元素都是存储在一个伪数组中，其中的元素是DOM元素，因此，转换为DOM元素的做法是取出这个DOM元素即可，使用数组取值的方法 （index索引）
  ```

- **this  ==>    $(this)** this 属于DOM中的方法，需要转化为jQuery对象使用







## 入口函数

- DOM结构加载完之后即可执行，不必等待所有外部资源加载完毕，比window.load 的效率高一点

- $(document).ready(function () {});
- $(function () { });









## 选择器

- **隐式迭代**：所匹配到的元素会后台自动循环遍历，不用自己循环注册事件

### 基础选择器

- $('css选择器') css选择器可以是 'div'  ''.box'  '#box'  'div ul' 'div>ul' 'div, ul' 等

### 筛选选择器

- **索引从零开始**

- $('li:first')：第一个元素

- $('li:last')：最后一个元素

- $('li:eq(2)')   等同 $('li').eq(index)
  - 查找指定索引的元素
  - .eq(index)   建议方式（可以使用变量）          或  $('css选择器 : eq(index)')

- $('li:odd')    查找索引为奇数 （结构上表现为偶数）

- $('li:even')  查找索引为偶数   (结构上表现为奇数)

### 筛选方法（重点）

- **.parent()**               父元素

- **.parents()  **            祖代元素，可以加参数，返回特定祖代

- **.children()**             子元素，可加特定参数，也可不加选择所有子代

- **.find()**                    后代元素，可加特定参数，也可与eq结合使用

- **.siblings() **            所有兄弟元素（除自身），若加参数，则为特定

- .nextAll()              后面的兄弟元素，若加参数，则为特定

- .prevAll()              前面的兄弟元素，若加参数，则为特定

- **.eq(index)**

  **在这些方法中灵活的与 .eq() 结合使用是很重要的**，jquery中提供了一个快捷方法  **.index()** 可以快速获取某一元素的索引值

- $('div').hasClass('aaa')   判断是否有某一类名，返回 true 或 false

### 状态选择器

- ：checked                             被选中的元素（checked属性值为checked的元素）

- 



## JQuery样式操作

- 直接操作style属性： .css() 方法

- 操作类样式，增删类名  **不会影响原有类名**

  ```js
  $(this).css('color');  
  // 只写属性名是获取，返回该属性的值
  
  // 第一种 .css()
  $(this).css('color', 'red');   // 单个
  $(this).css({                  // 多个 使用对象传参
      'color': 'red',
      'width': 200
  });
  
  
  // 第二种 
  $('div').addClass('current');     // 增加
  
  $('div').removeClass('current');  // 删除
  
  $('div').toggleClass('current');  // 切换
  
  ```







## JQuery属性操作

- **固有属性**          .prop()    只能对固有属性进行操作

  ```js
  // 获取 和 设置 **固有属性**
  // 获取 只传入一个参数
  $('.box').prop('src');
  // 设置 传入两个参数
  $('image').prop('src', '../images/1.jpg')
  
  ```

- **自定义属性 **     .attr()       只能对自定义属性进行操作

  ```js
  // 获取 和 设置 **自定义属性**
  // 获取 只传入一个参数
  $('.box').attr('x');
  // 设置 传入两个参数
  $('.box').attr('y', 100);
  ```







## 获取文本值

- **普通元素内容html()（相当于原生innerHTML)**

​	获取：html()   // 获取元素的内容

​	设置：html(''内容'')   // 设置元素的内容

- **普通元素文本内容 text()   (相当与原生innerText)**

​	获取：text()   // 获取元素的文本内容

​	设置：text(''文本内容'')   // 设置元素的文本内容

- **表单的值val()（ 相当于原生value)**

​	获取：val()   // 获取表单的值

​	设置：val(''内容'')  // 设置表单的值







## jQuery元素操作

### 增删改

- 创建元素  $(''<li></li>'');  
- **添加元素** 
  - 内部添加（element 的子代）
    - element.append(''内容'')                  把内容放入匹配元素内部最后面，类似原生 appendChild
    - element.prepend(''内容'')                 把内容放入匹配元素内部最前面。
  - 外部添加（element 的兄弟）
    - element.after(''内容'')                       把内容放入目标元素后面
    - element.before(''内容'')                    把内容放入目标元素前面 
- **删除元素** 
  - element.remove()                                   删除匹配的元素本身
  - element.empty()                                     删除匹配的元素集合中所有的子节点
  - element.html('''')                                     清空匹配的元素内容（删除所有子节点），以‘’进行覆盖，达到删除效果
- 改        可以使用.html('内容')达到修改效果

### 遍历元素

**jQuery 隐式迭代是对同一类元素做了同样的操作。如果想要给同一类元素做不同操作，就需要用到遍历。**

- 两种方式：**$的方法** $.each() 和  **$('选择器')对象的方法**   $('选择器').each()

- $.each()

  ```js
  $.each(object，function (index, element) { xxx;} ）
  // 可用于遍历任何对象，数组，主要是对对象或数组内的 **数据** 进行操作
  // 因为以 object 传入的数据不一定有DOM对象，所以大多是做一些逻辑上的数据操作
  // index 是每个元素的索引号;  element  遍历内容
  ```

  

- $('选择器').each()

  ```js
  $("div").each(function(index, domEle) { xxx; } ）
  // 匹配 $("div") 这个伪数组中的每一个 DOM 对象，对 DOM 对象进行一些 DOM 上的操作
  // index 是每个元素的索引号;  element 遍历内容
  ```

- 以上两种方法多需要用到对象，有对象才能遍历，区别在于其先后顺序，
  - 是先拿到一个对象，直接调用方法对其进行遍历
  - 还是先调用遍历方法，传入需要遍历的对象







## jQuery事件

### 事件类型

- 直接 .事件类型  就可以了       .click    .mouseout        .keydown

- change事件                           当对象发生变化是触发（只响应手动改变的，有其他事件引起的改变不响应）
- oninput事件                           当文本框输入时出发，输入一个，触发一次

### 事件注册

- 使用on()可进行事件监听，可以事件委托，对于动态创建的新元素也有效（普通的时间注册方式对新创建的元素无效）

  - 事件委托

  ```js
  $('ol').on('click keydown', 'input', function (e) {})
  事件元素.on('事件类型', '子元素', function() {} )  // 可直接找内部的子元素
  ```

  - 注册不同事件

  ``` js
  $(“div”).on({
  
    mouseover: function(){}, 
  
    mouseout: function(){},
  
    click: function(){}
  
  });
  ```

### 事件解绑

- off() 方法可以移除通过 on() 方法添加的事件处理程序

  ```js
  $("p").off()                  // 解绑p元素所有事件处理程序
  
  $("p").off( "click")          // 解绑p元素上面的点击事件 后面的 foo 是侦听函数名
  
  $("ul").off("click", "li");   // 解绑指定子元素的事件委托
  ```

  

- 如果有的事件只想触发一次， 可以使用 one()来绑定事件

### trigger()

- element.trigger("type")

  ```js
  $("p").trigger("click");              // 此时自动触发点击事件，不需要鼠标点击
  
  $('div').triggerHandler('click');     // 自动触发事件【这种触发事件不会触发默认行为】
  ```

### 事件对象

- event
  - element.on(事件类型, [selector], function( event ) {})   
- 阻止默认行为：event.preventDefault()   或者 return  false 
- 阻止冒泡： event.stopPropagation()









## jQuery效果

### 显示/隐藏

- time,   easing,  fn

```js
.hide()
.show()
.toggle()

// 参数都可以省略， 无动画直接显示。
// speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
// easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
// fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
```



### 滑入/滑出

- time,   easing, fn

  ```js
  .slideDown()
  .slideUp()
  .slideToggle()
  
  // 参数都可以省略， 无动画直接显示。
  // speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
  // easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
  // fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  ```

  

### 淡入/淡出

- time,   easing, fn

  ```js
  .fadeIn()
  .fadeOut()
  .fadeToggle()
  
  .fadeTo([[speed],opacity,[easing],[fn]])     
  // 注意fadeTo必须写两个参数，speed(时间)和opacity(透明度)
  
  
  // 参数都可以省略， 无动画直接显示。
  // speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
  // easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
  // fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。
  
  ```

  

### 自定义动画

  - animate(params, [speed], [easing], [fn]);

  ```js
  // ********* params: 想要更改的样式属性，以对象形式传递，必须写。           
  // 属性名可以不用带引号， 如果是复合属性则需要采取驼峰命名法 borderLeft。其余参数都可以省略。
  // speed：三种预定速度之一的字符串(“slow”,“normal”, or “fast”)或表示动画时长的毫秒数值(如：1000)。
  // easing：(Optional) 用来指定切换效果，默认是“swing”，可用参数“linear”。
  // ********* fn:  回调函数，在动画完成时执行的函数，每个元素执行一次。 
  ```



### 事件切换

  **hover([over,]out)**

  ```js
  // over:鼠标移到元素上要触发的函数（相当于mouseenter）        mouseenter==mouseover
  // out:鼠标移出元素要触发的函数（相当于mouseleave）           mouseleave==mouseout
  // 如果只写一个函数，则鼠标经过和离开都会触发它
$('.box').hover(function () {
    // 鼠标移到元素上要触发的函数
},
function () {
    // 鼠标移出元素外要触发的函数
});
  ```



### 动画队列及其停止排队

- .stop()             放到要停止的动画之前

 //类似原生 setAttribute()









## **数据缓存** data()

- data() 方法可以在指定的元素上存取数据，并不会修改 DOM 元素结构，所以元素上无法查看。一旦页面刷新，之前存放的数据都将被移除。
  - 相当与把DOM结构当作一个变量进行存储数据，虽然数据存储在DOM中，但因为其不是HTML结构，所以无法在DOM树上显示出来

 **附加数据语法**

data(''name'',''value'')   // 向被选元素附加数据   

**获取数据语法**

data(''name'')               //   向被选元素获取数据   

**例如：**

```
$('span').data('spanindex',3);

console.log($('span').data('spanindex'));
```



## 大小/坐标！！！

### 大小

- width()、height()                                         【只算width和height】

- innerWidth()、innerHeight()                       【包含padding+width】

- outerWidth()、outerHeight()                       【包含padding、border、width】

- outerWidth(true)、outerHeight(true)          【包含padding、border、margin、width】（加了 true 参数）



### 坐标

- 在jQuery中主要获取坐标的方法有三类
  - offset()
  - position()
  - scrollTop() / scrollLeft()

- offset() 方法，**可以获取和设置**

  - 返回或设置选中元素相对于文档的坐标位置，有**属性** top left 

  ```js
  // 获取， 其中有 left top 两个 **属性值**
  元素.offset().top
  元素.offset().left
  
  // 设置， 在offset()中传入对象进行设置      **不带单位**
  元素.offset( {top: 100} );             // 设置单个属性
  元素.offset( {top: 100，left: 50} );    // 设置多个属性
  ```

- position()方法， **只能获取，不能设置**  **属性** top left

  - 返回选中元素相对于 **有定位的父级元素** 偏移位置。若父级没有定位，会一直向上找，直到文档

  ```js
  元素.position().top
  元素.position().left
  ```

- scrollTop() / scrollLeft(),   获取和设置 元素被卷起的头部位置和左侧位置

  ```js
  // 获取
  元素.scrollTop();
  元素.scrollLeft();
  
  // 设置  带参数，没有单位，为数字类型即可
  元素.scrollTop( 100 );
  元素.scrollLeft( 200 );
  ```

  





  

  

  