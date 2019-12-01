# npm使用指南

http://blog.csdn.net/chszs

### npm介绍

npm全称为Node Package Manager，是一个基于Node.js的包管理器，也是整个Node.js社区最流行、支持的第三方模块最多的包管理器。至今，npm已经管理约24万个由开发者、公司和社区创建的模块。

### npm用法

npm的使用很简单，记住以下命令即可很好地使用它。

**npm init** 
	运行构建新项目的向导
**npm install module_name** 
	在项目中安装一个模块
**npm install -g module_name** 
	全局方式安装一个模块
**npm install module_name –save** 
	在项目中安装一个模块，并把此模块添加到项目配置文件package.json中，作为项目依赖
**npm install module_name –save-dev** 
	在项目中安装一个模块，并把此模块添加到项目配置文件package.json中，作为项目开发依赖（devDependency）
**npm list** 
	列出项目中已安装的所有模块
**npm list -g** 
	列出系统中全局安装的所有模块
**npm remove module_name** 
	从项目中移除已安装的模块
**npm remove -g module_name** 
	从系统的全局安装中移除已安装的模块
**npm remove module_name –save** 
	从项目中移除已安装的模块，并从配置依赖中移除依赖关系
**npm remove module_name –save-dev** 
	从项目中移除已安装的模块，并从配置依赖中移除开发依赖（devDependency）关系

**npm uninstall 模块**：

​	删除模块，但不删除模块留在package.json中的对应信息

**npm uninstall 模块 --save** 

​	删除模块，同时删除模块留在package.json中dependencies下的对应信息

**npm uninstall 模块 --save-dev** 

​	删除模块，同时删除模块留在package.json中devDependencies下的对应信息

**npm update module_name** 
	更新指定的已安装模块的版本
**npm update -g module_name** 
	更新指定的全局安装模块的版本
**npm -v** 
	显示npm包管理器的当前版本
**npm adduser username** 
	在npmjs.org创建一个账户
**npm whoami** 
	显示你在npmjs.org上的账户详细信息
**npm publish** 
	发布自己开发的模块到npmjs.org，要发布模块必须先有账户

### package.json说明

什么是Node.js的模块（Module）？在Node.js中，模块是一个库或框架，也是一个Node.js项目。Node.js项目遵循模块化的架构，当我们创建了一个Node.js项目，意味着创建了一个模块，这个模块的描述符文件，被称为package.json。

package.json内容出错，会导致项目出现bug，甚至阻止项目的运行。一个简单的package.json，其基本结构如下：

```
{
  "name": "test-project",
  "version": "1.0.0",
  "description": "test project",
  "private": true,
  "main": "app.js",
  "scripts": {
    "test": "node test.js",
    "start": "node app.js",
    "clean": "rm -rf node_modules"
  },
  "author": "lq",
  "license": "ISC",
  "dependencies": {
      "module-1": "*"
  },
  "devDependencies": {
      "module-2": "*"
  }
}
```


它是一个合法的JSON数据，其含义一目了然。 
要注意author参数，它的值是你在https://npmjs.org网站的有效账户名，遵循“账户名<邮件>”这样的格式，比如”User user@email.com”。

### Node.js模块的版本级别

Node.js模块有三种版本级别：

主要版本Major：X.0.0
次要版本Minor：0.X.0
补丁版本Patch：0.0.X
开发时应遵循这个约定。

npm的任务自动化
package.json中的scripts定义了一些任务，比如：

```
  "scripts": {
    "test": "node test.js",
    "start": "node app.js",
    "clean": "rm -rf node_modules"
  },
```


这个配置这定义了三个任务脚本：启动start、测试test、清理clean。要执行脚本，分别使用命令：

```
$ npm run start
$ npm run test
$ npm run clean
```


