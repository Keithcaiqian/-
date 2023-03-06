# 全局对象

global

```
setInterval
setTimeout
setImmediate  //类似于setInterval 0
console
__dirname  //获取当前运行模块所在的目录，不是global中的
__filename //获取当前模块的文件路径 
Buffer //类型化数组
process //操作进程 （cwd()、exit()、argv、platform、kill(pid)、env）
```

# 模块的查找

+ 绝对路径

```
require('C:\\Users\\k\\Desktop\\practice\\a.js');
```

+ 相对路径 ./ 或 ../

```
require('./a'); //最终会补全成绝对路径
require('./src');// 先找./src
                       ./src.js
                       ./src.json
                       ./src.node
                       ./src.mjs
                       ./src/index.js
```

+ 相对路径

```
require('ab'); //找本级node_modules中的ab文件，找不到再去上级找，最后找不到报错
```

+ package.json中的main字段

```
//表示包的默认入口
node ./    //会去找package.json中的main指定的字段
```

# modules原理

```
// function require(modulePath) {
//   //1. 将modulePath转换为绝对路径：D:\repository\NodeJS\源码\myModule.js
//   //2. 判断是否该模块已有缓存
//   // if(require.cache["D:\\repository\\NodeJS\\源码\\myModule.js"]){
//   //   return require.cache["D:\\repository\\NodeJS\\源码\\myModule.js"].result;
//   // }

//   //3. 读取文件内容
//   //4. 包裹到一个函数中

//   function __temp(module, exports, require,  __dirname, __filename) {
//     console.log("当前模块路径：", __dirname);
//     console.log("当前模块文件：", __filename);
//     exports.c = 3;
//     module.exports = {
//       a: 1,
//       b: 2
//     };
//     this.m = 5;
//   }

//   //6. 创建module对象
//   module.exports = {};
//   const exports = module.exports;

//   __temp.call(module.exports, module, exports, require, module.path, module.filename)
    // return module.exports;
// }
```

# fs模块

## readFile读取文件内容

```
const fs = require("fs"); 
const path = require("path"); //为了获取当前文件所在的目录

//除了require，其他位置不要写相对路径，因为相对路径相对的是命令行中的路径
const filename = path.resolve(__dirname, "./myfiles/1.txt");
//1、第一种转换成utf-8编码，默认是buffer类型，如果不是文字没必要转化
// fs.readFile(filename, "utf-8", (err, content) => {
//   console.log(content);
// });
//2、第二种转换成utf-8编码
// fs.readFile(filename, {encoding:'utf-8'}, (err, content) => {
//   console.log(content);
// });
//3、第三种转换成utf-8编码
// fs.readFile(filename, (err, content) => {
//   console.log(content。toString('utf-8'));
// });

// Sync函数是同步的，会导致JS运行阻塞，极其影响性能
// 通常，在程序启动时运行有限的次数即可

// const content = fs.readFileSync(filename, "utf-8");
// console.log(content);


//fs.promises.readFile是在promise出来后增加的，没有改以前的方法，是为了不导致以前的项目无法运行
async function test() {
  const content = await fs.promises.readFile(filename, "utf-8");
  console.log(content);
}
test();
```

## writeFile写入内容

```
const fs = require("fs");
const path = require("path");

//如果文件不存在，会自动创建，如果文件夹不存在，则会报错
const filename = path.resolve(__dirname, "./myfiles/2.txt");

async function test() {
  // await fs.promises.writeFile(filename, "阿斯顿发发放到发", {
  //   flag: "a" //append在原来内容后面追加内容
  // });
  const buffer = Buffer.from("abcde", "utf-8");
  await fs.promises.writeFile(filename, buffer);
  console.log("写入成功");
}

test();
```

## 手动复制文件

```
const fs = require("fs");
const path = require("path");

async function test() {
  const fromFilename = path.resolve(__dirname, "./myfiles/1.jpeg");
  const buffer = await fs.promises.readFile(fromFilename);
  
  const toFilename = path.resolve(__dirname, "./myfiles/1.copy.jpeg");
  await fs.promises.writeFile(toFilename, buffer);
  console.log("copy success！");
}

test();

```

## stat读取文件或目录的状态

```
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./myfiles/");
async function test() {
  const stat = await fs.promises.stat(filename);
  console.log(stat);
  console.log("是否是目录", stat.isDirectory());
  console.log("是否是文件", stat.isFile());
}

test();
```

## readdir读取目录中的文件和子目录

```
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "./myfiles/");
async function test() {
  const pathes = await fs.promises.readdir(dirname);
  console.log(pathes);
}

test();
```

## mkdir创建目录

```
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "./myfiles/1");
async function test() {
  await fs.promises.mkdir(dirname);
  console.log("创建目录成功");
}

test();
//创建文件用writeFile内容为空就可以了
```

## 手写exists,判断目录是否存在

```
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "./myfiles/3");

async function exists(filename) {
  try {
    await fs.promises.stat(filename);
    return true;
  } catch (err) {
    if (err.code === "ENOENT") {
      //文件不存在
      return false;
    }
    throw err;
  }
}

async function test() {
  const result = await exists(dirname);
  if (result) {
    console.log("目录已存在，无需操作");
  } else {
    await fs.promises.mkdir(dirname);
    console.log("目录创建成功");
  }
}

test();
```

## demo

```
const fs = require("fs");
const path = require("path");

class File {
  constructor(filename, name, ext, isFile, size, createTime, updateTime) {
    this.filename = filename;
    this.name = name;
    this.ext = ext;
    this.isFile = isFile;
    this.size = size;
    this.createTime = createTime;
    this.updateTime = updateTime;
  }

  async getContent(isBuffer = false) {
    if (this.isFile) {
      if (isBuffer) {
        return await fs.promises.readFile(this.filename);
      } else {
        return await fs.promises.readFile(this.filename, "utf-8");
      }
    }
    return null;
  }

  async getChildren() {
    if (this.isFile) {
      //文件不可能有子文件
      return [];
    }
    let children = await fs.promises.readdir(this.filename);
    children = children.map(name => {
      const result = path.resolve(this.filename, name);
      return File.getFile(result);
    });
    return Promise.all(children);
  }

  static async getFile(filename) {
    const stat = await fs.promises.stat(filename);
    const name = path.basename(filename); //文件名
    const ext = path.extname(filename); //后缀名
    const isFile = stat.isFile();
    const size = stat.size;
    const createTime = new Date(stat.birthtime);
    const updateTime = new Date(stat.mtime);
    return new File(filename, name, ext, isFile, size, createTime, updateTime);
  }
}

async function readDir(dirname) {
  const file = await File.getFile(dirname);
  return await file.getChildren();
}

async function test() {
  const dirname = path.resolve(__dirname, "./myfiles");
  const result = await readDir(dirname);
  const datas = await result[0].getChildren();
  console.log(datas);
}

test();	
```

# 文件流

## 可读流

为了节约内存

```
const fs = require("fs");
const path = require("path");

const filename = path.resolve(__dirname, "./abc.txt");
const rs = fs.createReadStream(filename, {
  encoding: "utf-8",
  highWaterMark: 1,
  autoClose: true //读完后会自动完毕，默认为true
});

rs.on("open", () => {
  console.log("文件被打开了");
});

rs.on("error", () => {
  console.log("出错了！！");
});

rs.on("close", () => {
  console.log("文件关闭了");
});
rs.on("data", chunk => {
  console.log("读到了一部分数据：", chunk);
  rs.pause(); //暂停
});

rs.on("pause", () => {
  console.log("暂停了");
  setTimeout(() => {
    rs.resume();
  }, 1000);
});

rs.on("resume", () => {
  console.log("恢复了");
});

rs.on("end", () => {
  console.log("全部数据读取完毕");
});

```

## 可写流

```
const fs = require("fs");
const path = require("path");

const filename = path.resolve(__dirname, "./temp/abc.txt");

const ws = fs.createWriteStream(filename, {
  encoding: "utf-8",
  highWaterMark: 16 * 1024
});

let i = 0;
//一直写，直到到达上限，或无法再直接写入
function write() {
  let flag = true;
  while (i < 1024 * 1024 * 10 && flag) {
    flag = ws.write("a"); //写入a，得到下一次还能不能直接写
    i++;
  }
}

write();

ws.on("drain", () => {
  write();
});
```

## 大文件读写操作

```
const fs = require("fs");
const path = require("path");

//方式1
async function method1() {
  const from = path.resolve(__dirname, "./temp/abc.txt");
  const to = path.resolve(__dirname, "./temp/abc2.txt");
  console.time("方式1");
  const content = await fs.promises.readFile(from);
  await fs.promises.writeFile(to, content);
  console.timeEnd("方式1");
  console.log("复制完成");
}

//方式2
async function method2() {
  const from = path.resolve(__dirname, "./temp/abc.txt");
  const to = path.resolve(__dirname, "./temp/abc2.txt");
  console.time("方式2");

  const rs = fs.createReadStream(from);
  const ws = fs.createWriteStream(to);
  rs.on("data", chunk => {
    //读到一部分数据
    const flag = ws.write(chunk);
    if (!flag) {
      //表示下一次写入，会造成背压
      rs.pause(); //暂停读取
    }
  });

  ws.on("drain", () => {
    //可以继续写了
    rs.resume();
  });

  rs.on("close", () => {
    //写完了
    ws.end(); //完毕写入流
    console.timeEnd("方式2");
    console.log("复制完成");
  });
}

method2();
```

+ 简写方式

```
const fs = require("fs");
const path = require("path");

//方式2
async function method2() {
  const from = path.resolve(__dirname, "./temp/abc.txt");
  const to = path.resolve(__dirname, "./temp/abc2.txt");
  console.time("方式2");

  const rs = fs.createReadStream(from);
  const ws = fs.createWriteStream(to);

  rs.pipe(ws);

  rs.on("close", () => {
    console.timeEnd("方式2");
  });
}

method2();
```

# 断点调试

node --inspect 启动模块  node进程会监听9229端口

# 跨域之JSONP

```
缺陷 会影响服务器的正常响应格式 
	只能使用get请求
```

# cookie 

##  一个不大不小的问题

```
储存在客户端
优点 储存在客户端，不占用服务器资源
缺点 只能是字符串格式；存储量有限；数据容易被获取；数据容易被修改；容易丢失
```

假设服务器有一个接口，通过请求这个接口，可以添加一个管理员

但是，是任何人都有权力做这种操作的

那么服务器如何知道请求接口的人是有权力的呢？

答案是：只有登录过的管理员才能做这种操作

可问题是，客户端和服务器的传输使用的是http协议，http协议是无状态的，什么叫无状态，就是***\*服务器不知道这一次请求的人，跟之前登录请求成功的人是不是同一个人\****



![](http://mdrs.yuanjin.tech/img/image-20200417161014030.png)



![](http://mdrs.yuanjin.tech/img/image-20200417161244373.png)



由于http协议的无状态，服务器***\*忘记\****了之前的所有请求，它无法确定这一次请求的客户端，就是之前登录成功的那个客户端。

\> 你可以把服务器想象成有着严重脸盲症的东哥，他没有办法分清楚跟他说话的人之前做过什么

于是，服务器想了一个办法

它按照下面的流程来认证客户端的身份

\1. 客户端登录成功后，服务器会给客户端一个出入证（令牌 token）

\2. 后续客户端的每次请求，都必须要附带这个出入证（令牌 token）



![](http://mdrs.yuanjin.tech/img/image-20200417161950450.png)



服务器发扬了认证不认人的优良传统，就可以很轻松的识别身份了。

但是，用户不可能只在一个网站登录，于是客户端会收到来自各个网站的出入证，因此，就要求客户端要有一个类似于卡包的东西，能够具备下面的功能：

\1. ***\*能够存放多个出入证\****。这些出入证来自不同的网站，也可能是一个网站有多个出入证，分别用于出入不同的地方

\2. ***\*能够自动出示出入证\****。客户端在访问不同的网站时，能够自动的把对应的出入证附带请求发送出去。

\3. ***\*正确的出示出入证\****。客户端不能将肯德基的出入证发送给麦当劳。

\4. ***\*管理出入证的有效期\****。客户端要能够自动的发现那些已经过期的出入证，并把它从卡包内移除。

能够满足上面所有要求的，就是cookie

cookie类似于一个卡包，专门用于存放各种出入证，并有着一套机制来自动管理这些证件。

卡包内的每一张卡片，称之为***\*一个cookie\****。

## cookie的组成

cookie是浏览器中特有的一个概念，它就像浏览器的专属卡包，管理着各个网站的身份信息。

每个cookie就相当于是属于某个网站的一个卡片，它记录了下面的信息：

\- key：键，比如「身份编号」

\- value：值，比如袁小进的身份编号「14563D1550F2F76D69ECBF4DD54ABC95」，这有点像卡片的条形码，当然，它可以是任何信息

\- domain：域，表达这个cookie是属于哪个网站的，比如`yuanjin.tech`，表示这个cookie是属于`yuanjin.tech`这个网站的

\- path：路径，表达这个cookie是属于该网站的哪个基路径的，就好比是同一家公司不同部门会颁发不同的出入证。比如`/news`，表示这个cookie属于`/news`这个路径的。（后续详细解释）

\- secure：是否使用安全传输（后续详细解释）

\- expire：过期时间，表示该cookie在什么时候过期

当浏览器向服务器发送一个请求的时候，它会瞄一眼自己的卡包，看看哪些卡片适合附带捎给服务器

如果一个cookie***\*同时满足\****以下条件，则这个cookie会被附带到请求中

\- cookie没有过期

\- cookie中的域和这次请求的域是匹配的

 \- 比如cookie中的域是`yuanjin.tech`，则可以匹配的请求域是`yuanjin.tech`、`www.yuanjin.tech`、`blogs.yuanjin.tech`等等

 \- 比如cookie中的域是`www.yuanjin.tech`，则只能匹配`www.yuanjin.tech`这样的请求域

 \- cookie是不在乎端口的，只要域匹配即可

\- cookie中的path和这次请求的path是匹配的

 \- 比如cookie中的path是`/news`，则可以匹配的请求路径可以是`/news`、`/news/detail`、`/news/a/b/c`等等，但不能匹配`/blogs`

 \- 如果cookie的path是`/`，可以想象，能够匹配所有的路径

\- 验证cookie的安全传输

 \- 如果cookie的secure属性是true，则请求协议必须是`https`，否则不会发送该cookie

 \- 如果cookie的secure属性是false，则请求协议可以是`http`，也可以是`https`

如果一个cookie满足了上述的所有条件，则浏览器会把它自动加入到这次请求中

具体加入的方式是，***\*浏览器会将符合条件的cookie，自动放置到请求头中\****，例如，当我在浏览器中访问百度的时候，它在请求头中附带了下面的cookie：

![](http://mdrs.yuanjin.tech/img/image-20200417170328584.png)

看到打马赛克的地方了吗？这部分就是通过请求头`cookie`发送到服务器的，它的格式是`键=值; 键=值; 键=值; ...`，每一个键值对就是一个符合条件的cookie。

***\*cookie中包含了重要的身份信息，永远不要把你的cookie泄露给别人！！！\****否则，他人就拿到了你的证件，有了证件，就具备了为所欲为的可能性。

## 如何设置cookie

由于cookie是保存在浏览器端的，同时，很多证件又是服务器颁发的

所以，cookie的设置有两种模式：

\- 服务器响应：这种模式是非常普遍的，当服务器决定给客户端颁发一个证件时，它会在响应的消息中包含cookie，浏览器会自动的把cookie保存到卡包中

\- 客户端自行设置：这种模式少见一些，不过也有可能会发生，比如用户关闭了某个广告，并选择了「以后不要再弹出」，此时就可以把这种小信息直接通过浏览器的JS代码保存到cookie中。后续请求服务器时，服务器会看到客户端不想要再次弹出广告的cookie，于是就不会再发送广告过来了。

**## 服务器端设置cookie**

服务器可以通过设置响应头，来告诉浏览器应该如何设置cookie

响应头按照下面的格式设置：

\```yaml

set-cookie: cookie1

set-cookie: cookie2

set-cookie: cookie3

...

\```

通过这种模式，就可以在一次响应中设置多个cookie了，具体设置多少个cookie，设置什么cookie，根据你的需要自行处理

其中，每个cookie的格式如下：

\```

键=值; path=?; domain=?; expire=?; max-age=?; secure; httponly

\```

每个cookie除了键值对是必须要设置的，其他的属性都是可选的，并且顺序不限

当这样的响应头到达客户端后，***\*浏览器会自动的将cookie保存到卡包中，如果卡包中已经存在一模一样的卡片（其他key、path、domain相同），则会自动的覆盖之前的设置\****。

下面，依次说明每个属性值：

\- ***\*path\****：设置cookie的路径。如果不设置，浏览器会将其自动设置为当前请求的路径。比如，浏览器请求的地址是`/login`，服务器响应了一个`set-cookie: a=1`，浏览器会将该cookie的path设置为请求的路径`/login`

\- ***\*domain\****：设置cookie的域。如果不设置，浏览器会自动将其设置为当前的请求域，比如，浏览器请求的地址是`http://www.yuanjin.tech`，服务器响应了一个`set-cookie: a=1`，浏览器会将该cookie的domain设置为请求的域`www.yuanjin.tech`

 \- 这里值得注意的是，如果服务器响应了一个无效的域，浏览器是不认的

 \- 什么是无效的域？就是响应的域连根域都不一样。比如，浏览器请求的域是`yuanjin.tech`，服务器响应的cookie是`set-cookie: a=1; domain=baidu.com`，这样的域浏览器是不认的。

 \- 如果浏览器连这样的情况都允许，就意味着张三的服务器，有权利给用户一个cookie，用于访问李四的服务器，这会造成很多安全性的问题

\- ***\*expire\****：设置cookie的过期时间。这里必须是一个有效的GMT时间，即格林威治标准时间字符串，比如`Fri, 17 Apr 2020 09:35:59 GMT`，表示格林威治时间的`2020-04-17 09:35:59`，即北京时间的`2020-04-17 17:35:59`。当客户端的时间达到这个时间点后，会自动销毁该cookie。

\- ***\*max-age\****：设置cookie的相对有效期。expire和max-age通常仅设置一个即可。比如设置`max-age`为`1000`，浏览器在添加cookie时，会自动设置它的`expire`为当前时间加上1000秒，作为过期时间。

 \- 如果不设置expire，又没有设置max-age，则表示会话结束后过期。

 \- 对于大部分浏览器而言，关闭所有浏览器窗口意味着会话结束。

\- ***\*secure\****：设置cookie是否是安全连接。如果设置了该值，则表示该cookie后续只能随着`https`请求发送。如果不设置，则表示该cookie会随着所有请求发送。

\- ***\*httponly\****：设置cookie是否仅能用于传输。如果设置了该值，表示该cookie仅能用于传输，而不允许在客户端通过JS获取，这对防止跨站脚本攻击（XSS）会很有用。 

 \- 关于如何通过JS获取，后续会讲解

 \- 关于什么是XSS，不在本文讨论范围

下面来一个例子，客户端通过`post`请求服务器`http://yuanjin.tech/login`，并在消息体中给予了账号和密码，服务器验证登录成功后，在响应头中加入了以下内容：

\```

set-cookie: token=123456; path=/; max-age=3600; httponly

\```

当该响应到达浏览器后，浏览器会创建下面的cookie：

\```yaml

key: token

value: 123456

domain: yuanjin.tech

path: /

expire: 2020-04-17 18:55:00 #假设当前时间是2020-04-17 17:55:00

secure: false #任何请求都可以附带这个cookie，只要满足其他要求

httponly: true #不允许JS获取该cookie

\```

于是，随着浏览器后续对服务器的请求，只要满足要求，这个cookie就会被附带到请求头中传给服务器：

\```yaml

cookie: token=123456; 其他cookie...

\```

现在，还剩下最后一个问题，就是如何删除浏览器的一个cookie呢？

如果要删除浏览器的cookie，只需要让服务器响应一个同样的域、同样的路径、同样的key，只是时间过期的cookie即可

***\*所以，删除cookie其实就是修改cookie\****

下面的响应会让浏览器删除`token`

\```yaml

cookie: token=; domain=yuanjin.tech; path=/; max-age=-1

\```

浏览器按照要求修改了cookie后，会发现cookie已经过期，于是自然就会删除了。

\> 无论是修改还是删除，都要注意cookie的域和路径，因为完全可能存在域或路径不同，但key相同的cookie

\>

\> 因此无法仅通过key确定是哪一个cookie

**## 客户端设置cookie**

既然cookie是存放在浏览器端的，所以浏览器向JS公开了接口，让其可以设置cookie

\```js

document.cookie = "键=值; path=?; domain=?; expire=?; max-age=?; secure";

\```

可以看出，在客户端设置cookie，和服务器设置cookie的格式一样，只是有下面的不同

\- 没有httponly。因为httponly本来就是为了限制在客户端访问的，既然你是在客户端配置，自然失去了限制的意义。

\- path的默认值。在服务器端设置cookie时，如果没有写path，使用的是请求的path。而在客户端设置cookie时，也许根本没有请求发生。因此，path在客户端设置时的默认值是当前网页的path

\- domain的默认值。和path同理，客户端设置时的默认值是当前网页的domain

\- 其他：一样

\- 删除cookie：和服务器也一样，修改cookie的过期时间即可

**# 总结**

以上，就是cookie原理部分的内容。

如果把它用于登录场景，就是如下的流程：

***\*登录请求\****

\1. 浏览器发送请求到服务器，附带账号密码

\2. 服务器验证账号密码是否正确，如果不正确，响应错误，如果正确，在响应头中设置cookie，附带登录认证信息（至于登录认证信息是设么样的，如何设计，要考虑哪些问题，就是另一个话题了，可以百度 jwt）

\3. 客户端收到cookie，浏览器自动记录下来

***\*后续请求\****

\1. 浏览器发送请求到服务器，希望添加一个管理员，并将cookie自动附带到请求中

\2. 服务器先获取cookie，验证cookie中的信息是否正确，如果不正确，不予以操作，如果正确，完成正常的业务流程



## 实现登录和认证

```
1 使用cookie-parser 中间件
2 登录后给予token 
		通过cookie给予：适配浏览器
		通过header给予： 适配其他端
3 对后序请求进行认证
         解析cookie或header中的token
         验证token  通过：继续后续处理； 未通过：给予错误
```

# session

```
储存在服务器端
优点 可以是任意格式；储存量理论上是无限的；数据难以被获取；数据难以篡改；不易丢失；
缺点 占用服务器资源
```

# jwt

随着前后端分离的发展，以及数据中心的建立，越来越多的公司会创建一个中心服务器，服务于各种产品线。

而这些产品线上的产品，它们可能有着各种终端设备，包括但不仅限于浏览器、桌面应用、移动端应用、平板应用、甚至智能家居

![image-20200422163727151](http://mdrs.yuanjin.tech/img/image-20200422163727151.png)

\> 实际上，不同的产品线通常有自己的服务器，产品内部的数据一般和自己的服务器交互。

\>

\> 但中心服务器仍然有必要存在，因为同一家公司的产品总是会存在共享的数据，比如用户数据

这些设备与中心服务器之间会进行http通信

一般来说，中心服务器至少承担着认证和授权的功能，例如登录：各种设备发送消息到中心服务器，然后中心服务器响应一个身份令牌（参见[cookie原理详解](http://www.yuanjin.tech/article/98)）

当这种结构出现后，就出现一个问题：它们之间还能使用传统的cookie方式传递令牌信息吗？

其实，也是可以的🐶，因为cookie在传输中无非是一个消息头而已，只不过浏览器对这个消息头有特殊处理罢了。

但浏览器之外的设备肯定不喜欢cookie，因为浏览器有着对cookie完善的管理机制，但是在其他设备上，就需要开发者自己手动处理了

jwt的出现就是为了解决这个问题

## 概述

jwt全称`Json Web Token`，强行翻译过来就是`json格式的互联网令牌`（算了，还是不要强行翻译了🐷）

它要解决的问题，就是为多种终端设备，提供***\*统一的、安全的\****令牌格式

![image-20200422165350268](http://mdrs.yuanjin.tech/img/image-20200422165350268.png)

因此，jwt只是一个令牌格式而已，你可以把它存储到cookie，也可以存储到localstorage，没有任何限制！

同样的，对于传输，你可以使用任何传输方式来传输jwt，一般来说，我们会使用消息头来传输它

比如，当登录成功后，服务器可以给客户端响应一个jwt：

\```

HTTP/1.1 200 OK

...

set-cookie:token=jwt令牌

authorization:jwt令牌

...

{..., token:jwt令牌}

\```

可以看到，jwt令牌可以出现在响应的任何一个地方，客户端和服务器自行约定即可。

\> 当然，它也可以出现在响应的多个地方，比如为了充分利用浏览器的cookie，同时为了照顾其他设备，也可以让jwt出现在`set-cookie`和`authorization或body`中，尽管这会增加额外的传输量。

当客户端拿到令牌后，它要做的只有一件事：存储它。

你可以存储到任何位置，比如手机文件、PC文件、localstorage、cookie

当后续请求发生时，你只需要将它作为请求的一部分发送到服务器即可。

虽然jwt没有明确要求应该如何附带到请求中，但通常我们会使用如下的格式：

\```

GET /api/resources HTTP/1.1

...

authorization: bearer jwt令牌

...

\```

\> 这种格式是OAuth2附带token的一种规范格式

\>

\> 至于什么是OAuth2，那是另一个话题了

这样一来，服务器就能够收到这个令牌了，通过对令牌的验证，即可知道该令牌是否有效。

它们的完整交互流程是非常简单清晰的

![image-20200422172837190](http://mdrs.yuanjin.tech/img/image-20200422172837190.png)

## 令牌的组成

为了保证令牌的安全性，jwt令牌由三个部分组成，分别是：

\1. header：令牌头部，记录了整个令牌的类型和签名算法

\2. payload：令牌负荷，记录了保存的主体信息，比如你要保存的用户信息就可以放到这里

\3. signature：令牌签名，按照头部固定的签名算法对整个令牌进行签名，该签名的作用是：保证令牌不被伪造和篡改

它们组合而成的完整格式是：`header.payload.signature`

比如，一个完整的jt令牌如下：

\```

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9.BCwUy3jnUQ_E6TqCayc7rCHkx-vxxdagUwPOWqwYCFc

\```

它各个部分的值分别是：

\- `header：eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9`

\- `payload：eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9`

\- `signature: BCwUy3jnUQ_E6TqCayc7rCHkx-vxxdagUwPOWqwYCFc`

下面分别对每个部分进行说明

### header

它是令牌头部，记录了整个令牌的类型和签名算法

它的格式是一个`json`对象，如下：

\```json

{

 "alg":"HS256",

 "typ":"JWT"

}

\```

该对象记录了：

\- alg：signature部分使用的签名算法，通常可以取两个值

 \- HS256：一种对称加密算法，使用同一个秘钥对signature加密解密

 \- RS256：一种非对称加密算法，使用私钥加密，公钥解密

\- typ：整个令牌的类型，固定写`JWT`即可

设置好了`header`之后，就可以生成`header`部分了

具体的生成方式及其简单，就是把`header`部分使用`base64 url`编码即可

\> `base64 url`不是一个加密算法，而是一种编码方式，它是在`base64`算法的基础上对`+`、`=`、`/`三个字符做出特殊处理的算法

\>

\> 而`base64`是使用64个可打印字符来表示一个二进制数据，具体的做法参考[百度百科](https://baike.baidu.com/item/base64/8545775?fr=aladdin)

浏览器提供了`btoa`函数，可以完成这个操作：

\```js

window.btoa(JSON.stringify({

 "alg":"HS256",

 "typ":"JWT"

}))

// 得到字符串：eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9

\```

同样的，浏览器也提供了`atob`函数，可以对其进行解码：

\```js

window.atob("eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9")

// 得到字符串：{"alg":"HS256","typ":"JWT"}

\```

\> nodejs中没有提供这两个函数，可以安装第三方库`atob`和`bota`搞定

\>

\> 或者，手动搞定

### payload

这部分是jwt的主体信息，它仍然是一个JSON对象，它可以包含以下内容：

\```json

{

 "ss"："发行者",

 "iat"："发布时间",

 "exp"："到期时间",

 "sub"："主题",

 "aud"："听众",

 "nbf"："在此之前不可用",

 "jti"："JWT ID"

}

\```

以上属性可以全写，也可以一个都不写，它只是一个规范，就算写了，也需要你在将来验证这个jwt令牌时手动处理才能发挥作用

上述属性表达的含义分别是：

\- ss：发行该jwt的是谁，可以写公司名字，也可以写服务名称

\- iat：该jwt的发放时间，通常写当前时间的时间戳

\- exp：该jwt的到期时间，通常写时间戳

\- sub：该jwt是用于干嘛的

\- aud：该jwt是发放给哪个终端的，可以是终端类型，也可以是用户名称，随意一点

\- nbf：一个时间点，在该时间点到达之前，这个令牌是不可用的

\- jti：jwt的唯一编号，设置此项的目的，主要是为了防止重放攻击（重放攻击是在某些场景下，用户使用之前的令牌发送到服务器，被服务器正确的识别，从而导致不可预期的行为发生）

可是到现在，看了半天，没有出现我想要写入的数据啊😂

当用户登陆成功之后，我可能需要把用户的一些信息写入到jwt令牌中，比如用户id、账号等等（密码就算了😳）

其实很简单，payload这一部分只是一个json对象而已，你可以向对象中加入任何想要加入的信息

比如，下面的json对象仍然是一个有效的payload

\```json

{

 "foo":"bar",

 "iat":1587548215

}

\```

`foo: bar`是我们自定义的信息，`iat: 1587548215`是jwt规范中的信息

最终，payload部分和header一样，需要通过`base64 url`编码得到：

\```js

window.btoa(JSON.stringify({

 "foo":"bar",

 "iat":1587548215

}))

// 得到字符串：eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9

\```

### signature

这一部分是jwt的签名，正是它的存在，保证了整个jwt不被篡改

这部分的生成，是对前面两个部分的编码结果，按照头部指定的方式进行加密

比如：头部指定的加密方法是`HS256`，前面两部分的编码结果是`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9`

则第三部分就是用对称加密算法`HS256`对字符串`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9`进行加密，当然你得指定一个秘钥，比如`shhhhh`

\```js

HS256(`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9`, "shhhhh")

// 得到：BCwUy3jnUQ_E6TqCayc7rCHkx-vxxdagUwPOWqwYCFc

\```

最终，将三部分组合在一起，就得到了完整的jwt

\```

eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJmb28iOiJiYXIiLCJpYXQiOjE1ODc1NDgyMTV9.BCwUy3jnUQ_E6TqCayc7rCHkx-vxxdagUwPOWqwYCFc

\```

由于签名使用的秘钥保存在服务器，这样一来，客户端就无法伪造出签名，因为它拿不到秘钥。

换句话说，之所以说无法伪造jwt，就是因为第三部分的存在。

而前面两部分并没有加密，只是一个编码结果而已，可以认为几乎是明文传输

\> 这不会造成太大的问题，因为既然用户登陆成功了，它当然有权力查看自己的用户信息

\>

\> 甚至在某些网站，用户的基本信息可以被任何人查看

\>

\> 你要保证的，是不要把敏感的信息存放到jwt中，比如密码

jwt的`signature`可以保证令牌不被伪造，那如何保证令牌不被篡改呢？

比如，某个用户登陆成功了，获得了jwt，但他人为的篡改了`payload`，比如把自己的账户余额修改为原来的两倍，然后重新编码出`payload`发送到服务器，服务器如何得知这些信息被篡改过了呢？

这就要说到令牌的验证了

## 令牌的验证

![image-20200422172837190](http://mdrs.yuanjin.tech/img/image-20200422172837190.png)

令牌在服务器组装完成后，会以任意的方式发送到客户端

客户端会把令牌保存起来，后续的请求会将令牌发送给服务器

而服务器需要验证令牌是否正确，如何验证呢？

首先，服务器要验证这个令牌是否被篡改过，验证方式非常简单，就是对`header+payload`用同样的秘钥和加密算法进行重新加密

然后把加密的结果和传入jwt的`signature`进行对比，如果完全相同，则表示前面两部分没有动过，就是自己颁发的，如果不同，肯定是被篡改过了。

\```

传入的header.传入的payload.传入的signature

新的signature = header中的加密算法(传入的header.传入的payload, 秘钥)

验证：新的signature == 传入的signature

\```

当令牌验证为没有被篡改后，服务器可以进行其他验证：比如是否过期、听众是否满足要求等等，这些就视情况而定了

注意：这些验证都需要服务器手动完成，没有哪个服务器会给你进行自动验证，当然，你可以借助第三方库来完成这些操作

## 总结

最后，总结一下jwt的特点：

\- jwt本质上是一种令牌格式。它和终端设备无关，同样和服务器无关，甚至与如何传输无关，它只是规范了令牌的格式而已

\- jwt由三部分组成：header、payload、signature。主体信息在payload

\- jwt难以被篡改和伪造。这是因为有第三部分的签名存在。

# MVC

```
---> 请求 ---> controller(控制器) --->数据(模型model) ---> 模板（view）---> 响应 //过时的东西
```

# 客户端缓存

```
# 缓存的基本原理

在一个C/S结构中，最基本的缓存分为两种：

- 客户端缓存
- 服务器缓存

**本文仅讨论客户端缓存**

所谓**客户端缓存**，顾名思义，是将某一次的响应结果保存在客户端（比如浏览器）中，而后续的请求仅需要从缓存中读取即可，极大的降低了服务器的处理压力。

客户端缓存的原理如下：

<img src="http://mdrs.yuanjin.tech/img/image-20200430202446870.png" alt="image-20200430202446870" style="zoom: 33%;" />

> 这只是一个简易的原理图，实际情况可能有差异

这里就设计到一个缓存策略的问题，这些问题包括：

- 哪些资源需要加入到缓存，哪些不需要？
- 缓存的时间是多久呢？
- 如果服务器的资源有改动，客户端如何更新缓存呢？
- 如果缓存过期了，可是服务器上的资源并没有发生变动，又该如何处理呢？
- .......

要回答这些问题，就必须要清楚`http`中关于缓存的协议

理解了http的缓存协议，自然就能回答上面的问题了。

# 来自服务器的缓存指令

当客户端发出一个`get`请求到服务器，服务器可能有以下的内心活动：「你请求的这个资源，我很少会改动它，干脆你把它缓存起来吧，以后就不要来烦我了」

为了表达这个美好的愿望，服务器在**响应头**中加入了以下内容：

​```
Cache-Control: max-age=3600
ETag: W/"121-171ca289ebf"
Date: Thu, 30 Apr 2020 12:39:56 GMT
Last-Modified: Thu, 30 Apr 2020 08:16:31 GMT
​```

这个响应头表达了下面的信息：

- `Cache-Control: max-age=3600`，我希望你把这个资源缓存起来，缓存时间是3600秒（1小时）
- `ETag: W/"121-171ca289ebf"`，这个资源的编号是`W/"121-171ca289ebf"`
- `Date: Thu, 30 Apr 2020 12:39:56 GMT`，我给你响应这个资源的服务器时间是格林威治时间`2020-04-30 12:39:56`
- `Last-Modified: Thu, 30 Apr 2020 08:16:31 GMT`，这个资源的上一次修改时间是格林威治时间`2020-04-30 08:16:31`

这个美好的缓存愿望，就这样通过响应头传递给客户端了

如果客户端是其他应用程序，可能并不会理会服务器的愿望，也就是说，可能根本不会缓存任何东西。

但是凑巧客户端是一个浏览器，它和服务器一直以来都是相亲相爱的小伙伴，当它看到服务器的这个响应头表达的美好愿望后，立即忙起来：

- 浏览器把这次请求得到的响应体缓存到本地文件中
- 浏览器标记这次请求的请求方法和请求路径
- 浏览器标记这次缓存的时间是3600秒
- 浏览器记录服务器的响应时间是格林威治时间`2020-04-30 12:39:56`
- 浏览器记录服务器给予的资源编号`W/"121-171ca289ebf"`
- 浏览器记录资源的上一次修改时间是格林威治时间`2020-04-30 08:16:31`

这一次的记录非常重要，它为以后浏览器要不要去请求服务器提供了各种依据。

<img src="http://mdrs.yuanjin.tech/img/image-20200430210430455.png" alt="image-20200430210430455" style="zoom:33%;" />

# 来自客户端的缓存指令

当客户端收拾好行李，准备再次请求`GET /index.js`时，它突然想起了一件事：我需要的东西在不在缓存里呢？

此时，客户端会到缓存中去寻找是否有缓存的资源

寻找的过程如下：

1. 缓存中是否有匹配的请求方法和路径？
2. 如果有，该缓存资源是否还有效呢？

以上两个验证会导致浏览器产生不同的行为

<img src="http://mdrs.yuanjin.tech/img/image-20200430212052228.png" alt="image-20200430212052228" style="zoom:50%;" />

<img src="http://mdrs.yuanjin.tech/img/image-20200430214301507.png" alt="image-20200430214301507" style="zoom:33%;" />

要验证是否有匹配的缓存非常简单，只需要验证当前的请求方法`GET`和当前的请求路径`/index.js`是否有对应的缓存存在即可

如果没有，就直接请求服务器，就和第一次请求服务器时一样，这种情况没有什么好讨论的

关键在于验证缓存是否有效

如何验证呢？

非常简单，就是把`max-age + Date`，得到一个过期时间，看看这个过期时间是否大于当前时间，如果是，则表示缓存还没有过期，仍然有效，如果不是，则表示缓存失效。

## 缓存有效

当浏览器发现缓存有效时，完全不会请求服务器，直接使用缓存即可得到结果

此时，如果你断开网络，会发现资源仍然可用

这种情况会极大的降低服务器压力，但当服务器更改了资源后，浏览器是不知道的，只要缓存有效，它就会直接使用缓存

## 缓存无效

当浏览器发现缓存已经过期，它**并不会简单的把缓存删除**，而是抱着一丝希望，想问问服务器，我**这个缓存还能继续使用吗**？

于是，浏览器向服务器发出了一个**带缓存的请求**

所谓带缓存的请求，无非就是加入了以下的请求头：

​```
If-Modified-Since: Thu, 30 Apr 2020 08:16:31 GMT
If-None-Match: W/"121-171ca289ebf"
​```

它们表达了下面的信息：

- `If-Modified-Since: Thu, 30 Apr 2020 08:16:31 GMT`，亲，你曾经告诉我，这个资源的上一次修改时间是格林威治时间`2020-04-30 08:16:31`，请问这个资源在这个时间之后有发生变动吗？
- `If-None-Match: W/"121-171ca289ebf"`，亲，你曾经告诉我，这个资源的编号是`W/"121-171ca289ebf`，请问这个资源的编号发生变动了吗？

其实，这两个问题可以合并为一个问题：快说！资源到底变了没有！

之所以要发两个信息，是为了兼容不同的服务器，因为有些服务器只认`If-Modified-Since`，有些服务器只认`If-None-Match`，有些服务器两个都认

> 目前的很多服务器，只要发现`If-None-Match`存在，就不会去看``If-Modified-Since`
>
> `If-Modified-Since`是`http1.0`版本的规范，`If-None-Match`是`http1.1`的规范

此时，问题又抛给了服务器，接下来，就是服务器的表演时间了

服务器可能会产生两个情况：

- 缓存已经失效
- 缓存仍然有效

如果是第一种情况——**缓存已经失效**，那么非常简单，服务器再次给予一个正常的响应（响应码`200` 带响应体），同时可以附带上新的缓存指令，这就回到了上一节——来自服务器的缓存指令

这样一来，客户端就会重新缓存新的内容

但如果服务器觉得**缓存仍然有效**，它可以通过一种极其简单的方式告诉客户端：

- 响应码为`304 Not Modified`
- 无响应体
- 响应头带上新的缓存指令，见上一节——来自服务器的缓存指令

这样一来，就相当于告诉客户端：「你的缓存资源仍然可用，我给你一个新的缓存时间，你那边更新一下就可以了」

于是，客户端就继续happy的使用缓存了

这样一来，可以最大程度的减少网络传输，因为如果资源还有效，服务器就不会传输消息体

它们完整的交互过程如下：

<img src="http://mdrs.yuanjin.tech/img/image-20200430225326001.png" alt="image-20200430225326001" style="zoom:33%;" />

# 细节

上面描述了客户端缓存的基本概念和过程

但其中仍然有不少细节值得我们注意

## Cache-Control

在上述的讲解中，`Cache-Control`是服务器向客户端响应的一个消息头，它提供了一个`max-age`用于指定缓存时间。

实际上，`Cache-Control`还可以设置下面一个或多个值：

- `public`：指示服务器资源是公开的。比如有一个页面资源，所有人看到的都是一样的。这个值对于浏览器而言没有什么意义，但可能在某些场景可能有用。本着「我告知，你随意」的原则，`http`协议中很多时候都是客户端或服务器告诉另一端详细的信息，至于另一端用不用，完全看它自己。
- `private`：指示服务器资源是私有的。比如有一个页面资源，每个用户看到的都不一样。这个值对于浏览器而言没有什么意义，但可能在某些场景可能有用。本着「我告知，你随意」的原则，`http`协议中很多时候都是客户端或服务器告诉另一端详细的信息，至于另一端用不用，完全看它自己。
- `no-cache`：告知客户端，你可以缓存这个资源，但是不要**直接**使用它。当你缓存之后，后续的每一次请求都需要附带缓存指令，让服务器告诉你这个资源有没有过期。见：「来自客户端的缓存指令 -  缓存无效」
- `no-store`：告知客户端，不要对这个资源做任何的缓存，之后的每一次请求都按照正常的普通请求进行。若设置了这个值，浏览器将不会对该资源做出任何的缓存处理。
- `max-age`：不再赘述

比如，`Cache-Control: public, max-age=3600`表示这是一个公开资源，请缓存1个小时。

## Expire

在`http1.0`版本中，是通过`Expire`响应头来指定过期时间点的，例如：

​```
Expire: Thu, 30 Apr 2020 23:38:38 GMT
​```

到了`http1.1`版本，已更改为通过`Cache-Control`的`max-age`来记录了。

## 记录缓存时的有效期

浏览器会按照服务器响应头的要求，自动记录缓存到本地文件，并设置各种相关信息

在这些信息中，**有效期**尤为关键，它决定了这个缓存可以使用多久

浏览器会根据服务器不同的响应情况，设置不同的有效期

具体的有效期设置，按照下面的流程进行：

<img src="http://mdrs.yuanjin.tech/img/image-20200501075337464.png" alt="image-20200501075337464" style="zoom:33%;" />

例如，当`max-age`设置为0时，缓存立即过期

虽然立即过期，但缓存仍然被记录下来，后续的请求通过缓存指令发送到服务器，来确认资源是否被更改。

因此，`Cache-Control: max-age=0`类似于`Cache-Control: no-cache`

## Pragma

这是`http1.0`版本的消息头

当该消息头出现在请求中时，是向服务器表达：不要考虑任何缓存，给我一个正常的结果。

在`http1.1`版本中，可以在**请求头**中加入`Cache-Control: no-cache`实现同样的含义。

> 是的，`Cache-Control`可以出现在请求头中

在`Chrome`浏览器中调试时，如果勾选了`Disable cache`，则发送的请求中会附带该信息

![image-20200501080330131](http://mdrs.yuanjin.tech/img/image-20200501080330131.png)

## Vary

有的时候，是否有缓存，不仅仅是判断请求方法和请求路径是否匹配，可能还要判断头部信息是否匹配。

此时，就可以使用`Vary`字段来指定要区分的消息头

比如，当使用`GET /personal.html`请求服务器时，请求头中`cookie`的值不一样，得到的页面也不一样

如果还按照之前的做法，仅仅匹配请求方法和请求路径，如果`cookie`变动，你可能得到的仍然是之前的页面。

正确的做法如下：

<img src="http://mdrs.yuanjin.tech/img/image-20200501082103089.png" alt="image-20200501082103089" style="zoom:33%;" />

## 使用版本号或hash

如果你是一个前端工程师，使用过`vue`或其他基于`webpack`搭建的工程

你会发现打包的结果中很多文件名类似于这样：

​```
app.68297cd8.css
​```

文件的中间部分使用了`hash`值

这样做的好处是，可以让客户端大胆的、长时间的缓存该文件，减轻服务器的压力

当文件改动后，它的文件`hash`值也会随之而变，比如变成了`app.446fccb8.css`

这样一来，客户端要请求新的文件时，就会发现路径从`/app.68297cd8.css`变成了`app.446fccb8.css`，由于之前的缓存路径无法匹配到，因此就会发送新的请求来获取新资源了。

以上是现代流行的做法。

而在古老的年代，还没有构建工具出现时，人们使用的办法是在资源路径后面加入版本号来获取新版本的文件

比如，页面中引入了一个css资源`app.css`，它可能的引入方式是：

​```html
<link href="/app.css?v=1.0.0">
​```

这样一来，缓存的路径是`/app.css?v=1.0.0`

当服务器的版本发生变化时，可以给予新的版本号，让html中的路径发生变动

​```html
<link href="/app.css?v=1.0.1">
​```

由于新的路径无法命中缓存，于是浏览器就会发送新的普通请求来获取这个资源

# 总结

最后，通过客户端和服务器两位大佬的视角，来总结一下以上内容

## 服务器视角

服务器无法知道客户端到底有没有像浏览器那样缓存文件，它只管根据请求的情况来决定如何响应

![image-20200501083702987](http://mdrs.yuanjin.tech/img/image-20200501083702987.png)

很多后端语言搭建的服务器都会自带自己的默认缓存规则，当然也支持不同程度的修改

## 浏览器视角

浏览器在发出请求时会判断要不要使用缓存

<img src="http://mdrs.yuanjin.tech/img/image-20200501084258712.png" alt="image-20200501084258712" style="zoom:50%;" />

当收到服务器响应时，会自动根据缓存指令进行处理

<img src="http://mdrs.yuanjin.tech/img/image-20200501084559394.png" alt="image-20200501084559394" style="zoom:50%;" />


```

# websocket

```
# socket

1. 客户端连接服务器（TCP / IP），三次握手，建立了连接通道
2. 客户端和服务器通过socket接口发送消息和接收消息，任何一端在任何时候，都可以向另一端发送任何消息
3. 有一端断开了，通道销毁



# http

1. 客户端连接服务器（TCP / IP），三次握手，建立了连接通道
2. 客户端发送一个http格式的消息（消息头 消息体），服务器响应http格式的消息（消息头 消息体）
3. 客户端或服务器断开，通道销毁



实时性的问题：

1. 轮询
2. 长连接



# websocket

专门用于解决实时传输的问题

1. 客户端连接服务器（TCP / IP），三次握手，建立了连接通道
2. 客户端发送一个http格式的消息（特殊格式），服务器也响应一个http格式的消息（特殊格式），称之为http握手
3. 双发自由通信，通信格式按照websocket的要求进行
4. 客户端或服务器断开，通道销毁
```

# CSRF攻击和防御

```
# CSRF 特点和原理

CSRF：Cross Site Request Forgery，跨站请求伪造

本质是：恶意网站把**正常用户**作为**媒介**，通过模拟正常用户的操作，攻击其**登录过**的站点。

<img src="http://mdrs.yuanjin.tech/img/image-20200508122744169.png" alt="image-20200508122744169" style="zoom:50%;" />

它的原理如下：

1. 用户访问正常站点，登录后，获取到了正常站点的令牌，以cookie的形式保存

<img src="http://mdrs.yuanjin.tech/img/image-20200508123116104.png" alt="image-20200508123116104" style="zoom:50%;" />

2. 用户访问恶意站点，恶意站点通过某种形式去请求了正常站点（请求伪造），迫使正常用户把令牌传递到正常站点，完成攻击

<img src="http://mdrs.yuanjin.tech/img/image-20200508123401591.png" alt="image-20200508123401591" style="zoom:50%;" />

# 防御

## cookie的SameSite

现在很多浏览器都支持**禁止跨域附带的cookie**，只需要把cookie设置的`SameSite`设置为`Strict`即可

`SameSite`有以下取值：

- Strict：严格，所有跨站请求都不附带cookie，有时会导致用户体验不好
- Lax：宽松，所有跨站的超链接、GET请求的表单、预加载连接时会发送cookie，其他情况不发送
- None：无限制

这种方法非常简单，极其有效，但前提条件是：用户不能使用太旧的浏览器

## 验证referer和Origin

页面中的二次请求都会附带referer或Origin请求头，向服务器表示该请求来自于哪个源或页面，服务器可以通过这个头进行验证

但某些浏览器的referer是可以被用户禁止的，尽管这种情况极少

## 使用非cookie令牌

这种做法是要求每次请求需要在请求体或请求头中附带token



请求的时候：authorization: token



## 验证码

这种做法是要求每个要防止CSRF的请求都必须要附带验证码

不好的地方是容易把正常的用户逼疯

## 表单随机数

这种做法是服务端渲染时，生成一个随机数，客户端提交时要提交这个随机数，然后服务器端进行对比

该随机数是一次性的



流程：

1. 客户端请求服务器，请求添加学生的页面，传递cookie
2. 服务器
   1. 生成一个随机数，放到session中
   2. 生成页面时，表单中加入一个隐藏的表单域`<input type="hidden" name="hash" value="<%=session['key'] %>">`
3. 填写好信息后，提交表单，会自动提交隐藏的随机数
4. 服务器
   1. 先拿到cookie，判断是否登录过
   2. 对比提交过来的随机数和之前的随机数是否一致
   3. 清除掉session中的随机数



## 二次验证

当做出敏感操作时，进行二次验证
```

# XSS攻击和防御

```
# XSS攻击和防御

XSS：Cross Site Scripting 跨站脚本攻击



## 存储型XSS

1. 恶意用户提交了恶意内容到服务器
2. 服务器没有识别，保存了恶意内容到数据库



1. 正常用户访问服务器
2. 服务器在不知情的情况下，给予了之前的恶意内容，让正常用户遭到攻击



## 反射型

1. 恶意用户分享了一个正常网站的链接，链接中带有恶意内容
2. 正常用户点击了该链接
3. 服务器在不知情的情况，把链接的恶意内容读取了出来，放进了页面中，让正常用户遭到攻击



## DOM型

1. 恶意用户通过任何方式，向服务器中注入了一些dom元素，从而影响了服务器的dom结构
2. 普通用户访问时，运行的是服务器的正常js代码
```

# nodeJs原理

![](D:\笔记\typora-user-images\node组成原理图.jpg)

```
1. 用户代码

JS代码，开发者编写的



2. 第三方库

大部分仍然是JS代码，由其他开发者编写



3. 本地模块代码

JS代码



4. V8引擎

c/c++代码，作用：把JS代码解释成为机器码

可以通过v8引擎的某种机制，扩展其功能



V8引擎的扩展和对扩展的编译，是通过一个工具：gyp工具

某些第三方库需要使用`node-gyp`工具进行构建，因此需要先安装`node-gyp`
```

# 线程和进程

```
# 进程

一个应用程序，总是通过操作系统启动的，当操作系统启动一个应用程序时，会给其分配一个进程

一个进程拥有**独立的、可伸缩的**内存空间，原则上不受其他进程干扰

进程之间是可以通信的，只要两个进程双方遵守一定的协议，比如ipc

**CPU在不同的进程之间切换执行**



虽然一个应用程序在启动时只有一个进程，但它在运行的过程中，可以开启新的进程，进程之间仍然保持相对独立

如果一个进程是直接由操作系统开启，则它叫做主进程

如果一个进程B是由进程A开启，则A是B的父进程，B是A的子进程，子进程会继承父进程的一些信息，但仍然保持相对独立



​```js
// nodejs 中开启子进程
const childProcess = require("child_process"); // 导入内置模块

childProcess.exec(在子进程运行的命令, (err, out, stdErr) => {
  // 回调函数中可以获取子进程的标准输出，这种数据交互是通过IPC完成的，nodejs已经帮你完成了处理
  // err：开启进程过程中发生的错误
  // out: 子进程输出的正常内容
  // stdErr: 子进程输出的错误内容
  // 子进程发生任何的错误，绝不会影响到父进程，它们的内存空间是完全隔离的
});

// 过去，nodejs没有提供给用户创建线程的接口，只能使用进程的方式
// 过去，nodejs还提供了cluster模块，通过另一种模式来创建进程
// 现在，nodejs已经提供了线程模块，对进程的操作已经很少使用了
​```

# 线程

操作系统启动一个进程（无论是主进程，还是子进程），都会自动为它分配一个线程，称之为主线程

**程序一定在线程上运行！！**

主线程在运行的过程中，可以创建多个线程，这些线程称之为子线程

当操作系统命令CPU去执行一个进程时，实际上，是在该进程的多个线程中切换执行

线程和进程很相似，它们都是独立运行，最大的区别在于：**线程的内存空间没有隔离**，共享进程的内存空间，线程之间的数据不用遵守任何协议，可以随意使用



什么时候要使用线程？

使用线程的主要目的，是为了充分使用多核cpu。线程执行过程中，尽量的不要阻塞。

最理想的线程效果：

1. 线程数等于cpu的核数
2. 线程永不阻塞
   1. 没有io
   2. 只存在大量运算
3. 线程相对独立，几乎不使用共享数据

线程一般处理cpu密集型操作（运算操作），而io密集型操作不适合使用线程，而适合使用异步



为了避免线程执行过程中共享数据产生的麻烦，nodejs使用独特的线程机制来尽力规避：

​```js
// 创建子线程的父线程
const { Worker } = require("worker_threads");
const worker = new Worker(线程执行的入口文件, {
  workerData: 开启线程时向其传递的数据,
}); // worker是子线程实例

// 通过EventEmitter监听子线程的事件
worker.on("exit", () => {
  // 当子线程退出时运行的事件
});
worker.on("message", (msg) => {
  // 收到子线程发送的消息时运行的事件
});
worker.postMessage(任意消息); // 父线程向子线程发送任意消息
worker.terminate(); // 退出子线程
​```



​```js
const {
  isMainThread, // 是否是主线程
  parentPort, // 用于与父线程通信的端口
  workerData, // 获取线程启动时传递的数据
  threadId, // 获取线程的唯一编号
} = require("worker_threads");

parentPort.on("message", (msg) => {
  // 当收到父线程发送的消息时，触发的事件
});
parentPort.postMessage(workerData); // 向父线程发送消息
​```


```

