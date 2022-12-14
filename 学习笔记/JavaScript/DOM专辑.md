## 引入：

​    • **DOM**，全称**Document Object Model**（文档对象模型）。
​    • **节点（Node）**，是构成我们网页的最基本的组成部分，网页中的每一个部分都可以称为是一个节点。
​    • **节点类型**：
​        • 文档节点（document）
​            -文档节点document，代表的是整个HTML文档，网页中的所有节点都是它的子节点。
​        • 元素节点（Element）
​            -HTML中的各种标签都是元素节点，这也是我们最常用的一个节点。
​        • 文本节点（Text）
​            -文本节点表示的是HTML标签以外的文本内容，任意非HTML的文本都是文本节点。
​        • 属性节点（Attr）
​            -属性节点表示的是标签中的一个一个的属性，这里要注意的是属性节点并非是元素节点的子节点，而是元素节点的一部分。





## 第一部分：DOM查询

### 通过document对象调用

- getElementById() 

​		方法：通过id属性获取一个元素节点对象

- getElementsByTagName()

​		方法：通过id属性获取一个元素节点对象

-  getElementsByName()

​		方法：通过name属性获取一组元素节点对象

- body

​		属性：保存了body标签的引用

- documentElement

​		属性：保存了html标签的引用

- all

​		属性：保存了页面中所有html标签的引用，是一个数组

- getElementsByClassName()

​		方法：可以根据class属性值获取一组元素节点对象，但是该方法不支持IE8及以下的浏览器.

- querySelector()

​		方法：需要一个选择器的字符串作为参数，可以根据一个CSS选择器来查询一个元素节点对象，该方法总会返回唯一的一个元素，如果满足					条件的元素有多个，那么它只会返回第一个IE8也支持，此方法放心使用，属实强！

- querySelectorAll()

​		方法：该方法和querySelector()用法类似，不同的是它会将符合条件的元素封装到一个数组中返回称该方法为王中王，可以代替所有DOM					查询！！



### 通过具体的元素节点调用

- getElementsByTagName()

​		方法：返回当前节点的指定标签名后代节点

- children

​		属性：可以获取当前元素的所有子元素，注：这是推荐的用法，返回的是元素节点。

- childNodes

​		属性：表示当前节点的所有子节点，注：包括文本节点，所以标签之间的空白也会被当做节点返回。

- firstElementChild

​		属性：获取当前元素的第一个子元素，不支持IE8及以下的浏览器。

- firstChild

​		属性：表示当前节点的第一个子节点，包括空白文本节点

- lastElementChild 

​		属性：返回指定元素的最后一个子元素

- lastChild

​		属性：表示最后一个子节点对象，包括文本节点。

- parentNode

​		属性：表示当前节点的父节点，父节点不可能是文本节点

- previousElementSibling

​		属性：获取前一个兄弟元素，IE8及以下不支持

- previousSibling

​		属性：表示当前节点的前一个兄弟节点，包括文本节点

- nextElementSibling

​		属性：下一个兄弟元素，IE8及以下不支持

- nextSibling

​		属性：表示当前节点的后一个兄弟节点，包括文本节点





## 第二部分：DOM对象属性

​    ①**一般属性的读取和赋值:**
​        直接使用"元素对象.属性名"，大部分属性可读可写。
​        比如：element.value,element.innerHtml,element.id....



​    ②**自定义属性的读取和赋值**：
​        基本同一般属性。
​    

​	③**class属性**：
​        读取：
​            ①elemen.className,返回class属性的内容。
​            ②element.classList,以数组的形式返回class属性的内容。
​        赋值：
​            element.className=""  直接设置类名称会覆盖掉原先的类名称。
​        添加类：
​            element.classList.add("")   向class属性中添加一个值
​        移除类：
​            element.classList.remove("")   删除class属性中被指定的类
​        检查是否包含某个类：
​            element.classList.contains("")
​        切换类：
​            btn.classList.toggle(“”);     要是有这个类，删除这个类。要是没有这个类，增加这个类
​            btn.classList.toggle(“类名称”,boolean值);
​            boolean值为false值时，默认删除这个类名称，为true时，是添加。



​	 ④**关于属性的一些方法**：
​        hasAttribute()                                  如果元素中存在指定的属性返回 true，否则返回false。
​        btn.setAttribute(“设置的自定义属性的名称”,“自定义的值”);
​        btn.getAttribute(“自定义属性的名称”);           返回的都是string类型



## 第三部分：DOM增删改

​    **①通过document对象调用**
​        createElement() 创建元素节点，以标签名为参数。
​        createTextNode() 创建文本节点，以内容为参数。



​    **②通过父节点调用**
​        appendChild(子节点) 把新的子节点添加到末尾。
​        insertBefore(新节点，旧节点) 在指定的子节点前面插入新的子节点。 
​        replaceChild(新节点，旧节点) 替换子节点。 
​        removeChild(子节点) 删除子节点。 



## 第四部分：DOM与CSS

**①有关CSS最重要的属性：style属性**
        调用方式：DOM对象.style
        1)通过DOM操作CSS只有style属性是支持读写的，其他方式都是只读的。
        2)style属性可以读取和修改元素的CSS样式，但是仅包括内联样式,意味着大部分情况下通过该属性读取不到当前DOM样式。
        3)如果在样式中写了!important，优先级高于行内，那么通过JS无法修改样式。
        4)访问具体样式的语法：DOM.style.样式名。样式名依据CSS变动为驼峰命名法！
            读取：DOM.style.width   只可读取行内样式
            修改：①DOM.style.width="200px" / ②DOM.style="width : 200px"



**②其他关于CSS的属性**：
    读取当前CSS属性，包括CSS样式表          （两者都是只读！）
        IE专用：元素.currentStyle.样式名
        其他浏览器： window.getComputedStyle(元素对象,null).属性名
    一些常用的CSS属性：
        clientWidth、clientHeight     
            这两个属性可以获取元素的可见宽度和高度（包括内容区和内边距），都是不带px的，返回都是一个数字，只读。
        offsetWidth、offsetHeight
            获取元素的整个的宽度和高度，包括内容区、内边距和边框，只读。
        offsetParent
            会获取到离当前元素最近的开启了定位的祖先元素，果所有的祖先元素都没有开启定位，则返回body。
        offsetLeft、offsetTop
            当前元素相对于其定位父元素的水平、垂直偏移量
        scrollWidth、scrollHeight
            可以获取元素整个滚动区域的宽度和高度
        scrollLeft、scrollTop
            可以获取水平、垂直滚动条已经滚动的距离
        两个公式：
            ①scrollHeight - scrollTop == clientHeight   说明垂直滚动条滚动到底了
            ②scrollWidth - scrollLeft == clientWidth    说明水平滚动条滚动到底



## 第五部分：DOM与事件：

**①事件句柄：**
        onblur 元素失去焦点。 
        onchange 域的内容被改变。 
        onclick 当用户点击某个对象时调用的事件句柄。 
        ondblclick 当用户双击某个对象时调用的事件句柄。 
        onfocus 元素获得焦点。 
        onkeydown 某个键盘按键被按下。 
        onkeypress 某个键盘按键被按下并松开。 
        onkeyup 某个键盘按键被松开。 
        onload 一张页面或一幅图像完成加载 
        onmousemove 鼠标被移动。 
        onmouseout 鼠标从某元素移开。 
        onmouseover 鼠标移到某元素之上。 
        onmouseup 鼠标按键被松开。 
 **②鼠标/键盘属性：**
        button 返回当事件被触发时，哪个鼠标按钮被点击。 
        clientX 返回当事件被触发时，鼠标指针的水平坐标。 
        clientY 返回当事件被触发时，鼠标指针的垂直坐标。 
**③event对象属性：**
        cancelable 返回布尔值，指示事件是否可拥可取消的默认动作。 
        target 返回触发此事件的元素（事件的目标节点）。 

**④事件对象**
	当事件的响应函数被触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数,
	在事件对象中封装了当前事件相关的一切信息，比如：鼠标的坐标、键盘哪个按键被按下、鼠标滚轮滚动的方向。。。
    显式的再函数参数列表中写下event：如function (event){}
**⑤事件的冒泡：（Bubble）**
	所谓的冒泡指的就是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发
	也就是说，有可能子元素未定义某个回调函数，由于祖先元素定义了这种回调函数，子元素达到触发事件时
	向上传导，触发了祖先元素的回调函数。
	当子元素和祖先元素同时定义了相同的响应函数，那么先执行当前触发的响应函数，再一级一级向上传导(见3.事件的传播)
	在开发中大部分情况冒泡都是有用的,如果不希望发生事件冒泡可以通过事件对象来取消冒泡
**⑥事件的委派：**
	指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素
	从而通过祖先元素的响应函数来处理事件，并且同过event.target我们可以得知是哪个对象被点击了.
	事件委派是利用了冒泡，通过委派可以减少事件绑定的次数，提高程序的性能
**⑦事件的绑定：**
    使用对象.事件 = 函数的形式绑定响应函数，它只能同时为一个元素的一个事件绑定一个响应函数，
    不能绑定多个，如果绑定了多个，则后边会覆盖掉前边的.
    addEventListener("事件名",回调函数1,回调函数2....)  - 通过这个方法也可以为元素绑定响应函数
**⑧事件的传播：**
    事件传播分成了三个阶段
        1.捕获阶段

​			在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件

​		2.目标阶段

​			事件捕获到目标元素，捕获结束开始在目标元素上触发事件

​		3.冒泡阶段

​			事件从目标元素向他的祖先元素传递，依次触发祖先元素上的事件























