

# UI框架

```
https://ng.ant.design/components/switch/zh
```

# 安装脚手架

```
npm install -g @angular/cli //安装一次过后就不需要在安装了
```

# 创建项目

```
ng new my-app
```

# 运行应用

Angular CLI 中包含一个服务器，方便你在本地构建和提供应用。

1. 导航到 workspace 文件夹，比如 `my-app`。
2. 运行下列命令：

```
cd my-app
ng serve --open
```

`ng serve` 命令会启动开发服务器、监视文件，并在这些文件发生更改时重建应用。

`--open`（或者只用 `-o` 缩写）选项会自动打开你的浏览器，并访问 `http://localhost:4200/`。

# 创建组件

npm中运行命令，创建完成后会自动在app.modules.ts中引入注册这个组件

```
ng g component components/home //在app文件下创建components文件(如果没有这个文件)，并创建home组件
```

![image-20200705171254774](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200705171254774.png)

新的组建在根组件中使用，也就是app.component.html

![image-20200705171640934](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200705171640934.png)



# 定义绑定改变数据

在ts文件中建数据，在html中使用

+ ts文件中

```
export class HomeComponent implements OnInit {
  name = '李'  //这个表示public
  private sex:string = '男' //string类型
  public age:any = 19  //任意类型
  public arr:Array<string>=['1','2','3'] //数组中每项必须为string类型

  //  声明属性的几种方式
  // public    共有       可以在这个类里面，和类外面使用
  // protected 保护类型    他只有在当前类和它的子类里面可以使用
  // private   私有       只有当前类才可以访问这个属性

}
```

+ html中

```
<p>{{name}}</p>
<p>{{age}}</p>
<p>{{sex}}</p>
```

+ 在constructor中改变打印值

```
  name = '李'
  constructor() { 
    console.log(this.name)
    this.name = '刘'
  }
```

# 引入图片

+ 本地图片

```
<img src="assets/image/s2.png" alt=""> //直接写assets/
```

+ 动态加载图片

```
<img [src]="imgUrl" alt=""> //imgUrl在ts中定义了
```



# 数据循环

+ *ngFor 普通循环

```
<li *ngFor="let item of arr">
    {{item}}
</li>
```

+ 数组的索引

```
<li *ngFor="let item of arr;let key=index">
    {{key}}:{{item.name}}
</li>
```

# 条件判断

+ *ngIf，没有else指令

```
<div *ngIf="flag">1</div>
<div *ngIf="!flag">2</div>
```

+ *ngSwitch

```
<div [ngSwitch]="pay">
    <div *ngSwitchCase="1">已经支付</div>
    <div *ngSwitchCase="2">未支付</div>
    <div *ngSwitchDefault>不清楚</div>
</div>
```

+ hidden

```
<div [hidden]="flag">1</div>
```



# class

+ ngClass

```
<div [ngClass]="{'blue': flag,'red':!flag}">
    ngClass演示
</div>

//css
.blue{
    color: blue;
}
.red{
    color: red;
}
//ts
flag = false
```

# 管道

+ 转换时间

```
<div>
    {{today | date:'yyyy-MM-dd HH:mm ss'}}
</div>

//ts
today = new Date()
```

# 事件

+ click

```
<button (click)='bindEvent()'>点击</button>

//ts
ngOnInit(): void {
}
bindEvent(){
  alert('执行方法')
}
```

+ 改变数据

```
<button (click)='changeData()'>改变支付状态</button>
//ts
pay = 1
changeData(){
	this.pay = 2
}
```

+ 表单事件

```
<input type="text" (input)='listenKey($event)'>
//ts
listenKey(e){
    console.log(e,e.target) //e.target获取dom节点
}
```

# 双向数据绑定---（MVVM)

+ 只针对表单  --- 需要引入FormsModule

```
//app.module.ts
import { FormsModule } from '@angular/forms';
imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule
 ],
 
//html
<input type="text" [(ngModel)]="num">
{{num}}
<button (click)='changeNum()'>改变num的值</button>
//ts
num=1
changeNum(){
    this.num = +this.num + 1
}
```

# 创建服务

+ 服务相当于公共方法

```
ng g service services/storage  //在app文件下创建services文件(如果没有)，然后创建storage服务
```

+ app.module.ts引入配置服务

```
import { StorageService } from './services/storage.service'

providers: [StorageService]
```

+ storage.service.ts服务文件中

```
constructor() { }

get(){
	return 'this is a service'
}
```

+ 哪个组件使用在哪个组件ts中引入

```
import { StorageService } from '../../services/storage.service'

constructor(public storage:StorageService) { 
    let s = storage.get()
    console.log(s)
}
```

+ service应用举例

```
  set(key:string,value:any){
    localStorage.setItem(key,JSON.stringify(value))
  }

  get(key:string){
    return JSON.parse(localStorage.getItem(key))
  }

  remove(key:string){
    localStorage.removeItem(key)
  }
```

# 获取dom节点

+ ngOnInit在组件和指令初始化完成后调用，并不真正的dom加载完成
+ ngAfterViewInit 视图加载完成后触发的方法(建议把获取dom节点放在这个函数中)

```
ngAfterViewInit(){
    let dom:any = document.getElementById('app');
}
```

+ 自定义方法中直接用原生方式获取

```
let dom:any = document.getElementById('app');
```



+ ViewChild

```
//html
<div #mybox>dom节点</div> //给dom起名字
//ts
import { Component, OnInit,ViewChild } from '@angular/core';//引入ViewChild
@ViewChild('mybox') myBox:any; //通过装饰器来获取这个dom节点
//在这个生命周期中获取并使用
ngAfterViewInit(){
    this.myBox.nativeElement.style.width = '100px';
    this.myBox.nativeElement.style.height = '100px';
    this.myBox.nativeElement.style.backgroundColor = 'red';
}
```

# 获取子组件

+ ViewChild

    + header.component.ts文件 子组件

    ```
    printName(){
        console.log('我是header组件')
    }
    ```

    + home.component.html 父组件

    ```
    <app-header #header></app-header>
    ```

    + home.component.ts 父组件

    ```
    import { Component, OnInit,ViewChild } from '@angular/core';
    @ViewChild('header') header:any;
    
    ngAfterViewInit(){
        this.header.printName() //不仅可以在这个生命周期用，也可以用在自定义方法
    }
    ```

# 父子组件中的通信

+ 子组件使用父组件中的数据和方法

    + 父组件中

    ```
    //html
    <app-header [title]='title' [run]='run' [home]='this'></app-header>
    //ts
    title = '我是home页的头部'
    run(){
    	console.log(run)
    }
    ```

    + 子组件中

    ```
    //html
    <p>{{title}}</p>
    //ts
    import { Component, OnInit , Input } from '@angular/core';//引入input装饰器
    @Input() title:any;//获取title
    @Input() run:any;//获取父组件方法
    @Input() home:any;//获取整个父组件
    
    usrParentFunction(){
    	this.run();//执行父组件的方法
    	this.home.run();
    }
    ```

+ 父组件使用子组件的数据

    + 可以用ViewChild  也可以用Output,EventEmitter

# 非父子组件的通信

+ 可以使用服务
+ 使用localStorage

# 生命周期函数

```
ngOnChanges()
```

当 Angular 设置或重新设置数据绑定的输入属性时响应。 该方法接受当前和上一属性值的 `SimpleChanges` 对象

注意，这发生的非常频繁，所以你在这里执行的任何操作都会显著影响性能。 欲知详情，参见本文档的[使用变更检测钩子](https://angular.cn/guide/lifecycle-hooks#onchanges)。

在 `ngOnInit()` 之前以及所绑定的一个或多个输入属性的值发生变化时都会调用。

```
ngOnInit()  //请求一般在这个中调用
```

在 Angular 第一次显示数据绑定和设置指令/组件的输入属性之后，初始化指令/组件。 欲知详情，参见本文档中的[初始化组件或指令](https://angular.cn/guide/lifecycle-hooks#oninit)。

在第一轮 `ngOnChanges()` 完成之后调用，只调用**一次**。

```
ngDoCheck()
```

检测，并在发生 Angular 无法或不愿意自己检测的变化时作出反应。 欲知详情和范例，参见本文档中的[自定义变更检测](https://angular.cn/guide/lifecycle-hooks#docheck)。

紧跟在每次执行变更检测时的 `ngOnChanges()` 和 首次执行变更检测时的 `ngOnInit()` 后调用。

```
ngAfterContentInit()
```

当 Angular 把外部内容投影进组件视图或指令所在的视图之后调用。

欲知详情和范例，参见本文档中的[响应内容中的变更](https://angular.cn/guide/lifecycle-hooks#aftercontent)。

第一次 `ngDoCheck()` 之后调用，只调用一次。

```
ngAfterContentChecked()
```

每当 Angular 检查完被投影到组件或指令中的内容之后调用。

欲知详情和范例，参见本文档中的[响应被投影内容的变更](https://angular.cn/guide/lifecycle-hooks#aftercontent)。

`ngAfterContentInit()` 和每次 `ngDoCheck()` 之后调用

```
ngAfterViewInit()
```

当 Angular 初始化完组件视图及其子视图或包含该指令的视图之后调用。

欲知详情和范例，参见本文档中的[响应视图变更](https://angular.cn/guide/lifecycle-hooks#afterview)。

第一次 `ngAfterContentChecked()` 之后调用，只调用一次。

```
ngAfterViewChecked()
```

每当 Angular 做完组件视图和子视图或包含该指令的视图的变更检测之后调用。

`ngAfterViewInit()` 和每次 `ngAfterContentChecked()` 之后调用。

```
ngOnDestroy()
```

每当 Angular 每次销毁指令/组件之前调用并清扫。 在这儿反订阅可观察对象和分离事件处理器，以防内存泄漏。 欲知详情，参见本文档中的[在实例销毁时进行清理](https://angular.cn/guide/lifecycle-hooks#ondestroy)。

在 Angular 销毁指令或组件之前立即调用。

# Rxjs

## 先创建服务，在服务中使用Rxjs

+ 1、引入  request.service.ts中，并定义方法

```
import {Observable} from 'rxjs';
export class RequestService {

  constructor() { }

  getRxjsData(){
    return new Observable(observer =>{
      setTimeout(()=>{
        let name = 'li'
        observer.next(name); //处理成功的数据
        observer.error('失败的数据') //处理失败的数据
      },1000)
    })
  }
}
```

+ 2、使用  home.component.ts中

```
import { RequestService } from '../../services/request.service'; //引入服务
 constructor(public storage:StorageService,public rxjs:RequestService) { }//定义服务
 ngOnInit(): void {
    let RxjsResult = this.rxjs.getRxjsData();
    RxjsResult.subscribe(data=>{ 
      console.log(data)
    })
  }
```

## 取消订阅

Rxjs和promise相似，但可以撤回操作，比promise更强大

```
ngOnInit(): void {
    let RxjsResult = this.rxjs.getRxjsData();
    let d = RxjsResult.subscribe(data=>{
      console.log(data)
    })
    setTimeout(()=>{
      d.unsubscribe() //取消订阅
    },500)
}
```

## 执行多次

promise只能执行一次。Rxjs可以执行多次

```
getRxjsData(){
    let num:any = 0;
    return new Observable(observer =>{
      setInterval(()=>{
        num++;
        observer.next(num);
      },1000)
    })
  }
```

```
ngOnInit(): void {
    let RxjsResult = this.rxjs.getRxjsData();
    RxjsResult.subscribe(data=>{
      console.log(data)
    })
  }
```

## 使用工具方法filter和map

```
//在使用的外部ts中引入
import {filter,map} from 'rxjs/operators'；
ngOnInit(): void {
    let RxjsResult = this.rxjs.getRxjsData();
    RxjsResult.pipe(
      filter((item:any)=>{
        return item % 2==0
      }),
      map((item:any)=>{
        return item * item
      })
    )
    .subscribe(data=>{
      console.log(data)
    })
  }
```

# 网络请求

+ 引入

app.module.ts中

```
import {HttpClientModule} from '@angular/common/http'; //引入
imports: [
    HttpClientModule
 ],
```

+ 要使用请求的ts中

```
//当作一个服务,httpheader是post请求，get请求不需要
import { HttpClient,HttpHeaders } from '@angular/common/http' 
constructor(public http:HttpClient) { }

  // get请求
  getData(){
    let api = 'http://a.itying.com/api/productlist';
    this.http.get(api).subscribe( res =>{
      console.log(res)
    })

  }
  // post请求
  postData(){
    //手动设置请求类型
    let httpOptions = {headers: new HttpHeaders({
      'Content-type':'application/json'
    })}
    let api = 'http://a.itying.com/api/productlist';
    this.http.post(api,{'name':'zhangsan'},httpOptions).subscribe( res =>{
      console.log(res)
    })
  }
```

+ jsonp请求解决跨域

app.module.ts中

```
import {HttpClientModule,HttpClientJsonpModule} from '@angular/common/http'; //引入
imports: [
    HttpClientModule,
    HttpClientJsonpModule
 ],
```

请求的ts中

```
  getData(){
    //jsonp请求服务器必须支持jsonp,用下面的方法验证接口是否支持jsonp
    /**
     * http://a.itying.com/api/productlist?callback=xxx;
     * http://a.itying.com/api/productlist?cb=xxx;
     */
    // 如果支持会返回一堆数据，例如:xxx({"result":[{"_id":"5ac0896ca880f20358495508"}]});
    let api = 'http://a.itying.com/api/productlist';
    this.http.jsonp(api,'callback').subscribe( res =>{
      console.log(res)
    })

  }
```

+ 也可以用axios请求

    + 封装axios

    ```
    axiosGet(api){
        return new Promise((resolve,reject)=>{
          axios.get(api)
          .then(res=>{
            resolve(res)
          })
        })
    }
    ```

    