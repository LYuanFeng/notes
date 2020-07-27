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



### node 全局变量

```
1. __filename
当前模块文件的带有完整绝对路径的文件名（意思具体到当前文件所在的文件夹所在的文件名）

2.__dirname
获得当前文件所在目录的完整目录名（意思只具体到当前文件所在的文件夹）

3.process 很复杂，是个对象
process.env node所运行的环境变量，是个对象
process.argv node运行的时候，输入的命令是怎么样的
```



### 模块化

```
1.导入模块----》require() --可以理解为js的管理，那个先导入就先运行那个文件
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



### fs文件模块

##### 文件读取（readFile，readFileSync）

```
1、异步读取fs.readFile；
​语法：fs.readFile(filename, [encoding], [callback(err,data)])
​接收参数：
filename    文件路径
options     option对象，包含 encoding，编码格式，该项是可选的。
callback    回调，传递2个参数 异常err 和 文件内容 data
​eg:
var fs = require('fs'); 
fs.readFile('content.txt','utf-8', function(err,data){ 
 if(err){ 
  console.log(err); 
 }else{ 
  console.log(data); 
 } 
})

2、同步读取fs.readFileSync
​语法：fs.readFileSync(filename, [encoding])
​接收参数：
filename    文件路径
options     option对象，包含 encoding，编码格式，该项是可选的。
​eg:
var fs = require('fs');
var contentText = fs.readFileSync('123.txt','utf-8'); 
console.log(contentText)
```

##### 读取目录列表（readdir， readdirSync）

```
1、异步读取fs.readdir;
​语法：fs.readdir(path, [callback(err,files)])
​接收参数：
path  目录路径
callback   回调，传递两个参数 err 和 files，files是一个包含 “ 指定目录下所有文件名称的” 数组。
​eg:
var fs = require('fs');
fs.readdir('readdirtest', function(err,files){
 if(err){
  console.log(err);
 }
 console.log(files);
})

2、同步读取fs.readdirSync
​语法：fs.readdirSync(path)
​eg:
var fs = require('fs');
var readDir = fs.readdirSync('readdirtest11');
console.log(readDir);
```

##### 文件写入（writeFile，writeFileSync）

```
1、异步读取fs.writeFile ；
​语法：fs.writeFile(filename, data, [options], [callback(err)])
​接收参数：
filename      (String)            文件名称
data        (String | Buffer)    将要写入的内容，可以使字符串 或 buffer数据。
options        (Object)           option数组对象，包含：
· encoding   (string)            可选值，默认 ‘utf8′，当data使buffer时，该值应该为 ignored。
· mode         (Number)        文件读写权限，默认值 438
· flag            (String)            默认值 ‘w'
callback {Function}  回调，传递一个异常参数err。
​eg:
fs.writeFile('message.txt', 'Hello Node', function (err) {
  if (err) throw err;
  console.log('It\'s saved!');
});

2、同步读取fs.writeFileSync
​语法：fs.writeFileSync(filename, data, [options])
​接收参数：
filename      (String)            文件名称
data        (String | Buffer)    将要写入的内容，可以使字符串 或 buffer数据。
options        (Object)           option数组对象，包含：
· encoding   (string)            可选值，默认 ‘utf8′，当data使buffer时，该值应该为 ignored。
· mode         (Number)        文件读写权限，默认值 438
· flag            (String)            默认值 ‘w'
​eg:
fs.writeFileSync('message.txt', 'Hello Node');
```

##### 刪除文件（unlink，unlinkSync）

```
1、异步刪除fs.unlink ；
​语法：fs.unlink(path, [callback(err)])
​接收参数：
path           文件路径
callback     回调，传递一个异常参数err。
​eg:
var fs = require('fs');
var filepath = '126.txt';
fs.unlink(filepath, function(err){
 if(err){
  throw err;
 }
 console.log('文件:'+filepath+'删除成功！');
})

2、同步刪除fs.unlinkSync ；
​语法：fs.unlinkSync(filepath);
​接收参数：
path           文件路径
​eg:
var fs = require('fs');
var filepath = '126.txt';
fs.unlinkSync(filepath);
```



### 创建服务器流程

```
1.引入http模块

let http = require('http')
let url = requier('url')//路径模块
let util = requier('util')//util模块是一类包罗万象的模块 
//创建一个server服务器
let server = http.createServer((req,res) => {
	res.statusCode = 200 //返回状态码
	res.setHeader('Content-Type', 'text/p;ain; charset=utf-8')//设置响应头
	//res.end('hello,node.js')//响应输出
	res.end(url.parse(req.url))
})
//server.listen 监听端口
server.listen(3000, '127.0.0.1', ()=>{
	console.log('服务器运行成功')
})


2.加载静态资源

let http = require('http')
let url = requier('url')//路径模块
let util = requier('util')//util模块是一类包罗万象的模块 
let fs = requier('fs')//引入文件模块
//创建一个server服务器
let server = http.createServer((req,res) => {
	//目的返回一个index.html 的文件
	var pathName = url.parse(req.url).pathname;//获取浏览器URL中路径最终文件名
	console.log("file:"+ pathName.substring(1))//去掉 / 斜杠
    var newpathname =  pathName.substring(1)//去掉 / 斜杠 
	fs.readFile(newpathname, (err, data) =>{//fs.readFile读取文件
		if(err){
			res.writeHead(404, {
				'Content-Type': 'text/html'
			})
		}else{
			res.writeHead(200, {
				'Content-Type': 'text/html'
			})
			res.write(data.toString())
		}
		
		res.end()
	})
})
//server.listen 监听端口
server.listen(3000, '127.0.0.1', ()=>{
	console.log('服务器运行成功')
})
```



### 注意事项

```
1.防止抛出全局Error
```



### 概念理解

```
1.RPC调用
	Remote Procedure Call(远程过程调用)---》服务器与服务器之间的一种通讯
2.Ajax
	浏览器与服务器之间的通讯
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

##### 创建一个express服务器



### 包作用

```
1: nodemon  ---> npm install -g  nodemon
	作用：在编写调试Node.js项目，修改代码后，需要频繁的手动close掉，然后再重新启动，非常繁琐。现在，我们可以使用nodemon这个工具，它的作用是监听代码文件的变动，当代码改变之后，自动重启。
  
2: express
	作用：Node的框架，可以比喻为JQ，简化代码操作，适合新手

3: koa 
	作用：Node的框架，和express同一个作者。适合老手
3.1: koa-mount
	作用：koa框架中的路由中间件
3.2: koa-static
	作用：koa框架中的静态资源加载中间件
```

