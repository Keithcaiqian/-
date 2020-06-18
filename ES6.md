# promise

一、
1、promise图解
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504181009217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25ld2JpZWs=,size_16,color_FFFFFF,t_70)
2、代码示例
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504181109203.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25ld2JpZWs=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504181230710.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25ld2JpZWs=,size_16,color_FFFFFF,t_70)
二、封装promiseAPI
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200504182015937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L25ld2JpZWs=,size_16,color_FFFFFF,t_70)

# async和await

+ 例子

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

