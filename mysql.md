# 启动关闭数据库

```
命令行输入 services.msc
然后找到MYSQL 改成手动
以后使用时 开启服务
```



关系型数据库

# 命令行启用

+ **连接到本机上的MYSQL**

```
mysql -uroot -p 然后输入密码
```

+ 查看数据库编码

```
1、show variables like 'character\_set\_%';
2、修改C:\ProgramData\MySQL\MySQL Server 5.7下的my.ini文件，用编辑器打开，修改下面的代码

default-character-set=utf8mb4
character-set-server=utf8mb4

3、把my.ini复制到安装目录下
4、重新启动mysql服务 在services.msc下
```

+ 查看当前拥有的数据库

```
show databases;
```

# navicat工具管理数据库

+ 百度去破解

# 常见字段类型

```
bit 占1位，0或1，false或true

int 占32位，整数
decimal(M,N) 能精确计算的实数，M是总的自位数，N是小数位数

char(n) 固定长度位n的字符
varchar(n) 长度可变，最大长度位n的字符
text 大量的字符

date 仅日期 格式：'YYYY-MM-DD'，范围：'1000-01-01'到'9999-12-31'
datatime 日期和时间 格式：'YYYY-MM-DD HH:MM:SS'，范围：'1000-01-01 00:00:00'到'9999-12-31 				23:59:59'
time 仅时间，格式：'HH:MM:SS'
```

# 表

+ 主键

```
唯一 不能修改  无业务意义
```

+ 外键

```
用于产生表关系的列
外键列会连接到另一张表(或自己)的主键
```

+ 表关系

```
1、一对一 a对应b，b也只对应a 如用户和用户信息  把任意一张表的主键同时设置为外键
2、一对多  一个a对应多个b，一个b对应一个a，a和b是一对多，b和a是多对一
		 例如 班级和学生 ，用户和文章
		 在多的一端表上设置外键，对应到另一张表的主键
3、多对多 一个a对应多个b，一个b对应多个a
		例如学生和老师
		需要新建一张关系表，关系表至少包含两个外键，分别对应到两张表
```

+ 增删改查

```
DML Data Manipulation Language 数据操控语言
增 CREATE
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN ),
                       ( value1, value2,...valueN );
查 Retrieve

改 UPDATE
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause];

删 DELETE
DELETE FROM table_name [WHERE Clause];

CRUD


```

## 单表查询

```
SELECT column_name,column_name
FROM table_name  （通常是表名）
[WHERE Clause]
[LIMIT N][ OFFSET M]

SELECT sex as 男 from `user`  定别名
SELECT * from `user` 查询所有列

//case后要加end end后的是别名
SELECT *, case ismale
when 1 then '男'
else '女'
end sex  
from employee;

```

+ distinct去重

```
-- 所有员工分布的地址  （- - 为注释）
select DISTINCT location from employee; //通常只查一列
```



+ where 条件查询

```
//关键词重复时要加上``号
SELECT *, case 
when ismale = 1 then '男'
else '女'
end sex,
case 
when salary>=10000 then '高'
when salary>=5000 then '中'
else '低'
end `level`
from employee;

//先运行from 再where 再select
SELECT * FROM employee
WHERE ismale = 1;

SELECT * FROM department
WHERE companyId in (1, 2); in是范围内

//null用is查
SELECT * from employee
WHERE location is not null;

SELECT * from employee
WHERE location is null;

SELECT * from employee
WHERE salary>=10000;

SELECT * from employee
WHERE salary BETWEEN 10000 and 12000;

SELECT * from employee
WHERE `name` like '%袁%'; //名字中有袁的 %代表前后有字的，模糊搜索

SELECT * from employee
WHERE `name` like '袁_'; //_为匹配一个字符

SELECT * from employee
WHERE `name` like '张%' and ismale=0 and salary>=12000;

SELECT * from employee
WHERE `name` like '张%' and (ismale=0 and salary>=12000
or
birthday>='1996-1-1');
```

+ order by 排序    asc升序 desc降序  

```
SELECT *, case ismale
when 1 then '男'
else '女'
end sex from employee
ORDER BY sex asc, salary desc;  //运行在select之后
```

+ limit 最后运行 n,m 跳过n条数据，取出m条数据

```
SELECT * from employee
limit 2,3;
```

## 练习

```
-- 查询user表，得到账号为admin，密码为123456的用户
-- 登录

SELECT * from `user`
WHERE loginid = 'admin' and loginpwd = '123123';

-- 查询员工表，按照员工的入职时间降序排序，并且使用分页查询
-- 查询第3页，每页5条数据
-- limit (page-1)*pagesize, pagesize

SELECT * FROM employee
ORDER BY employee.joinDate desc
LIMIT 10,5

-- 查询工资最高的女员工

SELECT * FROM employee
WHERE ismale = 0
ORDER BY salary desc
limit 0,1;
```

## 联表查询

+ 笛卡尔积

```
-- 1. 创建一张team表，记录足球队
-- 查询出对阵表

SELECT t1.name 主场, t2.name 客场 
FROM team as t1, team as t2
WHERE t1.id != t2.id;
```

+ 左连接，左外连接，left join

```
SELECT * 
from department as d left join employee as e
on d.id = e.deptId;
```

+ 右连接，右外连接，right join

```
SELECT * 
from employee as e right join department as d 
on d.id = e.deptId;
```

+ 内连接，inner join 

```
SELECT e.`name` as empname, d.`name` as dptname, c.`name` as companyname
from employee as e 
inner join department as d on d.id = e.deptId 
inner join company c on d.companyId = c.id;
```

## 练习

```
-- 2. 显示出所有员工的姓名、性别（使用男或女显示）、入职时间、薪水、所属部门（显示部门名称）、所属公司（显示公司名称）

SELECT e.`name` 员工姓名, 
case ismale when 1 then '男' else '女' end 性别,
e.joinDate 入职时间,
e.salary 薪水,
d.`name` 部门名称,
c.`name` 公司名称
FROM employee e 
inner join department d on e.deptId = d.id
inner join company c on d.companyId = c.id

-- 3. 查询腾讯和蚂蚁金服的所有员工姓名、性别、入职时间、部门名、公司名

SELECT e.`name` 员工姓名, 
case ismale when 1 then '男' else '女' end 性别,
e.joinDate 入职时间,
e.salary 薪水,
d.`name` 部门名称,
c.`name` 公司名称
FROM employee e 
inner join department d on e.deptId = d.id
inner join company c on d.companyId = c.id
WHERE c.`name` in ('腾讯科技', '蚂蚁金服')

-- 4. 查询渡一教学部的所有员工姓名、性别、入职时间、部门名、公司名

SELECT e.`name` 员工姓名, 
case ismale when 1 then '男' else '女' end 性别,
e.joinDate 入职时间,
e.salary 薪水,
d.`name` 部门名称,
c.`name` 公司名称
FROM employee e 
inner join department d on e.deptId = d.id
inner join company c on d.companyId = c.id
WHERE c.`name` like '%渡一%' AND d.`name` = '教学部';

-- 5. 列出所有公司员工居住的地址（要去掉重复）

select DISTINCT location from employee;
```

# 函数和数组

## 函数

+ 内置函数

```
+ 数学函数
SELECT ABS(-1);
SELECT CEIL(1.4);
SELECT ROUND(3.1415926, 3); //保留3位 ，不填的话保留整数位
SELECT TRUNCATE(3.1415926,3);

SELECT TRUNCATE(salary,0)
FROM employee

+聚合函数

SELECT AVG(salary) as `avg`, id 平均值
FROM employee;

-- 查询员工数量
SELECT COUNT(id)  
FROM employee;

SELECT count(id) as 员工数量,
avg(salary) as 平均薪资,
sum(salary) as 总薪资,
min(salary) as 最小薪资
FROM employee;

+字符和日期函数
SELECT CONCAT_WS('@', `name`,salary)
FROM employee;

SELECT CURDATE();

SELECT CURTIME();

SELECT TIMESTAMPDIFF(DAY,'2010-1-1 11:11:11','2010-1-2 11:11:12');

SELECT *, 
TIMESTAMPDIFF(YEAR, birthday, CURDATE()) as age
from employee
ORDER BY age;
```

## 分组

```
-- 查询员工分布的居住地，以及每个居住地有多少名员工
-- 天府三街 3
SELECT location, count(id) as empnumber
FROM employee
GROUP BY location
HAVING empnumber>=40

-- 查询所有薪水在10000以上的员工的分布的居住地，然后仅得到聚集地大于30的结果
SELECT location, count(id) as empnumber
FROM employee
WHERE salary>=10000
GROUP BY location
HAVING count(id)>=30
```

# 练习

```
-- 1. 查询渡一每个部门的员工数量

SELECT d.`name`, COUNT(e.id) as number
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` like '%渡一%'
GROUP BY d.id, d.`name`;

-- 2. 查询每个公司的员工数量

SELECT c.`name`, COUNT(e.id) as number
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
GROUP BY c.id, c.`name`

-- 3. 查询所有公司5年内入职的居住在万家湾的女员工数量

SELECT c.`name`, CASE WHEN r.number is NULL THEN 0 ELSE r.number END as number
FROM company c LEFT JOIN (SELECT c.id, c.`name`, COUNT(e.id) as number
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE TIMESTAMPDIFF(YEAR,e.joinDate,CURDATE())<=5
AND e.location like '%万家湾%'
GROUP BY c.id, c.`name`) as r on c.id = r.id

-- 4. 查询渡一所有员工分布在哪些居住地，每个居住地的数量

SELECT e.location, count(e.id) as empnumber
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` LIKE '%渡一%'
GROUP BY e.location

-- 5. 查询员工人数大于200的公司信息

SELECT * FROM company
WHERE id in (
SELECT c.id
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
GROUP BY c.id, c.`name`
HAVING count(e.id)>=200
)

-- 6. 查询渡一公司里比它平均工资高的员工

SELECT e.* 
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` LIKE '%渡一%' AND
e.salary>(
-- 查询渡一的平均薪资
SELECT avg(e.salary)
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` LIKE '%渡一%'
)

-- 7. 查询渡一所有名字为两个字和三个字的员工对应人数

SELECT CHAR_LENGTH(e.`name`) as 姓名长度, COUNT(E.ID) as 员工数量
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
WHERE c.`name` LIKE '%渡一%'
GROUP BY CHAR_LENGTH(e.`name`)
HAVING 姓名长度 in (2,3)

-- 8. 查询每个公司每个月的总支出薪水，并按照从低到高排序

SELECT c.`name`, SUM(e.salary) as sumofsalary
FROM company as c INNER JOIN department as d on c.id = d.companyId
INNER JOIN employee as e on d.id = e.deptId
GROUP BY c.id, c.`name`
ORDER BY sumofsalary
```

# 驱动程序

+ mysql2

```
npm install --save mysql2 安装
```

+ 普通回调

```
const mysql = require("mysql2");

// 创建一个数据库连接
const connection = mysql.createConnection({
  host: "localhost",
  user: "root",
  password: "ybybdwyJ42.",
  database: "companydb",
});

// simple query
// connection.query("SELECT * FROM `company`;", function (err, results) {
//   //err错误
//   //result查询结果
//   console.log(results); // results contains rows returned by server
// });

// connection.query(
//   "insert into company(`name`,location,buildDate) values('abbc', '阿萨德', curdate());",
//   (err, result) => {
//     console.log(result);
//   }
// );

// connection.query(
//   "update company set `name`='bcd' where id=4",
//   (err, result) => {
//     console.log(result);
//   }
// );

connection.query(
    "delete from company where id=4",
    (err, result) => {
      console.log(result);
    }
  );
```

+ promise

```
const mysql = require("mysql2/promise");

async function test() {
  // 创建一个数据库连接
  const connection = await mysql.createConnection({
    host: "localhost",
    user: "root",
    password: "ybybdwyJ42.",
    database: "companydb",
  });
//   console.log(connection);
  const [results] = await connection.query("SELECT * FROM `company`;");
  console.log(results);
  connection.end();
}
test();
```

+ 防止sql注入（用户通过注入sql语句到最终查询中，导致整个sql与预期行为不符），所以要用execute不要用query

```
const mysql = require("mysql2/promise");
const pool = mysql.createPool({ //连接池,不用关闭
  host: "localhost",
  user: "root",
  password: "ybybdwyJ42.",
  database: "companydb",
  multipleStatements: true, //允许运行多条sql语句
});

async function test(id) {
  // 创建一个数据库连接
  const sql = `select * from employee where \`name\` like concat('%', ?, '%') ;`;
  const [results] = await pool.execute(sql, [id]); //数组中每个值对应上面变量的每个？号
  console.log(results);
}
test("袁");
```

# ORM框架

+ node中的ORM： Sequelize 和 TypeORM  
+ Sequelize 使用参考文档

# md5

+ 安装

```
npm i md5
```

+ 特点

```
hash加密的一种
可以将任何一个字符串，加密成一个固定长度的字符串
单向加密：只能加密无法解密
同样的源字符串加密后得到的结果固定
```

# 时间

客户端与服务端与数据库要用utc或时间戳存取数据

moment库

```
const moment = require("moment");
moment.locale("zh-cn");
//得到当前时间，moment对象
// console.log(moment().toString());
// console.log(moment.utc().toString());

//得到当前时间戳
// console.log(moment().valueOf(), +moment());
// console.log(moment.utc().valueOf(), +moment.utc());

//根据指定的时间格式得到时间，时间格式：xxxx-xx-xx xxxx/xx/xx  时间戳
// console.log(moment(0).toString(), +moment(0));
// console.log(moment.utc(0).toString(), +moment.utc(0));
// const value = "1970-01-01 00:00:00";
// console.log(moment(value).toString(), +moment(value));
// console.log(moment.utc(value).toString(), +moment.utc(value));

//使用日期令牌转换
//令牌是一个格式化的字符串，例如： "YYYY-MM-DD HH:mm:ss"
const formats = ["YYYY-MM-DD HH:mm:ss", "YYYY-M-D H:m:s", "x"];
// console.log(moment.utc("1970-01-01 00:00:00", formats, true).toString());
// console.log(moment.utc("1970-1-1 0:0:0", formats, true).toString());
// console.log(moment.utc(0, formats, true).toString());
// const invalidMoment = moment.utc(
//   "Thu Jan 01 1970 00:00:00 GMT+0000",
//   formats,
//   true
// );
// console.log(invalidMoment.isValid()); // false
// console.log(invalidMoment.toString());
// console.log(+invalidMoment);

// 显示（发生在客户端）
// const m = moment.utc("2015-1-5 23:00:01", formats, true);
// console.log(m.local().format("YYYY年MM月DD日 HH点mm分ss秒"));

// const m = moment("2015-1-5 23:00:01", formats, true);
// const toServer = m.utc().format("YYYY-MM-DD HH:mm:ss");
// console.log(toServer);

const m = moment.utc("2020-4-14 9:00:01", formats, true);
console.log(m.local().fromNow());
```

# 数据验证

+ validator 用于验证某个字符串是否满足某个规则
+ validate.js  用于验证某个对象的树形是否满足某些规则

# 日志记录

+ 库  log4js

# express

+ 中间件

```
const express = require("express");
const app = express(); //创建一个express应用

app.use(require("./staticMiddleware"));

app.get("/news/abc", (req, res, next) => {
  console.log("handler1");
  // throw new Error("abc")
  // 相当于 next(new Error("abc"))
  next(new Error("abc"));
  //   next();
});

//能匹配  /news  /news/abc   /news/123   /news/ab/adfs
//不能匹配  /n   /a   /   /newsabc
app.use("/news", require("./errorMiddleware"));

const port = 5008;
app.listen(port, () => {
  console.log(`server listen on ${port}`);
});

## errorMiddleware.js
// 处理错误的中间件
module.exports = (err, req, res, next) => {
  if (err) {
    const errObj = {
      code: 500,
      msg: err instanceof Error ? err.message : err,
    };
    //发生了错误
    res.status(500).send(errObj);
  } else {
    next();
  }
};

## staticMiddleware.js
module.exports = (req, res, next) => {
  if (req.path.startsWith("/api")) {
    // 说明你请求的是 api 接口
    next();
  } else {
    // 说明你想要的是静态资源
    if (true) {
      res.send("静态资源");
    } else {
      next();
    }
  }
};
```

