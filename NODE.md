# NODE

- node 不是语言而是一个软件
- Nodejs是一个基于Chrome V8引擎的ECMAScript的运行环境



# 全局变量

- global - 	node中最大的一个对象，相当于浏览器中的window 

# 核心模块



80 Apache /http

3306 MySQL

443 https





res.sendFile() 使用绝对路径 读取并返回

写响应头 writeHead(200, 头部)  text/html.css.js......               mime 模块可以获取文件后缀





use() 可以找到文件后缀



设置访问顺序

使用 session 记录了一下，用于在需要的时候判断



中间件的使用 

```js
// app
app.use( 
	// 这里可以直接写方法，但自定义的方法中一定要有 next 参数，使用 next() 来退出中间件
	function (req,next) {
        ...
		next();
    }
)
```



使用 jsonwebtoken进行用户验证

​	



```js

// res.writeHead(statusCode, [reasonPhrase], [headers])

    第一个是HTTP状态码，如200(请求成功），404（未找到）等。
    第二个是告诉浏览器发送的数据类型
    第三个就是具体发送的是什么数据
    该格式可以识别HTML结构，编码格式是UTF-8
    res.writeHead(200,{‘Content-Type’:‘text/html;charset=UTF8’});
    该格式不可以识别HTML结构
    res.writeHead(200,{‘Content-Type’:‘text/plain;charset=UTF8’});
    该格式识别图片
    res.writeHead(200,{‘Content-Type’:‘image/jpg;charset=UTF8’});
    该格式识别样式
    res.writeHead(200,{‘Content-Type’:‘text/css;charset=UFT8’});
    最后一个告诉浏览器使用什么编码解析

// res.json('默认状态码是200')

// res.send(json对象，默认200状态码)

// res.status(404).send('发送指定状态码')

```