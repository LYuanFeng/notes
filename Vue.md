### 安装环境

```
安装node----》能使用npm
	安装npm --》 npm install -g npm 
	安装cnpm 国内装包工具--> npm install -g cnpm --registry=https://registry.npm.taobao.org
全局安装Vue脚手架
	npm i -g vue-cli
```



### 安装Vue方式 

```
1.》通过src超链接安装vue.js
2.》通过 npm i vue --save 命令下载包到本地
```



### 导入方式 

```
导入样式
	直接 import './xx.css'

导入模块
	npm安装的模块 import xxx form 'xxx'
	自定义的模块 import YYY form '路径_YYY.js'	
```



### 创建webpack项目

```
1.》命令 vue init webpack demo (webpack-->复杂模式；webpack-simple-->简单模式；demo-->项目名，要小写)
2.》项目结构意思
	build 打包配置文件所在的文件夹
		webpack.base.conf.js 打包核心配置
		build.js 构建生产版本，打生产的包
		dev-client.js 热更新的插件
		dev-server.js 启动开发项目的服务器
		check-versions.js 检查版本
	config 打包的配置
		index.js 开发配置
	src项目源码位置
		App.vue 入口组件
		main.js 项目入口文件
	static 静态资源文件
		components 文件夹，一般放公用的组件经常被复用，例如foot 、导航栏、模态框
	.babelrc ES6解析配置文件
	.gitignore Git忽略提交的配置
	.esitorconfig 编辑器的配置文件
	.postcssrc.js html添加前缀的配置
	index.html 我们单页面的入口
	package.json 记录包文件
	README.md 项目说明，描述性文件
	webpack.config.js webpack的配置
3.》让项目跑起来
	运行 npm install 下载依赖插件
	运行 npm run dev (dev-->要看package.json里scripts是不是dev; dev->开发模式) 
```



### 路由介绍

安装插件 npm i vue-router --save

```
1.》vue-router用来构建SPA
	<router-link></router-link>或者编程试路由$router.push({path:''}) -->就是a标签可以跳转
	<router-view></router-view> -->组件载体做的渲染，跳转后的展示位置
2.》动态路由匹配
	$route.params.xxxx 可以获取动态路由xxx的信息 （eg:ocalhost:8080/goods/xxx）
	模式一
	new Router({
		routes: [
			{
				path: '/goods/:goodsId' //--> :goodsId是动态的变量
				name: 'Goodist',  //-->在当前组件使用注册组件的名
				component: Goodist //-->导进来组件的名字
			}
		]
	})
	浏览器url-> localhost:8080/goods/888 (888是动态的,路由必须符合规则，要以goods开头的并且要有类		似888变量，的一个ID,要不然访问不了
	
	模式二
	new Router({
		routes: [
			{
				path: '/goods/:goodsId/user/:name' //--> :goodsId和:name是动态的变量
				name: 'Goodist',  //-->在当前组件使用注册组件的名
				component: Goodist //-->导进来组件的名字
			}
		]
	}) 
	浏览器url-> localhost:8080/goods/999/name/lee (goods开头后面跟着动态ID类似999，然后name开头	 后面跟着动态那么类似lee，必须符合这样是规则，要不然访问不了)
	
	使用场景-》商品详情页面，商品详情共用一个页面，根据ID的不同来展示不同的详情
	
3.》嵌套路由
	步骤一
	new Router({
		routes: [
			{
				path: '/goods' //一级路由
				name: 'Goodist',  //-->在当前组件使用注册组件的名
				component: Goodist //-->导进来组件的名字
				children:[
					{
						path: 'title' // 不可以 '/title',加/后就变成绝对路由了 
						name: 'title',  //-->二级在当前组件使用注册组件的名
						component: Title //-->导进来组件的名字
					},
					{
						path: 'img' // 不可以 '/img',加/后就变成绝对路由了 
						name: 'img',  //-->二级在当前组件使用注册组件的名
						component: Image //-->导进来组件的名字
					}
				]
			}
		]
	})
	步骤二 去Goodist里
	<template>
		<router-link to="/goods/title">显示标题</router-link> //路由跳转的连接 ‘to’ 属性会指定			跳转到哪里去,这个位置要写全二级URL--》 /goods/title
		<router-link to="/goods/img">显示图片</router-link>
		<router-view></router-view> //放子组件，二级路由渲染位置，上面的两个路由都切换渲染到这里
	</template>
	
	使用场景-》在选项卡中，顶部有数个导航栏，中间的主体显示的是内容；这个时候，整个页面是一个路由，然后点击	选项卡切换不同的路由来展示不同的内容，这个时候就是路由中嵌套路由。
	
4.》编程试路由
	顾名思义：通过写js来实现路由跳转，也就不需要router-link router-view来实现
	$router.push("name")
	$router.push({path:"name"})
	$router.push({path:"name?a=123"})或$router.push({path:"name",query:{a:123}})//可以传参
	$router.go(1)正数表前进，负数代表后退
	演示
	步骤一
	new Router({
		routes: [
			{
				path: '/goods' 
				name: 'Goodist',  //-->在当前组件使用注册组件的名
				component: Goodist //-->导进来组件的名字
			},
			{
				path: '/cart' 
				name: 'cart',  //-->在当前组件使用注册组件的名
				component: Cart //-->导进来组件的名字
			}
		]
	})
	
	步骤二
	假如要在Goodist组件里跳转到Cart组件
	那么就在Goodist组件做类似以下操作
	<template>
		<button @click="jump"> 跳转的到Cart </button>
	</template>
	methods:{
		jump() {
			this.$router.push("/cart")
			this.$router.push({path:"/cart?goodsId=123"})//传参写法，会通过get URL方式传参
			$router.go(-2)//后退两步，后退两个页面
		}
	}
	在Cart组件接受参数
	<template>
		<span> {{$route.query.goodsId}} </span>//$route不是$router,后面没有r。因为是单个路由，			的单数
	</template>
	
	$route.quer和$route.params区别二者还有点区别，直白的来说query相当于get请求，页面跳转的时候，可以	在地址栏看到请求参数，而params相当于post请求，参数不会再地址栏中显示
	
5.》命名路由和命名视图
	给路由定义不同的名字，根据名字匹配
	给不同的<router-view>定义名字，通过名字进行对应组件的渲染
	
	命名路由如下
	new Router({
		routes: [
			{
				path: '/goods' 
				name: 'Goodist',  //-->在当前组件使用注册组件的名
				component: Goodist //-->导进来组件的名字
			},
			{
				path: '/cart/' 
				name: 'cart',  //-->在当前组件使用注册组件的名
				component: Cart //-->导进来组件的名字
			},
			{
				path: '/title/:titleId' //动态参数
				name: 'title',  //-->在当前组件使用注册组件的名
				component: Title //-->导进来组件的名字
			}
		]
	})
	
	假如要在Goodist组件里跳转到Cart组件
	那么就在Goodist组件做类似以下操作
	<template>
		//注意要v-bind:to 来绑定，没有绑定就是原生属性了
		<router-link v-bind:to="{name: 'cart'}">跳转到Cart</router-link>
		
		//带路由参数跳转方式
		<router-link v-bind:to="{name:'title',params:{titleId:123}">跳转到Title</router-link>
	</template>
	
	命名视图如下
	<template>
		<div id="app">
			//根据name不同分以下三块显示渲染
			<router-view></router-view>//默认位置
			<router-view name='cart'></router-view>
			<router-view name='title'></router-view>
		</div>
	</template>
	new Router({
		routes: [
			{
				path: '/' 
				name: 'Goodist',  //-->在当前组件使用注册组件的名
				component:{
					default:Goodist,//默认显示
					cart:Cart,//根据cart显示Cart组件（注意大小写不同）
					title:Title//根据title显示Title组件（注意大小写不同）
				}
			}
		]
	})
	
6.》router mode
	A: history 更接近原生
	new Router({
		mode:'history'
		routes: [
			{
				path: '/goods/xxx' 
				name: 'Goodist',  
				component: Goodist 
			}
		]
	}) 
	路由URL写法 =》localhost:8080/goods/xxx
	
	B: hash 默认值URL里面有#号
	new Router({
		mode:'hash'
		routes: [
			{
				path: '/goods/xxx 
				name: 'Goodist',  
				component: Goodist 
			}
		]
	}) 
	路由URL写法 =》localhost:8080/#/goods/xxx
	

```



### 请求处理之——vue-resource

安装插件 npm i vue-resource --save

| 参数        | 类型                           | 描述                                                         |
| :---------- | :----------------------------- | :----------------------------------------------------------- |
| url         | `string`                       | 请求的目标URL                                                |
| body        | `Object`, `FormData`, `string` | 作为请求体发送的数据                                         |
| headers     | `Object`                       | 作为请求头部发送的头部对象                                   |
| params      | `Object`                       | 作为URL参数的参数对象                                        |
| method      | `string`                       | HTTP方法 (例如GET，POST，...)                                |
| timeout     | `number`                       | 请求超时（单位：毫秒） (`0`表示永不超时)                     |
| before      | `function(request)`            | 在请求发送之前修改请求的回调函数                             |
| progress    | `function(event)`              | 用于处理上传进度的回调函数 ProgressEvent                     |
| credentials | `boolean`                      | 是否需要出示用于跨站点请求的凭据                             |
| emulateHTTP | `boolean`                      | 是否需要通过设置`X-HTTP-Method-Override`头部并且以传统POST方式发送PUT，PATCH和DELETE请求。 |
| emulateJSON | `boolean`                      | 设置请求体的类型为`application/x-www-form-urlencoded`        |

全局拦截器 interceptors

```
Vue.http.interceptors.push((req,next)=>{
	//...
	//请求发送前的处理逻辑
	//...
	next((res)=>{
		//...
		//请求发送后的处理逻辑
		//...
		//根据请求状态，res参数会返回给successCallback或者errorCallback
		return res
	})
})
```

实例

```
<div id="add">
	<h1>vue-resource使用</h1>
	<a href="jacascript" v-on:click="get">Get请求</a>
	<a href="jacascript" @click="post">Post请求</a>
	<a href="jacascript" @click="jsonp">JSONP请求,跨域请求</a>
</div>
<script>
	var vm = new Vue({
            el:'#add',
            data:{
                msg:'Hello World!',
            },
            mounted:{
            	Vue.http.interceptors.push((req,next)=>{ //全局拦截器 interceptors
                    //...
                    //请求发送前的处理逻辑
                    //...
                    next((res)=>{
                        //...
                        //请求发送后的处理逻辑
                        //...
                        //根据请求状态，res参数会返回给successCallback或者errorCallback
                        return res
                    })
                })
            },
            http: {//做全局地址的配置， vue-resource的一个封装
            	root:"http://localhsh:1688/try" //地址公共部分
            },
            methods:{
                get:function(){
                    //可以直接this.$http，因为vue-resource已经挂摘到Vue里面去了
                    //params请求传参
                    //headers设置请求头 ，如token
                    //then请求后的结果 参数一成功的回调，参数失败的回调
                    this.$http.post('/try/ajax/demo_test_get.php',{
                    params:{userId:'110'},
                    headers:{token: 'asd'}
                    }).then(res =>{
                    	this.msg = res.data;
                    },err=>{
                    	this.msg = err;
                    });
                },
                post: function() {
                	//参数一 URL地址
                	//参数二 body请求参数
                	//参数三 请求的配置，如请求头
                	//then请求后的结果 参数一成功的回调，参数失败的回调
                	thiis.$http.post("demo_test_post.php",{
                		userId: "123"
                	},{
                		headers:{
                			token:'abc'
                		}
                	}).then(res => {
                		this.msg = res.data;
                	},err => {
                		this.msg = err;
                	})
                },
                sendJsonP(keyword) {
                    let url = 'https://www.baidu.com/sugrec?pre=1&p=3';
                    this.$http.jsonp(url, {
                      params: {
                        wd: keyword
                      },
                      jsonp: 'cb'//jsonp默认是callback,百度缩写成了cb，所以需要指定下                     			}
                    }).then(res => {
                      if (res.data.g) {
                        this.result = res.data.g.map(x => x['q']);
                      } else {
                        this.result = [];
                      }
                    });
                  }
            }
        });
</script>
```



### 请求处理之——axios

安装插件 npm i axios --save

实例

```
<div id="add">
	<h1>vue-resource使用</h1>
	<a href="jacascript" v-on:click="get">Get请求</a>
	<a href="jacascript" @click="post">Post请求</a>
	<a href="jacascript" @click="http">http请求</a>
</div>
<script>
    new Vue({
    	el: "app",
    	data:{
    		msg: ''
    	},
    	mounted: {
    		//请求前拦截
    		axios.interceptors.request.use(config => {
    			//这里做请求前要处理的业务逻辑
    			console.log("request init.")
    			return config
    		})
    		//响应前拦截
    		axios.interceptors.response.use(response => {
    			//这里做响应前要处理的业务逻辑
    			console.log("response init.")
    			return response
    		})
    	},
    	methods:{
    		get: function() {
    			//参数一 url地址
    			//参数二 一些设置 类似传参 和 headers
    			axios.get("../test_post.js",{
    				params: {
    					userId: '111',
    				}
    				headers:{
    					token: 'test token'
    				}
    			}).then(res => {
    				//成功
    				this.msg = res.data
    			}).catch( err => {
    				//失败
    				this.msg = err
    			})
    		},
    		post: function() {
    			//参数一 url地址
    			//参数二 body传参
    			//参数三 一些设置 例如headers 的token
    			axios.post("../test_post.js",{
    				userId: '111'
    			},{
    				headers:{
    					token: 'test token'
    				}
    			}).then(res => {
    				//成功
    				this.msg = res.data
    			}).catch( err => {
    				//失败
    				this.msg = err
    			})
    		},
    		http: function() {
    			axios({
    				url: '../test_http.js',
    				method: 'get', //请求类型 get post
    				data: { //post请求 传参
    					userId: '123'
    				},
    				params: { //get请求 传参
    					userId: '456'
    				},
    				headers: {
    					token: 'test-token'
    				}
    			}).then(res => {
    				//then 捕获结果
    				this.msg = res.data
    			})
    		}
    	}
    })
</script>
```



###  Vuex 状态管理

```
状态管理模式。它采用集中式存储管理应用的所有组件的状态。中大型单页应用，你可能会考虑如何把组件的共享状态抽取出来，以一个全局单例模式管理，不管在哪个组件，都能获取状态/触发行为
```

------

 **导入**

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

**State** 

```
const store = new Vuex.Store({
  state: {
    count: 0// 状态值只能通过下面的mutations去改变
  },
  mutations: {
    increment (state,num) {
      state.count++
    }
  }
})
获取值
this.$store.state.count; //state.count是new Vuex里面的定义键值对
改变状态值触发
this.$store.commit("increment", 100)//括号mutations里事件名,100是传参值,第二参数可传可不传
```

 **Getters** 

```
**Getters** 是个辅助工具,在其他属性上的一个衍生，类似于计算属性

const store = new Vuex.Store({
  state: {
    count: 0,// 状态值只能通过下面的mutations去改变
    name: 'jack'
  },
  mutations: {
    increment (state,num) {
      state.count++
    }
  },
  getters: {//get使用
  	userNmae(state) {  
  		retuen state.name + ',hello'
  	}
  }
})
使用
this.$store.getters.userNmae; //userNmae是state里的getters里的一个事件名
```

 **Mutations** 

 **Actions** 

```
**Actions** 提交的是 mutation的，唯一区别Action 可以包含任意异步操作

const store = new Vuex.Store({
  state: {
    count: 0// 状态值只能通过下面的mutations去改变
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
  	incrementActions(context,num){
  		context.commit("increment", num);
  }
})
改变状态值触发
this.$store.dispatch("incrementActions", 5)//括号mutations里事件名,5是传参值
```

 **Module** 