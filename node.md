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

