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



### 关于Vue.use()

```
疑问：导入文件到Vue，有些需要Vue.use()，有些不需要。为什么呢？
答案：要看导入的文件有没有install

1. 作用：不需要像组件那样，每次使用他前都需要先引入一次。对于插件只要在最开始引入一次，在任何组件就可以直接使用。（即：为 Vue 添加全局功能）

2. 使用场景：A.自定组件，全局使用。  B.自定义插件（有兴趣可发布上npm）

3. 例子
	3.1 需要use的插件文件--plugin.js
    import testToast from './toast.vue'
    let plugin = {
		//vue暴露出来的一个扩展方法，在 install 方法里，添加我们插件内容。install默认方法			
		//当外界在 use 这个组件的时候，就会调用本身的 install 方法，同时传一个Vue 这个类的参数。
    	install (Vue, options) {
    		//给vue注册一个全局属性
    		Vue.prototype.$hi = '你好！' 
    		
    		//给vue注册一个全局方法
            Vue.prototype.$arrLength = function (arr) { 
				return this.arr.length
            }
            
            //给vue添加一个全局组件
            Vue.component(testToast.name, testToast) // testPanel.name 组件的name属性
    	}
    }
    export default plugin
	
	3.2 需要导进去的全局组件-----toast.vue内容
	<template>
  	<div>***省略***</div>
  	<template>
  	<script>
    export default {
    // 这里需要注意下，采用的全局注入我们的组件，所以在后面因为我们的组件后，会直接使用这个name命名的标签
    name: 'test-toast',   
        data () {
            return {
            	checkedNumber: ''
            }
        }
    }
    
    3.3 需要在-----main.js 全局import 
    import Vue from 'vue'
    import App from './App'
    import router from './router'
    import testPlugin from './plugin.js' //导入我们自定义插件
    
    Vue.use(testPlugin)//use步骤重要
    
    new Vue({
    	el: '#app',
    	router,
    	components: {App},
    	temlate: '<App/>'
    })
    
    3.4 具体页面使用我们的插件-----haha.vue
    <template>
  	<div>
  		<test-toast></test-toast>//之前全局注册的组件 toast.vue
  	</div>
  	<template>
  	<script>
    export default {
    	name: 'haha',
        data () {
            return {
            	testArr: ['jj', 'hh']
            }
        },
        created() {
         console.log(this.$hi)//会调用上面插件的属性
         console.log(this.$arrLength(this.data.testArr))//会调用上面插件的方法
        }
    }
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



### Vue传值方式

前言：七种传值方式为

1. props -- 属性传值
2. $refs -- 父组件获取子组件数据
3. $parent -- 子组件获取父组件数据
4. $emit - $on -- 通知传值(广播传值)
5. Vuex -- 本地传值
6. 路由传值
7. provide与inject -- 子孙组件跨级访问父组件方法



在介绍组件传值之前先明确三种组件关系：父子组件、兄弟组件、无关系组件

![](.\img\Vue组件关系图.jpg)



上图关系基于：A、B组件同一时刻只存其一的情况下，其中：



- A是C、D、E的父组件，B是F、G、H的父组件
- C、D、E是A的子组件，F、G、H是B的子组件
- C和D、E是兄弟组件，F和G、H是兄弟组件。但E、F不是兄弟组件
- A和B是无关系组件，E和F也是无关系组件



#### 一、属性传值--props

**1.可传值类型**

- 固定值
- 绑定属性
- 方法
- 本类对象



**2.操作步骤**

①.父组件调用子组件的时候，绑定动态属性   

 <htitle mess="父组件给子组件传值"></htitle>

②. 在子组件里边通过props,接收父组件传过来的值



**3.适用场景**

仅适用于 父组件给子组件传值



**4.属性介绍**

组件属性定义：

```
props:["mess","bindMsg","run","fatherThis"],
```

**子组件验证也可传入参数的合法性：**

```
props:{
    'mess':String,
    'bindMsg':[String, Number],
    'run':Function, 
    'fatherThis':Object,
}
```

更多props请查看Vue官网:[https://cn.vuejs.org/v2/api/?#props](https://links.jianshu.com/go?to=https%3A%2F%2Fcn.vuejs.org%2Fv2%2Fapi%2F%3F%23props)



**5.示例代码**

父组件：

```
<template>
  <div id="app">
    <htitle mess="父组件给子组件传值了" :bindMsg="msg" :run="run" :fatherThis="this"></htitle>
  </div>
</template>
```



子组件：

```
<template>
    <div class="divfirst">
        <span>{{mess}}</span>
        <h1>{{bindMsg}}</h1>
        <button @click="run()">点击调用父组件方法</button>
        <button @click="getPrasent()">点击获取父组件实体（实体拿到可以使用用父组件的方法和属性了）</button>
    </div>
</template> 

<script>
export default {
    props:{
        'mess':String,
        'bindMsg':[String, Number],
        'run':Function, 
        'fatherThis':Object,
    },
    data(){
        return {}
    },
    methods:{
        getPrasent(){
            this.fatherThis.run();
            alert(this.fatherThis.msg);
        } 
    }
}
</script>
```



#### 二：父组件获取子组件数据--$refs

父组件通过$refs获取子组件的数据和方法

**1.可获取类型**

- 子组件属性
- 子组件方法



**2.操作步骤**

1.调用子组件的时候调用一个ref
 <v-fgsheader ref="header"></v-fgsheader>
 2.在父组件中通过
 this.$refs.header.属性
 this.$refs.header.方法



**3.适用场景**

子组件给父组件传值



**4.示例代码**

父组件

```
<template>
    <div class="FGSHome">
        <v-fgsheader ref="header"></v-fgsheader>
        <button @click="getChildProp()">获取子组件的属性的值</button>
        <button @click="getChildMethod()">获取子组件的方法</button>
    </div>
</template>

<script>
import FGSHeader from './FGSHeader.vue'
    export default{

        data(){
            return { }
        },
        components:{
            'v-fgsheader':FGSHeader,
        },
        methods: {
          getChildProp(){
              alert(this.$refs.header.msg);
          },  
          getChildMethod(){
              this.$refs.header.run();
          }
        },
    }
</script>
```



子组件

```
<script>
    export default{
        data(){
            return {
                msg:"我是子组件header的值哟"
            }
        },
        methods:{
            run(){
                alert("这是子组件Header的方法+"+this.msg);
            }
        }
    }
</script>
```



#### 三、子组件获取父组件数据--$parent

子组件通过$parent获取父组件的数据和方法，这种传值方式实际上类似于上边的属性传值中父组件给子组件的传递了子类对象`this`,只不过Vue官方给封装好了。



**1.可获取类型**

- 父组件属性
- 父组件方法



**2.操作步骤**

直接在子组件中使用`this.$parent.XX`，不需要做任何多余操作。



**3.适用场景**

父组件给子组件传值



**4.示例代码**

子组件

```
getFatherProp(){
    alert(this.$parent.fatherMsg); 
},
getFatherMethod(){
    this.$parent.fatherRun();
}
```



#### 四、通知传值(广播传值) -- $emit - $on

**1.可传值类型**

Vue官网只写了`[...args]`，故通知/广播传值我们定为只传基本数据类型，不能传方法。



**2.操作步骤**

1、新建一个js文件   然后引入vue  实例化vue  最后暴露这个实例

2、在要广播的地方引入刚才定义的实例

3、通过 VueEmit.$emit('名称','数据')传播数据

4、在接收收数据的地方通过 $on接收广播的数据
 VueEmit.$on('名称',function(){})



**3.适用场景**

适用于父子组件、兄弟组件间进行传值。
**注意**无关系组件不能用这种方式传值。(笔者理解是：对于上图中的A、B组件。假设A广播通知，B接收通知。挂载A的时候B卸载了，也就是说B被内存销毁了,B是接收不到广播的)



**4.属性介绍**

对于通知传值而言，可以一人广播，然后多者接收。



**5.示例代码**

vueEvent.js

```
import Vue from 'vue'
var vueEvents = new Vue();
export default vueEvents;
```



兄弟组件C(广播者)

```
import vueEvents from '../Model/vueEvent.js'

sendEmit(){
      var numbery =  (Math.random()+300).toFixed(3);
      vueEvents.$emit('notifyToNew',this.homeMsg+numbery);
 }
```



兄弟组件D(接收者)

```
import vueEvents from '../Model/vueEvent.js'
mounted(){
     var _this = this;
     vueEvents.$on("notifyToNew",function(data_P){
            //注意this的作用域
           console.log('广播传过来的值是'+data_P);
          _this.receive = data_P;
    })
}
```



####  五、本地传值--Vuex 状态管理

```
状态管理模式。它采用集中式存储管理应用的所有组件的状态。中大型单页应用，你可能会考虑如何把组件的共享状态抽取出来，以一个全局单例模式管理，不管在哪个组件，都能获取状态/触发行为
```

------

**准备工作**

1 src新建一个vuex文件夹
2 vuex文件夹里新建一个store.js
3 安装vuex  `npm install vuex --save`
4 在刚才创建的store.js 中引入vue、vuex 引入vuex 并且use



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





**JS自带的本地存储`localStorage`**

`localStorage`： String(可通过`JSON`进行json数据与String之间的转化)

存:

```
localStorage.setItem('tolist',JSON.stringify(this.tolist));
```

取：

```
var tolist = JSON.parse(localStorage.getItem('tolist'));
```



#### 六、路由传值

**注意：**

1.父组件push使用this.$router.push
2.在子组件中获取参数的时候是this.$route.params



**1 、动态路由传值**

```
  1.1 配置动态路由
      routes:[
         //动态路由参数  以冒号开头
         {path:'/user/:id',conponent:User}
       ]

   1.2 传值
     第一种写法 :  <router-link :to="'/user/'+item.id">传值</router-link>
     第二种写法 : goToUser(id) {
                    this.$router.push( {path:'/user/'+id});
                  }
   1.3 在对应页面取值
       this.$route.params;  //结果:{id:123}
```



**2、 Get传值(类似HTMLGet传值）**

```
 2.1 配置路由
     const routes = [{path:'/user',component:User},]
 2.2 传值  
     第一种写法 : <router-link :to="'/user/?id='+item.id">传值</router-link>
     第二种写法 : goToUser(id) {
                        //'user' 是路径名称
                      this.$router.push({path:'user',query:{ID:id}});
                  }
 2.3 在对应页面取值
     this.$route.query;  //结果 {id:123}
```

Tips：路径传递参数会拼接在URL路径后



**3 、命名路由push传值**

```
3.1 配置路由
   const routes = [{path:'/user',name: 'User',component:User},]
3.2 传值  
        goToUser(id) {
                //'User' 是路径重命名
              this.$router.push({name:'User',params:{ID:id}});
           }
3.3 在对应页面取值
       this.$route.params;  //结果:{id:123}
```

Tips：命名路由传递参数不在URL路径拼接显示



#### 七、跨级访问爷组件数据 -- provide与inject

**1.成对出现：**provide和inject是成对出现的

**2.作用**：用于父组件向子孙组件传递数据

**3.使用方法：**provide在父组件中返回要传给下级的数据，inject在需要使用这个数据的子辈组件或者孙辈等下级组件中注入数据。

**4.使用场景：**由于vue有$parent属性可以让子组件访问父组件。但孙组件想要访问祖先组件就比较困难。通过provide/inject可以轻松实现**跨级访问父组件**的数据

**5.示例代码**

父组件

```
<template>
    <div>
        <inject-test></inject-test>
    </div>
</template>
<script>
    import InjectTest from "./injectTest";
    export default{
        components: {InjectTest},
        name: 'ProvideTest',
        provide(){
            return {
                parentTest:this//通过provide提供自身所有属性
            }
        },
        methods: {
            printMessage(msg){
                alert(msg)
            }
        }
    }
</script>
```

子组件或孙组件

```
<template>
    <div>
        <button  @click="handleClick">点击我调用父组件方法</button>
    </div>
</template>
<script>
    export default{
        name: 'InjectTest',
        inject:['parentTest'],
        data(){
            return {}
        },
        methods: {
            //子组件调用父组件方法
            handleClick(){
                this.parentTest.printMessage("message!")
            }
        }
    }
</script>
```

