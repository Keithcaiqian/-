

# promise

一、
1、promise图解
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504181009217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25ld2JpZWs=,size_16,color_FFFFFF,t_70)
2、代码示例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504181109203.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25ld2JpZWs=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504181230710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25ld2JpZWs=,size_16,color_FFFFFF,t_70)
二、封装promiseAPI
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504182015937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25ld2JpZWs=,size_16,color_FFFFFF,t_70)

## **Promise的基本使用**

```
const pro = new Promise((resolve, reject)=>{
    // 未决阶段的处理
    // 通过调用resolve函数将Promise推向已决阶段的resolved状态
    // 通过调用reject函数将Promise推向已决阶段的rejected状态
    // resolve和reject均可以传递最多一个参数，表示推向状态的数据
})

pro.then(data=>{
    //这是thenable函数，如果当前的Promise已经是resolved状态，该函数会立即执行
    //如果当前是未决阶段，则会加入到作业队列，等待到达resolved状态后执行
    //data为状态数据
}, err=>{
    //这是catchable函数，如果当前的Promise已经是rejected状态，该函数会立即执行
    //如果当前是未决阶段，则会加入到作业队列，等待到达rejected状态后执行
    //err为状态数据
})
​```

**细节**

1. 未决阶段的处理函数是同步的，会立即执行
2. thenable和catchable函数是异步的，就算是立即执行，也会加入到事件队列中等待执行，并且，加入的队列是微队列
3. pro.then可以只添加thenable函数，pro.catch可以单独添加catchable函数
4. 在未决阶段的处理函数中，如果发生未捕获的错误，会将状态推向rejected，并会被catchable捕获
5. 一旦状态推向了已决阶段，无法再对状态做任何更改
6. **Promise并没有消除回调，只是让回调变得可控**
```

+ 例一

```
// 辅助函数,把传进来的对象拼接成url的字符串
        function toData(obj) {
            if (obj === null) {
                return obj;
            }
            let arr = [];
            for (let i in obj) {
                let str = i + "=" + obj[i];
                arr.push(str);
            }
            return arr.join("&");
        }
        // 封装Ajax
        function ajax(obj) {
            return new Promise((resolve, reject) => {
                //指定提交方式的默认值
                obj.type = obj.type || "get";
                //设置是否异步，默认为true(异步)
                obj.async = obj.async || true;
                //设置数据的默认值
                obj.data = obj.data || null;
                // 根据不同的浏览器创建XHR对象
                let xhr = null;
                if (window.XMLHttpRequest) {
                    // 非IE浏览器
                    xhr = new XMLHttpRequest();
                } else {
                    // IE浏览器
                    xhr = new ActiveXObject("Microsoft.XMLHTTP");
                }
                // 区分get和post,发送HTTP请求
                if (obj.type === "post") {
                    xhr.open(obj.type, obj.url, obj.async);
                    xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
                    let data = toData(obj.data);
                    xhr.send(data);
                } else {
                    let url = obj.url + "?" + toData(obj.data);
                    xhr.open(obj.type, url, obj.async);
                    xhr.send();
                }
                // 接收返回过来的数据
                xhr.onreadystatechange = function() {
                    if (xhr.readyState === 4) {
                        if (xhr.status >= 200 && xhr.status < 300 || xhr.status == 304) {
                            resolve(JSON.parse(xhr.responseText))
                        } else {
                            reject(xhr.status)
                        }
                    }
                }
            })
        }

        ajax({
            url: "./data/students.json?name=李华"
        }).then(resp => {
            console.log(resp)
        }, err => {
            console.log(err)
        })
```

+ 例二

```
const pro = new Promise((resolve, reject) => {
    console.log("a")
    resolve(1);
    setTimeout(() => {
        console.log("b")
    }, 0);
})
//pro: resolved
pro.then(data => {
    console.log(data)
})
pro.catch(err => {
    console.log(err)
})
console.log("c")

打印顺序
```

+ 例三

```
const pro = new Promise((resolve, reject) => {
    throw new Error("123"); // pro: rejected
})
pro.then(data => {
    console.log(data)
})
pro.catch(err => {
    console.log(err)
})
报错会自动触发rejected
```

+ 例四

```
const pro = new Promise((resolve, reject) => {
    throw new Error("abc");
    resolve(1); //无效
    reject(2); //无效
    resolve(3); //无效
    reject(4); //无效
})
pro.then(data => {
    console.log(data)
})
pro.catch(err => {
    console.log(err)
})
```

## promise串联

+ 定义

```
当后续的Promise需要用到之前的Promise的处理结果时，需要Promise的串联

Promise对象中，无论是then方法还是catch方法，它们都具有返回值，返回的是一个全新的Promise对象，它的状态满足下面的规则：

1. 如果当前的Promise是未决的，得到的新的Promise是挂起状态
2. 如果当前的Promise是已决的，会运行响应的后续处理函数，并将后续处理函数的结果（返回值）作为resolved状态数据，应用到新的Promise中；如果后续处理函数发生错误，则把返回值作为rejected状态数据，应用到新的Promise中。

**后续的Promise一定会等到前面的Promise有了后续处理结果后，才会变成已决状态**

如果前面的Promise的后续处理，返回的是一个Promise，则返回的新的Promise状态和后续处理返回的Promise状态保持一致。
```

+ 例一

```
function biaobai(god) {
    return new Promise(resolve => {
        console.log(`邓哥向${god}发出了表白短信`);
        setTimeout(() => {
            if (Math.random() < 0.3) {
                //女神同意拉
                resolve(true)
            } else {
                //resolve
                resolve(false);
            }
        }, 500);
    })
}

/*
    邓哥心中有三个女神
    有一天，邓哥决定向第一个女神表白，如果女神拒绝，则向第二个女神表白，直到所有的女神都拒绝，或有一个女神同意为止
    用代码模拟上面的场景
*/
const gods = ["女神1", "女神2", "女神3", "女神4", "女神5"];
let pro;
for (let i = 0; i < gods.length; i++) {
    if (i === 0) {
        pro = biaobai(gods[i]);
    }
    pro = pro.then(resp => {
        if (resp === undefined) {
            return;
        } else if (resp) {
            console.log(`${gods[i]}同意了`)
            return;
        } else {
            console.log(`${gods[i]}拒绝了`)
            if (i < gods.length - 1) {
                return biaobai(gods[i + 1]);
            }
        }
    })
}
```

+ 例二

```
const pro1 = new Promise((resolve, reject) => {
    resolve(1);
})

const pro2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(2);
    }, 3000);
})

pro1.then(result => {
    console.log("结果出来了，得到的是一个Promise")
    return pro2;
}).then(result => {
    console.log(result)
}).then(result => {
    console.log(result)
})
```

+ 例三

```
const pro1 = new Promise((resolve, reject) => {
    throw 1;
})
const pro2 = pro1.then(result => {
    return result * 2
}, err => {
    return err * 3;
});
pro1.catch(err => {
    return err * 2;
})
//pro2类型：Promise对象
//pro2的状态：
pro2.then(result => console.log(result * 2), err => console.log(err * 3))

//输出：6
```

+ 例四

```
 //获取李华所在班级的老师的信息
//1. 获取李华的班级id   Promise
//2. 根据班级id获取李华所在班级的老师id   Promise
//3. 根据老师的id查询老师信息   Promise
const pro = ajax({
    url: "./data/students.json"
})
pro.then(resp => {
    for (let i = 0; i < resp.length; i++) {
        if (resp[i].name === "李华") {
            return resp[i].classId; //班级id
        }
    }
}).then(cid => {
    return ajax({
        url: "./data/classes.json?cid=" + cid
    }).then(cls => {
        for (let i = 0; i < cls.length; i++) {
            if (cls[i].id === cid) {
                return cls[i].teacherId;
            }
        }
    })
}).then(tid => {
    return ajax({
        url: "./data/teachers.json"
    }).then(ts => {
        for (let i = 0; i < ts.length; i++) {
            if (ts[i].id === tid) {
                return ts[i];
            }
        }
    })
}).then(teacher => {
    console.log(teacher);
})
```

## 其他API

```
## 原型成员 (实例成员)

- then：注册一个后续处理函数，当Promise为resolved状态时运行该函数
- catch：注册一个后续处理函数，当Promise为rejected状态时运行该函数
- finally：[ES2018]注册一个后续处理函数（无参），当Promise为已决时运行该函数

## 构造函数成员 （静态成员）

- resolve(数据)：该方法返回一个resolved状态的Promise，传递的数据作为状态数据
  - 特殊情况：如果传递的数据是Promise，则直接返回传递的Promise对象
  
- reject(数据)：该方法返回一个rejected状态的Promise，传递的数据作为状态数据

- all(iterable)：这个方法返回一个新的promise对象，该promise对象在iterable参数对象里所有的promise对象都成功的时候才会触发成功，一旦有任何一个iterable里面的promise对象失败则立即触发该promise对象的失败。这个新的promise对象在触发成功状态以后，会把一个包含iterable里所有promise返回值的数组作为成功回调的返回值，顺序跟iterable的顺序保持一致；如果这个新的promise对象触发了失败状态，它会把iterable里第一个触发失败的promise对象的错误信息作为它的失败错误信息。Promise.all方法常被用于处理多个promise对象的状态集合。

- race(iterable)：当iterable参数里的任意一个子promise被成功或失败后，父promise马上也会用子promise的成功返回值或失败详情作为参数调用父promise绑定的相应句柄，并返回该promise对象
```

+ finally

```
const pro = new Promise((resolve, reject) => {
    reject(1);
})
pro.finally(() => console.log("finally1"))
pro.finally(() => console.log("finally2"))
pro.then(resp => console.log("then1", resp * 1));
pro.then(resp => console.log("then2", resp * 2));
pro.catch(resp => console.log("catch1", resp * 1));
pro.catch(resp => console.log("catch2", resp * 2));
```

+ resolve、reject

```
const pro = new Promise((resolve, reject) => {
    resolve(1);
})
//等效于：
const pro = Promise.resolve(1);

const pro = new Promise((resolve, reject) => {
    reject(1);
})
//等效于：
const pro = Promise.reject(1);

const p = new Promise((resolve, reject) => {
    resolve(3);
})
const pro = Promise.resolve(p);
//等效于
const pro = p;
console.log(pro === p)
```

+ all

```
function getRandom(min, max) {
    return Math.floor(Math.random() * (max - min)) + min;
}
const proms = [];
for (let i = 0; i < 10; i++) {
    proms.push(new Promise((resolve, reject) => {
        setTimeout(() => {
            if (Math.random() < 0.5) {
                console.log(i, "完成");
                resolve(i);
            } else {
                console.log(i, "失败")
                reject(i);
            }
        }, getRandom(1000, 5000));
    }))
}
//等到所有的promise变成resolved状态后输出: 全部完成
const pro = Promise.all(proms)
pro.then(datas => {
    console.log("全部完成", datas);
})
pro.catch(err => {
    console.log("有失败的", err);  //有一个失败的将不会再走then方法
})
console.log(proms);
```

+ race

```
function getRandom(min, max) {
    return Math.floor(Math.random() * (max - min)) + min;
}
const proms = [];
for (let i = 0; i < 10; i++) {
    proms.push(new Promise((resolve, reject) => {
        setTimeout(() => {
            if (Math.random() < 0.5) {
                console.log(i, "完成");
                resolve(i);
            } else {
                console.log(i, "失败")
                reject(i);
            }
        }, getRandom(1000, 5000));
    }))
}
//等到所有的promise变成resolved状态后输出: 全部完成
const pro = Promise.race(proms)
pro.then(data => {
    console.log("有人完成了", data);
})
pro.catch(err => {
    console.log("有人失败了", err);
})
console.log(proms);
```



# async和await

## 定义

```
async 和 await 是 ES2016 新增两个关键字，它们借鉴了 ES2015 中生成器在实际开发中的应用，目的是简化 Promise api 的使用，并非是替代 Promise。

## async

目的是简化在函数的返回值中对Promise的创建

async 用于修饰函数（无论是函数字面量还是函数表达式），放置在函数最开始的位置，被修饰函数的返回结果一定是 Promise 对象。

​```js

async function test(){
    console.log(1);
    return 2;
}

//等效于

function test(){
    return new Promise((resolve, reject)=>{
        console.log(1);
        resolve(2);
    })
}

​```

## await

**await关键字必须出现在async函数中！！！！**

await用在某个表达式之前，如果表达式是一个Promise，则得到的是thenable中的状态数据。

​```js

async function test1(){
    console.log(1);
    return 2;
}

async function test2(){
    const result = await test1();
    console.log(result);
}

test2();
​```

等效于

​```js

function test1(){
    return new Promise((resolve, reject)=>{
        console.log(1);
        resolve(2);
    })
}

function test2(){
    return new Promise((resolve, reject)=>{
        test1().then(data => {
            const result = data;
            console.log(result);
            resolve();
        })
    })
}

test2();

​```

如果await的表达式不是Promise，则会将其使用Promise.resolve包装后按照规则运行
```

+ 例一

```
//获取李华所在班级的老师的信息
//1. 获取李华的班级id   Promise
//2. 根据班级id获取李华所在班级的老师id   Promise
//3. 根据老师的id查询老师信息   Promise
async function getTeacher() {
    const stus = await ajax({
        url: "./data/students.json"
    })
    let cid;
    for (let i = 0; i < stus.length; i++) {
        if (stus[i].name === "李华") {
            cid = stus[i].classId;
        }
    }
    const cls = await ajax({
        url: "./data/classes.json"
    })
    let tid;
    for (let i = 0; i < cls.length; i++) {
        if (cls[i].id === cid) {
            tid = cls[i].teacherId;
        }
    }
    const ts = await ajax({
        url: "./data/teachers.json"
    })
    for (let i = 0; i < ts.length; i++) {
        const element = ts[i];
        if (element.id === tid) {
            console.log(element);
        }
    }
}

getTeacher();
```

+ 例二

```
function biaobai(god) {
    return new Promise(resolve => {
        console.log(`邓哥向${god}发出了表白短信`);
        setTimeout(() => {
            if (Math.random() < 0.3) {
                //女神同意拉
                resolve(true)
            } else {
                //resolve
                resolve(false);
            }
        }, 500);
    })
}

/*
    邓哥心中有三个女神
    有一天，邓哥决定向第一个女神表白，如果女神拒绝，则向第二个女神表白，直到所有的女神都拒绝，或有一个女神同意为止
    用代码模拟上面的场景
*/
(async () => {
    const gods = ["女神1", "女神2", "女神3", "女神4", "女神5"];
    for (let i = 0; i < gods.length; i++) {
        const g = gods[i];
        // 当前循环等待的Promise没有resolve，下一次循环不运行
        const result = await biaobai(g);
        if (result) {
            console.log(`${g}同意了，不用再表白了！！！`);
            break;
        } else {
            console.log(`${g}没有同意`)
        }
    }
})()
```

## 捕捉错误

```
async function getPromise() {
    if (Math.random() < 0.5) {
        return 1;
    } else {
        throw 2;
    }
}

async function test() {
    try {
        const result = await getPromise();
        console.log("正常状态", result)
    } catch (err) {
        console.log("错误状态", err);
    }
}

test();
```



```
//async把函数变为异步函数
//await只有在async中才可以使用，await后的函数为异步函数，获得成功的数据
//用catch捕获错误的回调
async function(){
   const res = await getData().catch(err => {
   		console.log(err)
   })
}
```

## 改造计时器函数

```
function delay(duration) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve();
        }, duration);
    })
}

async function biaobai(god) {
    console.log(`邓哥向${god}发出了表白短信`);
    await delay(500);
    return Math.random() < 0.3;
}
```

# 上传文件

+ 定义

```
流程：

1. 客户端将文件数据发送给服务器
2. 服务器保存上传的文件数据到服务器端
3. 服务器响应给客户端一个文件访问地址

> 测试地址：http://101.132.72.36:5100/api/upload
> 键的名称（表单域名称）：imagefile

请求方法：POST
请求的表单格式：multipart/form-data
请求体中必须包含一个键值对，键的名称是服务器要求的名称，值是文件数据

> HTML5中，JS仍然无法随意的获取文件数据，但是可以获取到input元素中，被用户选中的文件数据
> 可以利用HTML5提供的FormData构造函数来创建请求体
```

+ 例子

```
<img src="" alt="" id="imgAvatar">
<input type="file" id="avatar">
<button>上传</button>
<script>
    async function upload() {
        const inp = document.getElementById("avatar");
        if (inp.files.length === 0) {
            alert("请选择要上传的文件");
            return;
        }
        const formData = new FormData(); //构建请求体
        formData.append("imagefile", inp.files[0]);
        const url = "http://101.132.72.36:5100/api/upload"
        const resp = await fetch(url, {
            method: "POST",
            body: formData //自动修改请求头
        });
        const result = await resp.json();
        return result;
    }

    document.querySelector("button").onclick = async function() {
        const result = await upload();
        const img = document.getElementById("imgAvatar")
        img.src = result.path;
    }
</script>
```



# 字符串和正则表达式

## 更好的unicode支持

+ ```
    # 更好的Unicode支持
    
    早期，由于存储空间宝贵，Unicode使用16位二进制来存储文字。我们将一个16位的二进制编码叫做一个码元（Code Unit）。
    
    后来，由于技术的发展，Unicode对文字编码进行了扩展，将某些文字扩展到了32位（占用两个码元），并且，将某个文字对应的二进制数字叫做码点（Code Point）。
    
    ES6为了解决这个困扰，为字符串提供了方法：codePointAt，根据字符串码元的位置得到其码点。
    
    同时，ES6为正则表达式添加了一个flag: u，如果添加了该配置，则匹配时，使用码点匹配
    ```

+ ```
    const text = "𠮷"; //占用了两个码元（32位）
    
    console.log("字符串长度：", text.length);
    console.log("使用正则测试：", /^.$/u.test(text));
    console.log("得到第一个码元：", text.charCodeAt(0));
    console.log("得到第二个码元：", text.charCodeAt(1));
    
    //𠮷：\ud842\udfb7
    console.log("得到第一个码点：", text.codePointAt(0));
    console.log("得到第二个码点：", text.codePointAt(1));
    
    /**
     * 判断字符串char，是32位，还是16位
     * @param {*} char 
     */
    function is32bit(char, i) {
        //如果码点大于了16位二进制的最大值，则其是32位的
        return char.codePointAt(i) > 0xffff;
    }
    
    /**
     * 得到一个字符串码点的真实长度
     * @param {*} str 
     */
    function getLengthOfCodePoint(str) {
        var len = 0;
        for (let i = 0; i < str.length; i++) {
            //i在索引码元
            if (is32bit(str, i)) {
                //当前字符串，在i这个位置，占用了两个码元
                i++;
            }
            len++;
        }
        return len;
    }
    
    console.log("𠮷是否是32位的：", is32bit("𠮷", 0))
    console.log("ab𠮷ab的码点长度：", getLengthOfCodePoint("ab𠮷ab"))
    ```

## 更多的字符串API

+ ```
    # 更多的字符串API
    
    以下均为字符串的实例（原型）方法
    
    - includes
    
    判断字符串中是否包含指定的子字符串
    
    - startsWith
    
    判断字符串中是否以指定的字符串开始
    
    - endsWith
    
    判断字符串中是否以指定的字符串结尾
    
    - repeat
    
    将字符串重复指定的次数，然后返回一个新字符串。
    ```

+ ```
    const text = "大哥哥是狠人";
    
    console.log("是否包含“狠”：", text.includes("狠",3));
    console.log("是否以“成哥”开头：", text.startsWith("成哥",3));
    console.log("是否以“狠人”结尾：", text.endsWith("狠人",3));
    console.log("重复4次：", text.repeat(4));
    ```

## **正则中的粘连标记**

+ ```
    标记名：y
    
    含义：匹配时，完全按照正则对象中的lastIndex位置开始匹配，并且匹配的位置必须在lastIndex位置。
    ```

+ ```
    const text = "Hello World!!!";
    
    const reg = /W\w+/y;
    reg.lastIndex = 3;
    console.log("reg.lastIndex:", reg.lastIndex)
    console.log(reg.test(text))
    //从第三位开始必须满足条件才匹配成功
    ```

# 模板字符转

+ ```
    
    在模板字符串书写之前，可以加上标记:
    
    ​```js
    标记名`模板字符串`
    ​```
    
    标记是一个函数，函数参数如下：
    
    1. 参数1：被插值分割的字符串数组
    2. 后续参数：所有的插值
    ```

+ 例1

```
var text = String.raw`abc\t\nbcd`;

console.log(text);
```

+ 例2

```
var love1 = "秋葵";
var love2 = "香菜";

var text = myTag`邓哥喜欢${love1}，邓哥也喜欢${love2}。`;

//相当于： 
// text = myTag(["邓哥喜欢", "，邓哥也喜欢", "。"], "秋葵", "香菜")

function myTag(parts) {
    const values = Array.prototype.slice.apply(arguments).slice(1);
    let str = "";
    for (let i = 0; i < values.length; i++) {
        str += `${parts[i]}：${values[i]}`;
        if (i === values.length - 1) {
            str += parts[i + 1];
        }
    }
    return str;
}

console.log(text);
```

+ 例3

```
const container = document.getElementById("container");
const txt = document.getElementById("txt");
const btn = document.getElementById("btn");

btn.onclick = function(){
    container.innerHTML = safe`<p>
        ${txt.value}
    </p>
    <h1>
        ${txt.value}
    </h1>
    `;
}

function safe(parts){
    const values = Array.prototype.slice.apply(arguments).slice(1);
    let str = "";
    for (let i = 0; i < values.length; i++) {
        const v = values[i].replace(/</g, "&lt;").replace(/>/g, "&gt;");
        str += parts[i] + v;
        if (i === values.length - 1) {
            str += parts[i + 1];
        }
    }
    return str;
}
```

# 函数

## **参数默认值**

+ ```
    ## 使用
    
    在书写形参时，直接给形参赋值，附的值即为默认值
    
    这样一来，当调用函数时，如果没有给对应的参数赋值（给它的值是undefined），则会自动使用默认值。
    
    ## [扩展]对arguments的影响
    
    只要给函数加上参数默认值，该函数会自动变量严格模式下的规则：arguments和形参脱离
    
    ## [扩展]留意暂时性死区
    
    形参和ES6中的let或const声明一样，具有作用域，并且根据参数的声明顺序，存在暂时性死区。
    ```

+ ```
    function sum(a, b = 1, c = 2) {
        return a + b + c;
    }
    
    console.log(sum(10, undefined, undefined))
    console.log(sum(11))
    console.log(sum(1, undefined, 5))
    ```

## **剩余参数**

+ ```
    arguments的缺陷：
    
    1. 如果和形参配合使用，容易导致混乱
    2. 从语义上，使用arguments获取参数，由于形参缺失，无法从函数定义上理解函数的真实意图
    
    
    ES6的剩余参数专门用于手机末尾的所有参数，将其放置到一个形参数组中。
    
    语法:
    
    ​```js
    function (...形参名){
    
    }
    ​```
    
    **细节：**
    
    1. 一个函数，仅能出现一个剩余参数
    2. 一个函数，如果有剩余参数，剩余参数必须是最后一个参数
    ```

+ ```
    function test(a, b, ...args) {
        
    }
    
    test(1, 32, 46, 7, 34); 
    ```

## **展开运算符**

+ ```
    使用方式：```  ...要展开的东西  ```
    
    ## 对数组展开 ES6
    
    ## 对对象展开 ES7
    ```

+ ```
    const obj1 = {
        name: "成哥",
        age: 18,
        loves: ["邓嫂", "成嫂1", "成嫂2"],
        address: {
            country: "中国",
            province: "黑龙江",
            city: "哈尔滨"
        }
    }
    
    // 浅克隆到obj2
    const obj2 = {
        ...obj1,
        name: "邓哥",
        address: {
            ...obj1.address
        },
        loves: [...obj1.loves, "成嫂3"]
    };
    
    console.log(obj2)
    
    console.log(obj1.loves === obj2.loves)
    ```

## **明确函数的双重用途**

+ ```
    ES6提供了一个特殊的API，可以使用该API在函数内部，判断该函数是否使用了new来调用
    
    ​```js
    new.target 
    //该表达式，得到的是：如果没有使用new来调用函数，则返回undefined
    //如果使用new调用函数，则得到的是new关键字后面的函数本身
    ​```
    ```

+ ```
    function Person(firstName, lastName) {
        //判断是否是使用new的方式来调用的函数
    
        // //过去的判断方式
        // if (!(this instanceof Person)) {
        //     throw new Error("该函数没有使用new来调用")
        // }
    
        if (new.target === undefined) {
            throw new Error("该函数没有使用new来调用")
        }
        this.firstName = firstName;
        this.lastName = lastName;
        this.fullName = `${firstName} ${lastName}`;
    }
    
    const p1 = new Person("袁", "进");
    console.log(p1)
    
    
    
    const p2 = Person("袁", "进");
    console.log(p2);
    
    const p3 = Person.call(p1, "袁", "进")
    console.log(p3);
    ```

## **箭头函数**

+ ```
    回顾：this指向
    
    1. 通过对象调用函数，this指向对象
    2. 直接调用函数，this指向全局对象
    3. 如果通过new调用函数，this指向新创建的对象
    4. 如果通过apply、call、bind调用函数，this指向指定的数据
    5. 如果是DOM事件函数，this指向事件源
    
    ## 使用语法
    
    箭头函数是一个函数表达式，理论上，任何使用函数表达式的场景都可以使用箭头函数
    
    完整语法：
    
    ​```js
    (参数1, 参数2, ...)=>{
        //函数体
    }
    ​```
    
    如果参数只有一个，可以省略小括号
    
    ​```js
    参数 => {
    
    }
    ​```
    
    如果箭头函数只有一条返回语句，可以省略大括号，和return关键字
    
    ​```js
    参数 => 返回值
    ​```
    
    ## 注意细节
    
    - 箭头函数中，不存在this、arguments、new.target，如果使用了，则使用的是函数外层的对应的this、arguments、new.target
    - 箭头函数没有原型
    - 箭头函数不能作用构造函数使用
    
    ## 应用场景
    
    1. 临时性使用的函数，并不会可以调用它，比如：
       1. 事件处理函数
       2. 异步处理函数
       3. 其他临时性的函数
    2. 为了绑定外层this的函数
    3. 在不影响其他代码的情况下，保持代码的简洁，最常见的，数组方法中的回调函数
    ```

+ ```
    const numbers = [3, 7, 78, 3, 5, 345];
    
    const result = numbers.filter(num => num % 2 !== 0)
        .map(num => num * 2).reduce((a, b) => a + b)
    
    console.log(result);
    ```


# 对象

## **新增的对象字面量语法**

```
1. 成员速写

如果对象字面量初始化时，成员的名称来自于一个变量，并且和变量的名称相同，则可以进行简写

2. 方法速写

对象字面初始化时，方法可以省略冒号和function关键字

3. 计算属性名

有的时候，初始化对象时，某些属性名可能来自于某个表达式的值，在ES6，可以使用中括号来表示该属性名是通过计算得到的。
```

```
const prop1 = "name2";
const prop2 = "age2";
const prop3 = "sayHello2";

const user = {
    [prop1]: "111",
    [prop2]: 100,
    [prop3](){
        console.log(this[prop1], this[prop2])
    }
}

user[prop3]();

console.log(user)
```

## **Object的新增API**

```
1. Object.is

用于判断两个数据是否相等，基本上跟严格相等（===）是一致的，除了以下两点：

1) NaN和NaN相等
2) +0和-0不相等

2. Object.assign

用于混合对象

3. Object.getOwnPropertyNames 的枚举顺序

Object.getOwnPropertyNames方法之前就存在，只不过，官方没有明确要求，对属性的顺序如何排序，如何排序，完全由浏览器厂商决定。

ES6规定了该方法返回的数组的排序方式如下：

- 先排数字，并按照升序排序
- 再排其他，按照书写顺序排序

4. Object.setPrototypeOf

该函数用于设置某个对象的隐式原型

比如： Object.setPrototypeOf(obj1, obj2)，
相当于：  ``` obj1.__proto__ = obj2 ```
```

## **面向对象简介**

```
面向对象：一种编程思想，跟具体的语言


对比面向过程：

- 面向过程：思考的切入点是功能的步骤
- 面向对象：思考的切入点是对象的划分
```

```
/**
 * 大象
 */
function Elephant() {

}

/**
 * 冰箱
 */
function Frige() {

}

Frige.prototype.openDoor = function () {

}

Frige.prototype.closeDoor = function () {

}

Frige.prototype.join = function(something){
    this.openDoor();
    //装东西

    this.closeDoor();
}

//1. 冰箱门打开
// var frig = new Frige();
// frig.openDoor();

// //2. 大象装进去
// var ele = new Elephant();
// frig.join(ele);

// //3. 冰箱门关上
// frig.closeDoor();

var frig = new Frige();

frig.join(new Elephant());
```

##  **类：构造函数的语法糖**

```
## 传统的构造函数的问题

1. 属性和原型方法定义分离，降低了可读性
2. 原型成员可以被枚举
3. 默认情况下，构造函数仍然可以被当作普通函数使用

## 类的特点

1. 类声明不会被提升，与 let 和 const 一样，存在暂时性死区
2. 类中的所有代码均在严格模式下执行
3. 类的所有方法都是不可枚举的
4. 类的所有方法都无法被当作构造函数使用
5. 类的构造器必须使用 new 来调用
```

```
class Animal {
    constructor(type, name, age, sex) {
        this.type = type;
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    print() {
        console.log(`【种类】：${this.type}`);
        console.log(`【名字】：${this.name}`);
        console.log(`【年龄】：${this.age}`);
        console.log(`【性别】：${this.sex}`);
    }
}

const a = new Animal("狗", "旺财", 3, "男");
a.print();

for (const prop in a) {
    console.log(prop)
}
```

## **类的其他书写方式**

```
1. 可计算的成员名

2. getter和setter

Object.defineProperty 可定义某个对象成员属性的读取和设置

使用getter和setter控制的属性，不在原型上

3. 静态成员

构造函数本身的成员

使用static关键字定义的成员即静态成员

4. 字段初始化器（ES7）

注意：

1). 使用static的字段初始化器，添加的是静态成员
2). 没有使用static的字段初始化器，添加的成员位于对象上
3). 箭头函数在字段初始化器位置上，指向当前对象

5. 类表达式

6. [扩展]装饰器（ES7）(Decorator)

横切关注点

装饰器的本质是一个函数
```

```
const printName = "print";

class Animal {
    constructor(type, name, age, sex) {
        this.type = type;
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    //创建一个age属性，并给它加上getter，读取该属性时，会运行该函数
    get age() {
        return this._age + "岁";
    }

    //创建一个age属性，并给它加上setter，给该属性赋值时，会运行该函数
    set age(age) {
        if (typeof age !== "number") {
            throw new TypeError("age property must be a number");
        }
        if (age < 0) {
            age = 0;
        }
        else if (age > 1000) {
            age = 1000;
        }
        this._age = age;
    }

    [printName]() {
        console.log(`【种类】：${this.type}`);
        console.log(`【名字】：${this.name}`);
        console.log(`【年龄】：${this.age}`);
        console.log(`【性别】：${this.sex}`);
    }
}

var a = new Animal("狗", "旺财", 3, "男");
```

```
class Chess {
    constructor(name) {
        this.name = name;
    }

    static width = 50;

    static height = 50;

    static method() {

    }
}

console.log(Chess.width)
console.log(Chess.height)

Chess.method();
```

## **类的继承**

```
如果两个类A和B，如果可以描述为：B 是 A，则，A和B形成继承关系

如果B是A，则：

1. B继承自A
2. A派生B
3. B是A的子类
4. A是B的父类

如果A是B的父类，则B会自动拥有A中的所有实例成员。


新的关键字：

- extends：继承，用于类的定义
- super
  - 直接当作函数调用，表示父类构造函数
  - 如果当作对象使用，则表示父类的原型


注意：ES6要求，如果定义了constructor，并且该类是子类，则必须在constructor的第一行手动调用父类的构造函数

如果子类不写constructor，则会有默认的构造器，该构造器需要的参数和父类一致，并且自动调用父类构造器

【冷知识】

- 用JS制作抽象类
  - 抽象类：一般是父类，不能通过该类创建对象
- 正常情况下，this的指向，this始终指向具体的类的对象
```

```
class Animal {
    constructor(type, name, age, sex) {
        if (new.target === Animal) {
            throw new TypeError("你不能直接创建Animal的对象，应该通过子类创建")
        }
        this.type = type;
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    print() {
        console.log(`【种类】：${this.type}`);
        console.log(`【名字】：${this.name}`);
        console.log(`【年龄】：${this.age}`);
        console.log(`【性别】：${this.sex}`);
    }

    jiao() {
        throw new Error("动物怎么叫的？");
    }
}

class Dog extends Animal {
    constructor(name, age, sex) {
        super("犬类", name, age, sex);
        // 子类特有的属性
        this.loves = "吃骨头";
    }

    print() {
        //调用父类的print
        super.print();
        //自己特有的代码
        console.log(`【爱好】：${this.loves}`);
    }


    //同名方法，会覆盖父类
    jiao() {
        console.log("旺旺！");
    }
}

//下面的代码逻辑有误
const a = new Dog("旺财", 3, "公")
a.print();
```

```
function Animal(type, name, age, sex) {
    this.type = type;
    this.name = name;
    this.age = age;
    this.sex = sex;
}
Animal.prototype.print = function () {
    console.log(`【种类】：${this.type}`);
    console.log(`【名字】：${this.name}`);
    console.log(`【年龄】：${this.age}`);
    console.log(`【性别】：${this.sex}`);
}

function Dog(name, age, sex) {
    //借用父类的构造函数
    Animal.call(this, "犬类", name, age, sex);
}

Object.setPrototypeOf(Dog.prototype, Animal.prototype);

const d = new Dog("旺财", 3, "公");
d.print();
console.log(d);
```

```
class Animal {
    constructor(type, name, age, sex) {
        this.type = type;
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    print() {
        console.log(`【种类】：${this.type}`);
        console.log(`【名字】：${this.name}`);
        console.log(`【年龄】：${this.age}`);
        console.log(`【性别】：${this.sex}`);
    }

    jiao(){
        throw new Error("动物怎么叫的？");
    }
}

class Dog extends Animal {
    constructor(name, age, sex) {
        super("犬类", name, age, sex);
        // 子类特有的属性
        this.loves = "吃骨头";
    }

    print(){
        //调用父类的print
        super.print();
        //自己特有的代码
        console.log(`【爱好】：${this.loves}`);
    }


    //同名方法，会覆盖父类
    jiao(){
        console.log("旺旺！");
    }
}

const d = new Dog("旺财", 3, "公");
d.print();
console.log(d)
d.jiao();
```

# 解构

## 对象解构

```
**## 什么是解构**

使用ES6的一种语法规则，将一个对象或数组的某个属性提取到某个变量中

***\*解构不会对被解构的目标造成任何影响\****

**## 在解构中使用默认值**


\```js
{同名变量 = 默认值}
\```

**## 非同名属性解构**


\```js

{属性名:变量名}

\```
```

```
const user = {
    name: "kevin",
    age: 11,
    sex: "男",
    address: {
        province: "四川",
        city: "成都"
    }
}
// 先定义4个变量：name、age、gender、address
// 再从对象user中读取同名属性赋值（其中gender读取的是sex属性）
//sex不存在时，默认赋值123
let { name, age, sex: gender = 123, address } = user

console.log(name, age, gender, address)


const user = {
    name: "kevin",
    age: 11,
    sex: "男",
    address: {
        province: "四川",
        city: "成都"
    }
}
//解构出user中的name、province
//定义两个变量name、province
//再解构
const { name, address: { province } } = user;

console.log(name, address, province) //address报错

```

## 数组结构

```
const numbers = ["a", "b", "c", "d"];
const [n1, , , n4, n5 = 123] = numbers;
console.log(n1, n4, n5)

const user = {
    name: "kevin",
    age: 11,
    sex: "男",
    address: {
        province: "四川",
        city: "成都"
    }
}

//解构出name，然后，剩余的所有属性，放到一个新的对象中，变量名为obj
// name: kevin
// obj : {age:11, sex:"男", address:{...}}

const { name, ...obj } = user;
console.log(name, obj)


const article = {
    title: "文章标题",
    content: "文章内容",
    comments: [{
        content: "评论1",
        user: {
            id: 1,
            name: "用户名1"
        }
    }, {
        content: "评论2",
        user: {
            id: 2,
            name: "用户名2"
        }
    }]
}

//解构出第二条评论的用户名和评论内容
// name:"用户名2"  content:"评论2"

// const {
//     comments: [, {
//         content,
//         user: {
//             name
//         }
//     }]
// } = article;

// const [, {
//     content,
//     user: {
//         name
//     }
// }] = article.comments;

const {
    content,
    user: {
        name
    }
} = article.comments[1]

console.log(content, name)
```

## 参数解构

+ ```
    // function print(user) {
    //     console.log(`姓名：${user.name}`)
    //     console.log(`年龄：${user.age}`)
    //     console.log(`性别：${user.sex}`)
    //     console.log(`身份：${user.address.province}`)
    //     console.log(`城市：${user.address.city}`)
    // }
    
    function print({ name, age, sex, address: {
        province,
        city
    } }) {
        console.log(`姓名：${name}`)
        console.log(`年龄：${age}`)
        console.log(`性别：${sex}`)
        console.log(`身份：${province}`)
        console.log(`城市：${city}`)
    }
    
    const user = {
        name: "kevin",
        age: 11,
        sex: "男",
        address: {
            province: "四川",
            city: "成都"
        }
    }
    print(user)
    ```

+ ```
    // function ajax(options) {
    //     const defaultOptions = {
    //         method: "get",
    //         url: "/"
    //     }
    //     const opt = {
    //         ...defaultOptions,
    //         ...options
    //     }
    //     console.log(opt)
    // }
    
    function ajax({
        method = "get",
        url = "/"
    } = {}) {
        console.log(method, url)
    }
    
    ajax()
    ```


# 符号

## 普通符号

+ 定义

```
符号是ES6新增的一个数据类型，它通过使用函数 ```Symbol(符号描述)``` 来创建

符号设计的初衷，是为了给对象设置私有属性

私有属性：只能在对象内部使用，外面无法使用

符号具有以下特点：

- 没有字面量
- 使用 typeof 得到的类型是 symbol
- **每次调用 Symbol 函数得到的符号永远不相等，无论符号名是否相同**
- 符号可以作为对象的属性名存在，这种属性称之为符号属性
  - 开发者可以通过精心的设计，让这些属性无法通过常规方式被外界访问
  - 符号属性是不能枚举的，因此在 for-in 循环中无法读取到符号属性，Object.keys 方法也无法读取到符号属性
  - Object.getOwnPropertyNames 尽管可以得到所有无法枚举的属性，但是仍然无法读取到符号属性
  - ES6 新增 Object.getOwnPropertySymbols 方法，可以读取符号
- 符号无法被隐式转换，因此不能被用于数学运算、字符串拼接或其他隐式转换的场景，但符号可以显式的转换为字符串，通过 String 构造函数进行转换即可，console.log 之所以可以输出符号，是它在内部进行了显式转换
```

+ 例子

```
// const hero = (function () {
//     const getRandom = Symbol();

//     return {
//         attack: 30,
//         hp: 300,
//         defence: 10,
//         gongji() { //攻击
//             //伤害：攻击力*随机数（0.8~1.1)
//             const dmg = this.attack * this[getRandom](0.8, 1.1);
//             console.log(dmg);
//         },
//         [getRandom](min, max) { //根据最小值和最大值产生一个随机数
//             return Math.random() * (max - min) + min;
//         }
//     }
// })()

// console.log(hero);

const Hero = (() => {
    const getRandom = Symbol();

    return class {
        constructor(attack, hp, defence) {
            this.attack = attack;
            this.hp = hp;
            this.defence = defence;
        }

        gongji() {
            //伤害：攻击力*随机数（0.8~1.1)
            const dmg = this.attack * this[getRandom](0.8, 1.1);
            console.log(dmg);
        }

        [getRandom](min, max) { //根据最小值和最大值产生一个随机数
            return Math.random() * (max - min) + min;
        }
    }
})();

const h = new Hero(3, 6, 3);
console.log(h);
```

+ 非常规手段获取符号属性

```
const Hero = (() => {
    const getRandom = Symbol();

    return class {
        constructor(attack, hp, defence) {
            this.attack = attack;
            this.hp = hp;
            this.defence = defence;
        }

        gongji() {
            //伤害：攻击力*随机数（0.8~1.1)
            const dmg = this.attack * this[getRandom](0.8, 1.1);
            console.log(dmg);
        }

        [getRandom](min, max) { //根据最小值和最大值产生一个随机数
            return Math.random() * (max - min) + min;
        }
    }
})();

const h = new Hero(3, 6, 3);
const sybs = Object.getOwnPropertySymbols(Hero.prototype);
const prop = sybs[0];
console.log(h[prop](3, 5))
```

## 共享符号

+ 定义

```
根据某个符号名称（符号描述）能够得到同一个符号

​```js
Symbol.for("符号名/符号描述")  //获取共享符号
​```
```

+ 例子

```
const syb1 = Symbol.for("abc");
const syb2 = Symbol.for("abc");
console.log(syb1 === syb2)
const obj1 = {
    a: 1,
    b: 2,
    [syb1]: 3
}

const obj2 = {
    a: "a",
    b: "b",
    [syb2]: "c"
}

console.log(obj1, obj2);
```

+ 手写共享符号

```
const SymbolFor = (() => {
    const global = {};//用于记录有哪些共享符号
    return function (name) {
        console.log(global)
        if (!global[name]) {
            global[name] = Symbol(name);
        }
        console.log(global);
        return global[name];
    }
})();

const syb1 = SymbolFor("abc");

const syb2 = SymbolFor("abc");

console.log(syb1 === syb2);
```

## 知名符号

+ 定义

```
知名符号是一些具有特殊含义的共享符号，通过 Symbol 的静态属性得到

ES6 延续了 ES5 的思想：减少魔法，暴露内部实现！

因此，ES6 用知名符号暴露了某些场景的内部实现

1. Symbol.hasInstance

该符号用于定义构造函数的静态成员，它将影响 instanceof 的判定

​```js

obj instanceof A

//等效于

A[Symbol.hasInstance](obj) // Function.prototype[Symbol.hasInstance]

​```

2. [扩展] Symbol.isConcatSpreadable

该知名符号会影响数组的 concat 方法

3. [扩展] Symbol.toPrimitive

该知名符号会影响类型转换的结果

4. [扩展] Symbol.toStringTag

该知名符号会影响 Object.prototype.toString 的返回值

5. 其他知名符号
```

+ Symbol.hasInstance

```
function A() {

}

Object.defineProperty(A, Symbol.hasInstance, {
    value: function (obj) {
        return false;
    }
})

const obj = new A();

console.log(obj instanceof A);
// console.log(A[Symbol.hasInstance](obj));
```

+ Symbol.isConcatSpreadable

```
const arr = [3];
const arr2 = [5, 6, 7, 8];

arr2[Symbol.isConcatSpreadable] = false;

const result = arr.concat(56, arr2)

//  [3, 56, [5,6,7,8]]
//  [3, 56, 5, 6, 7, 8]

console.log(result)


const arr = [1];
const obj = {
    0: 3,
    1: 4,
    length: 2,
    [Symbol.isConcatSpreadable]: true
}

const result = arr.concat(2, obj)

console.log(result)
```

+ Symbol.toPrimitive

```
class Temperature {
    constructor(degree) {
        this.degree = degree;
    }

    [Symbol.toPrimitive](type) {
        if (type === "default") {
            return this.degree + "摄氏度";
        }
        else if (type === "number") {
            return this.degree;
        }
        else if (type === "string") {
            return this.degree + "℃";
        }
    }
}

const t = new Temperature(30);

console.log(t + "!");
console.log(t / 2);
console.log(String(t));
```

+ Symbol.toStringTag

```
class Person {

    [Symbol.toStringTag] = "Person"
}

const p = new Person();

const arr = [32424, 45654, 32]

console.log(Object.prototype.toString.apply(p));
console.log(Object.prototype.toString.apply(arr))
```

# 迭代器

+ 定义

```
1. 什么是迭代？

从一个数据集合中按照一定的顺序，不断取出数据的过程

2. 迭代和遍历的区别？

迭代强调的是依次取数据，并不保证取多少，也不保证把所有的数据取完

遍历强调的是要把整个数据依次全部取出

3. 迭代器

对迭代过程的封装，在不同的语言中有不同的表现形式，通常为对象

4. 迭代模式

一种设计模式，用于统一迭代过程，并规范了迭代器规格：

- 迭代器应该具有得到下一个数据的能力
- 迭代器应该具有判断是否还有后续数据的能力

## JS中的迭代器

JS规定，如果一个对象具有next方法，并且该方法返回一个对象，该对象的格式如下：

​```js
{value: 值, done: 是否迭代完成}
​```

则认为该对象是一个迭代器

含义：

- next方法：用于得到下一个数据
- 返回的对象
  - value：下一个数据的值
  - done：boolean，是否迭代完成
```

+ 例

```
const arr = [1, 2, 3, 4, 5];
//迭代数组arr
const iterator = {
    i: 0, //当前的数组下标
    next() {
        var result = {
            value: arr[this.i],
            done: this.i >= arr.length
        }
        this.i++;
        return result;
    }
}

//让迭代器不断的取出下一个数据，直到没有数据为止
let data = iterator.next();
while (!data.done) { //只要没有迭代完成，则取出数据
    console.log(data.value)
    //进行下一次迭代
    data = iterator.next();
}

console.log("迭代完成")
```

```
const arr1 = [1, 2, 3, 4, 5];
const arr2 = [6, 7, 8, 9];

// 迭代器创建函数  iterator creator
function createIterator(arr) {
    let i = 0;//当前的数组下标
    return { 
        next() {
            var result = {
                value: arr[i],
                done: i >= arr.length
            }
            i++;
            return result;
        }
    }
}

const iter1 = createIterator(arr1);
const iter2 = createIterator(arr2);
```

```
// 依次得到斐波拉契数列前面n位的值
// 1 1 2 3 5 8 13 .....

//创建一个斐波拉契数列的迭代器
function createFeiboIterator() {
    let prev1 = 1,
        prev2 = 1, //当前位置的前1位和前2位
        n = 1; //当前是第几位

    return {
        next() {
            let value;
            if (n <= 2) {
                value = 1;
            } else {
                value = prev1 + prev2;
            }
            const result = {
                value,
                done: false
            };
            prev2 = prev1;
            prev1 = result.value;
            n++;
            return result;
        }
    }
}

const iterator = createFeiboIterator();
```

## 迭代器与for--of---

+ 定义

```
 迭代器(iterator)：一个具有next方法的对象，next方法返回下一个数据并且能指示是否迭代完成
- 迭代器创建函数（iterator creator）：一个返回迭代器的函数

**可迭代协议**

ES6规定，如果一个对象具有知名符号属性```Symbol.iterator```，并且属性值是一个迭代器创建函数，则该对象是可迭代的（iterable）

> 思考：如何知晓一个对象是否是可迭代的？
> 思考：如何遍历一个可迭代对象？

## for-of 循环

for-of 循环用于遍历可迭代对象，格式如下

​```js
//迭代完成后循环结束
for(const item in iterable){
    //iterable：可迭代对象
    //item：每次迭代得到的数据
}
```

+ 例

```
const arr = [5, 7, 2, 3, 6];


// const iterator = arr[Symbol.iterator]();
// let result = iterator.next();
// while (!result.done) {
//     const item = result.value; //取出数据
//     console.log(item);
//     //下一次迭代
//     result = iterator.next();
// }

for (const item of arr) {  //等同于上面注释的代码，语法糖
    console.log(item)
}
```

+ 用途

```
var obj = {
    a: 1,
    b: 2,
    [Symbol.iterator]() {
        const keys = Object.keys(this);
        let i = 0;
        return {
            next: () => {
                const propName = keys[i];
                const propValue = this[propName];
                const result = {
                    value: {
                        propName,
                        propValue
                    },
                    done: i >= keys.length
                }
                i++;
                return result;
            }
        }
    }
}

const arr = [...obj];
console.log(arr);

function test(a, b) {
    console.log(a, b)
}

test(...obj);
```

# 生成器

+ 定义

```
1. 什么是生成器？

生成器是一个通过构造函数Generator创建的对象，生成器既是一个迭代器，同时又是一个可迭代对象

2. 如何创建生成器？

生成器的创建，必须使用生成器函数（Generator Function）

3. 如何书写一个生成器函数呢？

​```js
//这是一个生成器函数，该函数一定返回一个生成器
function* method(){

}
​```

4. 生成器函数内部是如何执行的？

生成器函数内部是为了给生成器的每次迭代提供的数据

每次调用生成器的next方法，将导致生成器函数运行到下一个yield关键字位置

yield是一个关键字，该关键字只能在生成器函数内部使用，表达“产生”一个迭代数据。

5. 有哪些需要注意的细节？

1). 生成器函数可以有返回值，返回值出现在第一次done为true时的value属性中
2). 调用生成器的next方法时，可以传递参数，传递的参数会交给yield表达式的返回值
3). 第一次调用next方法时，传参没有任何意义
4). 在生成器函数内部，可以调用其他生成器函数，但是要注意加上*号


6. 生成器的其他API

- return方法：调用该方法，可以提前结束生成器函数，从而提前让整个迭代过程结束
- throw方法：调用该方法，可以在生成器中产生一个错误
```

+ 生成器函数可以有返回值，返回值出现在第一次done为true时的value属性中

```
function* test() {
    console.log("第1次运行")
    yield 1;
    console.log("第2次运行")
    yield 2;
    console.log("第3次运行")
}

const generator = test();

//generator.next() 运行
```

```
const arr1 = [1, 2, 3, 4, 5];
const arr2 = [6, 7, 8, 9];

// 迭代器创建函数  iterator creator
function* createIterator(arr) {
    for (const item of arr) {
        yield item;
    }
}

const iter1 = createIterator(arr1);
const iter2 = createIterator(arr2);
```

+ 运行到return时结束

```
function* test() {
    console.log("第1次运行")
    yield 1;
    console.log("第2次运行")
    yield 2;
    console.log("第3次运行");
    return 10;
}

const generator = test();

//generator.next()
//generator.next()
//generator.next()
//generator.next()
```

+ 调用生成器的next方法时，可以传递参数，传递的参数会交给yield表达式的返回值

```
function* test() {
    console.log("函数开始")

    let info = yield 1;
    console.log(info)
    info = yield 2 + info;
    console.log(info)
}

const generator = test();

generator.next(); //运行到yield 1 结束，不执行等号左边的值
generator.next(2); //把2赋值给yield 1左边的info ,输出yield 2 + info 为 3
generator.next(3); //同上，输出 3
```

+ 在生成器函数内部，可以调用其他生成器函数，但是要注意加上*号

```
function* t1(){
    yield "a"
    yield "b"
}

function* test() {
    yield* t1();
    yield 1;
    yield 2;
    yield 3;
}

const generator = test();
```

+ return方法：调用该方法，可以提前结束生成器函数，从而提前让整个迭代过程结束

```
function* test() {
    console.log("第1次运行")
    yield 1;
    console.log("第2次运行")
    yield 2;
    console.log("第3次运行");
    return 10;
}

const generator = test();

generator.return(2)  //直接结束
```

+ throw方法：调用该方法，可以在生成器中产生一个错误

```
function* test() {
    yield* t1();
    yield 1;
    yield 2;
    yield 3;
}

const generator = test();
generator.throw(new Error('2321'))
```

## 手写async await相似的功能，处理异步数据

```
function* task() {
    const d = yield 1;
    console.log(d)
    // //d : 1
    const resp = yield fetch("http://www.baidu.com")
    const result = yield resp.json();
    console.log(result);
}

run(task)

function run(generatorFunc) {
    const generator = generatorFunc();
    let result = generator.next(); //启动任务（开始迭代）, 得到迭代数据
    handleResult();
    //对result进行处理
    function handleResult() {
        if (result.done) {
            return; //迭代完成，不处理
        }
        //迭代没有完成，分为两种情况
        //1. 迭代的数据是一个Promise
        //2. 迭代的数据是其他数据
        if (typeof result.value.then === "function") {
            //1. 迭代的数据是一个Promise
            //等待Promise完成后，再进行下一次迭代
            result.value.then(data => {
                result = generator.next(data)
                handleResult();
            },err => {
                result = generator.throw(err)
                handleResult();
            })
        } else {
            //2. 迭代的数据是其他数据，直接进行下一次迭代
            result = generator.next(result.value)
            handleResult();
        }
    }
}
```

# 集合

## set集合

```
# set 集合

> 一直以来，JS只能使用数组和对象来保存多个数据，缺乏像其他语言那样拥有丰富的集合类型。因此，ES6新增了两种集合类型（set 和 map），用于在不同的场景中发挥作用。

**set用于存放不重复的数据**

1. 如何创建set集合

​```js
new Set(); //创建一个没有任何内容的set集合

new Set(iterable); //创建一个具有初始内容的set集合，内容来自于可迭代对象每一次迭代的结果


		const s1 = new Set();
        console.log(s1);

        const s2 = new Set("asdfasfasf");
        console.log(s2);
​```

2. 如何对set集合进行后续操作

- add(数据): 添加一个数据到set集合末尾，如果数据已存在，则不进行任何操作
  - set使用Object.is的方式判断两个数据是否相同，但是，针对+0和-0，set认为是相等
  		const s1 = new Set();
        s1.add(1);
        s1.add(2);
        s1.add(3);
        s1.add(1); //无效
        s1.add(+0);
        s1.add(-0); //无效
- has(数据): 判断set中是否存在对应的数据
- delete(数据)：删除匹配的数据，返回是否删除成功
- clear()：清空整个set集合
- size: 获取set集合中的元素数量，只读属性，无法重新赋值

3. 如何与数组进行相互转换

​```js
const s = new Set([x,x,x,x,x]);
// set本身也是一个可迭代对象，每次迭代的结果就是每一项的值
const arr = [...s];

    //数组去重
    const arr = [45, 7, 2, 2, 34, 46, 6, 57, 8, 55, 6, 46];
    const result = [...new Set(arr)];
    console.log(result);

	//字符串去重
    const str = "asf23sdfgsdgfsafasdfasfasfasfsafsagfdsfg";
    const s = [...new Set(str)].join("");
    console.log(s);
​```

4. 如何遍历

1). 使用for-of循环
		for (const item of s1) {
            console.log(item)
        }
2). 使用set中的实例方法forEach
		s1.forEach((item, index, s) => {
            console.log(item, index, s);
        })
        console.log(s1);
        console.log("总数为：", s1.size);

注意：set集合中不存在下标，因此forEach中的回调的第二个参数和第一个参数是一致的，均表示set中的每一项
```

+ 应用

```
// 两个数组的并集、交集、差集 （不能出现重复项），得到的结果是一个新数组
const arr1 = [33, 22, 55, 33, 11, 33, 5];
const arr2 = [22, 55, 77, 88, 88, 99, 99];

//并集
// const result = [...new Set(arr1.concat(arr2))];
console.log("并集", [...new Set([...arr1, ...arr2])]);

const cross = [...new Set(arr1)].filter(item => arr2.indexOf(item) >= 0);
//交集
console.log("交集", cross)

//差集
// console.log("差集", [...new Set([...arr1, ...arr2])].filter(item => arr1.indexOf(item) >= 0 && arr2.indexOf(item) < 0 || arr2.indexOf(item) >= 0 && arr1.indexOf(item) < 0))
console.log("差集", [...new Set([...arr1, ...arr2])].filter(item => cross.indexOf(item) < 0))
```

+ 手写set

```
class MySet {
    constructor(iterator = []) {
        //验证是否是可迭代的对象
        if (typeof iterator[Symbol.iterator] !== "function") {
            throw new TypeError(`你提供的${iterator}不是一个可迭代的对象`)
        }
        this._datas = [];
        for (const item of iterator) {
            this.add(item);
        }
    }

    get size() {
        return this._datas.length;
    }

    add(data) {
        if (!this.has(data)) {
            this._datas.push(data);
        }
    }

    has(data) {
        for (const item of this._datas) {
            if (this.isEqual(data, item)) {
                return true;
            }
        }
        return false;
    }

    delete(data) {
        for (let i = 0; i < this._datas.length; i++) {
            const element = this._datas[i];
            if (this.isEqual(element, data)) {
                //删除
                this._datas.splice(i, 1);
                return true;
            }
        }
        return false;
    }

    clear() {
        this._datas.length = 0;
    }

    *[Symbol.iterator]() {
        for (const item of this._datas) {
            yield item;
        }
    }

    forEach(callback) {
        for (const item of this._datas) {
            callback(item, item, this);
        }
    }

    /**
     * 判断两个数据是否相等
     * @param {*} data1 
     * @param {*} data2 
     */
    isEqual(data1, data2) {
        if (data1 === 0 && data2 === 0) {
            return true;
        }
        return Object.is(data1, data2);
    }
}
```

## map集合

```
# map集合

键值对（key value pair）数据集合的特点：键不可重复

map集合专门用于存储多个键值对数据。

在map出现之前，我们使用的是对象的方式来存储键值对，键是属性名，值是属性值。

使用对象存储有以下问题：

1. 键名只能是字符串
2. 获取数据的数量不方便
3. 键名容易跟原型上的名称冲突


1. 如何创建map

​```js
new Map(); //创建一个空的map
new Map(iterable); //创建一个具有初始内容的map，初始内容来自于可迭代对象每一次迭代的结果，但是，它要求每一次迭代的结果必须是一个长度为2的数组，数组第一项表示键，数组的第二项表示值
​```

2. 如何进行后续操作

- size：只读属性，获取当前map中键的数量
- set(键, 值)：设置一个键值对，键和值可以是任何类型
  - 如果键不存在，则添加一项
  - 如果键已存在，则修改它的值
  - 比较键的方式和set相同
- get(键): 根据一个键得到对应的值
- has(键)：判断某个键是否存在
- delete(键)：删除指定的键
- clear(): 清空map

		const mp1 = new Map([["a", 3], ["b", 4], ["c", 5]]);
        const obj = {};
        mp1.set(obj, 6456);
        mp1.set("a", "abc");
        mp1.set(obj, 111);
        
        console.log(mp1)
        console.log("总数：", mp1.size);
        console.log("get('a')", mp1.get("a"));
        console.log("has('a')", mp1.has("a"));


3. 和数组互相转换

和set一样

4. 遍历

- for-of，每次迭代得到的是一个长度为2的数组
- forEach，通过回调函数遍历
  - 参数1：每一项的值
  - 参数2：每一项的键
  - 参数3：map本身
  
  		const mp = new Map([
            ["a", 3],
            ["c", 10],
            ["b", 4],
            ["c", 5]
        ]);
        const result = [...mp]
        console.log(result);

        // for (const [key, value] of mp) {
        //     console.log(key, value)
        // }

        mp.forEach((value, key, mp) => {
            console.log(value, key, mp)
        })
```

+ 手写map

```
class MyMap {
    constructor(iterable = []) {
        //验证是否是可迭代的对象
        if (typeof iterable[Symbol.iterator] !== "function") {
            throw new TypeError(`你提供的${iterable}不是一个可迭代的对象`)
        }
        this._datas = [];
        for (const item of iterable) {
            // item 也得是一个可迭代对象
            if (typeof item[Symbol.iterator] !== "function") {
                throw new TypeError(`你提供的${item}不是一个可迭代的对象`);
            }
            const iterator = item[Symbol.iterator]();
            const key = iterator.next().value;
            const value = iterator.next().value;
            this.set(key, value);
        }

    }

    set(key, value) {
        const obj = this._getObj(key);
        if (obj) {
            //修改
            obj.value = value;
        }
        else {
            this._datas.push({
                key,
                value
            })
        }
    }

    get(key) {
        const item = this._getObj(key);
        if (item) {
            return item.value;
        }
        return undefined;
    }

    get size() {
        return this._datas.length;
    }

    delete(key) {
        for (let i = 0; i < this._datas.length; i++) {
            const element = this._datas[i];
            if (this.isEqual(element.key, key)) {
                this._datas.splice(i, 1);
                return true;
            }
        }
        return false;
    }

    clear() {
        this._datas.length = 0;
    }

    /**
     * 根据key值从内部数组中，找到对应的数组项
     * @param {*} key 
     */
    _getObj(key) {
        for (const item of this._datas) {
            if (this.isEqual(item.key, key)) {
                return item;
            }
        }
    }

    has(key) {
        return this._getObj(key) !== undefined;
    }

    /**
     * 判断两个数据是否相等
     * @param {*} data1 
     * @param {*} data2 
     */
    isEqual(data1, data2) {
        if (data1 === 0 && data2 === 0) {
            return true;
        }
        return Object.is(data1, data2);
    }

    *[Symbol.iterator]() {
        for (const item of this._datas) {
            yield [item.key, item.value];
        }
    }

    forEach(callback) {
        for (const item of this._datas) {
            callback(item.value, item.key, this);
        }
    }
}
```

# 代理与反射

## Reflect

```
1. Reflect是什么？

Reflect是一个内置的JS对象，它提供了一系列方法，可以让开发者通过调用这些方法，访问一些JS底层功能

由于它类似于其他语言的**反射**，因此取名为Reflect

2. 它可以做什么？

使用Reflect可以实现诸如 属性的赋值与取值、调用普通函数、调用构造函数、判断属性是否存在与对象中  等等功能

3. 这些功能不是已经存在了吗？为什么还需要用Reflect实现一次？

有一个重要的理念，在ES5就被提出：减少魔法、让代码更加纯粹

这种理念很大程度上是受到函数式编程的影响

ES6进一步贯彻了这种理念，它认为，对属性内存的控制、原型链的修改、函数的调用等等，这些都属于底层实现，属于一种魔法，因此，需要将它们提取出来，形成一个正常的API，并高度聚合到某个对象中，于是，就造就了Reflect对象

因此，你可以看到Reflect对象中有很多的API都可以使用过去的某种语法或其他API实现。

4. 它里面到底提供了哪些API呢？

- Reflect.set(target, propertyKey, value): 设置对象target的属性propertyKey的值为value，等同于给对象的属性赋值
- Reflect.get(target, propertyKey): 读取对象target的属性propertyKey，等同于读取对象的属性值

		const obj = {
            a: 1,
            b: 2
        }
        // obj.a = 10;

        Reflect.set(obj, "a", 10);
        console.log(Reflect.get(obj, "a"))
        
- Reflect.apply(target, thisArgument, argumentsList)：调用一个指定的函数，并绑定this和参数列表。等同于函数调用

 		function method(a, b){
            console.log("method", a, b);
        }

        // method(3, 4);

        Reflect.apply(method, null, [3, 4])
        
- Reflect.deleteProperty(target, propertyKey)：删除一个对象的属性

        const obj = {
            a: 1,
            b: 2
        }
        // delete obj.a;
        Reflect.deleteProperty(obj, "a");
        
- Reflect.defineProperty(target, propertyKey, attributes)：类似于Object.defineProperty，不同的是如果配置出现问题，返回false而不是报错
- Reflect.construct(target, argumentsList)：用构造函数的方式创建一个对象

        function Test(a, b) {
            this.a = a;
            this.b = b;
        }

        // const t = new Test(1, 3);
        const t = Reflect.construct(Test, [1, 3]);
        console.log(t)
        
- Reflect.has(target, propertyKey): 判断一个对象是否拥有一个属性

        const obj = {
            a: 1,
            b: 2
        }

        // console.log("a" in obj);
        console.log(Reflect.has(obj, "a"));
        
- 其他API：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect
```

## Proxy

```
代理：提供了修改底层实现的方式

​```js

//代理一个目标对象
//target：目标对象
//handler：是一个普通对象，其中可以重写底层实现(可以重写所有Reflect的方法)
//返回一个代理对象
new Proxy(target, handler)
​```
```

```
const obj = {
    a: 1,
    b: 2
}

const proxy = new Proxy(obj, {
    set(target, propertyKey, value) {
        // console.log(target, propertyKey, value);
        // target[propertyKey] = value;
        Reflect.set(target, propertyKey, value);
    },
    get(target, propertyKey) {
        if (Reflect.has(target, propertyKey)) {
            return Reflect.get(target, propertyKey);
        } else {
            return -1;
        }
    },
    has(target, propertyKey) {
        return false;
    }
});
// console.log(proxy);
// proxy.a = 10;
// console.log(proxy.a);

console.log(proxy.d);
console.log("a" in proxy);
```

## 观察者模式

​	有一个对象，是观察者，它用于观察另外一个对象的属性值变化，当属性值变化后会收到一个通知，可能会做一些事。

+ 旧版

```
//创建一个观察者
function observer(target) {
    const div = document.getElementById("container");
    const ob = {};
    const props = Object.keys(target);
    for (const prop of props) {
        Object.defineProperty(ob, prop, {
            get() {
                return target[prop];
            },
            set(val) {
                target[prop] = val;
                render();
            },
            enumerable: true
        })
    }
    render();

    function render() {
        let html = "";
        for (const prop of Object.keys(ob)) {
            html += `
                <p><span>${prop}：</span><span>${ob[prop]}</span></p>
            `;
        }
        div.innerHTML = html;
    }

    return ob;
}
const target = {
    a: 1,
    b: 2
}
const obj = observer(target)
```

+ 新版

```
<div id="container">

</div>

<script>
    //创建一个观察者
    function observer(target) {
        const div = document.getElementById("container");
        const proxy = new Proxy(target, {
            set(target, prop, value) {
                Reflect.set(target, prop, value);
                render();
            },
            get(target, prop){
                return Reflect.get(target, prop);
            }
        })
        render();

        function render() {
            let html = "";
            for (const prop of Object.keys(target)) {
                html += `
                    <p><span>${prop}：</span><span>${target[prop]}</span></p>
                `;
            }
            div.innerHTML = html;
        }

        return proxy;
    }
    const target = {
        a: 1,
        b: 2
    }
    const obj = observer(target)
</script>
```

## 偷懒的构造函数

```
class User {

}

function ConstructorProxy(Class, ...propNames) {
    return new Proxy(Class, {
        construct(target, argumentsList) {
            const obj = Reflect.construct(target, argumentsList)
            propNames.forEach((name, i) => {
                obj[name] = argumentsList[i];
            })
            return obj;
        }
    })
}

const UserProxy = ConstructorProxy(User, "firstName", "lastName", "age")

const obj = new UserProxy("袁", "进", 18);
console.log(obj)

class Monster {

}

const MonsterProxy = ConstructorProxy(Monster, "attack", "defence", "hp", "rate", "name")

const m = new MonsterProxy(10, 20, 100, 30, "怪物")
console.log(m);
```

## 可验证的函数参数

```
function sum(a, b) {
    return a + b;
}

function validatorFunction(func, ...types) {
    const proxy = new Proxy(func, {
        apply(target, thisArgument, argumentsList) {
            types.forEach((t, i) => {
                const arg = argumentsList[i]
                if (typeof arg !== t) {
                    throw new TypeError(`第${i+1}个参数${argumentsList[i]}不满足类型${t}`);
                }
            })
            return Reflect.apply(target, thisArgument, argumentsList);
        }
    })
    return proxy;
}

const sumProxy = validatorFunction(sum, "number", "number")
console.log(sumProxy(1, 2))
```

# 增强的数组功能

## 新增的数组API

```
## 静态方法

- Array.of(...args): 使用指定的数组项创建一个新数组
		const arr = Array.of(5)
        // const arr = new Array(1)
        console.log(arr);
        
- Array.from(arg): 通过给定的类数组 或 可迭代对象 创建一个新的数组。
		const divs = document.querySelectorAll("div")
        const result = Array.from(divs)
        console.log(result);

## 实例方法

- find(callback): 用于查找满足条件的第一个元素
        const arr = [{
                name: "a",
                id: 1
            },
            {
                name: "b",
                id: 2
            },
            {
                name: "c",
                id: 3
            },
            {
                name: "d",
                id: 4
            },
            {
                name: "e",
                id: 5
            },
            {
                name: "f",
                id: 6
            },
            {
                name: "g",
                id: 7
            }
        ]

        //找到id为5的对象
        const result = arr.find(item => item.id === 5)
        const resultIndex = arr.findIndex(item => item.id === 5)
        console.log(result, resultIndex);
- findIndex(callback)：用于查找满足条件的第一个元素的下标
- fill(data)：用指定的数据填充满数组所有的内容
        // 创建了一个长度为100的数组，数组的每一项是"abc"
        const arr = new Array(100);
        arr.fill("abc"); 
        
- copyWithin(target, start?, end?): 在数组内部完成复制
        const arr = [1, 2, 3, 4, 5, 6];
        //从下标2开始，改变数组的数据，数据来自于下标0位置开始
        // arr.copyWithin(2); // [1, 2, 1, 2, 3, 4]

        // arr.copyWithin(2, 1); // [1, 2, 2, 3, 4, 5]

        // arr.copyWithin(2, 1, 3); // [1, 2, 2, 3, 5, 6]
        console.log(arr)
        
- includes(data)：判断数组中是否包含某个值，使用Object.is匹配
        const arr = [45, 21, 356, 66 , 6, NaN, 723, 54];

        console.log(arr.indexOf(66) >= 0)
        console.log(arr.includes(NaN));
```

## 类型化数组

```
## 数字存储的前置知识

一个字节是八位，是1024kb

1. 计算机必须使用固定的位数来存储数字，无论存储的数字是大是小，在内存中占用的空间是固定的。

2. n位的无符号整数能表示的数字是2^n个，取值范围是：0 ~ 2^n - 1

3. n位的有符号整数能表示的数字是2^n个，取值范围是：-2^(n-1) ~ 2^(n-1) - 1

4. 浮点数表示法可以用于表示整数和小数，目前分为两种标准：
   1. 32位浮点数：又称为单精度浮点数，它用1位表示符号，8位表示阶码，23位表示尾数
   2. 64位浮点数：又称为双精度浮点数，它用1位表示符号，11位表示阶码，52位表示尾数

5. JS中的所有数字，均使用双精度浮点数保存

## 类型化数组

类型化数组：用于优化多个数字的存储

具体分为：

- Int8Array： 8位有符号整数（-128 ~ 127）
- Uint8Array： 8位无符号整数（0 ~ 255）
- Int16Array: ...
- Uint16Array: ...
- Int32Array: ...
- Uint32Array: ...
- Float32Array:
- Float64Array

1. 如何创建数组

​```js

new 数组构造函数(长度)

数组构造函数.of(元素...)

数组构造函数.from(可迭代对象)

new 数组构造函数(其他类型化数组)

​```

        // const arr = new Int32Array(10);
        const arr = Uint8Array.of(12, 5, 6, 7);
        console.log(arr);
        // console.log(arr.length);
        // console.log(arr.byteLength);


2. 得到长度

​```js
数组.length   //得到元素数量
数组.byteLength //得到占用的字节数

        
        const arr1 = Int32Array.of(35111, 7, 3, 11);
        const arr2 = new Int8Array(arr1);
        console.log(arr1 === arr2);
        console.log(arr1, arr2);

3. 其他的用法跟普通数组一致，但是：

- 不能增加和删除数据，类型化数组的长度固定
- 一些返回数组的方法，返回的数组是同类型化的新数组

        const arr = Int8Array.of(125, 7, 3, 11);
        const arr2 = arr.map(item => item * 2)
        console.log(arr2);

        // arr[1] = 100;
        // console.log(arr);
        // console.log(arr[1])
        // for (const item of arr) {
        //     console.log(item)
        // }

        // arr[4] = 1000; //无效
        // delete arr[0]; //无效
        // console.log(arr)
```

## ArrayBuffer

```
ArrayBuffer：一个对象，用于存储一块固定内存大小的数据。

​```js

new ArrayBuffer(字节数)

​```

可以通过属性```byteLength```得到字节数，可以通过方法```slice```得到新的ArrayBuffer

        //创建了一个用于存储10个字节的内存空间
        const bf = new ArrayBuffer(10);
		//从第四位取到第六位，不包含第六位
        const bf2 = bf.slice(3, 5);

        console.log(bf, bf2);

## 读写ArrayBuffer

1. 使用DataView

通常会在需要混用多种存储格式时使用DataView
        //创建了一个用于存储10个字节的内存空间
        const bf = new ArrayBuffer(10);
		//从第三位开始取，取四位。
        const view = new DataView(bf, 3, 4); //3---偏移量，4---数量

        // console.log(view);
		
        view.setInt16(1, 3);//1---偏移量，3---数量 （注意是在view上开始的偏移量，不是bf）
        console.log(view);

        console.log(view.getInt16(1));

2. 使用类型化数组

实际上，每一个类型化数组都对应一个ArrayBuffer，如果没有手动指定ArrayBuffer，类型化数组创建时，会新建一个ArrayBuffer

        const bf = new ArrayBuffer(10); //10个字节的内存
        const arr1 = new Int8Array(bf);
        const arr2 = new Int16Array(bf);
        console.log(arr1 === arr2);
        console.log(arr1.buffer === arr2.buffer);
        arr1[0] = 10;
        console.log(arr1)
        console.log(arr2);
        
        
        const bf = new ArrayBuffer(10); //10个字节的内存
        const arr = new Int16Array(bf);
        arr[0] = 2344; //操作了两个字节
        console.log(arr);
```

## 制作黑白图片

```
    <div style="display: flex;">
        <img src="./img/liao.jpg" alt="">
        <button onclick="change()">转换</button>
        <canvas width="100" height="117"></canvas>
    </div>

    <script>
        /*
         * 画布中的1个图像是由多个像素点组成，每个像素点拥有4个数据：红、绿、蓝、alpha
         * 把一个图像变成黑白，只需要将图像的每个像素点设置成为红绿蓝的平均数即可
         */

        function change() {
            const img = document.querySelector("img");
            const cvs = document.querySelector("canvas");
            const ctx = cvs.getContext("2d");

            ctx.drawImage(img, 0, 0);
            //得到画布某一个区域的图像信息
            const imageData = ctx.getImageData(0, 0, img.width, img.height);
            console.log(imageData);
            for (let i = 0; i < imageData.data.length; i += 4) {
                //循环一个像素点
                const r = imageData.data[i];
                const g = imageData.data[i + 1];
                const b = imageData.data[i + 2];
                const avg = (r + g + b) / 3;

                imageData.data[i] = imageData.data[i + 1] = imageData.data[i + 2] = avg;
            }
            //将图像数据设置到画布
            ctx.putImageData(imageData, 0, 0);
        }
    </script>
```

## 二进制数据

```
        async function test(){
            const resp = await fetch("./img/liao.jpg")
            const blob = await resp.blob();
            const bf = await blob.arrayBuffer();
            const arr = new Int8Array(bf, 3, 2);
            console.log(arr)
        }
```

