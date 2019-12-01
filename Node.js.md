# node.js



### HTTP协议

```
1.》请求（浏览器发给服务器）
	请求行： GET/index.html HTTP/1.1  (调试)
	请求头： 以键值对的形式呈现，告诉服务器浏览器的一些信息（eg: User-Agent 告诉服务器，我们现在是通过	 PC、移动端访问的）
	请求体：一般只有POST才有，就是浏览器交给服务器的数据
	
2.》响应 （服务器返回给浏览器）
	状态行：HTTP/ 200 OK 调试
	请求头： 以键值对的形式呈现，告诉服务器浏览器的一些信息（eg: User-Agent 告诉服务器，我们现在是通过	 PC、移动端访问的）
	请求体：一般只有POST才有，就是浏览器交给服务器的数据
```



### 请求方式

```
1.get请求
	通常是URL后面带值（eg:URL?xxx）
	相同的URL会有缓存，可以通过URL后加随机参数解决
2.post请求
	通常对象键值对JSON
3.link标签的href属性 请求css资源
4.script标签的src属性 请求js资源
5.a标签的href属性
5.img标签的src属性 请求图片资源地址，或者是个base64图片资源
---------以上情况都会想服务器发请求----------------
```



### node.js 启动方法

```
1.原始node命令
	在项目目录下输入命令： node xxx.js
```



### 模块化

```
1.导入模块----》require()
	导入自定义模块
		const xxx = require(--path路径--xxx.js)
	导入第三方模块
		const xxx = require('模块包名')
2.导出模块-----》
	导出一个对象
		module.exports = {}
		
		module.exports.xxx = xxx
		module.exports.yyy = yyy
		这种写法，module是可以省略的

	如果只需要在一个模块中，导出唯一的一个成员
		module.exports.xxx = xxx
		module.exports = 成员

	开发建议:
		如果要到处多个成员，就到处成对象，最好这样写
			module.exports = {}
			
		如果只需要导出唯一一个
			module.exports = 成员
```



### express框架

```
1.》查express版本 express --version
2.》全局安装express生成器命令 npm i -g express-generator
3.》创建一个express项目 ，在项目目录下命令 express server
	目录内容如下
	bin 文件夹，可执行文件
	pubilc 静态资源文件夹
	routes 路由文件
	view 文件
	app.js 入口文件
	
	
```

