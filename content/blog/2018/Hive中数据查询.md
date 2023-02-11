---
title: "Hive中数据查询"
date: 2018-09-08 18:05:10
draft: false
---
**1.基本查询**

全表和特定列查询

1)全表查询
hive (default)> select /* from emp;

2)选择特定列查询

hive (default)> select empno, ename from emp;

注意:
(1)SQL 语言大小写不敏感。
(2)SQL 可以写在一行或者多行
(3)关键字不能被缩写也不能分行
(4)各子句一般要分行写。
(5)使用缩进提高语句的可读性

常用函数

1)求总行数(count)
hive (default)> select count(/*) cnt from student;

2)求工资的最大值(max)

hive (default)> select max(sal) max_sal from student;

3)求工资的最小值(min)

hive (default)> select min(sal) min_sal from student;

4)求工资的总和(sum)

hive (default)> select sum(sal) sum_sal from student;

5)求工资的平均值(avg)

hive (default)> select avg(sal) avg_sal from student;

Limit语句

典型的查询会返回多行数据，Limit子句用于限制返回的行数
hive (default)> select /* from student limit 5;

**2.Like和RLike**

1)使用 Like运算选择类似的值

2)选择条件可以包含字符或数字：
% 代表零个或多个字符(任意个字符)。
_ 代表一个字符

3)RLike 子句是 Hive 中这个功能的一个扩展,其可以通过 Java 的正则表达式这个更强大的语言来指定匹配条件

4)案例

案例实操
(1)查找以 2 开头薪水的员工信息
hive (default)> select /* from student where sal Like '2%';

(2)查找第二个数值为 2 的薪水的员工信息

hive (default)> select /* from student where sal Like '_2%';

(3)查找薪水中含有 2 的员工信息

hive (default)> select /* from student where sal RLike '[2]';

**3.分组**

Having语句

1)having 与 where 不同点：

(1)where 针对表中的列发挥作用,查询数据;having 针对查询结果中的列发挥作用,筛选数据。
(2)where 后面不能写分组函数,而 having 后面可以使用分组函数。
(3)having 只用于 group by 分组统计语句。

2)案例
求每个部门的平均工资
hive (default)> select deptno, avg(sal) from emp group by deptno;

求每个部门的平均薪水大于 2000 的部门

hive (default)> select deptno, avg(sal) avg_sal from emp group by deptno having avg_sal >2000;

**4.Join语句**

等值Join

Hive 支持通常的 SQL JOIN 语句,但是只支持等值连接,不支持非等值连接。
案例实操：

根据员工表和部门表中的部门编号相等,查询员工编号、员工名称和部门编号
hive (default)> select e.empno, e.ename, d.deptno, d.dname from emp e join dept d on e.deptno = d.deptno;

内连接

内连接:只有进行连接的两个表中都存在与连接条件相匹配的数据才会被保留下来
hive (default)> select e.empno, e.ename, d.deptno from emp e join dept d on e.deptno = d.deptno;

左外连接

左外连接:JOIN 操作符左边表中符合 WHERE 子句的所有记录将会被返回
hive (default)> select e.empno, e.ename, d.deptno from emp e left join dept d on e.deptno = d.deptno;

右外连接

右外连接:JOIN 操作符右边表中符合 WHERE 子句的所有记录将会被返回
hive (default)> select e.empno, e.ename, d.deptno from emp e right join dept d on e.deptno = d.deptno;

满外连接

满外连接:将会返回所有表中符合 WHERE 语句条件的所有记录。如果任一表的指定字段没有符合条件的值的话,那么就使用null值替代
hive (default)> select e.empno, e.ename, d.deptno from emp e full join dept d on e.deptno = d.deptno;

多表连接

注意:连接 n 个表,至少需要 n-1 个连接条件。例如:连接三个表,至少需要两个连接条件

多表连接查询：
hive (default)>select e.ename, d.deptno, l. loc_name from join emp e dept d ON d.deptno = e.deptno JOIN location l ON d.loc = l.loc;

大多数情况下,Hive 会对每对 JOIN 连接对象启动一个 MapReduce 任务。本例中会首先启动一个 MapReduce job 对表 e 和表 d 进行连接操作,然后会再启动一个 MapReduce job将第一个 MapReduce job 的输出和表 l;进行连接操作。
注意:为什么不是表 d 和表 l 先进行连接操作呢?这是因为 Hive 总是按照从左到右的顺序执行的

笛卡尔积

1)笛卡尔集会在下面条件下产生:
(1)省略连接条件
(2)连接条件无效
(3)所有表中的所有行互相连接

2)案例实操
hive (default)> select empno, deptno from emp, dept; FAILED: SemanticException Column deptno Found in more than One Tables/Subqueries

连接谓词中不支持or

hive (default)> select e.empno, e.ename, d.deptno from emp e join dept d on e.deptno = d.deptno or e.ename=d.ename; 错误的

**5.排序**

全局排序

1)使用 order by 子句排序
asc(ascend): 升序(默认)
desc(descend): 降序
2)order by 子句在 select语句的结尾
3)案例实操
(1)查询员工信息按工资升序排列
hive (default)> select /* from emp order by sal;

(2)查询员工信息按工资降序排列

hive (default)> select /* from emp order by sal desc;

按照别名排序

按照员工薪水的 2 倍排序
hive (default)> select ename, sal/*2 twosal from emp order by twosal;

多个列排序

按照部门和工资升序排序
hive (default)> select ename, deptno, sal from emp order by deptno, sal ;

每个Mapreduce内部排序

Sort By:每个 MapReduce 内部进行排序,对全局结果集来说不是排序。
1)设置 reduce 个数
hive (default)> set mapreduce.job.reduces=3;

2)查看设置 reduce 个数

hive (default)> set mapreduce.job.reduces;

3)根据部门编号降序查看员工信息

hive (default)> select /* from emp sort by empno desc;

4)将查询结果导入到文件中(按照部门编号降序排序)

hive (default)> insert overwrite local directory '/opt/module/datas/sortby-result' select /* from emp sort by deptno desc;

分区排序

Distribute By:类似 MR 中 partition,进行分区,结合 sort by 使用。
注意,Hive 要求 DISTRIBUTE BY 语句要写在 SORT BY 语句之前。对于 distribute by 进行测试,一定要分配多 reduce 进行处理,否则无法看到 distribute by的效果

案例实操：
先按照部门编号分区,再按照员工编号降序排序
hive (default)> set mapreduce.job.reduces=3; hive (default)> insert overwrite local directory '/opt/module/datas/distribute-result' select /* from emp distribute by deptno sort by empno desc;

Cluster by

当 distribute by 和 sorts by 字段相同时,可以使用 cluster by 方式。
cluster by 除了具有 distribute by 的功能外还兼具 sort by 的功能。但是排序只能是倒序排
序,不能指定排序规则为 ASC 或者 DESC。
以下两种写法等价
select /* from emp cluster by deptno; select /* from emp distribute by deptno sort by deptno;

注意:按照部门编号分区,不一定就是固定死的数值,可以是 20 号和 30 号部门分到一个分区里面去