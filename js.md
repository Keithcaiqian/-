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
function debounce(fun,time){
	var timer = null
	return function(){
		clearTimeout(timer)
		timer = setTimeout(()=>{
			fun()
		},time)
	}
}
```

+ 节流

```
function throttle(fun,time){
	var timer = null;
	return function(){
		if(timer){
			return
		}
		timer = setTimeout(()=>{
			fun();
			timer = null
		})
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

