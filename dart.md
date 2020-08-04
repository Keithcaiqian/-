Dart

## 前置工作

+ 使用vscode必须先安装Flutter和Dart插件、Code Runner；
+ dart SDK下载地址  https://gekorm.com/dart-windows/

```
C:\Users\k>dart --version
Dart VM version: 2.8.4 (stable) (Wed Jun 3 12:26:04 2020 +0200) on "windows_x64"
```

+ demo演示

![image-20200729111336841](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200729111336841.png)

## 语法

+ 入口方法（两种）

```
main() {
  print('hello word');
}
//表示main方法没有返回值
void main() {
  print('hello word');
}
```

## 定义变量

```
main() {
  //var定义的变量，会被自动推断类型
  var str = 'nfkd';
  var num = 123;
  print(str);
  print(num);
  // 字符串
  String st = 'fsdf';
  print(st);
  //数字类型
  int number = 1234;
  number=324324;
  print(number);
  //var\String\int都是变量，可以改变值

  // var myStr = '';
  // myStr = 1234;
  // print(myStr);
  //会报错定义的时候会推断为string类型，然后赋值number类型，所有会报错
}
```

## 常量(不可以修改)

```
const PI = 3.1415;
final PI = 3.1415;

区别：final可以开始不赋值 只能赋值一次；final不仅有const的编译时常量的特性，最重要的它是运行时的常量，
并且final是惰性初始化，即在运行时第一次使用前才初始化
final a=new DateTime.now();
const a=new DateTime.now(); //会报错
```

## 数据类型

+ string字符串

    + 三个点可以显示原格式

    ![image-20200730160039926](C:\Users\k\AppData\Roaming\Typora\typora-user-images\image-20200730160039926.png)

    + 拼接

    ```
      String str1 = '你好';
      String str2 = 'world';
      print('$str1$str2');
      print(str1 + str2);
    ```

+ int整型，不可以赋值为浮点型

```
int a= 123;
```

+ double浮点型，可以赋值为整型

```
double a =12.1;
a = 12; //12.0
```

+ 运算符 + - * / %
+ boolean布尔类型

```
bool flag = true;
print(flag);
```

+ if()else{}和js中一样，注意dart中没有类型转化
+ list(数组/集合)

```
    //1、第一种定义List的方式
      var l1=['aaa','bbbb','cccc'];
      print(l1);
      print(l1.length);
      print(l1[1]);

    //2、第二种定义List的方式
    // var l2=new List();

    // l2.add('张三');
    // l2.add('李四');
    // l2.add('王五');

    // print(l2);
    // print(l2[2]);

    //3、定义List指定类型

    var l3=new List<String>(); //数组中全为对象
    l3.add('张三');

    print(l3);
```

+ map 相当于js中的json对象

```
  //第一种定义 Maps的方式

    var person={
      "name":"张三",
      "age":20,
      "work":["程序员","送外卖"]
    };

    print(person);

    print(person["name"]);

    print(person["age"]);

    print(person["work"]);

   //第二种定义 Maps的方式

    var p=new Map();

    p["name"]="李四";
    p["age"]=22;
    p["work"]=["程序员","送外卖"];
    print(p);

    print(p["age"]);
```

+ is判断数据类型

```
  var str='1234';

  if(str is String){
    print('是string类型');
  }else if(str is int){

     print('int');
  }else{
     print('其他类型');
  }
```

## 运算符

+ 算数运算符 +- * / %

```
  int a=13;
  int b=5;

  print(a+b);   //加
  print(a-b);   //减
  print(a*b);   //乘
  print(a/b);   //除
  print(a%b);   //其余
  print(a~/b);  //取整

  var c=a*b;
  print(c);
```

+ 关系运算符 ==  ！=  >  <  >=  <=

```
  int a=5;
  int b=3;

  print(a==b);   //判断是否相等
  print(a!=b);   //判断是否不等
  print(a>b);   //判断是否大于
  print(a<b);   //判断是否小于
  print(a>=b);   //判断是否大于等于
  print(a<=b);   //判断是否小于等于
```

+ 逻辑运算符

```
  /* ! 取反 */ 

  // bool flag=false;
  // print(!flag);   //取反



 /* &&并且:全部为true的话值为true 否则值为false */ 

  // bool a=true;
  // bool b=true;

  // print(a && b);


 /* ||或者：全为false的话值为false 否则值为true */ 

  // bool a=false;
  // bool b=false;

  // print(a || b);
```

+ 赋值运算符

```
//  1、基础赋值运算符   =   ??=      
        // int a=10;
        // int b=3;
        // print(a);

        // int c=a+b;   //从右向左


    // b??=23;  表示如果b为空的话把 23赋值给b
        
        // int b=6;
        // b??=23;
        // print(b);
      
        // int b;
        // b??=23;
        // print(b);


//2、  复合赋值运算符   +=  -=  *=   /=   %=  ~/=

    // var a=12;
    // a=a+10;
    // print(a);

    // var a=13;
    // a+=10;   //表示a=a+10
    // print(a);

   var a=4;

   a*=3;  //a=a*3;

   print(a);
```

+ 条件表达式

```
  //1、if  else   switch case 
    // bool flag=true;

    // if(flag){
    //   print('true');
    // }else{
    //   print('false');
    // }

  //判断一个人的成绩 如果大于60 显示及格   如果大于 70显示良好  如果大于90显示优秀

  // var score=41;
  // if(score>90){
  //   print('优秀');
  // }else if(score>70){
  //    print('良好');
  // }else if(score>=60){
  //   print('及格');
  // }else{
  //   print('不及格');
  // }

  // var sex="女";
  // switch(sex){
  //   case "男":
  //     print('性别是男');
  //     break;
  //   case "女":
  //     print('性别是女');
  //     print('性别是女');
  //     break;
  //   default:
  //     print('传入参数错误');
  //     break;
  // }

  //2、三目运算符 

  // var falg=true;
  // var c;
  // if(falg){
  //     c='我是true';
  // }else{
  //   c="我是false";
  // }
  // print(c);

  bool flag=false;
  String c=flag?'我是true':'我是false';
  print(c);
   
  //3  ??运算符

  // var a;
  // var b= a ?? 10;
  // print(b);   10

  var a=22;
  var b= a ?? 10;
  print(b);
```

+ ++ -- 自增，自减

```
  /*
     ++  --   表示自增 自减 1

    在赋值运算里面 如果++ -- 写在前面 这时候先运算 再赋值，如果++ --写在后面 先赋值后运行运算


    var a=10;
    var b=a--;

    print(a);  //9
    print(b);  //10
  
  */


    // var a=10;

    // a++;   //a=a+1;

    // print(a);



    // var a=10;
    // a--;    //a=a-1;
    // print(a);




    // var a=10;
    // var b=a++;

    // print(a);  //11
    // print(b);  //10



    // var a=10;
    // var b=++a;

    // print(a);  //11
    // print(b);  //11



    
    // var a=10;
    // var b=--a;

    // print(a);  //9
    // print(b);  //9



    // var a=10;
    // var b=a--;

    // print(a);  //9
    // print(b);  //10



    var a=10;

    ++a;

    print(a);
```



## 类型转化

```
 //1、Number与String类型之间的转换

      // Number类型转换成String类型 toString()

      // String类型转成Number类型  int.parse()


      // String str='123';

      // var myNum=int.parse(str);

      // print(myNum is int);


      // String str='123.1';

      // var myNum=double.parse(str);

      // print(myNum is double);


      //  String price='12';

      // var myNum=double.parse(price);

      // print(myNum);

      // print(myNum is double);


      //报错
      // String price='';

      // var myNum=double.parse(price);

      // print(myNum);

      // print(myNum is double);



    // try  ... catch
    //  String price='';
    //   try{
    //     var myNum=double.parse(price);

    //     print(myNum);

    //   }catch(err){
    //        print(0);
    //   } 


    // var myNum=12;

    // var str=myNum.toString();

    // print(str is String);
 
  
 // 2、其他类型转换成Booleans类型

        // isEmpty:判断字符串是否为空

        
        // var str='';
        // if(str.isEmpty){
        //   print('str空');
        // }else{
        //   print('str不为空');
        // }


        // var myNum=123;

        // if(myNum==0){
        //    print('0');
        // }else{
        //   print('非0');
        // }


        // var myNum;

        // if(myNum==0){
        //    print('0');
        // }else{
        //   print('非0');
        // }



        // var myNum;
        // if(myNum==null){
        //    print('空');
        // }else{
        //   print('非空');
        // }


        var myNum=0/0;
        
        // print(myNum);

        if(myNum.isNaN){
          print('NaN');
        }
```

## for循环

```
// for基本语法
          for (int i = 1; i<=100; i++) {   
            print(i);
          }

        	//第一步，声明变量int i = 1;
        	//第二步，判断i <=100
        	//第三步，print(i);
        	//第四步，i++
        	//第五步 从第二步再来，直到判断为false

```

## while  do while 

```
/*
	语法格式:
		
		while(表达式/循环条件){			
			
		}	
		
    		
		do{
			语句/循环体
			
		}while(表达式/循环条件);
		
				

		注意： 1、最后的分号不要忘记
			    2、循环条件中使用的变量需要经过初始化
		      3、循环体中，应有结束循环的条件，否则会造成死循环。
*/

// int i=1;
    // var sum=0;
    // while(i<=100){
    //    sum+=i;
    //    i++;
    // }
    // print(sum);
    
// int i=1;
    // var sum=0;
    // do{
    //    sum+=i;
    //    i++;
    // }while(i<=100);
    // print(sum);
    
//while 和 do while的区别   第一次循环条件不成立的情况下，while不执行，do while执行一次
```

## break continue

```
/*
			break语句功能:
          1、在switch语句中使流程跳出switch结构。
          2、在循环语句中使流程跳出当前循环,遇到break 循环终止，后面代码也不会执行
          
          强调:
          1、如果在循环中已经执行了break语句,就不会执行循环体中位于break后的语句。
          2、在多层循环中,一个break语句只能向外跳出一层

        break可以用在switch case中 也可以用在 for 循环和 while循环中

      continue语句的功能:
			  
        【注】只能在循环语句中使用,使本次循环结束，即跳过循环体重下面尚未执行的语句，接着进行下次的是否执行循环的判断。

        continue可以用在for循环以及 while循环中，但是不建议用在while循环中，不小心容易死循环


*/
```

## list

```
/*
List里面常用的属性和方法：

    常用属性：
        length          长度
        reversed        翻转
        isEmpty         是否为空
        isNotEmpty      是否不为空
    常用方法：  
        add         增加
        addAll      拼接数组
        indexOf     查找  传入具体值
        remove      删除  传入具体值
        removeAt    删除  传入索引值
        fillRange   修改   
        insert(index,value);            指定位置插入    
        insertAll(index,list)           指定位置插入List
        toList()    其他类型转换成List  
        join()      List转换成字符串
        split()     字符串转化成List
        forEach   
        map
        where
        any
        every

*/
```

```
//List里面的属性：
    // List myList=['香蕉','苹果','西瓜'];
    // print(myList.length);
    // print(myList.isEmpty);
    // print(myList.isNotEmpty);
    // print(myList.reversed);  //对列表倒序排序
    // var newMyList=myList.reversed.toList();
    // print(newMyList);

//List里面的方法：


    // List myList=['香蕉','苹果','西瓜'];
    //myList.add('桃子');   //增加数据  增加一个

    // myList.addAll(['桃子','葡萄']);  //拼接数组

    // print(myList);

    //print(myList.indexOf('苹x果'));    //indexOf查找数据 查找不到返回-1  查找到返回索引值


    // myList.remove('西瓜');

    // myList.removeAt(1);

    // print(myList);
  



    // List myList=['香蕉','苹果','西瓜'];

    // myList.fillRange(1, 2,'aaa');  //修改

    //  myList.fillRange(1, 3,'aaa');  


    // myList.insert(1,'aaa');      //插入  一个

    // myList.insertAll(1, ['aaa','bbb']);  //插入 多个

    // print(myList);


    // List myList=['香蕉','苹果','西瓜'];

    // var str=myList.join('-');   //list转换成字符串

    // print(str);

    // print(str is String);  //true


    var str='香蕉-苹果-西瓜';

    var list=str.split('-');

    print(list);

    print(list is List);
```

## set

```
//Set 

//用它最主要的功能就是去除数组重复内容

//Set是没有顺序且不能重复的集合，所以不能通过索引去获取值

void main(){

  
  // var s=new Set();
  // s.add('香蕉');
  // s.add('苹果');
  // s.add('苹果');

  // print(s);   //{香蕉, 苹果}

  // print(s.toList()); 


  List myList=['香蕉','苹果','西瓜','香蕉','苹果','香蕉','苹果'];

  var s=new Set();

  s.addAll(myList);

  print(s);

  print(s.toList());


  
}
```

## map

```
/*
  映射(Maps)是无序的键值对：

    常用属性：
        keys            获取所有的key值
        values          获取所有的value值
        isEmpty         是否为空
        isNotEmpty      是否不为空
    常用方法:
        remove(key)     删除指定key的数据
        addAll({...})   合并映射  给映射内增加属性
        containsValue   查看映射内的值  返回true/false
        forEach   
        map
        where
        any
        every


*/

void main(){

 
  // Map person={
  //   "name":"张三",
  //   "age":20
  // };


  // var m=new Map();
  // m["name"]="李四";
  
  // print(person);
  // print(m);

//常用属性：

    // Map person={
    //   "name":"张三",
    //   "age":20,
    //   "sex":"男"
    // };

    // print(person.keys.toList());
    // print(person.values.toList());
    // print(person.isEmpty);
    // print(person.isNotEmpty);


//常用方法：
    Map person={
      "name":"张三",
      "age":20,
      "sex":"男"
    };

    // person.addAll({
    //   "work":['敲代码','送外卖'],
    //   "height":160
    // });

    // print(person);



    // person.remove("sex");
    // print(person);



    print(person.containsValue('张三'));
}
```

## forEach map where any every

```
/*
        forEach     
        map         
        where       
        any
        every
*/
void main(){


      //  List myList=['香蕉','苹果','西瓜'];

      // for(var i=0;i<myList.length;i++){
      //   print(myList[i]);
      // }


      // for(var item in myList){
      //   print(item);
      // }


      // myList.forEach((value){
      //     print("$value");
      // });





      // List myList=[1,3,4];

      // List newList=new List();

      // for(var i=0;i<myList.length;i++){

      //   newList.add(myList[i]*2);
      // }
      // print(newList);





      // List myList=[1,3,4];      
      // var newList=myList.map((value){
      //     return value*2;
      // });
      //  print(newList.toList());





      // List myList=[1,3,4,5,7,8,9];

      // var newList=myList.where((value){
      //     return value>5;
      // });
      // print(newList.toList());



      // List myList=[1,3,4,5,7,8,9];

      // var f=myList.any((value){   //只要集合里面有满足条件的就返回true

      //     return value>5;
      // });
      // print(f);



      // List myList=[1,3,4,5,7,8,9];

      // var f=myList.every((value){   //每一个都满足条件返回true  否则返回false

      //     return value>5;
      // });
      // print(f);






      // set

      // var s=new Set();

      // s.addAll([1,222,333]);

      // s.forEach((value)=>print(value));



      //map

       Map person={
          "name":"张三",
          "age":20
        };

        person.forEach((key,value){            
            print("$key---$value");
        });





}
```

## 方法

+ 方法基本格式

```
/*
  内置方法/函数：

      print();

  自定义方法：
      自定义方法的基本格式：

      返回类型  方法名称（参数1，参数2,...）{
        方法体
        return 返回值;
      }
*/

void printInfo(){
  print('我是一个自定义方法');
}

int getNum(){
  var myNum=123;
  return myNum;
}

String printUserInfo(){

  return 'this is str';
}


List getList(){

  return ['111','2222','333'];
}

void main(){

  // print('调用系统内置的方法');

  // printInfo();
  // var n=getNum();
  // print(n);


  // print(printUserInfo());


  // print(getList());

  // print(getList());
  

//演示方法的作用域
  void xxx(){

      aaa(){

          print(getList());
          print('aaa');
      }
      aaa();
  }

  // aaa();  错误写法 

  xxx();  //调用方法


}
```

+ 方法的使用

```
//调用方法传参

main() {
  

//1、定义一个方法 求1到这个数的所有数的和      60    1+2+3+。。。+60


 /*
    int sumNum(int n){
      var sum=0;
      for(var i=1;i<=n;i++)
      {
        sum+=i;
      }
      return sum;
    } 

    var n1=sumNum(5);
    print(n1);
    var n2=sumNum(100);
    print(n2);

 */
       


//2、定义一个方法然后打印用户信息


    // String printUserInfo(String username,int age){  //行参
    //     return "姓名:$username---年龄:$age";
    // }

    // print(printUserInfo('张三',20)); //实参





//3、定义一个带可选参数的方法
    

    // String printUserInfo(String username,[int age]){  //行参

    //   if(age!=null){
    //     return "姓名:$username---年龄:$age";
    //   }
    //   return "姓名:$username---年龄保密";

    // }

    // // print(printUserInfo('张三',21)); //实参

    // print(printUserInfo('张三'));
   



//4、定义一个带默认参数的方法


    // String printUserInfo(String username,[String sex='男',int age]){  //行参

    //   if(age!=null){
    //     return "姓名:$username---性别:$sex--年龄:$age";
    //   }
    //   return "姓名:$username---性别:$sex--年龄保密";

    // }

  // print(printUserInfo('张三'));

  // print(printUserInfo('小李','女'));

  //  print(printUserInfo('小李','女',30));



//5、定义一个命名参数的方法

  // String printUserInfo(String username,{int age,String sex='男'}){  //行参

  //     if(age!=null){
  //       return "姓名:$username---性别:$sex--年龄:$age";
  //     }
  //     return "姓名:$username---性别:$sex--年龄保密";

  // }

  // print(printUserInfo('张三',age:20,sex:'未知'));



//6、实现一个 把方法当做参数的方法


  // var fn=(){

  //   print('我是一个匿名方法');
  // };      
  // fn();




  //方法
  fn1(){
    print('fn1');
  }

  //方法
  fn2(fn){
    
    fn();
  }

  //调用fn2这个方法 把fn1这个方法当做参数传入
  fn2(fn1);


}
```

+ 匿名方法，自执行方法，递归，闭包

```
//匿名方法

  // var printNum=(){

  //   print(123);
  // };

  // printNum();
  
//自执行方法


    // ((int n){
    //   print(n);
    //   print('我是自执行方法');
    // })(12);
    
//方法的递归
      var sum=1;			
      fn(n){
          sum*=n;
          if(n==1){
            return ;
          }         
          fn(n-1);
          
      }

      fn(5);      
      print(sum);    
      
      
/*
闭包：

    1、全局变量特点:    全局变量常驻内存、全局变量污染全局
    2、局部变量的特点：  不常驻内存会被垃圾机制回收、不会污染全局  


  /*  想实现的功能：

        1.常驻内存        
        2.不污染全局   

          产生了闭包,闭包可以解决这个问题.....  

          闭包: 函数嵌套函数, 内部函数会调用外部函数的变量或参数, 变量或参数不会被系统回收(不会释放内存)
  
	        闭包的写法： 函数嵌套函数，并return 里面的函数，这样就形成了闭包。

    */  
*/

//闭包

  		fn(){
        var a=123;  /*不会污染全局   常驻内存*/
        return(){			
          a++;			
          print(a);
        };        
      }     
      var b=fn();	
      b();
      b();
      b();
    
```

## 类

```
/*

面向对象编程(OOP)的三个基本特征是：封装、继承、多态      

      封装：封装是对象和类概念的主要特性。封装，把客观事物封装成抽象的类，并且把自己的部分属性和方法提供给其他对象调用, 而一部分属性和方法则隐藏。
                
      继承：面向对象编程 (OOP) 语言的一个主要功能就是“继承”。继承是指这样一种能力：它可以使用现有类的功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。
            
      多态：允许将子类类型的指针赋值给父类类型的指针, 同一个函数调用会有不同的执行效果 。


Dart所有的东西都是对象，所有的对象都继承自Object类。

Dart是一门使用类和单继承的面向对象语言，所有的对象都是类的实例，并且所有的类都是Object的子类

一个类通常由属性和方法组成。

*/
```

### 创建类 使用类

```
//类必须要放在main函数外面
class Person{
  String name="张三";
  int age=23;
  void getInfo(){
      // print("$name----$age");
      print("${this.name}----${this.age}");
  }
  void setInfo(int age){
    this.age=age;
  }

}
void main(){

  //实例化

  // var p1=new Person();
  // print(p1.name);
  // p1.getInfo();

  Person p1=new Person();
  // print(p1.name);

  p1.setInfo(28);
  p1.getInfo();
}
```

### 默认构造函数

```
//不传参
class Person{
  String name='张三';
  int age=20; 
  //默认构造函数
  Person(){
    print('这是构造函数里面的内容  这个方法在实例化的时候触发');
  }
  void printInfo(){   
    print("${this.name}----${this.age}");
  }
}

//传参
class Person{
  String name;
  int age; 
  //默认构造函数
  Person(String name,int age){
      this.name=name;
      this.age=age;
  }
  void printInfo(){   
    print("${this.name}----${this.age}");
  }
}
//传参简化
class Person{
  String name;
  int age; 
  //默认构造函数的简写
  Person(this.name,this.age);
  void printInfo(){   
    print("${this.name}----${this.age}");
  }
}
//使用
void main(){
  
  Person p1=new Person('张三',20);
  p1.printInfo();


  Person p2=new Person('李四',25);
  p2.printInfo();

}
```

### 命名构造函数

```
/*
dart里面构造函数可以写多个
*/
class Person{
  String name;
  int age; 
  //默认构造函数的简写
  Person(this.name,this.age);
  //构造函数
  Person.now(){
    print('我是命名构造函数');
  }
  //构造函数
  Person.setInfo(String name,int age){
    this.name=name;
    this.age=age;
  }

  void printInfo(){   
    print("${this.name}----${this.age}");
  }
}

void main(){
  // var d=new DateTime.now();   //实例化DateTime调用它的命名构造函数
  // print(d);

  //Person p1=new Person('张三', 20);   //默认实例化类的时候调用的是 默认构造函数

  //Person p1=new Person.now();   //命名构造函数

  Person p1=new Person.setInfo('李四',30);
  p1.printInfo(); 
}
```

### 把类单独抽离成一个模块

+ person.dart文件

```
class Person{
  String name;
  int age; 
  //默认构造函数的简写
  Person(this.name,this.age);
  
  Person.now(){
    print('我是命名构造函数');
  }

  Person.setInfo(String name,int age){
    this.name=name;
    this.age=age;
  }

  void printInfo(){   
    print("${this.name}----${this.age}");
  }
}
```

+ 使用的dart文件中

```
import 'lib/Person.dart';

void main(){

  Person p1=new Person.setInfo('李四1',30);
  p1.printInfo(); 

}
```

### dart类中的私有方法和私有属性

使用私有方法或属性，必须单独抽离成一个模块才生效

+ Animal.dart文件

```
class Animal{
  String _name;   //私有属性
  int age; 
  //默认构造函数的简写
  Animal(this._name,this.age);

  void printInfo(){   
    print("${this._name}----${this.age}");
  }

  String getName(){ 
    return this._name;
  } 
  void _run(){
    print('这是一个私有方法');
  }

  execRun(){
    this._run();  //类里面方法的相互调用
  }
}
```

+ 使用的dart文件中

```
/*
Dart和其他面向对象语言不一样，Data中没有 public  private protected这些访问修饰符合

但是我们可以使用_把一个属性或者方法定义成私有。

*/

import 'lib/Animal.dart';

void main(){
 
 Animal a=new Animal('小狗', 3);

 print(a.getName());

  a.execRun();   //间接的调用私有方法

}
```

### getter和setter修饰符的用法

```
class Rect{
  num height;
  num width; 
  
  Rect(this.height,this.width);
  get area{
    return this.height*this.width;
  }
  set areaHeight(value){
    this.height=value;
  }
}

void main(){
  Rect r=new Rect(10,4);
  // print("面积:${r.area()}");   
  r.areaHeight=6;

  print(r.area);
}
```

### 类中的初始化列表

```
// Dart中我们也可以在构造函数体运行之前初始化实例变量

class Rect{

  int height;
  int width;
  Rect():height=2,width=10{
    
    print("${this.height}---${this.width}");
  }
  getArea(){
    return this.height*this.width;
  } 
}

void main(){
  Rect r=new Rect();
  print(r.getArea()); 
   
}
```

### 类中静态方法和静态成员

```
/*
Dart中的静态成员:

  1、使用static 关键字来实现类级别的变量和函数

  2、静态方法不能访问非静态成员，非静态方法可以访问静态成员

*/
class Person {
  static String name = '张三';
  int age=20;
  
  static void show() {
    print(name);
  }
  void printInfo(){  /*非静态方法可以访问静态成员以及非静态成员*/

      // print(name);  //访问静态属性
      // print(this.age);  //访问非静态属性

      show();   //调用静态方法
  }
  static void printUserInfo(){//静态方法

        print(name);   //静态属性
        show();        //静态方法

        //print(this.age);     //静态方法没法访问非静态的属性

        // this.printInfo();   //静态方法没法访问非静态的方法
        // printInfo();

  }

}
```

### 对象操作符

```
/*
Dart中的对象操作符:

    ?     条件运算符 （了解）
    as    类型转换
    is    类型判断
    ..    级联操作 （连缀）  (记住)
*/

class Person {
  String name;
  num age;
  Person(this.name,this.age);
  void printInfo() {
    print("${this.name}---${this.age}");  
  }
  
}

main(){ 

  // Person p;
  // p?.printInfo(); //有就执行，没有就不执行

  //  Person p=new Person('张三', 20);
  //  p?.printInfo();

    // Person p=new Person('张三', 20);
	
    // if(p is Person){
    //     p.name="李四";
    // } 
    // p.printInfo();
    // print(p is Object);

    // var p1;

    // p1='';

    // p1=new Person('张三1', 20);

    // // p1.printInfo();
    // (p1 as Person).printInfo();

    
  //  Person p1=new Person('张三1', 20);

  //  p1.printInfo();

  //  p1.name='张三222';
  //  p1.age=40;
  //  p1.printInfo();



   Person p1=new Person('张三1', 20);

   p1.printInfo();

   p1..name="李四"
     ..age=30
     ..printInfo(); 
}
```

### 继承

```
Dart中的类的继承：  
    1、子类使用extends关键词来继承父类
    2、子类会继承父类里面可见的属性和方法 但是不会继承构造函数
    3、子类能复写父类的方法 getter和setter

*/

class Person {
  String name;
  num age; 
  Person(this.name,this.age);
  void printInfo() {
    print("${this.name}---${this.age}");  
  }
}


class Web extends Person{
  Web(String name, num age) : super(name, age){

  }
  
}

main(){ 

  // Person p=new Person('李四',20);
  // p.printInfo();

  // Person p1=new Person('张三',20);
  // p1.printInfo();


  Web w=new Web('张三', 12);

  w.printInfo();


}
```

```
class Person {
  String name;
  num age; 
  Person(this.name,this.age);
  void printInfo() {
    print("${this.name}---${this.age}");  
  }
}


class Web extends Person{
  String sex;
  //加了自己的属性sex
  Web(String name, num age,String sex) : super(name, age){
    this.sex=sex;
  }
  run(){
   print("${this.name}---${this.age}--${this.sex}");  
  }
  
}

main(){ 

  // Person p=new Person('李四',20);
  // p.printInfo();

  // Person p1=new Person('张三',20);
  // p1.printInfo();


  Web w=new Web('张三', 12,"男");

  w.printInfo();

  w.run();

}
```

### 继承给命名构造函数传参

```
class Person {
  String name;
  num age; 
  Person(this.name,this.age);
  Person.xxx(this.name,this.age);
  void printInfo() {
    print("${this.name}---${this.age}");  
  }
}


class Web extends Person{
  String sex;
  Web(String name, num age,String sex) : super.xxx(name, age){
    this.sex=sex;
  }
  run(){
   print("${this.name}---${this.age}--${this.sex}");  
  }
  
}

main(){ 

  // Person p=new Person('李四',20);
  // p.printInfo();

  // Person p1=new Person('张三',20);
  // p1.printInfo();


  Web w=new Web('张三', 12,"男");

  w.printInfo();

  w.run();

}
```

### 覆写父类的方法

```
class Person {
  String name;
  num age; 
  Person(this.name,this.age);
  void printInfo() {
    print("${this.name}---${this.age}");  
  }
  work(){
    print("${this.name}在工作...");
  }
}

class Web extends Person{
  Web(String name, num age) : super(name, age);

  run(){
    print('run');
  }
  //覆写父类的方法
  @override       //可以写也可以不写  建议在覆写父类方法的时候加上 @override 
  void printInfo(){
     print("姓名：${this.name}---年龄：${this.age}"); 
  }
  @override
  work(){
    print("${this.name}的工作是写代码");
  }

}
```

### 调用父类的方法

```
class Person {
  String name;
  num age; 
  Person(this.name,this.age);
  void printInfo() {
    print("${this.name}---${this.age}");  
  }
  work(){
    print("${this.name}在工作...");
  }
}

class Web extends Person{
  Web(String name, num age) : super(name, age);

  run(){
    print('run');
    super.work();  //自类调用父类的方法
  }
  //覆写父类的方法
  @override       //可以写也可以不写  建议在覆写父类方法的时候加上 @override 
  void printInfo(){
     print("姓名：${this.name}---年龄：${this.age}"); 
  }
  

}


main(){ 

  Web w=new Web('李四',20);

  // w.printInfo();

  w.run();
 
}
```

## 抽象类

+ Dart中抽象类: Dart抽象类主要用于定义标准，子类可以继承抽象类，也可以实现抽象类接口。

     1、抽象类通过abstract 关键字来定义

     2、Dart中的抽象方法不能用abstract声明，Dart中没有方法体的方法我们称为抽象方法。

     3、如果子类继承抽象类必须得实现里面的抽象方法

     4、如果把抽象类当做接口实现的话必须得实现抽象类里面定义的所有属性和方法。

     5、抽象类不能被实例化，只有继承它的子类可以

    

    extends抽象类 和 implements的区别：

     1、如果要复用抽象类里面的方法，并且要用抽象方法约束自类的话我们就用extends继承抽象类

     2、如果只是把抽象类当做标准的话我们就用implements实现抽象类

+ 案例：定义一个Animal 类要求它的子类必须包含eat方法

```
abstract class Animal{
  eat();   //抽象方法
  run();  //抽象方法  
  printInfo(){
    print('我是一个抽象类里面的普通方法');
  }
}

class Dog extends Animal{
  @override
  eat() {
     print('小狗在吃骨头');
  }

  @override
  run() {
    // TODO: implement run
    print('小狗在跑');
  }  
}
class Cat extends Animal{
  @override
  eat() {
    // TODO: implement eat
    print('小猫在吃老鼠');
  }

  @override
  run() {
    // TODO: implement run
    print('小猫在跑');
  }

}

main(){

  Dog d=new Dog();
  d.eat();
  d.printInfo();

   Cat c=new Cat();
  c.eat();

  c.printInfo();


  // Animal a=new Animal();   //抽象类没法直接被实例化

}
```

### 多态

+ Dart中的多态：

    允许将子类类型的指针赋值给父类类型的指针, 同一个函数调用会有不同的执行效果 

    子类的实例赋值给父类的引用。

    多态就是父类定义一个方法不去实现，让继承他的子类去实现，每个子类有不同的表现。

    ```
    abstract class Animal{
      eat();   //抽象方法 
    }
    
    class Dog extends Animal{
      @override
      eat() {
         print('小狗在吃骨头');
      }
      run(){
        print('run');
      }
    }
    class Cat extends Animal{
      @override
      eat() {   
        print('小猫在吃老鼠');
      }
      run(){
        print('run');
      }
    }
    
    main(){
    
      // Dog d=new Dog();
      // d.eat();
      // d.run();
    
    
      // Cat c=new Cat();
      // c.eat();
    
    
      Animal d=new Dog();
    
      d.eat();
    
      Animal c=new Cat();
    
      c.eat();
    
    }
    ```

### 接口

+ 和Java一样，dart也有接口，但是和Java还是有区别的。

     首先，dart的接口没有interface关键字定义接口，而是普通类或抽象类都可以作为接口被实现。

     同样使用implements关键字进行实现。

     但是dart的接口有点奇怪，如果实现的类是普通类，会将普通类和抽象中的属性的方法全部需要覆写一遍。

     而因为抽象类可以定义抽象方法，普通类不可以，所以一般如果要实现像Java接口那样的方式，一般会使用抽象类。

     建议使用抽象类定义接口。

+ ```
    
    /*
    定义一个DB库 支持 mysql  mssql  mongodb
    
    mysql  mssql  mongodb三个类里面都有同样的方法
    
    */
    
    
    abstract class Db{   //当做接口   接口：就是约定 、规范
        String uri;      //数据库的链接地址
        add(String data);
        save();
        delete();
    }
    
    class Mysql implements Db{
      
      @override
      String uri;
    
      Mysql(this.uri);
    
      @override
      add(data) {
        // TODO: implement add
        print('这是mysql的add方法'+data);
      }
    
      @override
      delete() {
        // TODO: implement delete
        return null;
      }
    
      @override
      save() {
        // TODO: implement save
        return null;
      }
    
      remove(){
          
      }
    
      
    }
    
    class MsSql implements Db{
      @override
      String uri;
      @override
      add(String data) {
        print('这是mssql的add方法'+data);
      }
    
      @override
      delete() {
        // TODO: implement delete
        return null;
      }
    
      @override
      save() {
        // TODO: implement save
        return null;
      }
    
    
    }
    
    main() {
    
      Mysql mysql=new Mysql('xxxxxx');
    
      mysql.add('1243214');
    }
    ```

### 接口文件分离

+ Db.dart文件

```
abstract class Db{   //当做接口   接口：就是约定 、规范
    String uri;      //数据库的链接地址
    add(String data);
    save();
    delete();
}
```

+ MsSql.dart文件

```
import 'Db.dart';


class MsSql implements Db{
  @override
  String uri;
  @override
  add(String data) {
    print('这是mssql的add方法'+data);
  }

  @override
  delete() {
    // TODO: implement delete
    return null;
  }

  @override
  save() {
    // TODO: implement save
    return null;
  }


}
```

+ Mysql.dart文件

```
import 'Db.dart';


class Mysql implements Db{
  
  @override
  String uri;

  Mysql(this.uri);

  @override
  add(data) {   
    print('这是mysql的add方法'+data);
  }

  @override
  delete() {   
    return null;
  }

  @override
  save() {   
    return null;
  }

  
}
```

+ 使用

```
// import 'lib/Mysql.dart';
import 'lib/MsSql.dart';

main() {

  // Mysql mysql=new Mysql('xxxxxx');

  // mysql.add('1243214');

  MsSql mssql=new MsSql();
  mssql.uri='127.0.0.1';

  mssql.add('增加的数据');

}
```

###  一个类实现多个接口

```
abstract class A{
  String name;
  printA();
}

abstract class B{
  printB();
}

class C implements A,B{  
  @override
  String name;  
  @override
  printA() {
    print('printA');
  }
  @override
  printB() {
    // TODO: implement printB
    return null;
  }
  
}


void main(){
  C c=new C();
  c.printA();
}
```

## mixins

### 定义

mixins的中文意思是混入，就是在类中混入其他功能。

在Dart中可以使用mixins实现类似多继承的功能

因为mixins使用的条件，随着Dart版本一直在变，这里讲的是Dart2.x中使用mixins的条件：

 1、作为mixins的类只能继承自Object，不能继承其他类

 2、作为mixins的类不能有构造函数

 3、一个类可以mixins多个mixins类

 4、mixins绝不是继承，也不是接口，而是一种全新的特性

### 使用

+ demo1

```
class A {
  String info="this is A";
  void printA(){
    print("A");
  }
}

class B {
  void printB(){
    print("B");
  }
}

class C with A,B{
  
}

void main(){
  
  var c=new C();  
  c.printA();
  c.printB();
  print(c.info);

}
```

+ demo2

```
class Person{
  String name;
  num age;
  Person(this.name,this.age);
  printInfo(){
    print('${this.name}----${this.age}');
  }
  void run(){
    print("Person Run");
  }
}

class A {
  String info="this is A";
  void printA(){
    print("A");
  }
  void run(){
    print("A Run");
  }
}

class B {  
  void printB(){
    print("B");
  }
  void run(){
    print("B Run");
  }
}

class C extends Person with B,A{
  C(String name, num age) : super(name, age);
  
}

void main(){  
  var c=new C('张三',20);  
  c.printInfo();
  // c.printB();
  // print(c.info);

   c.run(); //打印出最后一个超类的run方法执行的结果


}
```

+ demo3

```
class A {
  String info="this is A";
  void printA(){
    print("A");
  }
}

class B {
  void printB(){
    print("B");
  }
}

class C with A,B{
  
}

void main(){  
   var c=new C();  
   
  print(c is C);    //true
  print(c is A);    //true
  print(c is B);   //true


  // var a=new A();

  // print(a is Object);

}
```

## 泛型

### 泛型方法

```
/*

通俗理解：泛型就是解决 类 接口 方法的复用性、以及对不特定数据类型的支持(类型校验)

*/


//只能返回string类型的数据

  // String getData(String value){
  //     return value;
  // }
  

//同时支持返回 string类型 和int类型  （代码冗余）


  // String getData1(String value){
  //     return value;
  // }

  // int getData2(int value){
  //     return value;
  // }



//同时返回 string类型 和number类型       不指定类型可以解决这个问题


  // getData(value){
  //     return value;
  // }





//不指定类型放弃了类型检查。我们现在想实现的是传入什么 返回什么。比如:传入number 类型必须返回number类型  传入 string类型必须返回string类型
 
  //第一个T校验返回类型。第二个校验输入类型，第三个T跟随第二个T的类型
  // T getData<T>(T value){
  //     return value;
  // }

  getData<T>(T value){
      return value;
  }

void main(){

    // print(getData(21));

    // print(getData('xxx'));

    // getData<String>('你好');

    print(getData<int>(12));

}
```

### 泛型类

```
//集合List 泛型类的用法



//案例：把下面类转换成泛型类，要求List里面可以增加int类型的数据，也可以增加String类型的数据。但是每次调用增加的类型要统一


//  class PrintClass{
//       List list=new List<int>();
//       void add(int value){
//           this.list.add(value);
//       }
//       void printInfo(){          
//           for(var i=0;i<this.list.length;i++){
//             print(this.list[i]);
//           }
//       }
//  }

//  PrintClass p=new PrintClass();
//     p.add(1);
//     p.add(12);
//     p.add(5);
//     p.printInfo();




 class PrintClass<T>{
      List list=new List<T>();
      void add(T value){
          this.list.add(value);
      }
      void printInfo(){          
          for(var i=0;i<this.list.length;i++){
            print(this.list[i]);
          }
      }
 }



main() {  
    // PrintClass p=new PrintClass();
    // p.add(11);
    // p.add('xxx');
    // p.add(5);
    // p.printInfo();



  // PrintClass p=new PrintClass<String>();

  // p.add('你好');

  //  p.add('哈哈');

  // p.printInfo();




  PrintClass p=new PrintClass<int>();

  p.add(12);

   p.add(23);

  p.printInfo();








 
  // List list=new List();
  // list.add(12);
  // list.add("你好");
  // print(list);



  // List list=new List<String>();

  // // list.add(12);  //错误的写法

  // list.add('你好');
  // list.add('你好1');

  // print(list);





  // List list=new List<int>();

  // // list.add("你好");  //错误写法
  // list.add(12); 

  // print(list);



}
```

### 泛型接口

```
/*
Dart中的泛型接口:

    实现数据缓存的功能：有文件缓存、和内存缓存。内存缓存和文件缓存按照接口约束实现。

    1、定义一个泛型接口 约束实现它的子类必须有getByKey(key) 和 setByKey(key,value)

    2、要求setByKey的时候的value的类型和实例化子类的时候指定的类型一致


*/


  // abstract class ObjectCache {
  //   getByKey(String key);
  //   void setByKey(String key, Object value);
  // }

  // abstract class StringCache {
  //   getByKey(String key);
  //   void setByKey(String key, String value);
  // }


  // abstract class Cache<T> {
  //   getByKey(String key);
  //   void setByKey(String key, T value);
  // }


abstract class Cache<T>{
  getByKey(String key);
  void setByKey(String key, T value);
}

class FlieCache<T> implements Cache<T>{
  @override
  getByKey(String key) {    
    return null;
  }

  @override
  void setByKey(String key, T value) {
   print("我是文件缓存 把key=${key}  value=${value}的数据写入到了文件中");
  }
}

class MemoryCache<T> implements Cache<T>{
  @override
  getByKey(String key) {   
    return null;
  }

  @override
  void setByKey(String key, T value) {
       print("我是内存缓存 把key=${key}  value=${value} -写入到了内存中");
  }
}
void main(){


    // MemoryCache m=new MemoryCache<String>();

    //  m.setByKey('index', '首页数据');


     MemoryCache m=new MemoryCache<Map>();

     m.setByKey('index', {"name":"张三","age":20});
}
```

## dart中的库

### 定义

前面介绍Dart基础知识的时候基本上都是在一个文件里面编写Dart代码的，但实际开发中不可能这么写，模块化很重要，所以这就需要使用到库的概念。

在Dart中，库的使用时通过import关键字引入的。

library指令可以创建一个库，每个Dart文件都是一个库，即使没有使用library指令来指定。

Dart中的库主要有三种：

  1、我们自定义的库   

​     import 'lib/xxx.dart';

  2、系统内置库    

​     import 'dart:math';  

​     import 'dart:io'; 

​     import 'dart:convert';

  3、Pub包管理系统中的库 

​    https://pub.dev/packages

​    https://pub.flutter-io.cn/packages

​    https://pub.dartlang.org/flutter/

​    1、需要在自己想项目根目录新建一个pubspec.yaml

​    2、在pubspec.yaml文件 然后配置名称 、描述、依赖等信息

​    3、然后运行 pub get 获取包下载到本地 

​    4、项目中引入库 import 'package:http/http.dart' as http; 看文档使用

### 导入自己本地的库

```
import 'lib/Animal.dart';
main(){
  var a=new Animal('小黑狗', 20);
  print(a.getName());
}
```

### 导入系统内置库

```
// import 'dart:io';
import "dart:math";
main(){
 
    print(min(12,23));

    print(max(12,25));
    
}
```

### 导入内置库实现请求数据

```
import 'dart:io';
import 'dart:convert';


void main() async{
  var result = await getDataFromZhihuAPI();
  print(result);
}


//api接口： http://news-at.zhihu.com/api/3/stories/latest
getDataFromZhihuAPI() async{
  //1、创建HttpClient对象
  var httpClient = new HttpClient();  
  //2、创建Uri对象
  var uri = new Uri.http('news-at.zhihu.com','/api/3/stories/latest');
  //3、发起请求，等待请求
  var request = await httpClient.getUrl(uri);
  //4、关闭请求，等待响应
  var response = await request.close();
  //5、解码响应的内容
  return await response.transform(utf8.decoder).join();
}
```

### async、await

```
/*
async和await
  这两个关键字的使用只需要记住两点：
    只有async方法才能使用await关键字调用方法
    如果调用别的async方法必须使用await关键字


async是让方法变成异步。
await是等待异步方法执行完成。


*/

void main() async{
  var result = await testAsync();
  print(result);

}

//异步方法
testAsync() async{
  return 'Hello async';
}

```

### 导入pub包管理系统中的库

```
/*
pub包管理系统:


1、从下面网址找到要用的库
        https://pub.dev/packages
        https://pub.flutter-io.cn/packages
        https://pub.dartlang.org/flutter/

2、创建一个pubspec.yaml文件，内容如下

    name: xxx
    description: A new flutter module project.
    dependencies:  
        http: ^0.12.0+2
        date_format: ^1.0.6

3、配置dependencies

4、运行pub get 获取远程库

5、看文档引入库使用
*/
import 'dart:convert' as convert;
import 'package:http/http.dart' as http;
import 'package:date_format/date_format.dart';

main() async {
  // var url = "http://www.phonegap100.com/appapi.php?a=getPortalList&catid=20&page=1";

  //   // Await the http get response, then decode the json-formatted responce.
  //   var response = await http.get(url);
  //   if (response.statusCode == 200) {
  //     var jsonResponse = convert.jsonDecode(response.body);
     
  //     print(jsonResponse);
  //   } else {
  //     print("Request failed with status: ${response.statusCode}.");
  //   }


  
    print(formatDate(DateTime(1989, 2, 21), [yyyy, '*', mm, '*', dd]));

}
```

### Dart库的重命名 Dart冲突解决

```
/*
1、冲突解决
当引入两个库中有相同名称标识符的时候，如果是java通常我们通过写上完整的包名路径来指定使用的具体标识符，甚至不用import都可以，但是Dart里面是必须import的。当冲突的时候，可以使用as关键字来指定库的前缀。如下例子所示：

    import 'package:lib1/lib1.dart';
    import 'package:lib2/lib2.dart' as lib2;


    Element element1 = new Element();           // Uses Element from lib1.
    lib2.Element element2 = new lib2.Element(); // Uses Element from lib2.

*/

import 'lib/Person1.dart';
import 'lib/Person2.dart' as lib;

main(List<String> args) {
  Person p1=new Person('张三', 20);
  p1.printInfo();


  lib.Person p2=new lib.Person('李四', 20);

  p2.printInfo();

}
```

### 部分导入

```
/*
部分导入
  如果只需要导入库的一部分，有两种模式：

     模式一：只导入需要的部分，使用show关键字，如下例子所示：

      import 'package:lib1/lib1.dart' show foo;

     模式二：隐藏不需要的部分，使用hide关键字，如下例子所示：

      import 'package:lib2/lib2.dart' hide foo;      

*/

// import 'lib/myMath.dart' show getAge;

 import 'lib/myMath.dart' hide getName;

void main(){
//  getName();
  getAge();
}
```

### 延迟加载

```
/*
延迟加载

    也称为懒加载，可以在需要的时候再进行加载。
    懒加载的最大好处是可以减少APP的启动时间。

    懒加载使用deferred as关键字来指定，如下例子所示：

    import 'package:deferred/hello.dart' deferred as hello;

    当需要使用的时候，需要使用loadLibrary()方法来加载：

    greet() async {
      await hello.loadLibrary();
      hello.printGreeting();
    }

*/
```

