# transform

转换关键词：**transform** 在其后跟着不同的值进行不同的变换操作



# 种类

- 平移
- 旋转
- 缩放





# 二维变换

## 平移

- **transform: translate(x,y)**
- 只能设置两个值，当只设置一个值时，默认该值为x轴上的
- 值可以为负，百分比（以自己为基准）
- 变换基准为原位置，而且不脱标（原来的位置是被占着）

```js
   /* 位移 */
   /* transform: translate(100px,100px); */
   /* transform: translate(-100px,-100px); */
   /* transform: translate(100px); */
   transform: translate(-50%,-50%);

   // 负值的 -50% 可用着盒子居中时
```







## 旋转

- **transform:rotate(xxx deg)**
- 旋转中心：transform-origin

```js
   /* 旋转 */
   /* 顺时针旋转45度 */
   /* transform: rotate(45deg);   */

   /* 逆时针旋转45度 */
   /* transform: rotate(-45deg);  */

	// 默认的旋转中心在 盒子的中心点上
	// 设置 
	transform-origin: left top;

	// 1. 水平取值：left| center| right |像素
	// 2. 垂直取值：top | center| bottom|像素
	
	// 这两个坐标是参照盒子的 -原点- 来的，即 --左上角--
```





## 缩放

- **transform:scale(number,number)**

```js
	// 成比例缩放 
	/* 缩放-放大2.5倍数 */
	transform: scale(2.5)
	transform: scale(0.5)
	// 之后的大小是乘以这个系数 小于1缩小 大于1 放大

	// 分别缩放
	transform: scale(2.1, 0.3)
	// 分别表示        宽    高
	
```





# 三维变换

- 近大远小
- 后面遮挡不可见
- perspective: 像素值;                                  --- 视距,加在父盒子上(有transform属性的父元素上)
- transform-style: preserve-3d;                    --- 保持盒子立体，加在父盒子上



## 平移

- transform: translatX() translateY() translateZ()
- 三维的平移变换有三个坐标方向，不能合在一起写，要写多个的话加空格进行连接

- **透视：perspective**



## 旋转

- transform: rotateX(xxx deg); 
- transform: rotateY(xxx deg); 
- transform: rotateZ(xxx deg); 



- 对于立方体是由多个盒子旋转和位移组成的，多个盒子需要有一个父盒子
- 若要显示并且要保持立体空间，需要给父盒子设置样式属性 **`transform-style: preserve-3d; `** 





# 动画/过渡

- 过渡：**transition: all 300ms;**          - 谁需要过渡效果而且有变化就加给谁，不局限于二/三维变换，大小，背景，位置都可以用





- 动画
  - 先定义，再使用

```js
语法:
@keyframes 动画序列名称 {
  from {
      开始状态
  }  
  to {
      结束状态
  }
}
或者
或者
@keyframes 动画序列名称 {
  0%{
      开始状态
  }
  10%
  .....   // 任意多个或没有
  100%{
      结束状态
  }
}
```





### 2.2 调用动画（播放电影）

- 各种相关设置属性

> | 属性                          | 描述                                                         |
> | :---------------------------- | :----------------------------------------------------------- |
> | @keyframes                    | 定义动画                                                     |
> | **animation-name**            | 规定 @keyframes 动画的名称。                                 |
> | **animation-duration**        | 规定动画完成一个周期所花费的时间。                           |
> | animation-timing-function     | 规定动画的速度曲线。默认是 "ease" 缓冲，linear匀速，steps(数字)步长。 |
> | animation-delay               | 规定动画何时开始。默认是 0。                                 |
> | **animation-iteration-count** | 规定动画被播放的次数。默认是 1。还有 infinite                |
> | animation-direction           | 动画是否在下一周期逆向地播放。默认是 "normal"，alternate逆播放 |
> | animation-play-state          | 规定动画是否正在运行或暂停。默认是 "running"。还有“paused”   |
> | animation-fill-mode           | 规定动画结束后状态，保持 forwards 回到起始 backwards         |
> | **animation**                 | 所有动画属性的简写属性                                       |

- 调用动画

```js
// 调用动画
animation-name: name             -- @keyframes 动画的名称。
animation-duration: time         -- 规定动画完成一个周期所花费的时间。
animation-timing-function: func  -- 规定动画的速度曲线。默认是 "ease" 缓冲，linear匀速，steps(数字)步长。
animation-delay: time            -- 延迟时间
```



- 简写格式：

  ```
  animation: 动画的名称 动画持续的时间 运动的曲线 延迟时间 动画的次数 是否可以逆播 是否保持结束状态;
  ```

