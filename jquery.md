# 常用api

+ 用$包起来的原生dom将变为jquery对象

+ get 在本身查找，获得原生dom,

```
get(0)    get(odd)  get(even) //odd奇数  even偶数
```

+ eq 在本身查找

```
eq(0) eq(odd)  eq(even)
```

+ find在下级查找，可以往下多级
+ filter 在本身过滤符合条件的
+ not 与filter功能相反
+ is 判断是否是相同元素，返回true或false

```
$(e.target).is('li')
```

+ has 有这个子元素的元素

```
$("p").has("span")  //返回拥有一个 <span> 元素在其内的所有 <p> 元素：
```

+ add 和

```
$("p").add("span").css("backgroundColor","red");p和span加css样式
```

+ end  回退操作
+ css   改变样式

```
1、$('.demo').css('width','100px')
2、$('.demo').css({
	width:'100px',height:'100px'
})
3、$('.demo').css({width:'+=100px'})
```

+ attr、prop 给属性取值赋值（prop只能给天生的属性赋值）

```
$('.demo').attr('class')     $('.demo').attr('class','test')
$('input').prop('checked')    $('input').prop('checked',true) //主要用来selected、checked等
```

+ html  给dom中重新添加内容，原有内容被覆盖  ， 取值  ----可以是节点

```
$('.demo').html('<span>hello world</span>')   //赋值
let content = $('.demo').html()  //取值
```

+ test 文本  给dom中重新添加内容，原有内容被覆盖  ， 取值 ----只能是文本

```
$('.demo').test('hello world')   //赋值
let content = $('.demo').test()  //取值
```

+ addClass 添加类名

```
$('.demo').addClass('active')
```

+ removeClass 移除类名

```
$('.demo').removeClass('active')
```

+ hasClass  是否有这个类名，返回true或false

```
$('.demo').has('active')
```

+ val 赋值、获取表单元素的val值

```
$('input').val() //取值
$('input').val('hello') //赋值
```

+ next 下一个兄弟元素节点
+ prev 上一个兄弟元素节点
+ nextAll  下边一群兄弟元素节点
+ prevAll  上边一群兄弟元素节点
+ siblings 除了自己外的所有兄弟元素节点
+ parent 上一级
+ parents 上面所有层级的父级
+ closest 找离自己最近的父级，包括自己
+ offsetParent 找离自己最近的有定位的父级
+ slice 截取

```
$('li').clice(1,5) 截取第一位到第五位返回jq对象
```

+ insertBefore

```
$('.demo').insertBefore('.wrap') //demo插到wrap的前面
```

+ before 

```
$('.demo').before ($('.wrap')) //wrap在demo前面
```

+ insertAfter   与insertBefore相反
+ after    与before相反
+ appendTo  

```
$('.demo').appendTo('.wrap')  //demo加到wrap中
```

+ append

```
$('.demo').append($('.wrap'))  //wrap加到demo中
```

+ prependTo与appendTo相反
+ prepend与append相反
+ remove 移除dom

```
$('.demo').remove()  删除demo
```

+ data 赋值、取值

```
$('.demo').data('id')  //取data-id的值
$('.demo').data('id','123')  //给data-id赋值
```

+ show、hide  显示隐藏元素  改变的是display为none或原本的值