# less

+ 变量

```
@width: 10px;
@height: @width + 10px;

//使用
#header {
  width: @width;
  height: @height;
}
```

+ 混合

    ```
    .bordered {
      border-top: dotted 1px black;
      border-bottom: solid 2px black;
    }
    
    //使用
    .post a {
      color: red;
      .bordered();
    }
    ```

+ 嵌套语法

```
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

+ 函数

```
.p_style(@p_h,@p_font,@p_clr,@p_align,@mar,@pad) {
  height: @p_h;
  line-height: @p_h;
  margin: @mar;
  padding: @pad;
  font-size: @p_font;
  color: @p_clr;
  text-align: @p_align;
}

//使用
.sec_tit {
   clear: both;
   .p_style(40px,@font24,#707070,left,none,none);
}
```



+ &符号

```
//header加active时的样式
#header {
  &.active{
	color:red;
  }
  color: black;
}
```

+ calc

    + 为了与css保持兼容

    ```
    width : calc(~"100% - 30px");
    ```

+ 导入

```
@import "library"; // library.less，less文件可以省略后缀
@import "typo.css";//导入的时css
```



# scss

+ 用法与less基本一致
+ 变量采用$符号

```
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

+ 导入

```
// _base.scss文件
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```
// styles.scss
@use 'base';

.inverse {
  background-color: base.$primary-color;
  color: white;
}
```


# css

## 单行省略

```
div{
	overflow: hidden;
    text-overflow:ellipsis;
    white-space: nowrap;
}
```

## 隐藏元素

```
 opacity:0; z-index=-1;这样就可使隐藏这个没用的容器。
```

