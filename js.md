



# 时间格式化

```
const formatTime2 = date => {
    var date = new Date(date)
    const year = date.getFullYear()
    const month = date.getMonth() + 1
    const day = date.getDate()
    const hour = date.getHours()
    const minute = date.getMinutes()
    const second = date.getSeconds()

    return [year, month, day].join('-') + ' ' + hour.toString().padStart(2,'0') + ':'
     + minute.toString().padStart(2,'0') + ':' + second.toString().padStart(2,'0')
}
```

# break continue 和 return 的用法和区别

+ #### break：直接结束一个循环，跳出循环体。break以后的循环体中的语句不会继续执行，循环体外面的会执行

+ #### continue：中止本次循环，继续下次循环。continue以后的循环体中的语句不会继续执行，下次循环继续执行，循环体外面的会执行

+ #### return：return的功能是结束一个方法。 一旦在循环体内执行return，将会结束该方法，循环自然也随之结束。与continue和break不同的是，return直接结束整个方法，不管这个return处于多少层循环之内。

# 防抖和节流

+ 防抖

```
/**
 * 函数防抖
 */
this.myPlugin.debounce = function (callback, time) {
    var timer;
    return function () {
        clearTimeout(timer);//清除之前的计时
        var args = arguments; //利用闭包保存参数数组
        timer = setTimeout(function () {
            callback.apply(null, args);
        }, time);
    }
}

```

+ 节流

```
/**
 * 函数节流
 *immediately 是否立即执行的节流
 */
this.myPlugin.throttle = function (callback, time, immediately) {
    if (immediately === undefined) {
        immediately = true;
    }
    if (immediately) {
        var t;
        return function () {
            if (immediately) {
                if (!t || Date.now() - t >= time) { //之前没有计时 或 距离上次执行的时间已超过规定的值
                    callback.apply(null, arguments);
                    t = Date.now(); //得到的当前时间戳
                }
            }
        }
    }
    else {
        var timer;
        return function () {
            if (timer) {
                return;
            }
            var args = arguments; //利用闭包保存参数数组
            timer = setTimeout(function () {
                callback.apply(null, args);
                timer = null;
            }, time);
        }
    }
}
```

# 正则

+ 应用例子

    ```
    this.value = this.value.replace(/[^\d.]/g,""); //清除"数字"和"."以外的字符
    this.value = this.value.replace(/^\./g,""); //验证第一个字符是数字而不是
    this.value = this.value.replace(/\.{2,}/g,"."); //只保留第一个. 清除多余的
    this.value = this.value.replace(".","$#$").replace(/\./g,"").replace("$#$",".");
    this.value = this.value.replace(/^(\-)*(\d+)\.(\d\d).*$/,'$1$2.$3'); //只能输入两个小数
    ```

    

# switch

```
switch(type){
     case "1":
        ......
        break；
     case "2":
        ......
        break；
     default:
        .....
        break;
}                   
```

# sort 数组排序

```
arr.sort((a,b)=>{
	return a-b; //升序，同理b-a为降序
})
```

# FileReader（上传图片）

+ 上传图片并获取图片的url

```
<input class="avatorInp" style="display: none" type="file" accept="image/*">
//上传图片的input
$('#appleWalletModal').on('change','.bigIconInp',function (e) {
    var files = e.target.files[0];
    getImgUrl(files,(res)=>{
    	$(img).attr('src',res)
    })
})
function getImgUrl(file,fun){
    var reader = new FileReader();
    reader.readAsDataURL(file);
    reader.onloadstart = function (ev) {
        console.log('图片正在上传中');
    }
    reader.onload = function (ev) {
       if(fun){
       	 fun(reader.result)
       }
    }
}
```

# JS定义

一个完整的 JavaScript 实现应该由下列三 个不同的部分组成。

  核心（ECMAScript）  文档对象模型（DOM）  浏览器对象模型（BOM） 

由 ECMA-262定义的 ECMAScript与 Web 浏览器没有依赖关系。实际上，这门语言本身并不包含输 入和输出定义。ECMA-262定义的只是这门语言的基础，而在此基础之上可以构建更完善的脚本语言。 我们常见的 Web 浏览器只是 ECMAScript 实现可能的宿主环境之一。宿主环境不仅提供基本的 ECMAScript 实现，同时也会提供该语言的扩展，以便语言与环境之间对接交互。而这些扩展——如 DOM，则利用 ECMAScript的核心类型和语法提供更多更具体的功能，以便实现针对环境的操作。其他 宿主环境包括 Node（一种服务端 JavaScript平台）和 Adobe Flash



JavaScript是一种专为与网页交互而设计的脚本语言，由下列三个不同的部分组成：

  ECMAScript，由 ECMA-262定义，提供核心语言功能； 

 文档对象模型（DOM），提供访问和操作网页内容的方法和接口；

  浏览器对象模型（BOM），提供与浏览器交互的方法和接口。

 JavaScript的这三个组成部分，在当前五个主要浏览器（IE、Firefox、Chrome、Safari和 Opera）中 都得到了不同程度的支持。

# 嵌入代码与外部文件 

在 HTML中嵌入 JavaScript代码虽然没有问题，但一般认为好的做法还是尽可能使用外部文件来 包含 JavaScript代码。不过，并不存在必须使用外部文件的硬性规定，但支持使用外部文件的人多会强 调如下优点。 

 可维护性：遍及不同 HTML页面的 JavaScript会造成维护问题。但把所有 JavaScript文件都放在 一个文件夹中，维护起来就轻松多了。而且开发人员因此也能够在不触及 HTML标记的情况下， 集中精力编辑 JavaScript代码。  可缓存：浏览器能够根据具体的设置缓存链接的所有外部 JavaScript文件。也就是说，如果有两个 页面都使用同一个文件，那么这个文件只需下载一次。因此，终结果就是能够加快页面加载的 速度。 

 适应未来：通过外部文件来包含 JavaScript 无须使用前面提到 XHTML 或注释 hack。HTML 和 XHTML包含外部文件的语法是相同的

# 标识符

所谓标识符，就是指变量、函数、属性的名字，或者函数的参数。标识符可以是按照下列格式规则 组合起来的一或多个字符：

  第一个字符必须是一个字母、下划线（_）或一个美元符号（$）；

  其他字符可以是字母、下划线、美元符号或数字

# 弹框弹出与隐藏

+ html、js

```
    <div class="container">
        <div class="box">
            <div class="modal">
                <div>1</div>
                <div>1</div>
                <div>1</div>
                <div>1</div>
            </div>
        </div>
    </div>
    <script src="./jquery.min.js"></script>
    <script>   
        $('.box').on('click',function(){
            $('.modal').show();
        })
        $('.modal').on('click',function(e){
            e.stopPropagation();
        })
        $(document).on('click',function(e){
            if(e.target.className !== 'box'){
                $('.modal').hide();
            }
            
        })
    </script>
```

+ css

```
html,body{
    padding: 0;
    margin: 0;
}
.container {
    width: 100%;
    height: 100%;
    .box{
        width: 100px;
        height: 30px;
        background-color: red;
        position: relative;
        .modal{
            width: 200px;
            height: 300px;
            background-color: blue;
            position: absolute;
            left: 0;
            top: 40px;
            display: none;
            div{
                background-color: yellow;
                border: 1px solid #000;
            }
        }
    }
}
```

+ 效果

![image-20200806105812378](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200806105812378.png)

+ modal方式

```
<div class="transparentModal" style="display: block;"></div>
<div class="demo" style=""> 
      <div>111</div>
      <div>222</div>
      <div>333</div>
</div>

//css
//透明modal
.transparentModal{
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 11000;
    display: none;
    background-color: rgba(0,0,0,0);
}
.demo{
   z-index: 11000;
}

//js
transparentModal.on('click',function () {
      transparentModal.find('.demo').hide();
      transparentModal.hide();
});
                                               
```

# 阻止a标签跳转

```
< a href="javascript:void(0);">统计</ a> //javascript后面可以写js代码
```

# 多级表头

<table id="settingTable" class="table table-striped table-bordered table-hover">
    <thead>
        <tr>
            <th colspan="1" rowspan="2">
                部门
            </th>
            <th colspan="1" rowspan="2">
                工种
            </th>
            <th class="abilityEvaluate" colspan="3" rowspan="1">
                能力评估
            </th>
            <th colspan="1" rowspan="2">
                操作
            </th>
        </tr>
        <tr>
            <th>SL1</th>
            <th>SL2</th>
            <th>SL3</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                Mark
            </td>
            <td>
                Otto
            </td>
            <td>
                Mark
            </td>
            <td>
                Otto
            </td>
            <td>
                makr124
            </td>
            <td>
                <div class="editBtn">
                    <div class="editIcon"></div>
                    <div class="editWord">编辑</div>
                </div>
            </td>
        </tr>
    </tbody>
</table>

# 可编辑内容元素

```
contenteditable="true"
```

# ajax请求html中的内容

```
$('.navBar a').on('click',function(e){
    e.preventDefault();
    $.ajax({
        type:'GET',
        url:'html/demo.html',
        cache:false,
        dataType:'html',
        success(res){
            console.log(res)
        }
    })
})
```

# 根据文字生成图片

```
<div class="container">
        <img src="" alt="">
    </div>
    <script src="./jquery.min.js"></script>
    <script>
        // text,需要生成的文字
    // font，字体样式
    var drawLogo = function(options){
    
        // options参数 
        // text:文字  backColor:背景颜色 size：canvas尺寸  font：文字大小及样式
    
        // 创建画布
        let canvas = document.createElement('canvas');
        // 绘制文字环境
        let context = canvas.getContext('2d');
        // 设置字体
        context.font = options.font;
        // 获取字体宽度
        let width = context.measureText(options.text).width;
        // 如果宽度不够 240
        if (width < 240) {
            width = 240;
        } else {
            width = width + 30;
        }
        // 画布宽度
        canvas.width = options.size || 100;
        // 画布高度
        canvas.height = options.size || 100;
        // 填充白色
        context.fillStyle = options.backColor || '#f40';
        // 绘制文字之前填充白色
        context.fillRect(0, 0, canvas.width, canvas.height);
        // 设置字体
        context.font = options.font || "48px serif";
        // 设置水平对齐方式
        context.textAlign = 'center';
        // 设置垂直对齐方式
        context.textBaseline = 'middle';
        // 设置字体颜色
        context.fillStyle = '#fff';
        // 绘制文字（参数：要写的字，x坐标，y坐标）
        context.fillText(options.text, canvas.width / 2, canvas.height / 2);
        // 生成图片信息
        let dataUrl = canvas.toDataURL('image/png');
        return dataUrl;
    }
    var src = drawLogo({text:'坤'});
    $('img').attr('src',src);
    </script>
```

# 获取下拉框的值和文本

```
options.type = addCareerModal.find('.careerName').val();
options.name = addCareerModal.find('.careerName option:selected').text().trim();
```

# 上传文件

## FormData模拟form表单

+ jquery的ajax封装

```
let request = function (url, params, successData, errorData) {
      
            showLoadingPage();
            $.ajax({
                url: homePath + url,
                type: 'post',
                async: false,
                data: params,
                contentType:false, //上传文件必须
                processData:false, //上传文件必须
                cache:false, //上传文件必须
                headers: {
                    "X-CSRF-TOKEN": token
                },
                success: function (res) {
                    hideLoadingPage();
                    if (successData) {
                        successData(res);
                    }
                }
             })
 }
              
```

+ 请求封装

```
var toolUploadData = function (options,fun) {
        requestJS.request('/tool/tool/upload', options, function (data) {
            if (fun){
                fun(data.data);
            }
        }, function (err) {
            showErrorFunction(err);
        });
};
```

+ html

```
<input type="file" class="fileDataList" style="display: none">
```

+ js

```
addMemberModal.find(".fileDataList").change(function (e) {
            var o = this.files[0];
            imgType=this.files[0].type;

            let fd = new FormData();
            fd.append('file',o);
            fd.append('type','user');

            toolUploadData(fd,(res)=>{
                console.log(res)
            });
})
```

# 字面量和变量

+ 字面量都是一些不可改变的值

```
如： 1 2 3 ‘第三方接口’
```

+ 变量可以保存字面量

```
var a = 123;
```

# 文件下载

```
< a href=" " download="${item.name}">下载</ a>

window.location.href = .....

function downFile(options){
    var config  = $.extend(true, {
        method: 'post',
        url:downLoadUrl,
    }, options);
    var $iframe = $('<iframe id="down-file-iframe" />');
    var $form   = $('<form target="down-file-iframe" method="' + config.method + '" />');
    $form.attr('action', config.url);
    for (var key in config.data) {
        $form.append('<input type="hidden" name="' + key + '" value="' + config.data[key] + '" />');
    }
    $iframe.append($form);
    $(document.body).append($iframe);
    $form[0].submit();
    $iframe.remove();
}


const token = "自行定义";//如果有 
/** * 向指定路径发送下载请求 * @param{String} url 请求路径 */
function downLoadByUrl(url) {
    var xhr = new XMLHttpRequest(); //GET请求,请求路径url,async(是否异步) 
    xhr.open('POST', url, true); //设置请求头参数的方式,如果没有可忽略此行代码
    xhr.setRequestHeader("token", token);
    //设置响应类型为 blob 
    xhr.responseType = 'blob';
    //关键部分 
    xhr.onload = function (e) {
        //如果请求执行成功 
        if (this.status == 200) {
            var blob = this.response;
            // var filename = "我是文件名.xxx";//如123.xls 
            var a = document.createElement('a');
            blob.type = "application/octet-stream";
            //创键临时url对象 
            var url = URL.createObjectURL(blob);
            a.href = url;
            // a.download=filename; 
            a.click();
            //释放之前创建的URL对象 
            window.URL.revokeObjectURL(url);
        }
    };
    //发送请求
    xhr.send();
}
```

# bootstrap弹框modal不能复制

```
$.fn.modal.Constructor.prototype.enforceFocus = function(){}; //解决modal弹框不能复制
```



# 原型和原型链

## 原型及原型链

+ new一个函数有返回值时和无返回值的情况

```
function text (){
            return {}
        }
console.log(new text())
function test (){

}
console.log(new test())
```

![image-20200914203924296](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200914203924296.png)

+ 原型的关系

![](C:\Users\k\AppData\Roaming\Typora\typora-user-images\普通对象是通过new 函数创建的.jpg)

![](C:\Users\k\AppData\Roaming\Typora\typora-user-images\隐式原型的指向.jpg)

+ 如何得到创建obj的构造函数名称

```
function A() {}

function B() {}

function create() {
    if (Math.random() < 0.5) {
        return new A();
    } else {
        return new B();
    }
}

var obj = create();
//如何得到创建obj的构造函数名称
console.log(obj.__proto__.constructor.name);
```

+ this 及 函数一般建立在原型上，不浪费内存

![image-20200914212416106](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200914212416106.png)

![image-20200914212433091](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200914212433091.png)

当直接调用User();时，this指向window

+ 原型链

![image-20200914213304764](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200914213304764.png)

+ 面试题

```
function User() {}
User.prototype.sayHello = function() {}

var u1 = new User();
var u2 = new User();

console.log(u1.sayHello === u2.sayHello); //true
console.log(User.prototype.constructor); //User Function
console.log(User.prototype === Function.prototype); // false
console.log(User.__proto__ === Function.prototype); // true
console.log(User.__proto__ === Function.__proto__); // true
console.log(u1.__proto__ === u2.__proto__);  // true
console.log(u1.__proto__ === User.__proto__); // false
console.log(Function.__proto__ === Object.__proto__); // true
console.log(Function.prototype.__proto__ === Object.prototype.__proto__); // false
console.log(Function.prototype.__proto__ === Object.prototype); // true
```

![](C:\Users\k\AppData\Roaming\Typora\typora-user-images\链条的全貌.jpg)

**# 原型和原型链**

\- 所有对象都是通过```new 函数```创建

\- 所有的函数也是对象

 \- 函数中可以有属性

\- 所有对象都是引用类型



**## 原型 prototype**



所有函数都有一个属性：prototype，称之为函数原型



默认情况下，prototype是一个普通的Object对象



默认情况下，prototype中有一个属性，constructor，它也是一个对象，它指向构造函数本身。



**## 隐式原型 __proto__**



所有的对象都有一个属性：```__proto__```，称之为隐式原型



默认情况下，隐式原型指向创建该对象的函数的原型。



当访问一个对象的成员时：



\1. 看该对象自身是否拥有该成员，如果有直接使用

\2. 在原型链中依次查找是否拥有该成员，如果有直接使用



猴子补丁：在函数原型中加入成员，以增强起对象的功能，猴子补丁会导致原型污染，使用需谨慎。



**## 原型链**



特殊点：



\1. Function的__proto__指向自身的prototype

\2. Object的prototype的__proto__指向null



## 原型链的应用

```
# 原型链的应用

## 基础方法

W3C不推荐直接使用系统成员__proto__

**Object.getPrototypeOf(对象)**

获取对象的隐式原型

**Object.prototype.isPrototypeOf(对象)**

判断当前对象(this)是否在指定对象的原型链上

**对象 instanceof 函数**

判断函数的原型是否在对象的原型链上

**Object.create(对象)**

创建一个新对象，其隐式原型指向指定的对象

**Object.prototype.hasOwnProperty(属性名)**

判断一个对象**自身**是否拥有某个属性

## 应用

**类数组转换为真数组**

​```js
Array.prototype.slice.call(类数组);
​```

**实现继承**

默认情况下，所有构造函数的父类都是Object

圣杯模式
```

### 继承（圣杯模式）

+ help.js(工具类js)

```
if (!this.myPlugin) {
    this.myPlugin = {};
}
/**
 * 继承
 */
//老版写法
this.myPlugin.inherit = (function () {
    var Temp = function () { }
    return function (son, father) {
        Temp.prototype = father.prototype;
        son.prototype = new Temp();
        son.prototype.constructor = son;
        son.prototype.uber = father.prototype;
    }
}());
//新版写法
this.myPlugin.inherit = function(son,father){
	son.prototype = Obiect.create(father.prototype);
	son.prototype.constructor = son;
	son.prototypr.uber = father.prototype;
}
```

+ 使用

```
<script src="../../plugin/helpers.js"></script>

function User(firstName, lastName, age) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.age = age;
    this.fullName = this.firstName + " " + this.lastName;
}

User.prototype.sayHello = function() {
    console.log(`大家好，我叫${this.fullName}，今年${this.age}岁了`);
}

function VIPUser(firstName, lastName, age, money) {
    User.call(this, firstName, lastName, age);
    this.money = money;
}

//先继承完，再定义子类的原型方法
myPlugin.inherit(VIPUser, User);

VIPUser.prototype.upgrade = function() {
    console.log(`使用了${100}元软妹币，升级了！`);
    this.money -= 100;
}

var vUser = new VIPUser("李", "成", 1, 100);
```

# 属性描述符

```
# 属性描述符

属性描述符的配置参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty

属性描述符：它表达了一个属性的相关信息（元数据），它本质上是一个对象。

1. 数据属性 普通定义的对象的键就是数据属性

2. 存取器属性  通过Object.defineProperty定义的属性，包含get和set函数时称为存取器属性
   1. 当给它赋值，会自动运行一个函数
   2. 当获取它的值时，会自动运行一个函数


其他的属性描述符


**Object.getOwnPropertyDescriptor**

获取某个对象的某个属性的属性描述符对象（该属性必须直接属于该对象）
```

## 存取器属性

+ 像 div.innerText = 2,div.style.backgroundColor = 'red'等都是用了属性描述符
+ 例一

```
function User(name, age) {
    this.name = name;
    //年龄的取值范围是 0 - 100
    //如果年龄的值小于了0，则赋值为0，如果年龄的值大于了100，则赋值为100
    //如果用if。。else的话，外边定义u.age会导致值可能不在范围内
    var _age;
    //函数中的this就是指对象本身，函数中的第三个对象就是指属性描述符
    Object.defineProperty(this, "age", {
        get: function() {
            return _age;
        },
        set: function(val) {
            if (val < 0) {
                val = 0;
            } else if (val > 100) {
                val = 100;
            }
            _age = val;
        }
    })

    this.age = age;
}

var u = new User("abc", -1);
u.age = u.age + 10000;
console.log(u.age);
```

+ 例二

```
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .move {
            position: fixed;
            left: 0;
            top: 0;
            width: 100px;
            height: 100px;
            border-radius: 50%;
            background: red;
        }
    </style>
</head>

<body>
    <div class="move">
        <!-- 该div自动向右下移动，遇到边界反弹 -->
    </div>
    <script>
        var div = document.querySelector(".move");
        var config = {
            _x: 0,
            _y: 0,
            xDis: 2,
            yDis: 2,
            duration: 16,
            width: 100,
            height: 100
        }

        Object.defineProperty(config, "x", {
            get: function() {
                return this._x;
            },
            set: function(val) {
                if (val < 0) {
                    val = 0;
                } else if (val > document.documentElement.clientWidth - this.width) {
                    val = document.documentElement.clientWidth - this.width;
                }
                this._x = val;
                div.style.left = val + "px";
            }
        })

        Object.defineProperty(config, "y", {
            get: function() {
                return this._y;
            },
            set: function(val) {
                if (val < 0) {
                    val = 0;
                } else if (val > document.documentElement.clientHeight - this.height) {
                    val = document.documentElement.clientHeight - this.height;
                }
                this._y = val;
                div.style.top = val + "px";
            }
        })
    </script>
</body>
```

## 其他属性

```
<script>
    var obj = {
        x: 1,
        y: 2
    };

    //obj的name属性值固定为abc，而且不能被重新赋值

    // Object.defineProperty(obj, "name", {
    //     get: function() {
    //         return "abc";
    //     }
    // })

    Object.defineProperty(obj, "name", {
        value: "abc",
        writable: false, //可读属性
        enumerable: true //不可迭代 遍历
    })

    for(var prop in obj){
        console.log(prop);
    }
</script>
```

# 执行上下文

```
# 执行上下文

函数执行上下文：一个函数运行之前，创建的一块内存空间，空间中包含有该函数执行所需要的数据，为该函数执行提供支持。

执行上下文栈：call stack，所有执行上下文组成的内存空间。

栈：一种数据结构，先进后出，后进先出。

全局执行上下文：所有JS代码执行之前，都必须有该环境。

JS引擎始终执行的是栈顶的上下文。

## 执行上下文中的内容

1. this指向

1). 直接调用函数，this指向全局对象
2). 在函数外，this指向全局对象
3). 通过对象调用或new一个函数，this指向调用的对象或新对象

2. VO 变量对象

Variable Object：VO 中记录了该环境中所有声明的参数、变量和函数

Global Object: GO，全局执行上下文中的VO

Active Object：AO，当前正在执行的上下文中的VO


1). 确定所有形参值以及特殊变量arguments
2). 确定函数中通过var声明的变量，将它们的值设置为undefined，如果VO中已有该名称，则直接忽略。
3). 确定函数中通过字面量声明的函数，将它们的值设置为指向函数对象，如果VO中已存在该名称，则覆盖。

当一个上下文中的代码执行的时候，如果上下文中不存在某个属性，则会从之前的上下文寻找。
```

![image-20200918215746032](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200918215746032.png)

# 作用域链

```
# 作用域链

1. VO中包含一个额外的属性，该属性指向创建该VO的函数本身
2. 每个函数在创建时，会有一个隐藏属性```[[scope]]```，它指向创建该函数时的AO
3. 当访问一个变量时，会先查找自身VO中是否存在，如果不存在，则依次查找```[[scope]]```属性。

某些浏览器会优化作用域链，函数的```[[scope]]```中仅保留需要用到的数据。
```

+ 例1

```
var g = 0;

function A() {
    var a = 1;

    function B() {
        var b = 2;
        var C = function() {
            var c = 3;
            console.log(c, b, a, g);
        }
        C();
    }

    B();
}

A();
```

![image-20200922203446559](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200922203446559.png)

+ 例2

```
var count = 100;
function A() {
    var count = 0;
    return function() {
        count++;
        console.log(count);
    }
}

var test = A();

test();
test();
test();

console.log(count);

//闭包广义上是，函数用到了外边的变量
狭义上是用到了外边作用域消失留下的变量
```

+ 例3

```
var a = 1;

function A() {
    console.log(a);
}

function Special() {
    var a = 5;
    var B = A;
    B();
}

Special();
```

+ 例4

```
// function A() {
//     for (var i = 0; i < 10; i++) {
//         setTimeout(function () {
//             console.log(i);
//         }, 1000)
//     }
// }

// A();

// console.log(i);


for (var i = 0; i < 3; i++) {
    (function (i) {
        setTimeout(function () {
            console.log(i);
        }, 1000)
    }(i));
}
```

# 事件循环

```
# 事件循环

异步：某些函数不会立即执行，需要等到某个时机成熟后才会执行，该函数叫做异步函数。

浏览器的线程：

1. JS执行引擎：负责执行JS代码
2. 渲染线程：负责渲染页面
3. 计时器线程：负责计时
4. 事件监听线程：负责监听事件
5. http网络线程：负责网络通信

事件队列：一块内存空间，用于存放执行时机到达的异步函数。当JS引擎空闲（执行栈没有可执行的上下文），它会从事件队列中拿出第一个函数执行。

事件循环：event loop，是指函数在执行栈、宿主线程、事件队列中的循环移动。
```

![image-20200922211100235](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200922211100235.png)

```
document.querySelector("button").onclick = function A() {
    setTimeout(function B() {
        console.log("异步代码")
    }, 0);
    loop();
}

function loop() {
    for (var i = 0; i < 10000; i++) {
        console.log(i);
    }
}

loop();
```

# 对象混合和克隆

```
/**
 * obj2混合到obj1产生新的对象
 */
this.myPlugin.mixin = function (obj1, obj2) {
    return Object.assign({}, obj1, obj2);
    // var newObj = {};
    // //复制obj2的属性
    // for (var prop in obj2) {
    //     newObj[prop] = obj2[prop];
    // }
    // //找到obj1中有但是obj2中没有的属性
    // for (var prop in obj1) {
    //     if (!(prop in obj2)) {
    //         newObj[prop] = obj1[prop];
    //     }
    // }
    // return newObj;
}

/**
 * 克隆一个对象
 * @param {boolean} deep 是否深度克隆
 */
this.myPlugin.clone = function (obj, deep) {
    if (Array.isArray(obj)) {
        if (deep) {
            //深度克隆
            var newArr = [];
            for (var i = 0; i < obj.length; i++) {
                newArr.push(this.clone(obj[i], deep));
            }
            return newArr;
        }
        else {
            return obj.slice(); //复制数组
        }
    }
    else if (typeof obj === "object") {
        var newObj = {};
        for (var prop in obj) {
            if (deep) {
                //深度克隆
                newObj[prop] = this.clone(obj[prop], deep);
            }
            else {
                newObj[prop] = obj[prop];
            }
        }
        return newObj;
    }
    else {
        //函数、原始类型
        return obj; //递归的终止条件
    }
}
```

# 正则表达式

## 普通字符

### 匹配中文

```
匹配中文： ```[\u4e00-\u9FA5]```
```

普通字符包括没有显式指定为元字符的所有可打印和不可打印字符。这包括所有大写和小写字母、所有数字、所有标点符号和一些其他符号。

| 字符   | 描述                                                         |
| :----- | :----------------------------------------------------------- |
| [ABC]  | 匹配 **[...]** 中的所有字符，例如 **[aeiou]** 匹配字符串 "google runoob taobao" 中所有的 e o u a 字母。![img](https://www.runoob.com/wp-content/uploads/2014/03/E691DDE1-E5CB-4EA8-8D16-759BD0D2B09D.jpg) |
| [^ABC] | 匹配除了 **[...]** 中字符的所有字符，例如 **[^aeiou]** 匹配字符串 "google runoob taobao" 中除了 e o u a 字母的所有字母。![img](https://www.runoob.com/wp-content/uploads/2014/03/ED971D92-30F4-4768-A2C7-02A84A3A9DEB.jpg) |
| [A-Z]  | [A-Z] 表示一个区间，匹配所有大写字母，[a-z] 表示所有小写字母。![img](https://www.runoob.com/wp-content/uploads/2014/03/C5E357BD-65E3-4EB3-9D80-10D096F19287.jpg) |
| .      | 匹配除换行符（\n、\r）之外的任何单个字符，相等于 [^\n\r]。![img](https://www.runoob.com/wp-content/uploads/2014/03/0FD7E77D-38A7-43BC-B51A-7DBA23A77756.jpg) |
| [\s\S] | 匹配所有。\s 是匹配所有空白符，包括换行，\S 非空白符，包括换行。![img](https://www.runoob.com/wp-content/uploads/2014/03/47CA6C59-64CF-433A-909E-1E342349A4E0.jpg) |
| \w     | 匹配字母、数字、下划线。等价于 [A-Za-z0-9_]![img](https://www.runoob.com/wp-content/uploads/2014/03/F35A5971-3519-4CAE-8BEC-9DE8F4A55257.jpg) |

## 非打印字符

非打印字符也可以是正则表达式的组成部分。下表列出了表示非打印字符的转义序列：

| 字符 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| \cx  | 匹配由x指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。x 的值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。 |
| \f   | 匹配一个换页符。等价于 \x0c 和 \cL。                         |
| \n   | 匹配一个换行符。等价于 \x0a 和 \cJ。                         |
| \r   | 匹配一个回车符。等价于 \x0d 和 \cM。                         |
| \s   | 匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注意 Unicode 正则表达式会匹配全角空格符。 |
| \S   | 匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。                  |
| \t   | 匹配一个制表符。等价于 \x09 和 \cI。                         |
| \v   | 匹配一个垂直制表符。等价于 \x0b 和 \cK。                     |

## 特殊字符

所谓特殊字符，就是一些有特殊含义的字符，如上面说的 **runoo\*b** 中的 *****，简单的说就是表示任何字符串的意思。如果要查找字符串中的 ***** 符号，则需要对 ***** 进行转义，即在其前加一个 **\**: **runo\*ob** 匹配 runo*ob。

许多元字符要求在试图匹配它们时特别对待。若要匹配这些特殊字符，必须首先使字符"转义"，即，将反斜杠字符**\** 放在它们前面。下表列出了正则表达式中的特殊字符：

| 特别字符 | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| $        | 匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 '\n' 或 '\r'。要匹配 $ 字符本身，请使用 \$。 |
| ( )      | 标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用。要匹配这些字符，请使用 \( 和 \)。 |
| *        | 匹配前面的子表达式零次或多次。要匹配 * 字符，请使用 \*。     |
| +        | 匹配前面的子表达式一次或多次。要匹配 + 字符，请使用 \+。     |
| .        | 匹配除换行符 \n 之外的任何单字符。要匹配 . ，请使用 \. 。    |
| [        | 标记一个中括号表达式的开始。要匹配 [，请使用 \[。            |
| ?        | 匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。要匹配 ? 字符，请使用 \?。 |
| \        | 将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '\\' 匹配 "\"，而 '\(' 则匹配 "("。 |
| ^        | 匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号表达式中使用时，表示不接受该方括号表达式中的字符集合。要匹配 ^ 字符本身，请使用 \^。 |
| {        | 标记限定符表达式的开始。要匹配 {，请使用 \{。                |
| \|       | 指明两项之间的一个选择。要匹配 \|，请使用 \|。               |

## 限定符

限定符用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配。有 ***** 或 **+** 或 **?** 或 **{n}** 或 **{n,}** 或 **{n,m}** 共6种。

正则表达式的限定符有：

| 字符  | 描述                                                         |
| :---- | :----------------------------------------------------------- |
| *     | 匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。* 等价于{0,}。 |
| +     | 匹配前面的子表达式一次或多次。例如，'zo+' 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。 |
| ?     | 匹配前面的子表达式零次或一次。例如，"do(es)?" 可以匹配 "do" 、 "does" 中的 "does" 、 "doxy" 中的 "do" 。? 等价于 {0,1}。 |
| {n}   | n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，但是能匹配 "food" 中的两个 o。 |
| {n,}  | n 是一个非负整数。至少匹配n 次。例如，'o{2,}' 不能匹配 "Bob" 中的 'o'，但能匹配 "foooood" 中的所有 o。'o{1,}' 等价于 'o+'。'o{0,}' 则等价于 'o*'。 |
| {n,m} | m 和 n 均为非负整数，其中n <= m。最少匹配 n 次且最多匹配 m 次。例如，"o{1,3}" 将匹配 "fooooood" 中的前三个 o。'o{0,1}' 等价于 'o?'。请注意在逗号和两个数之间不能有空格。 |

以下正则表达式匹配一个正整数，**[1-9]**设置第一个数字不是 0，**[0-9]\*** 表示任意多个数字：

```
/[1-9][0-9]*/
```

![img](https://www.runoob.com/wp-content/uploads/2014/03/F249891D-F3D9-48D5-A3CB-6FD8FD029117.jpg)

请注意，限定符出现在范围表达式之后。因此，它应用于整个范围表达式，在本例中，只指定从 0 到 9 的数字（包括 0 和 9）。

这里不使用 + 限定符，因为在第二个位置或后面的位置不一定需要有一个数字。也不使用 ? 字符，因为使用 ? 会将整数限制到只有两位数。

如果你想设置 0~99 的两位数，可以使用下面的表达式来至少指定一位但至多两位数字。

```
/[0-9]{1,2}/
```

上面的表达式的缺点是，只能匹配两位数字，而且可以匹配 0、00、01、10 99 的章节编号仍只匹配开头两位数字。

改进下，匹配 1~99 的正整数表达式如下：



```
/[1-9][0-9]?/
```

或

```
/[1-9][0-9]{0,1}/
```

***\**\** 和 \**+\** 限定符都是贪婪的，因为它们会尽可能多的匹配文字，只有在它们的后面加上一个 ? 就可以实现非贪婪或最小匹配。**

例如，您可能搜索 HTML 文档，以查找在 **h1** 标签内的内容。HTML 代码如下：

```
<h1>RUNOOB-菜鸟教程</h1>
```

**贪婪：**下面的表达式匹配从开始小于符号 (<) 到关闭 h1 标记的大于符号 (>) 之间的所有内容。

```
/<.*>/
```

![img](https://www.runoob.com/wp-content/uploads/2014/03/AD8F3320-2F2E-4513-9BB5-84450D62783D.jpg)

**非贪婪：**如果您只需要匹配开始和结束 h1 标签，下面的非贪婪表达式只匹配 <h1>。

```
/<.*?>/
```

![img](https://www.runoob.com/wp-content/uploads/2014/03/A6E72665-CE61-46F4-A72B-A34BC13F5820.jpg)

也可以使用以下正则表达式来匹配 h1 标签，表达式则是：

```
/<\w+?>/
```

![img](https://www.runoob.com/wp-content/uploads/2014/03/C6E89F76-D059-4600-A507-74C42306A790.jpg)

通过在 *****、**+** 或 **?** 限定符之后放置 **?**，该表达式从"贪婪"表达式转换为"非贪婪"表达式或者最小匹配。



## 定位符

定位符使您能够将正则表达式固定到行首或行尾。它们还使您能够创建这样的正则表达式，这些正则表达式出现在一个单词内、在一个单词的开头或者一个单词的结尾。

定位符用来描述字符串或单词的边界，**^** 和 **$** 分别指字符串的开始与结束，**\b** 描述单词的前或后边界，**\B** 表示非单词边界。

正则表达式的定位符有：

| 字符 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| ^    | 匹配输入字符串开始的位置。如果设置了 RegExp 对象的 Multiline 属性，^ 还会与 \n 或 \r 之后的位置匹配。 |
| $    | 匹配输入字符串结尾的位置。如果设置了 RegExp 对象的 Multiline 属性，$ 还会与 \n 或 \r 之前的位置匹配。 |
| \b   | 匹配一个单词边界，即字与空格间的位置。                       |
| \B   | 非单词边界匹配。                                             |

**注意**：不能将限定符与定位符一起使用。由于在紧靠换行或者单词边界的前面或后面不能有一个以上位置，因此不允许诸如 **^\*** 之类的表达式。

若要匹配一行文本开始处的文本，请在正则表达式的开始使用 **^** 字符。不要将 **^** 的这种用法与中括号表达式内的用法混淆。

若要匹配一行文本的结束处的文本，请在正则表达式的结束处使用 **$** 字符。

若要在搜索章节标题时使用定位点，下面的正则表达式匹配一个章节标题，该标题只包含两个尾随数字，并且出现在行首：

```
/^Chapter [1-9][0-9]{0,1}/
```

真正的章节标题不仅出现行的开始处，而且它还是该行中仅有的文本。它即出现在行首又出现在同一行的结尾。下面的表达式能确保指定的匹配只匹配章节而不匹配交叉引用。通过创建只匹配一行文本的开始和结尾的正则表达式，就可做到这一点。

```
/^Chapter [1-9][0-9]{0,1}$/
```

匹配单词边界稍有不同，但向正则表达式添加了很重要的能力。单词边界是单词和空格之间的位置。非单词边界是任何其他位置。下面的表达式匹配单词 Chapter 的开头三个字符，因为这三个字符出现在单词边界后面：

```
/\bCha/
```

**\b** 字符的位置是非常重要的。如果它位于要匹配的字符串的开始，它在单词的开始处查找匹配项。如果它位于字符串的结尾，它在单词的结尾处查找匹配项。例如，下面的表达式匹配单词 Chapter 中的字符串 ter，因为它出现在单词边界的前面：

```
/ter\b/
```

下面的表达式匹配 Chapter 中的字符串 apt，但不匹配 aptitude 中的字符串 apt：

```
/\Bapt/
```

字符串 apt 出现在单词 Chapter 中的非单词边界处，但出现在单词 aptitude 中的单词边界处。对于 **\B** 非单词边界运算符，位置并不重要，因为匹配不关心究竟是单词的开头还是结尾。

------

## 选择

用圆括号 **()** 将所有选择项括起来，相邻的选择项之间用 **|** 分隔。

**()** 表示捕获分组，**()** 会把每个分组里的匹配的值保存起来， 多个匹配值可以通过数字 n 来查看(**n** 是一个数字，表示第 n 个捕获组的内容)。

![img](https://www.runoob.com/wp-content/uploads/2014/03/366574CC-3706-4B4C-8782-1BFF4CF57582.jpg)

![img](https://www.runoob.com/wp-content/uploads/2014/03/82A7298A-2A94-49E3-AA27-A7778EE89711.jpg)

但用圆括号会有一个副作用，使相关的匹配会被缓存，此时可用 **?:** 放在第一个选项前来消除这种副作用。

其中 **?:** 是非捕获元之一，还有两个非捕获元是 **?=** 和 **?!**，这两个还有更多的含义，前者为正向预查，在任何开始匹配圆括号内的正则表达式模式的位置来匹配搜索字符串，后者为负向预查，在任何开始不匹配该正则表达式模式的位置来匹配搜索字符串。

### 以下列出 ?=、?<=、?!、?<!= 的使用区别

**exp1(?=exp2)**：查找 exp2 前面的 exp1。

![img](https://www.runoob.com/wp-content/uploads/2014/03/reg-111.jpg)

**(?<=exp2)exp1**：查找 exp2 后面的 exp1。

![img](https://www.runoob.com/wp-content/uploads/2014/03/reg-222.jpg)

**exp1(?!exp2)**：查找后面不是 exp2 的 exp1。

![img](https://www.runoob.com/wp-content/uploads/2014/03/reg-333.jpg)

**(?：查找前面不是 exp2 的 exp1。

![img](https://www.runoob.com/wp-content/uploads/2014/03/reg-444.jpg)

## 贪婪匹配

\> 正则表达式，默认情况下，适用贪婪模式

\> 在量词后，加上?，表示进入非贪婪模式

```
var reg = /\d+/g;
var reg = /\d+？/g; //打破贪婪匹配
var s = "1234abc123aaa";
console.log(s, reg.lastIndex, reg.test(s), reg.lastIndex);
console.log(s, reg.lastIndex, reg.test(s), reg.lastIndex);
console.log(s, reg.lastIndex, reg.test(s), reg.lastIndex);
console.log(s, reg.lastIndex, reg.test(s), reg.lastIndex);
console.log(s, reg.lastIndex, reg.test(s), reg.lastIndex);
console.log(s, reg.lastIndex, reg.test(s), reg.lastIndex);

//判断匹配多少处
// var n = 0;
// while (reg.test(s)) {
//     n++;
// }

// console.log(`匹配了${n}次`);
```

##  正则实例成员

\- global

\- ignoreCase

\- multiline

\- source

\- test方法：验证某个字符串是否满足规则

\- exec方法：execute，执行匹配，得到匹配结果。

```
var reg = /\d+/g;
var s = "1234abc123aaa";

//得到所有的匹配结果和位置
while (result = reg.exec(s)) {
    console.log(`匹配结果：${result[0]}，出现位置：${result.index}`);
}

//判断匹配多少处
// var n = 0;
// while (reg.test(s)) {
//     n++;
// }

// console.log(`匹配了${n}次`);
```

## 字符串对象中的正则方法

\- split

\- replace

\- search

\- match

```
var s = "hello world\tjavascript\nyes";
//将空白字符替换为逗号
console.log(s);
s = s.replace(/\s*\b[a-z]\s*/g, function(match){
    return match.toUpperCase().trim();
});
console.log(s);

//按照逗号、空格、横杠、制表符进行分割
// var result = s.split(/[, \-\s]/);
// console.log(s, result);
// var result = s.match(/\d+/)
// console.log(s, result);

// var result = s.search(/\d+/g)
// console.log(s, result);
// result = s.search(/\d+/g)
// console.log(s, result);
```

## 例子

+ 书写一个正则表达式，去匹配一个字符串，得到匹配的次数，和匹配的结果

```
var reg = /\d{3}/g;
var s = "433afdsaf34542fsdssfsd234";
var n = 0;
var str = "";
while (result = reg.exec(s)) {
    n++;
    str += result[0] + "\n";
}
str = `匹配${n}次\n` + str;
console.log(str);
```

+ 得到一个字符串中中文字符的数量

```
var reg = /[\u4e00-\u9fa5]/g;
var s = "fgdgg啊手动sdf阀梵蒂冈sd234";
var n = 0;
while (reg.test(s)) {
    n++;
}
console.log(n);
```

+ 过滤敏感词，有一个敏感词数组，需要将字符串中出现的敏感词替换为四个星号

```
var senWords = ["色情", "暴力", "卢本伟", "贸易战"];
//将字符串中敏感词汇替换为指定的字符串
function removeSensitiveWords(s, rep) {
    var reg = new RegExp(`(${senWords.join("|")})+`, "g");
    return s.replace(reg, rep);
}

console.log(removeSensitiveWords("sdffs色情暴力sfsfs卢本伟牛逼dsdf贸易战sf", "****"))
```

+ 只能输入正整数

```
 this.value = $(this).val().replace(/^(0+)|[^\d]+/g,''); //不包括0
  this.value = $(this).val().replace(/^(0.)|[^\d]+/g,'');//包括0
   this.value = this.value.replace(/[^\d.]/g, ""); //清除"数字"和"."以外的字符 
   this.value = this.value.replace(/^\./g, ""); //验证第一个字符是数字而不是 this.value = this.value.replace(/\.{2,}/g, "."); //只保留第一个. 清除多余的 
   this.value = this.value.replace(".", "$#$").replace(/\./g, "").replace("$#$", ".");
   this.value = this.value.replace(/^(\-)*(\d+)\.(\d\d).*$/, '$1$2.$3'); //只能输入两个小数
```



##  捕获组

+ 用小括号包裹的部分叫做捕获组，捕获组会出现在匹配结果中

```
var reg = /(\d[a-z])([a-z]+)/g;
var s = "2afsdf-5fdgdfg-9asddf";
while (result = reg.exec(s)) {
    console.log(result);
}
```

+ 捕获组可以命名，叫做具名捕获组

```
var s = "2015-5-1, 2019-6-19, 2000-04-28";
//得到每一个日期，并得到每个日期的年月日
var reg = /(?<year>\d{4})-(?<month>\d{1,2})-(?<day>\d{1,2})/g;
while (result = reg.exec(s)) {
    console.log(result);
}
```

+ 非捕获组，只是把括号内东西当成一个整体，不捕获，节省效率

```
var s = "2015-5-1, 2019-6-19, 2000-04-28";
//得到每一个日期，并得到每个日期的年月日
var reg = /(?:\d{4})-(?:\d{1,2})-(?:\d{1,2})/g;
while (result = reg.exec(s)) {
    console.log(result);
}
```

+ replace

```
var s = "2015-5-1,- 2019-6-19,- 2000-04-28";
var reg = /(\d{4})-(\d{1,2})-(\d{1,2})/g;
s = s.replace(reg, function(match,g1,g2,g3){
    console.log(match,g1,g2,g3)
    return `${g1}/${g2}/${g3}`;
})
// s = s.replace(reg, "$1/$2/$3")
console.log(s);
```

## 反向引用

在正则表达式中，使用某个捕获组，```\捕获组编号```

```
// var reg = /(\d{2})\1/; //重复出现一次
// var s = "1313";

// console.log(reg.test(s));

var s = "aaaaaaaabbbbbbbbbccccccdefgggggggg";
//找出该字符串中连续的字符
var reg = /(?<char>\w)\k<char>+/g;   //\k<char>表示char具名的捕获组重复一次或多次
while (result = reg.exec(s)) {
    console.log(result[1]);
}
```

## 正向断言(预查)

检查某个字符后面的字符是否满足某个规则，该规则不成为匹配结果，并且不称为捕获组

```
// var s = "sdfsdf3434343sdfsa545454dfsdfsfsd6754";
// var reg = /[a-zA-Z](?=\d+)/g;
// while (result = reg.exec(s)) {
//     console.log(result);
// }

// var s = "334353456";
// // var result = "43,534,512,312";
// var reg = /\B(?=(\d{3})+$)/g;
// s = s.replace(reg, ",");
// console.log(s);
```

## 负向断言(预查)

检查某个字符后面的字符是否不满足某个规则，该规则不成为匹配结果，并且不称为捕获组

```
// var s = "afg43223444wr423424243";
// var reg = /[a-zA-Z](?!\d+)/g;
// while (result = reg.exec(s)) {
//     console.log(result);
// }

// 判断密码强度
// 要求密码中必须出现小写字母、大写字母、数字、特殊字符(!@#_,.)，6-12位
// var s = "asdfsdAf234."
// var reg = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#_,.]).{6,12}$/;
// console.log(reg.test(s));

// 判断密码强度
// 密码长度必须是6-12位
// 出现小写字母、大写字母、数字、特殊字符(!@#_,.)  -> 强
// 出现小写字母、大写字母、数字  -> 中
// 出现小写字母、大写字母  -> 轻
// 其他  -> 不满足要求

function judgePwd(pwd) {
    if (/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[!@#_,.]).{6,12}$/.test(pwd)) {
        return "强";
    } else if (/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{6,12}$/.test(pwd)) {
        return "中";
    } else if (/^(?=.*[a-z])(?=.*[A-Z]).{6,12}$/.test(pwd)) {
        return "轻";
    } else {
        return "不满足要求";
    }
}

console.log(judgePwd("asdADFF4.343"));
```

# js粘贴截图图片

1.ctrl+v粘贴图片都是监听paste时间实现的，复制的数据都存在clipboardData下面，虽然打印显示数据长度为0，但是还是可以获取数据的

![img](https://images2017.cnblogs.com/blog/1075863/201711/1075863-20171122162705821-2126317405.png)

 2.打印clipboardData.items发现是一个DataTransferItem。

![img](https://images2017.cnblogs.com/blog/1075863/201711/1075863-20171122165643274-423065393.png)

3.DataTransferItem有个getAsFile()的方法，可以获取文件

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

此时就可以获取到blob对象了，这时候可以选择显示在页面上，也可以选择发送给后台

3.1显示图片

　　3.1.1执行下面代码即可,使用blob对象显示

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

效果图

![img](https://images2017.cnblogs.com/blog/1075863/201711/1075863-20171122171019008-1754499842.gif)

　　3.1.2使用base64码显示，需要借助FileReader

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

3.2上传到后台

　　3.2.1生成formData,这里生成formData

![img](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif) View Code

 

3.3完整代码

完整代码如下

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        body {
            display: -webkit-flex;
            display: flex;
            -webkit-justify-content: center;
            justify-content: center;
        }
    </style>
</head>
<body>
<textarea></textarea>
<div contenteditable style="width: 300px;height: 100px; border:1px solid">
    <img src="" id="imgNode">
</div>
</body>
<script>
    document.addEventListener('paste', function (event) {
        console.log(event);
        var isChrome = false;
        if (event.clipboardData || event.originalEvent) {
            //某些chrome版本使用的是event.originalEvent
            var clipboardData = (event.clipboardData || event.originalEvent.clipboardData);
            if(clipboardData.items){
                // for chrome
                var  items = clipboardData.items,
                    len = items.length,
                    blob = null;
                isChrome = true;
                for (var i = 0; i < len; i++) {
                    console.log(items[i]);
                    if (items[i].type.indexOf("image") !== -1) {
                        //getAsFile() 此方法只是living standard firefox ie11 并不支持
                        blob = items[i].getAsFile();
                    }
                }
                if(blob!==null){
                    var blobUrl=URL.createObjectURL(blob);
                    //blob对象显示
                    document.getElementById("imgNode").src=blobUrl;
                    var reader = new FileReader();
                    //base64码显示
                   /* reader.onload = function (event) {
                        // event.target.result 即为图片的Base64编码字符串
                        var base64_str = event.target.result;

                        document.getElementById("imgNode").src=base64_str;
                    }
                    reader.readAsDataURL(blob);*/var fd = new FormData(document.forms[0]);
                    fd.append("the_file", blob, 'image.png');
                    //创建XMLHttpRequest对象
                    var xhr = new XMLHttpRequest();
                    xhr.open('POST','/image' );
                    xhr.onload = function () {
                        if ( xhr.readyState === 4 ) {
                            if ( xhr.status === 200 ) {
                                var data = JSON.parse( xhr.responseText );
                                console.log(data);
                            } else {
                                console.log( xhr.statusText );
                            }
                        };
                    };
                    xhr.onerror = function (e) {
                        console.log( xhr.statusText );
                    }
                    xhr.send(fd);
                }
            }
        }
    })
</script>
</html>
```

# data-XXX传递对象

```
//数据必须为JSON格式
let obj = {
    "url":res,
    "name":name,
    "type":'image/jpeg'
};
obj = JSON.stringify(obj);//转化为字符串
//一定要注意data-obj用单引号
$(this).append(`<div class="pictureList" data-obj='${obj}' data-url="${homePath+res}">`);

//接收
let obj = $(item).data('obj');

```

# 阻止页面默认事件

```
document.addEventListener("touchmove",function(e){
   e.preventDefault();
},{passive:false});
```

# 判断系统及浏览器

+ 判断安卓或ios系统

```
<!--判断是否是安卓机-->
function detect(){
    var equipmentType = "";
    var agent = navigator.userAgent.toLowerCase();
    var android = agent.indexOf("android");
    var iphone = agent.indexOf("iphone");
    var ipad = agent.indexOf("ipad");
    if(android != -1){
        equipmentType = "android";
    }
    if(iphone != -1 || ipad != -1){
        equipmentType = "ios";
    }
    return equipmentType;
}
//android 返回 android
//ios 返回 ios
```

+ 判断是否使用微信浏览器打开

```
//判断是否是微信浏览器
var isWeixinBrowser = () => {
    var agent = navigator.userAgent.toLowerCase();
    if (agent.match(/MicroMessenger/i) == "micromessenger") {
        return true;
    } else {
        return false;
    }
};
//是 返回 true
//不是 返回 false
```

# 解决click与touchstart事件冲突

## 一 · 业务场景的描述

------

 

 

在对已完成的PC站点进行移动端适配时，我们想要站点在移动设备上有更快的响应速度，以带给用户更好的体验，此时，我们应该使用移动设备专用的事件系统，例如，使用 `touchstart` 事件代替 `click` 事件。

为什么这样效果会更好呢？根据Google开发者文档中的描述：

移动设备上的浏览器将会在 `click` 事件触发时延迟 300ms ，以确保这是一个“单击”事件而非“双击”事件。

而对于 `touchstart` 事件而言，则会在用户手指触碰屏幕的一瞬间触发所绑定的事件。所以，使用 `touchstart` 替换 `click` 事件的意义在于，帮助用户在每次点击时节省 **300ms** 的时间。在页面频繁需要点击，或者点击发生在动画中，对动画流畅度有较高要求的情境下，使用这种技术是非常必要的。

但是，让我们回到我们的初始场景，在 PC端站点适配移动端时 我们不能简单的进行 `touchstart`和 `click` 事件的替换，因为PC并不能识别 `touchstart` 事件。

 

## 二 · 产生冲突的原因

------

 

当然，我们可以给某个元素同时绑定 `touchstart` 和 `click` 事件，但这将会导致本篇文章解决的问题 -- 这两个事件在移动设备上会发生冲突。

由于移动设备能够同时识别 `touchstart` 和 `click` 事件，因此当用户点击目标元素时，绑定在目标元素上的 `touchstart` 事件与 `click` 事件（约300ms后）会依次被触发，也就是说，**我们所绑定的回调函数会被执行两次！**。这显然不是我们想要的结果。

 

## 三 · 解决方案



针对这样的情境，有以下两种解决方案：

### （一）使用 preventDefault

第一种解决方案是使用事件对象中的 `preventDefault` 方法,`preventDefault` 方法的作用在于：阻止元素默认事件行为的发生，但有意思的是，当我们在目标元素同时绑定 `touchstart` 和 `click` 事件时，在 `touchstart` 事件回调函数中使用该方法，可以阻止后续 `click` 事件的发生。

这从道理上是讲不通的，毕竟，我们添加的 `click` 事件并不是元素的“默认事件”，但它确实奏效了，或者说，被浏览器实现了，因此我们可以使用该方法解决移动设备上 `touchstart` 事件与 `click` 事件的冲突问题，具体代码如下：

```
const Button = document.getElementById(``"targetButton"``)` `Button.addEventListener(``"touchstart"``, e => {``  ``e.preventDefault()``  ``console.log(``"touchstart event!"``)``})` `Button.addEventListener(``"click"``, e => {``  ``e.preventDefault()``  ``console.log(``"click event!"``)``})
```

当你在浏览器上模拟移动设备后点击目标元素，只会在控制台看到 `touchstart event!` 字段，很显然，`click` 事件被成功阻止了。

总结

 使用该方法的优点在于简单粗暴，直接有效，能够很好的实现我们的目标，但缺点在于， `preventDefault` 方法为阻止 `click` 事件的方式是浏览器实现上的，而不是 `preventDefault` 原理上的，这会带来一些不确定性，虽然我暂时尚未发现该方法失效的具体场景。

### （二）基于功能检测绑定事件

我们可以通过判断浏览器是否支持 `touchstart` 事件来封装元素的点击事件，这样客户端会根据当前环境判定元素应该绑定的事件类型，代码如下：

```
const Button = document.getElementById(``"targetButton"``)` `
const clickEvent = (function() {
if('ontouchstart' in document.documentElement === true)  
	return 'touchstart';
	else
	return 'click';
})();
Button.addEventListener(clickEvent, e => {
	console.log(``"things happened!"``)
})
```

总结

该方法的优点在于，我们通过增加一次判断，为元素减少了一个不必要的事件绑定，从而避免了 `touchstart` 与 `click` 事件的冲突问题。这种方法避免了我们书写两次同样的代码，并且相较于第一种方法更加符合逻辑，因此是我所推荐的。



# 解决移动端页面滚动误触发touchend事件



## 问题

在移动端页面进行优化时，一般使用touch事件替代鼠标相关事件，用的较多的是使用touchend事件替代PC端的click和mouseup事件。

但是，touchend事件在页面滚动时有个问题。在滚动完成后，如果当前触点的位置所指的元素绑定了touchend事件，这时便会触发该元素的touchend事件，造成误操作。

## 解决方法

解决方法很简单，就是在页面滚动时停止touchend事件冒泡，这样就可以防止触发touchend事件。

### 使用方法

引入该函数并进行调用。

```js
function stopTouchendPropagationAfterScroll(){
    var locked = false;

    window.addEventListener('touchmove', function(ev){
       locked || (locked =   true,window.addEventListener('touchend',stopTouchendPropagation, true));		                    	                      
    }, true);
    function stopTouchendPropagation(ev){
        ev.stopPropagation();
        window.removeEventListener('touchend', stopTouchendPropagation, true);
        locked = false;
    }
}
```

### 另外说明

在移动端，scroll事件是在滚动结束后才会触发一次，而touchmove事件是在滑动过程中多次触发，使用scroll会比使用touchmove在性能上有一定优化。但是，上面代码之所以不用scroll事件，而用touchmove事件，是为了同时适用于小于一个屏幕高度的页面，所以也是不得已使用touchmove

# 获取随机数

```
function getRandom(min, max) {
    return Math.floor(Math.random() * (max - min) + min);
}
```

