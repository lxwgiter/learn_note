## Vue.js是什么

**定义：**

​	一套用于构建用户界面的**渐进式框架**



**特点：**

- 采用**组件化**模式，提高代码复用率、让代码更好维护
  - 体现为后缀名为vue的文件，.vue文件中包含了HTML、CSS、JavaScript代码，便于抽取和封装

- **声明式编码**，让编码人员无需直接操作DOM，提高开发效率
  - 体现在Vue模板，让我们不再做原生DOM的复杂操作，而是让Vue做

- 使用虚拟DOM+优秀的Diff算法，尽量复用DOM节点
- 遵循 MVVM 模式
- 编码简洁, 体积小, 运行效率高, 适合移动/PC 端开发
- 它本身只关注 UI, 也可以引入其它第三方库开发项目



 **Vue 周边库**

1. vue-cli: vue 脚手架
2. vue-resource
3. axios
4. vue-router: 路由
5. vuex: 状态管理
6. element-ui: 基于 vue 的 UI 组件库(PC 端)



**学习文档：**

[https://v2.cn.vuejs.org/]: 



## Vue开发环境配置

### 使用Vue

> Vue源代码分为开发版本和生产版本，区别在于：
>
> - 开发版本包含完整的警告和调试模式
> - 生产版本体积小，但是删除了警告

- 方式一：在线引入(CDN加速)

  - 开发版本：

  - <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>

  - 生产版本：

  - <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14"></script>

- 方式二：下载到本地



### Vue devtools

当我们运行一个vue代码时，浏览器会提示如下内容：



![](E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\1.png)



- 第一个提示解决办法：安装Vue devtools插件

- 第二个提示解决办法：在使用Vue之前先进行配置：

```js
Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
```

扩展：

- **当引入Vue.js文件后，Vue会被注册到全局，以构造函数的形式存在**

```html
<script type="text/javascript" >
   Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
   console.log(Vue);
</script>
```

<img src="E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\2.png"  />



- Vue.config.productionTip其实就是修改Vue的全局配置，更多全局配置移步API
- **查看Vue实例对象**

![](E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\4.png)



## Vue之HelloWorld

### 使用Vue做一个HelloWorld案例



```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8" />
      <title>初识Vue</title>
      <!-- 引入Vue -->
      <script type="text/javascript" src="../js/vue.js"></script>
   </head>
   <body>
      <!-- 准备好一个容器 -->
      <div id="demo">
         <h1>Hello，{{name.toUpperCase()}}，{{address}}</h1>
      </div>

      <script type="text/javascript" >
         Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。
         //创建Vue实例
         new Vue({
            el:'#demo', //el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串。
            data:{ //data中用于存储数据，数据供el所指定的容器去使用，值我们暂时先写成一个对象。
               name:'hello',
               address:'world'
            }
         })
      </script>
   </body>
</html>
```

### 案例分析

- 我们可以使用devtools去调试Vue对象

<img src="E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\3.png" style="zoom: 80%;" />



- 网络请求中有个以**favicon.ico**为资源的请求，这是在请求标签页图标，WebStrom可以自动处理，vscode需要安装live server插件，这个插件帮助我们模拟服务器。



### 案例总结

- 想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象；
- 配置对象里可以配置el、data等属性，el用于指定Vue挂载的容器，data用于存储容器需要使用的数据
- root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法；
- root容器里的代码被称为【Vue模板】；
  - 对模板的解析步骤为，加载容器(模板)、加载Vue对象，Vue对象解析模板、返回模板

- Vue实例和容器是一一对应的；
  - 当多个容器对应一个Vue实例，只有最开始的容器会被正确处理
  - 当多个Vue实例对应一个模板，会直接报错

- {{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性；
- 一旦data中的数据发生改变，那么页面中用到该数据的地方也会自动更新；



## Vue模板

html 中包含了一些 JS 语法代码，语法分为两种，分别为：

1. 插值语法（双大括号表达式）
2. 指令（以 v-开头）



### 模板语法

- 插值语法

​		功能: 用于解析标签体内容

​		书写位置：**用于HTML标签的标签体中**

​		写法：{{xxx}}，xxx是js表达式，且可以直接读取到data中的所有属性。



- 指令语法

​		功能: 解析标签属性、解析标签体内容、绑定事件

​		书写位置：**HTML的标签属性上**

​		写法：v-bind:href="xxx" 或  简写为 :href="xxx"，xxx同样要写js表达式，且可以直接读取到data中的所有属性。



```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8" />
      <title>模板语法</title>
      <!-- 引入Vue -->
      <script type="text/javascript" src="../js/vue.js"></script>
   </head>
   <body>
      <div id="root">
         <h1>插值语法</h1>
         <h3>你好，{{name}}</h3>
         <hr/>
         <h1>指令语法</h1>
         <a v-bind:href="school.url.toUpperCase()" x="hello">点我去{{school.name}}学习1</a>
         <a :href="school.url" x="hello">点我去{{school.name}}学习2</a>
      </div>
   </body>

   <script type="text/javascript">
      Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

      new Vue({
         el:'#root',
         data:{
            name:'jack',
            school:{
               name:'尚硅谷',
               url:'http://www.atguigu.com',
            }
         }
      })
   </script>
</html>
```

### 常用模板

#### v-bind

- 作用：用于给某属性绑定一个值
- 缩写：“  **:**  ”
- 可用于自定义属性
- 单向绑定：数据只能从data流向页面。



#### v-model

- 作用：用于给某属性绑定一个值
- 缩写：v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值。
- 双向绑定：数据不仅能从data流向页面，还可以从页面流向data。
- 双向绑定一般都应用在表单类元素上（如：input、select等）



```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8" />
      <title>数据绑定</title>
      <!-- 引入Vue -->
      <script type="text/javascript" src="../js/vue.js"></script>
   </head>
   <body>
      <div id="root">
         <!-- 普通写法 -->
         单向数据绑定：<input type="text" v-bind:value="name"><br/>
         双向数据绑定：<input type="text" v-model:value="name"><br/>

         <!-- 简写 -->
         单向数据绑定：<input type="text" :value="name"><br/>
         双向数据绑定：<input type="text" v-model="name"><br/>

         <!-- 如下代码是错误的，因为v-model只能应用在表单类元素（输入类元素）上 -->
         <!-- <h2 v-model:x="name">你好啊</h2> -->
      </div>
   </body>

   <script type="text/javascript">
      Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

      new Vue({
         el:'#root',
         data:{
            name:'尚硅谷'
         }
      })
   </script>
</html>
```



## el与data的两种写法

- el有2种写法
  - new Vue时候配置el属性。
  - 先创建Vue实例，随后再通过vm.$mount('#root')指定el的值。

- data有2种写法
  - 对象式
  - 函数式(不包括箭头函数)

```html
<!DOCTYPE html>
<html>
   <head>
      <meta charset="UTF-8" />
      <title>el与data的两种写法</title>
      <!-- 引入Vue -->
      <script type="text/javascript" src="../js/vue.js"></script>
   </head>
   <body>
      <div id="root">
         <h1>你好，{{name}}</h1>
      </div>
   </body>

   <script type="text/javascript">
      Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

      //el的另一种写法
     const v = new Vue({
         data:{
            name:'尚硅谷'
         }
      })
      v.$mount('#root') 

      //data的函数式写法
      new Vue({
         el:'#root',
         data(){
            console.log('@@@',this) //此处的this是Vue实例对象，若是箭头函数，则this为window
            return{
               name:'尚硅谷'
            }
         }
      })
   </script>
</html>
```



## MVVM模型

​	MVVM模型不是Vue原创，而是Vue的设计借鉴了MVVM模型



**什么是MVVM模型**

1. M：模型(Model) ：对应 data 中的数据
2. V：视图(View) ：模板
3. VM：视图模型(ViewModel) ： Vue 实例对象



<img src="E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\5.png" style="zoom: 67%;" />







## 数据代理



### Object.defineProperty

作用：用于给某个对象设置属性

用法：**Object.defineProperty(对象，属性名，配置项)**

参数说明：

- **对象**：指明方法作用于那个对象

- **属性名**：字符串类型

- **配置项**：表示一个配置对象，该对象可以用以下**四个属性值**和**两个方法**

  - **四个属性**：
    - **value**：用于指定属性的值
    - **enumerable**: 控制属性是否可以枚举(for in)，默认值是false
    - **writable**: 控制属性是否可以被修改，默认值是false
    - **configurable**:  控制属性是否可以被删除，默认值是false

  - **两个方法**：
    - **get()**:当有人读取该对象的该属性时，get函数(getter)就会被调用，且返回值就是get方法返回的值
    - **set(value):**当有人修改该对象的该属性时，set函数(setter)就会被调用，且会收到修改的具体值



**对四个属性进行如下验证**：

```js
let person = {
   name:'张三',
   sex:'男',
}
//Object.defineProperty(对象，属性，配置项)
Object.defineProperty(person,'age',{
   value:18,
   //enumerable:true, //控制属性是否可以枚举，默认值是false
   // writable:true, //控制属性是否可以被修改，默认值是false
   // configurable:true //控制属性是否可以被删除，默认值是false
})
//不可枚举性验证
console.log(Object.keys(person))
//不可删除不可修改性验证
console.log(person)
```



<img src="E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\7.png"  />



**对两个方法进行如下验证：**



```js
let number = 18
let person = {
   name:'张三',
   sex:'男',
}
//Object.defineProperty(对象，属性，配置项)
Object.defineProperty(person,'age',{
   //当有人读取person的age属性时，get函数(getter)就会被调用，且返回值就是age的值
   get(){
      console.log('有人读取age属性了')
      return number
   },
   //当有人修改person的age属性时，set函数(setter)就会被调用，且会收到修改的具体值
   set(value){
      console.log('有人修改了age属性，且值是',value)
      number = value
   }
})
console.log(person)
```



![](E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\8.png)





### 什么是数据代理

数据代理：**通过一个对象代理对另一个对象中属性的操作（读/写）**



```js
let obj = {x:100}
let obj2 = {y:200}

Object.defineProperty(obj2,'x',{
   get(){
      return obj.x
   },
   set(value){
      obj.x = value
   }
})
```

> obj2实现了对obj中x属性的代理



### Vue中的数据代理

Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）

Vue中数据代理的好处：让开发者更加方便的操作data中的数据



**原理：**

1. 通过Object.defineProperty()把data对象中所有属性添加到vm上。
2. 为每一个添加到vm上的属性，都指定一个getter/setter。
3. 在getter/setter内部去操作（读/写）data中对应的属性。



示例：

```html
<!-- 准备好一个容器-->
<div id="root">
   <h2>学校名称：{{name}}</h2>
   <h2>学校地址：{{address}}</h2>
</div>
```



```js
const vm = new Vue({
   el:'#root',
   data:{
      name:'尚硅谷',
      address:'宏福科技园'
   }
})
console.log(vm);
```

**打印vm如下：**

![](E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\9.png)





**原先的data对象在Vue实例中表现为_data属性，证明如下：**



```js
let data={
   name:'尚硅谷',
   address:'宏福科技园'
};
const vm = new Vue({
   el:'#root',
   data
})
console.log(vm._data===data);//true
```

**这个代理模型可以表示为：**





<img src="E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\6.png" style="zoom: 80%;" />



> **扩展：点开_data对象后，仍显示"点击查看属性"字样，这不是因为做了数据代理，而是因为数据劫持。**
>
> **这是因为Vue要实现data中数据改变立刻刷新页面而做的监听。**



## 事件处理

### 事件的基本使用

- 模板中使用v-on:xxx 或 @xxx 绑定事件，其中xxx是事件名；
- 事件的回调需要配置在methods对象中，最终会在vm上；
- methods中配置的函数，不要用箭头函数！否则this就不是vm了；
- methods中配置的函数，都是被Vue所管理的函数，this的指向是vm 或 组件实例对象；
- @click="demo" 和 @click="demo($event)" 效果一致，但后者可以传参；后者书要是为了解决多个参数无法传递event对象的情况，所以用$event对标识



**演示：**

```html
<!-- 准备好一个容器-->
<div id="root">
   <h2>欢迎来到{{name}}学习</h2>
   <!-- <button v-on:click="showInfo">点我提示信息</button> -->
   <button @click="showInfo1">点我提示信息1（不传参）</button>
   <button @click="showInfo2($event,66)">点我提示信息2（传参）</button>
</div>
```



```js
const vm = new Vue({
   el:'#root',
   data:{
      name:'尚硅谷',
   },
   methods:{
      showInfo1(event){
         console.log(event.target.innerText)
         console.log(this) //此处的this是vm
      },
      showInfo2(event,number){
         console.log(event,number)
         console.log(event.target.innerText)
         console.log(this) //此处的this是vm
      }
   }
})
```



![](E:\BaiduNetdiskDownload\学习笔记\Vue2&Vue3\img\10.png)





### 事件修饰符

- prevent：阻止默认事件（常用）；
- stop：阻止事件冒泡（常用）；
- once：事件只触发一次（常用）；
- capture：使用事件的捕获模式，捕获阶段调用函数；
- self：只有event.target是当前操作的元素时才触发事件，也可以用于阻止冒泡。
- passive：事件的默认行为立即执行，无需等待事件回调执行完毕；



**prevent修饰符的使用：**

```html
<!-- 阻止浏览器默认行为（常用） -->
<a href="http://www.baidu.com" @click.prevent="showInfo">点我提示信息</a>
```



**stop修饰符的使用**：

```html
<!-- 阻止事件冒泡（常用） -->
<div class="demo1" @click="showInfo">
   <button @click.stop="showInfo">点我提示信息</button>
</div>
```



**once修饰符的使用：**

```html
<!-- 事件只触发一次（常用） -->
<button @click.once="showInfo">点我提示信息</button>
```



**capture修饰符的使用**

```html
<!-- 使用事件的捕获模式 -->
<div class="box1" @click.capture="showMsg(1)">
   div1
   <div class="box2" @click="showMsg(2)">
      div2
   </div>
</div>
```

**self修饰符的使用：**

```html
<!-- 只有event.target是当前操作的元素时才触发事件； -->
<div class="demo1" @click.self="showInfo">
   <button @click="showInfo">点我提示信息</button>
</div>
```

**passive修饰符的使用：**



```html
<!-- 事件的默认行为立即执行，无需等待事件回调执行完毕； -->
<!-- 页面效果是滚动条，demo是计算量极大的函数，当我们滑动滚轮时，demo不计算完成，滚动条就不会滑动，passive就是用来解决这种情况的 -->
<ul @wheel.passive="demo" class="list">
   <li>1</li>
   <li>2</li>
   <li>3</li>
   <li>4</li>
</ul>
```

> 区分onwheel事件和onscroll事件
>
> - onwheel：鼠标滚轮的滚动，默认是非passive的
> - onscroll：滚动条的滚动，默认是passive的

**事件修饰符的组合使用**

```html
<!--表示即阻止默认行为，也阻止冒泡-->
<a href="http://www.baidu.com" @click.prevent.stop="showInfo">点我提示信息</a>
```





### 键盘事件

- **Vue中事先定义的按键别名：**

  - 回车 => enter

  - 删除 => delete (捕获“删除”和“退格”键)

  - 退出 => esc

  - 空格 => space

  - 换行 => tab (特殊，必须配合keydown去使用)

  - 上 => up

  - 下 => down

  - 左 => left

  - 右 => right



- **Vue未提供别名的按键，可以使用按键原始的key值去绑定，但注意要转为kebab-case（驼峰转短杠）**



- **系统修饰键（用法特殊）：ctrl、alt、shift、meta**
  - 配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发。
  - 配合keydown使用：正常触发事件。



- **使用keyCode去指定具体的按键，比如通过event.keyCode（不推荐，该方法可能被废除）**
- **Vue.config.keyCodes.自定义键名 = 键码，可以去定制按键别名**



**组合键的事件**

```html
<!--只有当同时按下ctrl+y时才会触发事件-->
<input type="text" placeholder="按下回车提示输入" @keydown.ctrl.y="showInfo">
```



































