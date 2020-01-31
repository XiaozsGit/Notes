- 首先要明确，TS也是一种语言，而既然是一种语言，那么他就拥有它自己的一些语法，就像java语言中那些类和接口的继承一样的，要学习这门语言中的语法



```tsx
// 数组类型的定义 
// 1 - 
let list: number[] = [1, 2, 3];  // 数组项是数字数组
// 2 - 
let list: Array<number> = [1, 2, 3] // 数组项是数字的数组

// 特殊 
// 元组 - 对于已知数组长度的数组进行的一种定义，数组项的类型不一定相同
let x: [string, number];
// 注意要点： 已知数组长度


// 类型的声明还有 any | void （可以对函数声明，确定的是函数的返回值）
// any 和 void 相当于是一组反义词，void是没有任何类型，只能被赋值为 underfined 或者 null
// underfined 和 null 是任何类型的子类型，也就是说他们也可以赋给 number 等（待测试）
// 类型的声明式ts相对于js中最大的区别
// 类型的声明可以组合，由某种自定义的类型组成的数组或对象


// never 类型
// 被定义为 never 类型的函数必须存在永远无法到达的终点，大概意思就是不能返回，这个过程不能结束
```





create（value） 好像是一个判断传入的数据是否是对象类型的方法





类型断言  2种

相当于告诉 ts 我十分确切的知道我在操作的这个数据是什么类型，你使用这个方法就行了（好比类型转换）



```js
// 1 使用 <类型> 
let someValue: any = "this is a string"; 
// 从数据类型上看，someValue 可能是其他类型，不能使用length方法，所以要转一下
// 或者理解为，虽然我是定义的 any 但是在实际情况中 我知道这一定会是一个 string 
let strLength: number = (<string>someValue).length; 
// 2 使用 as 关键字
let someValue: any = "this is a string"; 
let strLength: number = (someValue as string).length; 
```





```tsx
// 接口的定义
interface SquareConfig {
  color?: string;
  width?: number;
}
```

