favicon.ico是网站的图标

按住shift点刷新是强制刷新

alt+鼠标左键同时编辑多行

# Vue

##### 特色

###### 声明式编程

跟js渲染的命令式编码不同，声明式更加优雅

![image-20210913205932943](C:\Users\Bomb12138\AppData\Roaming\Typora\typora-user-images\image-20210913205932943.png)

###### 虚拟DOM

先转成虚拟DOM，再转成真实DOM。在数据变化时会先与之前的虚拟DOM进行比对，有修改的部分会修改虚拟DOM，然后增加渲染，而不是像传统方式的整个重新渲染。

![image-20210913210254953](C:\Users\Bomb12138\AppData\Roaming\Typora\typora-user-images\image-20210913210254953.png)

##### 数据代理原理

![image-20210916143210041](C:\Users\Bomb12138\AppData\Roaming\Typora\typora-user-images\image-20210916143210041.png)

## vue各类写法

### 指令

#### 不要把v-if和v-show放在一个标签里

因为v-for的执行优先级高于v-if，每次都会先执行循环，带来性能浪费

```html
 <p v-if="isShow" v-for="item in items">
        {{ item.title }}
    </p>
```

##### 解决

外面放一个template来将两个指令分开

```javascript
<template v-if="isShow">
    <p v-for="item in items">
</template>
```

如果条件出现在循环内部，可通过计算属性`computed`提前过滤掉那些不需要显示的项

```js
computed: {
    items: function() {
      return this.list.filter(function (item) {
        return item.isShow
      })
    }
}
```

#### 事件

@click

@submit

#### 事件修饰符

##### 可以多个修饰符连着写，多重效果。如：@click.prevent.click

- prevent：阻止默认事件

- stop：阻止事件冒泡

- once：事件只触发一次

- capture：使用事件的捕获模式

- self：只有event.target是当前操作的元素才触发事件

- passive：事件的默认行为立即执行，无需等待事件回调执行完毕

#### 绑定样式

##### class样式

写法：:class="xxx" xxx可以是字符串、对象、数组

- 字符串写法适用于：类名不确定，要动态获取
- 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定
- 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用

##### style样式

- :style="{fontSize:xxx}" 其中xxx是动态值
- :style="[a,b]"其中a、b是样式对象

#### 内置指令

- v-cloak

  - 一个特殊属性，在vue.js加载出来接管html时会自动删掉v-cloak。使用css配合v-cloak可以解决网速慢时页面展示出{{xx}}括号的问题

  - ```css
    [v-cloak]{
                display:none
    }
    ```

- v-html

  - 可以被解析为html节点。v-html会替换掉节点中所有的内容。v-html可以识别html结构，有xss风险

  - ```js
    <div v-html="str"></div>  
    new Vue({
            el:'#root',
            data:{
                str:'<h3>你好啊!</h3>'
            },
    })
    ```

    

- v-once

  - 在初次动态渲染后，就视为静态内容，写死了。可以优化性能

  - ```html
    <h2 v-once>初始化n的值时 {{n}}</h2>
    ```

- v-pre

  - ```html
    跳过节点的编译过程。可利用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译，提高性能
    <h2 v-pre>vue其实很简单</h2>
    ```

- v-text

  - 向所在节点中渲染文本内容。跟插值语法区别：会替换掉节点原来的内容

  - ```js
    <div v-text="name"></div> 
    new Vue({
            el:'#root',
            data:{
                name:'尚硅谷',
            },
        })
    ```

#### 键盘事件

```css
@keyup.enter ="showInfo"
可以连着写，表示两个键一起按时触发。回车和退格一起按时触发
@keyup.enter.delete
```

## 生命周期

##### 创建前 创建(发送后端请求)

##### 挂载前  挂载mounted(重要)

##### 更新前 更新

##### 销毁前beforeDestroy(重要) 销毁

![生命周期](D:\Typora\生命周期.png)

## 组件

##### 实现*应用*中局部功能代码和资源的集合。

###### 想要什么组件直接引入即可

![image-20211020184502026](D:\Typora\image-20211020184502026.png)

##### 组件可以被细分的非常小

![image-20211020183836908](D:\Typora\image-20211020183836908.png)

##### 非单文件组件

###### 一个文件中包含n个组件

##### 单文件组件

###### 一个文件中只包含有1个组件

#### 传统方式的编写

###### 只存在模块化，无组件化。一个js文件一般就是一个模块

想要哪个部分的html代码只能进行html的复制粘贴和css、js文件的复制粘贴

![image-20211020183953585](D:\Typora\image-20211020183953585.png)

##### 组件化与模块化的区别

![image-20211020184647696](D:\Typora\image-20211020184647696.png)

#### Vue与VueComponent的关系

![image-20211021222716816](D:\Typora\image-20211021222716816.png)

## 脚手架 CLI

###### CLI：command line interface

###### 背后的打包工具是webpack

##### 安装

![image-20211022141454023](D:\Typora\image-20211022141454023.png)

#### 目录分析

###### babel.config.js babel配置

###### package-lock.json 包的版本和源地址

###### package.json 包的说明书

###### src/main.js 首页的js

### 总结TodoList案例

##### 组件化编码流程：

- 拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突。

- .实现动态组件：考虑好数据的存放位置，数据是一个组件在用，还是一些组件在用：
  - 一个组件在用：放在组件自身即可。
  - 一些组件在用：放在他们共同的父组件上（<span style="color:red">状态提升</span>）。

- 实现交互：从绑定事件开始。

##### props适用于：

- 父组件 ==> 子组件 通信

- 子组件 ==> 父组件 通信（要求父先给子一个函数）

##### 使用v-model时要切记：v-model绑定的值不能是props传过来的值，因为props是不可以修改的！

##### props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做。



##### 全局事件总线的本质就是自定义事件

![image-20211028173645711](D:\Typora\image-20211028173645711.png)

###### 消息订阅与发布与上图原理相同，不过一般vue里不用。而且在vue开发工具里看不到。

# 面试

##### computed

###### 定义

要用的属性不存在，要通过已有属性计算得来

###### 原理

底层借助了object.defineproperty方法提供的getter和setter

与methods实现相比，内部有缓存机制(复用)，效率更高，有调试方法

##### watch

- computed能完成的,watch都能完成

- watch能完成的功能,computed不一定能完成.例如:watch可以进行异步操作

##### mixin

将组件的公共部分抽取出来

###### 局部混入

可以单独给某个组件用

###### 全局混入

所有组件都会自带，用于写插件

##### 插件

用于加强vue，里面可以定义各种全局，包括mixin全局混入

#### react、vue中的key有什么作用？(key的内部原理) 用于diff算法

##### 虚拟dom中key的作用：

- key是虚拟dom对象的标识，当数据发生变化时，vue会根据新数据生成新的虚拟dom

- 随后vue进行新虚拟dom与旧虚拟dom的差异比较

##### 对比规则

- 旧虚拟dom中找到了与新虚拟dom相同的key
  - 若虚拟dom中内容没变，直接使用之前的真实dom
  - 若虚拟dom中内容变了，则直接生成新的真实dom，随后替换掉页面中之前的真实dom

- 旧虚拟dom中未找到与新虚拟dom相同的key
  - 创建新的真实dom，随后渲染到页面

##### 用index作为key可能会引发的问题

（1）若对数据进行逆序添加、逆序删除等破坏顺序操作：

​          会产生没有必要的真实dom更新==>界面效果没问题，但效率低

（2）如果结构中还包含输入类的dom(input/checkbox...)：

​          会产生错误dom更新==>界面会有问题

##### 开发中如何选择key？

- 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值
- 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展

​        示，使用index作为key是没有问题的

#### 数据代理双向绑定原理

##### vue2

通过Object.defineProperty()把data对象中所有属性添加到vm上 数据劫持

为每一个添加到vm上的属性，都指定一个getter/setter。在getter/setter内部去操作(读/写)data中对应的属性

##### vue3

通过代理：拦截对象中任意属性的变化，包括属性的增删改

  通过反射，对被代理对象的属性进行操作   

##### 反射就是在不知道对象的出处时，在运行时可以获取到该对象的属性或者方法

#### VUE的ajax解决方案

##### 原生xhr

##### jquery $.get

##### axios(重点)

##### fetch(跟xhr一级，不用xhr发请求时的)

##### vue-resource

#### 解决跨域问题

- cors 由后端人员解决，服务器返回的时候多返回一个标识

- jsonp 巧妙，但只能解决get请求的问题。

- 代理服务器  使用nginx和vue-cli的配置来实现

![image-20211028225906860](D:\Typora\image-20211028225906860.png)

##### 请求和响应全部由代理服务器8080来中转，public是代理服务器的根路径

### nextTick原理

###### 一般都是多次更改数据后在统一解析模板。在下次解析模板后再执行，要基于新的DOM进行某些操作时，才用这个。

例如：点击编辑后显示输入框，在新出现的输入框中显示焦点。

将里面的回调放入宏队列，然后MutationObserver在微队列中，所以解析模板后才会调用nextTick。

## Vuex(Store)

##### 概念

 在Vue中实现集中式状态（数据）管理的一个Vue插件，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信。

##### 何时使用？

多个组件需要共享数据时。

##### State

##### Getter

##### Mutations

##### Actions

#### 组件通信的方式

- props 父传子 (自带数据校验) 子传父(父给子传一个函数，子组件调用)
- 自定义事件 子传父
- 全局事件总线 (本质是自定义事件)  (bus共享数据)             emit        on
- 消息订阅与发布 类似全局时间总线                                    publish subscribe
- 插槽 父向子传结构 子传给父数据
- vuex

### VUEX流程图

##### 中途三步都由store管理

###### 其中dispatch可能有很多步

![image-20211101155057408](D:\Typora\image-20211101155057408.png)

![image-20211101155216041](D:\Typora\image-20211101155216041.png)

##### mutation和action区别

###### 别说转发请求这种了....

- mutation：专注于修改State，理论上是修改State的唯一途径。

- action：业务代码、异步请求、如果没啥可写的可以被删去。

##### mapState(普通state数据)和mapGetter(普通getter数据)用于简化计算属性的写法

##### mapAction(Action方法)和mapMutation(Mutation方法)用于简化方法的写法

## 路由

##### 路由就是一组key-value的对应关系。

- key是路径，value可能是function或Component

- 多个路由，需要经过路由器的管理

##### 前端路由

- value是Component，用于展示页面内容

- 当浏览器的路径改变时，对应的组件就会显示

##### SPA(single page web application)单页面应用

- 不开启新页签，只是局部更新
- 数据通过ajax请求获取

### vue-router

插件库，专门用来实现SPA应用

##### 注意点

- 路由组件通常存放在```pages```文件夹，一般组件通常存放在```components```文件夹。
- 通过切换，“隐藏”了的路由组件，默认是被销毁掉的，需要的时候再去挂载。
- 每个组件都有自己的```$route```属性，里面存储着自己的路由信息。
  - query参数   (接收参数){{$route.query.id}}
  - param参数 (接收参数){{$route.param.id}}

- 整个应用只有一个router，可以通过组件的```$router```属性获取到。

##### 传参

```html
//带参 
<router-link :to="{
        name:'xiangqing',
        params:{
        id:m.id,
        title:m.title
        }
 }">{{m.title}}</router-link>
//不带参
<router-link class="list-group-item" active-class="active" to="/home/news">News</router-link>
```

##### 取参

- {{$route.query.id}}

- {{$route.param.id}}

- props:['id'] 页面中直接写id就行，简化代码

##### replace和push

reaplace不将之前的组件压入栈，而是取代，无法回退

push将之前的组件压入栈，可以回退

```html
<router-link replace class="list-group-item" active-class="active" to="/home">Home</router-link>
```

##### 函数式路由

![image-20211104213058896](D:\Typora\image-20211104213058896.png)

![image-20211104213027841](D:\Typora\image-20211104213027841.png)

##### 缓存路由组件

被切走时不会被销毁

![image-20211104212957027](D:\Typora\image-20211104212957027.png)

###### 缓存组件后才拥有的回调函数

activated

deactivated

##### 路由标签

router-link  代替a标签

router-view 放组件的地方

##### 路由生命周期

beforeRouteEnter

beforeRouteLeave

##### 路由守卫

保护路由的安全(权限问题)

###### 前置守卫

切换前执行

###### 后置守卫

切换后执行

###### 独享路由守卫

只对某个组件设置，进出组件时启动

##### history与hash模式，浏览器路由的两种模式

##### hash(原理是利用html锚点)

带#，用于线下用

##### history(原理是H5推出的history身上的api)

不带#，线上用,兼容性差 ，

##### router-link、router-view、keep-alive原理

都是组件，所以需要在Router对象里面用vue.components注册组件

##### router、route、routes区别

- router:路由器对象
- routes:路由列表
- route:当前激活的路由的信息

## 打包工具

###### gulp

###### grunt

###### webpack

##### vite

可快速冷启动，无需打包

轻量快速的热重载

