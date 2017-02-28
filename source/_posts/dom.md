---
title: dom
date: 2016-11-23 15:00:33
categories: Javascript
tags:
     - Javascript
---

---------
```
<body>
    <div id="div">
        <div class="div1" name="div">hello world<span>奥斯卡看卡</span></div>
        <div class="div2" name="div">哈哈哈</div>
    </div>
</body>
```

## 获取元素节点

> getElementById 返回的结果是以对象的形式，其余为类数组（NodeList对象）

```
var　divId = document.getElementById("div");
console.log(divId)
var divTag = document.getElementsByTagName("div");
console.log(divTag)
var divClass = document.getElementsByClassName("div1");
console.log(divClass)
var divName = document.getElementsByName("div");
console.log(divName)   
```
## javascript 的css选择器

document.querySelector()
document.querySelectorAll()
```
var div =document.querySelector("#div>div");
console.log(div) //输出为div1的对象形式
var div = document.querySelectorAll("div");
console.log(div) //输出为所有div
```

## 文档结构和遍历

一.作为节点数的文档
 1. parentNode    获取该节点的父节点
 2. childNodes    获取该节点的子节点数组
 3. firstChild    获取该节点的第一个子节点
 4. lastChild    获取该节点的最后一个子节点
 5. nextSibling    获取该节点的下一个兄弟元素
 6. previoursSibling    获取该节点的上一个兄弟元素
 7. nodeType    节点的类型，9代表Document节点，1代表Element节点，3代表Text节点，8代表Comment节点，11代表DocumentFragment节点
 8. nodeVlue    Text节点或Comment节点的文本内容
 9. nodeName    元素的标签名(如P,SPAN,#text(文本节点),DIV)，以大写形式表示

```
var span = document.getElementsByTagName("span");
console.log(span[0].parentNode) // //输出为div1的对象形式
var div = document.getElementById("div");
console.log(div.childNodes) //输出为
/*NodeList[5]
{0:text,
1:div.div1,
2:text,
3:div.div2,
4:text,
length:5}*/
console.log(div.firstChild)//文本节点
console.log(div.childNodes[1].firstChild.nodeValue) //文本节点的ｖａｌｕｅ
console.log(span[0].parentNode.nextSibling.nextSibling) // 输出为div２对象　因为第一个nextSibling是一个text节点
var text="";
for(var i=0;i<div.childNodes.length;i++){
    if(div.childNodes[i].nodeType==1){　//元素节点
        text+=div.childNodes[i].nodeName.toLowerCase()+":"+div.childNodes[i].innerText+"----"+"\n";
    }else if(div.childNodes[i].nodeType==3){ //text节点
        text+=div.childNodes[i].nodeName.toLowerCase()+":"+div.childNodes[i].nodeValue+"----"+"\n";
    }
}
console.log(text)
```

二.作为元素树的文档 // 忽略掉文本节点
1、firstElementChild         第一个子元素节点
2、lastElementChild          最后一个子元素节点
3、nextElementSibling        下一个兄弟元素节点
4、previousElementSibling    前一个兄弟元素节点
5、childElementCount         子元素节点个数量

```
<div id="div1">
    <p id="p1" class="class1"> 我是第一个P</p>
    <p id="p2" class="class2">我是第二个P</p>
</div>
var node = document.getElementById("div1");
var node1 = node.firstElementChild;
var node2 = node.lastElementChild;

alert(node.childElementCount);  //输出2，div1一共有两个非文档子元素节点
alert(node1.innerHTML);         //输出 我是第一个P
alert(node2.innerHTML);         //输出 我是第二个P
alert(node2.previousElementSibling.innerHTML);  //输出 我是第一个P(第二个元素节点的上一个非文本元素节点是P1)
alert(node1.nextElementSibling.innerHTML);      //输出 我是第二个P(第一个元素节点的下一个兄弟非文本节点是P2)
```

## javascript操作HTML属性

> 1、属性的读取，此处要注意的是，某些HTML属性名称在javascript之中是保留字，因此会有些许不同，如class,lable中的for在javascript中变为htmlFor,className。

```
<div id="div1">
    <p id="p1" class="class1"> 我是第一个P</p>
    <img src="123.jpg" alt="我是一张图片" id="img1" />
    <input type="text" value="我是一个文本框" id="input1" />
</div>

window.onload = function () {
    var nodeText = document.getElementById("input1");
    alert(nodeText.value);        //输出 我是一个文本框
    var nodeImg = document.getElementById("img1");
    alert(nodeImg.alt);            //输出 我是一张图片
    var nodeP = document.getElementById("p1");
    alert(nodeP.className);        //输出 class1    注意获取class是className，如果写成nodeP.class则输出undefined
}
```

> 属性的设置，此处同样要注意的是保留字

```
<div id="div1">
    <img src="1big.jpg" alt="我是一张图片" id="img1" onclick="fun1()" />
</div>

function fun1() {
    document.getElementById("img1").src = "1small.jpg";        //改变图片的路径属性。实现的效果为，当点击图片时，大图变小图。
}
```

> 非标准HTML属性  //注意这两个方法是不必理会javascript保留字的，HTML属性是什么就怎么写。

getAttribute();   
setAttribute();

```
<div id="div1">
    <img src="1big.jpg" alt="我是一张图片" class="imgClass" id="img1" onclick="fun1()" />
</div>

function fun1() {
    document.getElementById("img1").setAttribute("src", "1small.jpg");
    alert(document.getElementById("img1").getAttribute("class"));
}
```

> Attr节点的属性

attributes属性  非Element对象返回null，Element一般返回Attr对象。Attr对象是一个特殊的Node,通过name与value获取属性名称与值。

```
var div = document.getElementsByTagName("div");
console.log(div[1].attributes) //返回div1的所有属性 {0:class,1:name}
console.log(div[1].attributes[0].name) //返回div1的第一个属性的nodeName　结果为class
console.log(div[1].attributes[0].value) //返回div1的第一个属性的nodeValue　结果为div1
```

> 元素的内容

1、innerText、textContent    innerText与textContent的区别，当文本为空时，innerText是""，而textContent是undefined
2、innerHTML

```
<div id="div1">
    <p id="p1">我是第一个P</p>
    <p id="p2">我是第<b>二</b>个P</p>
</div>

window.onload = function () {
    alert(document.getElementById("p1").innerText);  //注意火狐浏览器不支持innerText
    alert(document.getElementById("p1").textContent);    //基本都支持textContent
    document.getElementById("p1").textContent = "我是p1，javascript改变了我";    //设置文档Text
    alert(document.getElementById("p2").textContent);
    alert(document.getElementById("p2").innerHTML); //innerHTML与innerText的区别，就是对HTML代码的输出方式Text不会输出HTML代码
}
```

> 创建，插入，删除节点

1、document.createTextNode()    创建一个文本节点
 
 ```
<div id="div1">
     <p id="p1">我是第一个P</p>
     <p id="p2">我是第二个P</p>
</div>

window.onload = function () {
     var textNode = document.createTextNode("<p>我是一个javascript新建的节点</p>");
     document.getElementById("div1").appendChild(textNode);
}
结果为：
<div id="div1">
    <p id="p1">我是第一个P</p>
    <p id="p2">我是第二个P</p>
    我是一个javascript新建的节点
</div>
 ```
 
2、document.createElement()    创建一个元素节点
 
 ```
 <div id="div1">
     <p id="p1">我是第一个P</p>
     <p id="p2">我是第二个P</p>
 </div>
 window.onload = function () {
     var pNode = document.createElement("p");
     pNode.textContent = "新建一个P节点";
     document.getElementById("div1").appendChild(pNode);
 }
 结果为：
 <div id="div1">
     <p id="p1">我是第一个P</p>
     <p id="p2">我是第二个P</p>
     <p>新建一个P节点</p>
 </div>
 ```
 
3、插入节点
appendChild()    //将一个节点插入到调用节点的最后面
insertBefore()   //接受两个参数，第一个为待插入的节点，第二个指明在哪个节点前面，如果不传入第二个参数，报错。

```
<div id="div1">
    <p id="p1">我是第一个P</p>
</div>

window.onload = function () {
    var pNode1 = document.createElement("p");
    pNode1.textContent = "insertBefore插入的节点";
    var pNode2 = document.createElement("p");
    pNode2.textContent = "appendChild插入的节点";
    document.getElementById("div1").appendChild(pNode2);
    document.getElementById("div1").insertBefore(pNode1,document.getElementById("p1"));
}
结果为：
<div id="div1">
    <p>insertBefore插入的节点</p>
    <p id="p1">我是第一个P</p>
    <p>appendChild插入的节点</p>
</div>
```

4、删除和替换节点。
1)removeChild();    由父元素调用，删除一个子节点。注意是直接父元素调用，删除直接子元素才有效，删除孙子元素就没有效果了。

```
<div id="div1">
    <p id="p1">我是第一个P</p>
    <p id="p2">我是第二个P</p>
</div>

window.onload = function () {
    var div1 = document.getElementById("div1");
    div1.removeChild(document.getElementById("p2"));
}
执行之后代码变为：
<div id="div1">
    <p id="p1">我是第一个P</p>    //注意到第二个P元素已经被移除了
</div>
```

2)replaceChild()    //删除一个子节点，并用一个新节点代替它，第一个参数为新建的节点，第二个节点为被替换的节点

```
<div id="div1">
    <p id="p1">我是第一个P</p>
    <p id="p2">我是第二个P</p>
</div>

window.onload = function () {
    var div1 = document.getElementById("div1");
    var span1 = document.createElement("span");
    span1.textContent = "我是一个新建的span";
    div1.replaceChild(span1,document.getElementById("p2"));
}
执行完成后HTML代码变为：
<div id="div1">
    <p id="p1">我是第一个P</p>
    <span>我是一个新建的span</span>    //留意到p2节点已经被替换为span1节点了
</div>
```

> javascript操作元素CSS　通过元素的style属性可以随意读取和设置元素的CSS样式

```
<div id="div1" style="width:100px; height:100px; background-color:red"></div>
window.onload = function () {
    alert(document.getElementById("div1").style.backgroundColor);
    document.getElementById("div1").style.backgroundColor = "yellow";
}
```

> IE有许多好用的方法，后来都被其他浏览器抄袭了，比如这个contains方法。如果A元素包含B元素，则返回true(可以A包含A)，否则false。唯一不支持这个方法的是IE的死对头firefox。

```
<div id="parent">
  <p>
    <strong id="child" >本例子会在火狐中会报错。</strong>
  </p>
</div>
window.onload = function(){
    var A = document.getElementById('parent'),
    B = document.getElementById('child');
    alert(A.contains(A));　//true
    alert(A.contains(B));　//true
    alert(B.contains(A));  //false
  }
```

> cloneNode(true/false) 克隆节点　　true为深度克隆　false为只克隆节点本身

```
var p = document.createElement("p");
p.id ="ppp";
p.innerText = "我是新创建的p元素";
var cloneNode = p.cloneNode(true); //<p id="ppp">我是新创建的p元素</p>
var cloneNode = p.cloneNode(false);//默认为false <p id="ppp"></p>
```

> addEventListener() 第一个参数是事件名，第二个是回调函数，第三个参数为true表示捕获，false表示冒泡。

addEventListener()将指定的事件监听器注册到目标对象上，当目标对象触发制定的事件时，指定的回调函数就会触发。目标对象可以是 文档上的元素、 document、 window 或者XMLHttpRequest(比如onreadystatechange事件)。IE8及以下不支持此方法且只有事件冒泡没有事件捕获。IE9开始支持此方法，也就有了事件捕获。

```
var div1 = document.getElementById("div1");
div1.addEventListener("click", listener, false);
function listener() {
    console.log('test');
}
var cloneHtml = div1.cloneNode(true);
document.body.appendChild(cloneHtml);
/*
注意：
    当对某一个元素1既绑定了捕获事件，又绑定了冒泡事件时：
    当这个元素1并不是触发事件的那个元素2时，则触发顺序会按照先 捕获 后 冒泡 的顺序触发；
    当这个元素1就是最底层的触发事件的元素时，则这个元素没有捕获和冒泡的区别，谁先绑定就先触发谁。
*/
var div2 = document.getElementById("div2");
div2.addEventListener("click", listener2, true);
function listener2() {
    console.log('test2');
}
div2.addEventListener("click", listener1, false);
function listener1() {
    console.log('test1');
}
// 按绑定顺序执行，两个`addEventLister()`颠倒过来则执行顺序也变化
// 如果再对`div1`绑定一个捕获、一个冒泡，则会先触发捕获 再 触发冒泡，与绑定顺序无关
```

removeEventListener():与addEventListener()绑定事件对应的就是移除已绑定的事件
注意：只能通过removeEventListener()解绑有名字的函数，对于绑定的匿名函数无法解除绑定。
```
var div2 = document.getElementById("div2");
div2.addEventListener("click", listener2, true);
function listener2() {
    console.log('test2');
}
div2.removeEventListener("click", listener2, true);
```

 attachEvent()、detachEvent() IE8及以下使用这两个方法绑定和解绑事件，当然，IE9+也支持这个事件。但这个方法绑定的事件默认为冒泡也只有冒泡。
 
```
// 这里需要在事件前加 on
div2.attachEvent("onclick", listener1);
function listener1() {
    console.log('test');
    console.log(this);
}
div2.detachEvent("onclick", listener1);
//和addEventListener()一样，也不能解绑匿名函数。

//标准事件和IE事件中的阻止默认事件和冒泡事件也有很大区别。
var div2 = document.getElementById("div2");
if (div2.addEventListener) {
    div2.addEventListener("click", function(e) {
        e.preventDefault(); // 阻止默认事件
        e.stopPropagation(); // 阻止冒泡
        console.log(e.target.innerHTML);
    }, false);
} else {
    div2.attachEvent("onclick", function() {
        var e = window.event;
        e.returnValue = false; // 阻止默认事件
        e.cancelBubble = true; // 阻止冒泡
        console.log(e.srcElement.innerHTML);
    });
}
```

 自定义事件：createEvent()
 
 createEvent()用于创建一个新的 event ，而后这个 event 必须调用它的 init() 方法进行初始化。最后就可以在目标元素上使用dispatchEvent()调用新创建的event事件了。
 initEvent(type, bubbles, cancelable)
 type表示自定义的事件类型，bubbles表示是否冒泡，cancelable表示是否阻止默认事件。
 target.dispatchEvent(ev)
 target就是要触发自定义事件的DOM元素
 
 ```
 var div1 = document.getElementById("div1");
 div1.addEventListener("message", function(){
     console.log('test');
 }, false);

 var div2 = document.getElementById("div2");
 div2.addEventListener("message", function(e){
     console.log(this);
     console.log(e);
 }, false);
 var ev = document.createEvent("Event");
 ev.initEvent("message", false, true); // 起泡参数变为true，div1的事件就会触发
 div2.dispatchEvent(ev);
 ```