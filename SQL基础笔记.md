# SQL语句学习（基础）

## 1 查询 （select）

- ### select （要求的查询项） from （表名） where （表达式 附加条件）order by （要求项）（desc）；

- （要求的查询项）： 项目名     （或） 表名.项目名  （或）  函数（项目名）

  函数： count () 计数器        avg()          max()             sum()             min() .....

  

- （表名） 可有多张表

- （表达式 附加条件） 可以使用联合多张表一起查询，注意多张表中数据的对应（方法：选择一个相同的项目相等 表名1.sno = 表名2.sno）

  - is not null            null            like             in             =               <                >  	between--and
    - 占位符              _   一个字符 （张_）               %  多个字符   此情况下多为用like关键字(张%)

- **自连接查询（自己复制一份自己的表，并命名一个标识符）**

  **1、列出那些专业相同的学生相应的姓名及专业信息**

  select  a.姓名, b.姓名, 专业 from  学生a,  学生b where  a.学号 <> b.学号  and  a.专业 =  b.专业

  **2、求至少选修1号课和2号课的学生的学号**

  select  x.学号  from  选课x,  选课y  where  x.学号 = y.学号  and  x.课号 = "1"  and  y.课号="2"



- limit n,m   从n开始查，查m个,n从0开始 (也就是说，要得到 6 - 10 条数据，那么对应 limit 5, 5 
- order by desc(asc) 要放到 limit 前面 where 后 
- 从查询的逻辑关系里找到命令的前后关系

