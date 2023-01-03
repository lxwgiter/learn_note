## 1、什么是node.js

- node.js是JavaScript的运行环境，用编程术语来讲，Node.js 是一个 JavaScript 运行时（Runtime）

- 在 Node.js 之前，JavaScript 只能运行在浏览器中，作为网页脚本使用。

- 有了 Node.js 以后，JavaScript 就可以脱离浏览器，像其它编程语言一样直接在计算机上使用。

- node.js使用V8引擎

- V8 引擎借鉴了 Java 虚拟机和 C++ 编译器的众多技术，它将 JavaScript 代码直接编译成原生机器码，并且使用了缓存机制来提高性能，这使得 JavaScript 的运行速度可以媲美二进制程序。

- nodejs的特点是异步、非阻塞，但是单线程的环境，nodejs如何做到这一点呢：通过回调函数。

  > 传统语言解决异步问题：
  >
  > ​	通过多线程



**nodejs的特点：**

- 单线程、异步、非阻塞
- 统一API



## 2、node.js和JavaScript的区别

- node.js：脱离了浏览器，失去了宿主对象，仅有ECMAScript，但是保留了js常用的功能，如：consloe.log
- JavaScript：ECMAScript、BOM、DOM



## 3、promise引入

**同步和异步：**



- 同步
    - 通常情况代码都是自上向下一行一行执行的
    - 前边的代码不执行后边的代码也不会执行
    - 同步的代码执行会出现阻塞的情况
    - 一行代码执行慢会影响到整个程序的执行

- 异步
  - 一段代码的执行不会影响到其他的程序

**通过回调函数实现异步：**

```javascript
function sum(a, b, cb) {
    setTimeout(() => {
        cb(a + b)
    }, 1000)
}

sum(123, 456, (result)=>{
     console.log(result)
})
```

**基于回到函数带来的问题：**



1. 代码可读性差
2. 可调试性差



**引入Promise**



-  Promise是一个可以用来存储数据的对象
-  Promise存储数据的方式比较特殊，这种特殊方式使得Promise可以用来存储异步调用的数据

## 4、Promise

- Promise 是一个 ECMAScript 6 提供的类，目的是更加优雅地书写复杂的异步任务。
- 由于 Promise 是 ES6 新增加的，所以一些旧的浏览器并不支持
- Promise 对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）、rejected（已失败）
- 一个promise对象只能改变一次状态，成功或者失败后都会返回结果数据。
- Promise构造函数接收一个函数作为参数，该函数的两个参数分别是 resolve 和 reject
- then 方法可以接收两个回调函数作为参数，第一个回调函数是Promise对象的状态改变为 resoved 是调用，第二个回调函数是 Promise 对象的状态变为 rejected 时调用。其中第二个参数可以省略。
- Promise 只不过是一种更良好的编程风格。



Promise中维护了**两个隐藏属性**：

- **PromiseResult**
  - 用来存储数据

- **PromiseState**
  - 记录Promise的状态（三种状态）
  - state只能修改一次，修改以后永远不会在变
  - pending   （进行中）
  - fulfilled（完成） 通过resolve存储数据时
  - rejected（拒绝，出错了） 出错了或通过reject存储数据时
  



**流程：**

- 当Promise创建时，PromiseState初始值为pending，
- 当通过resolve存储数据时 PromiseState 变为fulfilled（完成）->PromiseResult变为存储的数据
- 当通过reject存储数据或出错时 PromiseState 变为rejected（拒绝，出错了）->PromiseResult变为存储的数据 或 异常对象
  
- 当我们通过then读取数据时，相当于为Promise设置了回调函数，
- 如果PromiseState变为fulfilled，则调用then的第一个回调函数来返回数据
- 如果PromiseState变为rejected，则调用then的第二个回调函数来返回数据

### 构造 Promise

现在我们新建一个 Promise 对象：

Promise 构造函数只有一个参数，是一个函数，这个函数在构造之后会直接被异步运行，所以我们称之为起始函数。起始函数包含两个参数resolve 和 reject。**当 Promise 被构造时，起始函数会被异步执行**。

```js
new Promise(function (resolve, reject) {
    // 要做的事情...
});
```

通过新建一个 Promise 对象好像并没有看出它怎样 "更加优雅地书写复杂的异步任务"。我们之前遇到的异步任务都是一次异步，如果需要多次调用异步函数呢？例如，如果我想分三次输出字符串，第一次间隔 1 秒，第二次间隔 4 秒，第三次间隔 3 秒：



**实例**:

```js
setTimeout(function () {
    console.log("First");
    setTimeout(function () {
        console.log("Second");
        setTimeout(function () {
            console.log("Third");
        }, 3000);
    }, 4000);
}, 1000);
```



这段程序实现了这个功能，但是它是用 **"函数瀑布**" 来实现的。可想而知，在一个复杂的程序当中，用 "函数瀑布" 实现的程序无论是维护还是异常处理都是一件特别繁琐的事情，而且会让缩进格式变得非常冗赘。



**现在我们用 Promise 来实现同样的功能：**

```js
new Promise(function (resolve, reject) {
    setTimeout(function () {
        console.log("First");
        resolve();
    }, 1000);
}).then(function () {
    return new Promise(function (resolve, reject) {//一个Promise对象代表一个异步操作，这里开启了新的异步操作
        setTimeout(function () {
            console.log("Second");
            resolve();
        }, 4000);
    });
}).then(function () {
    setTimeout(function () {
        console.log("Third");
    }, 3000);
});
```

### resolve 和 reject 

resolve 和 reject 都是函数，其中调用 resolve 代表一切正常，reject 是出现异常时所调用的：

```js
new Promise(function (resolve, reject) {
    var a = 0;
    var b = 1;
    if (b == 0) reject("Divide zero");
    else resolve(a / b);
}).then(function (value) {
    console.log("a / b = " + value);
}).catch(function (err) {
    console.log(err);
}).finally(function () {
    console.log("End");
});
//结果为：
//a / b = 0
//End
```

### .then() .catch() 和 .finally() 

Promise 类有 .then() .catch() 和 .finally() 三个方法，这三个方法的参数都是一个函数，.then() 可以将参数中的函数添加到当前 Promise 的正常执行序列，.catch() 则是设定 Promise 的异常处理序列，.finally() 是在 Promise 执行的最后一定会执行的序列。 .then() 传入的函数会按顺序依次执行，有任何异常都会直接跳到 catch 序列：

```js
new Promise(function (resolve, reject) {
    console.log(1111);
    resolve(2222);
}).then(function (value) {
    console.log(value);
    return 3333;
}).then(function (value) {
    console.log(value);
    throw "An error";
}).catch(function (err) {
    console.log(err);
});
//执行结果：
//1111
//2222
//3333
//An error
```

> resolve() 中可以放置一个参数用于向下一个 then 传递一个值，then 中的函数也可以返回一个值传递给 then。但是，**如果 then 中返回的是一个 Promise 对象，那么下一个 then 将相当于对这个返回的 Promise 进行操作**，这一点从刚才的计时器的例子中可以看出来。
>
> reject() 参数中一般会传递一个异常给之后的 catch 函数用于处理异常。
>
> **then中参数的来源：**
>
> - 上一个异步操作resolve()方法中放置的参数
> - 上一个异步操作return的值(除Promise对象)

**计时器抽取封装：**

```js
function print(delay, message) {
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
            console.log(message);
            resolve();
        }, delay);
    });
}

print(1000, "First").then(function () {
    return print(4000, "Second");
}).then(function () {
    print(3000, "Third");
});
```



### 回答常见的问题（FAQ）

**Q: then、catch 和 finally 序列能否顺序颠倒？**

A: 可以，效果完全一样。但不建议这样做，最好按 then-catch-finally 的顺序编写程序。



**Q: 除了 then 块以外，其它两种块能否多次使用？**

A: 可以，finally 与 then 一样会按顺序执行，但是 catch 块只会执行第一个，除非 catch 块里有异常。所以最好只安排一个 catch 和 finally 块。



**Q: then 块如何中断？**

A: then 块默认会向下顺序执行，return 是不能中断的，可以通过 throw 来跳转至 catch 实现中断。



**Q: 什么时候适合用 Promise 而不是传统回调函数？**

A: 当需要多次顺序执行异步操作的时候，例如，如果想通过异步方法先后检测用户名和密码，需要先异步检测用户名，然后再异步检测密码的情况下就很适合 Promise。



**Q: Promise 是一种将异步转换为同步的方法吗？**

A: 完全不是。Promise 只不过是一种更良好的编程风格。



**Q: 什么时候我们需要再写一个 then 而不是在当前的 then 接着编程？**

A: 当你又需要调用一个异步任务的时候。



### 示例：

```js
var promise =new Promise(function(resolve,reject){
    //To Do 要异步执行的事情，这个异步执行的事情有可能成功执行完毕，那么Promise将是fulfilled状态，如果执行失败则是rejected;
    //下面测试代码，人为设置为rejected状态;
    reject("将当前构建的Promise对象的状态由pending（进行中）设置为rejected（已拒绝）"); //当然此处也可以设置为fulfilled(已完成)状态
})

promise.then(//调用第一个then()
    success=>{
        console.log("异步执行成功，状态为：fulfilled，成功后返回的结果是："+success);
        return(" 当前 success ");
    },
    error=>{
        console.log("异步执行失败，状态为rejected，失败后返回的结果是："+error);
        return(" 当前 error ");
    }
).then(
    //调用第二个then() 因为调用第一个then()方法返回的是一个新的promise对象，此对象的状态由上面的success或者error两个回调函数的执行情况决定的：
    //如果回调函数能正常执行完毕，则新的promise对象的状态为fulfilled，下面执行success2,如果回调函数无法正常执行，则promise状态为rejected;下面执行error2
    success2=>{
        console.log("第一个then的回调函数执行成功 成功返回结果："+success2);
        throw(" 当前 success2 ");//自定义异常抛出
    },
    error2=>{
        console.log("第一个then的回调函数执行失败 失败返回结果："+error2);
        return(" 当前 error2 ");
    }
).catch(err=>{
    //当success2或者error2执行报错时，catch会捕获异常;
    console.log("捕获异常："+err);
});

//上述代码,打印如下:
//异步执行失败，状态为rejected，失败后返回的结果是：将当前构建的Promise对象的状态由pending（进行中）设置为rejected（已拒绝）
//第一个then的回调函数执行成功 成功返回结果： 当前 error
//捕获异常： 当前 success2
```



## 5、异步函数

**异步函数（async function）是 ECMAScript 2017 (ECMA-262) 标准的规范，几乎被所有浏览器所支持，除了 Internet Explorer。**



我们在上面的Promise中，编写了一个定时器函数，使用async、await可以使异步更简单：

```js
async function asyncFunc() {
    await print(1000, "First");
    await print(4000, "Second");
    await print(3000, "Third");
}
asyncFunc();
```

这岂不是将异步操作变得像同步操作一样容易了吗？

这次的回答是肯定的，异步函数 async function 中可以使用 await 指令，await 指令后必须跟着一个 Promise，异步函数会在这个 Promise 运行中暂停，直到其运行结束再继续运行。

异步函数实际上原理与 Promise 原生 API 的机制是一模一样的，只不过更便于程序员阅读。

处理异常的机制将用 try-catch 块实现：



```js
async function asyncFunc() {
    try {
        await new Promise(function (resolve, reject) {
            throw "Some error"; // 或者 reject("Some error")
        });
    } catch (err) {
        console.log(err);
        // 会输出 Some error
    }
}
asyncFunc();
```

如果 Promise 有一个正常的返回值，await 语句也会返回它：

```js
async function asyncFunc() {
    let value = await new Promise(
        function (resolve, reject) {
            resolve("Return value");
        }
    );
    console.log(value);
}
asyncFunc();
//结果：Return value
```



## 6、宏任务和微任务



现在有这样一段代码，你能说出它在控制台中的输出结果吗？



```js
console.log(1)

Promise.resolve().then(() => {
    console.log(2)
})

console.log(3)
```

要真正的理解这一段代码，我们必须要先搞懂Promise中的实例方法then到底是在做什么？之前在学习Promise时，我们就已经说过了，then相当于为Promise设置了一个回调函数，当Promise中的数据处理完毕时，便会调用then所设置的回调函数来继续后续任务。

上例中，我们通过Promise.resolve()创建了一个理解完成的Promise，那么按道理讲then中的回调函数应该立刻执行啊？因为Promise已经完成了啊？所以打印的顺序不应该是“1 2 3”吗？如果能想到这些那么证明之前讲解的Promise你已经理解的差不多了，但是还不够准确！

then中的回调函数会在Promise完成后被调用，但是注意并不是立刻就调用，而是采用一种和定时器类似的处理方式，讲函数放入到一个任务队列中，而队列中的代码会在调用栈中的代码执行完毕后才会执行。也就是说then中的代码总是在当前调用栈中的代码执行完后才执行。所以上边代码的输出结果应该为：“1 3 2”

那么问题又来了，如果是这样的代码呢？



```js
setTimeout(()=>{
    console.log(1)
})

Promise.resolve().then(() => {
    console.log(2)
})
```

错误的分析：setTimeout是定时器，它会在一段时间后将函数放入到任务队列中，而我们没有指定时间，也就意味着函数会立刻放入到任务队列中。then同样也是将函数放入到任务队列中，并且这个Promise是一个立即完成的Promise所以函数也是立刻进入任务队列。那么按照执行顺序来讲，定时器在前，then在后，所以定时器中的函数应该先进入队列，队列又是先进先出的，所以应该先1后2。

上边的分析看似合理，实际上是不对的。因为setTimeout和then虽然都将函数放入到队列中，但是却不是同一个队列。为了更合理的处理异步任务，ES标准规定了一个内部的队列“PromiseJobs”，这个队列是专门用来放置由Promise产生的回调函数的（then、catch、finally），这个队列我们通常被称为“微任务队列（microtask queue）”。相对的，setTimeout这些方法是将函数放入到了“宏任务队列（macrotask queue）”。

简单来说，任务队列有两个，宏任务队列和微任务队列。代码执行时，宏任务进入到宏任务队列，微任务进入到微任务队列。那么哪些任务时微任务，哪些任务是宏任务呢？其实大部分的任务都属于宏任务。而微任务通常在代码运行时产生，通常是由Promise所创建的，Promise的then、catch、finally中的回调函数会作为微任务进入到微任务队列中。

JS代码执行时，每一个宏任务执行完毕后，JS引擎会立即执行微任务队列中的所有任务，然后才是执行宏任务队列中的任务。换句话中then中的回调函数（微任务）会先于定时器中的回调函数（宏任务）执行。所以上例中代码的执行结果应该为：“2 1”。

如果上边的内容你理解了，可以尝试分析一下这段代码：



```js
console.log(1);

setTimeout(() => console.log(2));

Promise.resolve().then(() => console.log(3));

Promise.resolve().then(() => setTimeout(() => console.log(4)));

Promise.resolve().then(() => console.log(5));

setTimeout(() => console.log(6));

console.log(7);
//1->7->3->5->2->6->4
```



## 7、包管理器

- **引入**

​	随着项目复杂度的提升，在开发中不可能所有的代码都要手动一行一行的编写，于是我们就需要一些将一些现成写好的代码引入到我们的项目中来帮助我们完成开发，就像是我们之前使用jQuery。jQuery这种外部代码在项目中，我们将其称之为包。越是复杂的项目，其中需要引入的包也就越多。随着包数量的增多，包管理的问题也被抬上了桌面。如何下载包？如何删除包？如何更新包？等等一些列的问题在等待我们处理。包管理器便是帮助我们解决这个问题的工具。



### NPM

​	node中的包管理局叫做npm（node package manage），npm是世界上最大的包管理库。作为开发人员，我们可以将自己开发的包上传到npm中共别人使用，也可以直接从npm中下载别人开发好的包，在自家项目中使用。



**npm由以下三个部分组成**：

1. npm网站 （通过npm网站可以查找包，也可以管理自己开发提交到npm中的包。https://www.npmjs.com/）
2. npm CLI（Command Line Interface 即 命令行）（通过npm的命令行，可以在计算机中操作npm中的各种包（下载和上传等））
3. 仓库（仓库用来存储包以及包相关的各种信息）

> npm在按照node时已经一起安装，所以只有你的node正常安装了，npm自然就可以直接使用了。可以在命令行中输入`npm -v`来查看npm是否安装成功。

### Package.Json

包是一组编写好的代码，可以直接引入到项目中使用。具体来说，包其实就是一个文件夹，文件夹中会包含一个或多个js文件，这些js文件就是包中存放的各种代码。除了必要的代码外，包中还有一个东西是必须的，它叫做包的描述文件 —— package.json.顾名思义，它就是一个用来描述包的json文件。它里边需要一个json格式的数据（json对象），在json文件中通过各个属性来描述包的基本信息，像包名、版本、依赖等包相关的一切信息。它大概长成这个样子：

```json
{
  "name": "my-awesome-package",
  "version": "1.0.0",
  "author": "Your Name <email@example.com>"
}
```

**package.json中包含有哪些字段呢？**

**name**（必备）

- 包的名称，可以包含小写字母、_和-

**version**（必备）

- 包的版本，需要遵从x.x.x的格式
- 规则：
  - 版本从1.0.0开始
  - 修复错误，兼容旧版（补丁）1.0.1、1.0.2
  - 添加功能，兼容旧版（小更新）1.1.0
  - 更新功能，影响兼容（大更新）2.0.0

**author**

- 包的作者，格式：Your Name <email@example.com>

**description**

- 包的描述

**repository**

- 仓库地址（git）

**scripts**

- 自动脚本

> 通常情况下，我们的自己创建的每一个node项目，都可以被认为是一个包。都应该为其创建package.json描述文件。同时，npm可以帮助我们快速的创建package.json文件。只需要进入项目并输入`npm init`即可进入npm的交互界面，只需根据提示输入相应信息即可。输入后根据提示输入相应信息即可

### NPM常用指令及说明

- 安装包

```bash
npm install 包名
```

```bash
npm i 包名
```

```bash
npm install 包名@版本号
```
```bash
npm install 
```

最后一个命令：**会自动安装package.json中所有依赖的包**



- 卸载包

```bash
npm uninstall 包名
```

- 设置镜像

```bash
npm set registry https://registry.npmmirror.com
```

- 恢复默认镜像

```bash
npm config delete registry
```

> 当安装包时，会做三件事：
>
> - 联网到仓库下载包
> - 修改package.json文件，在dependencies字段中将刚刚下载的包设置为依赖项
> - 安装包后项目中会自动生成package.lock.json文件，这个文件主要是用来记录当前项目的下包的结构和版本的，提升重新下载包的速度，以及确保包的版本正确。

### Yarn

​	用于替换npm的包管理器

**命令**

- yarn init （初始化，创建package.json）
- yarn add xxx（添加依赖）
- yarn add xxx -D（添加开发依赖）
- yarn remove xxx（移除包）
- yarn（自动安装依赖）
- yarn run（执行自定义脚本）
- yarn <指令>（执行自定义脚本）
- yarn global add（全局安装）
- yarn global remove（全局移除）
- yarn global bin（全局安装目录）



**Yarn镜像配置**

配置：



```bash
yarn config set registry https://registry.npmmirror.com
```

恢复：



```bash
yarn config delete registry
```



### Pnpm

​	也是node的包管理器



**安装**

```bash
npm install -g pnpm
```

**命令**

- pnpm init（初始化项目，添加package.json）
- pnpm add xxx（添加依赖）
- pnpm add -D xxx（添加开发依赖）
- pnpm add -g xxx（添加全局包）
- pnpm install（安装依赖）
- pnpm remove xxx（移除包）
- Pnpm镜像配置



**配置：**

```bash
pnpm config set registry https://registry.npmmirror.com
```

**恢复**：

```bash
pnpm config delete registry
```
