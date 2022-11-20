# 参考网站：

[SQL WHERE 子句 | 菜鸟教程 (runoob.com)](https://www.runoob.com/sql/sql-where.html)；

[MySQL 函数 | 菜鸟教程 (runoob.com)](https://www.runoob.com/mysql/mysql-functions.html)

# 函数用法及例题

###### 1、IFNULL

用于判断第一个表达式是否为 NULL，如果为 NULL 则返回第二个参数的值，如果不为 NULL 则返回第一个参数的值。这里的offset  1，查询到的信息跳过1条，即第二条。-->Leetcode.176

```sql
select
    ifnull(
    (select distinct salary 
        from Employee 
        order by salary desc 
        limit 1 offset 1),
        null)  
        as SecondHighestSalary
```

###### 2、limit与offset

limit 2,1：跳过2条取出1条数据,即读取第3条数据；limit 2 offset 1：跳过1条取两条，即读取第2,3条。筛选出第n高的薪水-->Leetcode.176

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
set N = N-1;
  RETURN (
      # Write your MySQL query statement below.
      select ifnull(
          (select distinct salary from Employee 
           order by salary desc limit N,1),null) 
      as getNthHighestSalary
      
  );
END
```

###### 3、rank

1. rank() over
作用：查出指定条件后的进行排名，条件相同排名相同，排名间断不连续。说明：例如学生排名，使用这个函数，成绩相同的两名是并列，下一位同学空出所占的名次。即：1 1 3 4 5 5 7

2. dense_rank() over
作用：查出指定条件后的进行排名，条件相同排名相同，排名不间断不连续。说明：和rank() over 的作用相同，区别在于dense_rank() over 排名是密集连续的。例如学生排名，使用这个函数，成绩相同的两名是并列，下一位同学接着下一个名次。即：1 1 2 3 4 5 5 6
3. row_number() over
    作用：查出指定条件后的进行排名，条件相同排名也不相同，排名间断不连续。说明：这个函数不需要考虑是否并列，即使根据条件查询出来的数值相同也会进行连续排序。即：1 2 3 4 5 6

使用小提示dense_rank()  over 后面跟排序的依据的列，下面是用了一个排序好的列(order by score desc)。注意：如果select中有一列是用rank()这类函数，其他的列都会按着他这列规定好的顺序排。

```sql
# Write your MySQL query statement below
select score, dense_rank() over (order by score desc) 
as 'rank'  #这个rank之所以要加引号，因为rank本身是个函数，直接写rank会报错
from scores;
```

###### 4、-->Leetcode.180

###### 5、->Leetcode.181

```sql
SELECT
    a.Name AS 'Employee'
FROM
    Employee AS a,
    Employee AS b
WHERE
    a.ManagerId = b.Id
        AND a.Salary > b.Salary
;

SELECT
     a.NAME AS Employee
FROM Employee AS a JOIN Employee AS b
     ON a.ManagerId = b.Id
     AND a.Salary > b.Salary
     #and 换成where 也可以。
     # where a.Salary > b.Salary
;

```

###### 6、临时表

note：join 既可以join一个表，也可以join临时表；select时也可以选择一个临时表的select结果。-->Leetcode.1158

```sql
# 1、用临时表
select Email from
(
  select Email, count(Email) as num
  from Person
  group by Email
) as statistic #Every derived table must have its own alias
# 即 对于子表必须要设置名称
where num > 1
;

# 2、 使用 GROUP BY 和 HAVING 条件
select Email
from Person
group by Email
having count(Email) > 1;
```

###### 7、in or not in

```sql
select customers.name as 'Customers'
from customers
where customers.id not in
(
    select customerid from orders
);

select c.Name as Customers 
from Customers as c
left join Orders as o on c.Id = o.CustomerId
where o.Id is null
# mine
select Name as "Customers" from  Customers
where Customers.Id not in (select CustomerId from Orders )
```

###### 8、where子句中的运算符

（1）where在group by前， having在group by 之后

（2）聚合函数（avg、sum、max、min、count），不能作为条件放在where之后，但可以放在having之后

[SQL HAVING 子句 | 菜鸟教程 (runoob.com)](https://www.runoob.com/sql/sql-having.html)	

```sql
SELECT Websites.name, Websites.url 
FROM Websites 
WHERE EXISTS (SELECT count FROM access_log WHERE Websites.id = access_log.site_id AND count > 200);

#上面并没有join
SELECT Websites.name, Websites.url 
FROM Websites 
WHERE EXISTS (SELECT count FROM access_log inner join Websites where Websites.id = access_log.site_id AND access_log.count > 200);
```

###### 9、datediff

第一个参数-第二个=日期差-->Leetcode.197，

```sql
select w1.id as "id"
from Weather as w1 join Weather as w2
on datediff(w1.recordDate,w2.recordDate) = 1
where w1.Temperature > w2.Temperature
```

###### 10、exist

-->Leetcode.607，

[MySQL SQL语句EXISTS - 墨天轮 (modb.pro)](https://www.modb.pro/db/181473)

select 1 from table 中的1是一常量（可以为任意数值），查到的所有行的值都是它，但从效率上来说，select 1 > select xxx > select \*，因为不用查字典表.一般用来当做判断子查询是否成功（即是否有满足条件的时候使用）

这个判断就是当(select 1 from stuA.id = stuB.id)这个查询如果有返回值的话表示当前查询满足条件。
一般来说想简单的话用select 1，当然也可以用select * ,或者select 任何字段，因为这里仅仅是表明子查询有结果就行了，至于是什么结果无所谓。

###### 11、case when

-->Leetcode.626

[mysql的if 和 case when - caibaotimes - 博客园 (cnblogs.com)](https://www.cnblogs.com/caibaotimes/p/14221663.html)

（1）条件判断，IF（condition，arg1， arg2）如果condition为真，arg1，否则arg2。

（2）case when 条件1 then 结果1 when 条件2 then 结果2  else 结果n end

两种方式类似，详细题目见

本题中，官方还用到from 多表，和join两表的效率是相同的。值得注意的是，from 一个count 的数值也是可以的。



###### 12、日期范围问题

```sql
# leetcod 1084
select s.product_id , p.product_name 
from Sales s left join Product p
on s.product_id = p.product_id
group by s.product_id
having min(s.sale_date) >= "2019-01-01" and max(s.sale_date) <= "2019-03-31"
# 不能在having后用between，因为提示Unknown column 'sale_date' in 'having clause'，尝试用子查询。
# 依然有问题，会显示期间的某一笔，与题目不符。
# having后也不能用datediff
select product_id,product_name from (
    select s.product_id , p.product_name ,s.sale_date
    from Sales s left join Product p
    on s.product_id = p.product_id
    group by s.product_id) as newtab
where sale_date between "2019-01-01" and "2019-03-31"

```

注：between是左闭右开。但注意：order_date between '2019-01-01' and '2019-12-31'。日期的话没有分和秒，between and 是两边都包含，所以12-31也会被算进去，当然如果是datetime就得改成<2020-01-01



https://www.runoob.com/sql/func-extract.html)



###### 13、Leetcode.1484

group_concat()

###### 14、行列转换-->Leetcode.1795

```sql
# 列转行
SELECT product_id, 'store1' store, store1 price FROM products WHERE store1 IS NOT NULL
UNION
SELECT product_id, 'store2' store, store2 price FROM products WHERE store2 IS NOT NULL
UNION
SELECT product_id, 'store3' store, store3 price FROM products WHERE store3 IS NOT NULL;

#列转行
SELECT 
  product_id,
  SUM(IF(store = 'store1', price, NULL)) 'store1',
  SUM(IF(store = 'store2', price, NULL)) 'store2',
  SUM(IF(store = 'store3', price, NULL)) 'store3' 
FROM
  Products1 
GROUP BY product_id ;

```

union ：操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。多个 SELECT 语句会删除重复的数据。

union all 会连接包括重复值在内的。

带where 要放在每一个select的union后。

###### 15、查找存在任意确实的数据

```sql
1、
select * from (
    select employee_id
    from employees e1
    where employee_id not in (select employee_id from salaries)
    union
    select employee_id
    from salaries s1
    where employee_id not in (select employee_id from employees)
) t order by t.employee_id asc

union all语句是可以直接接order by的

2、改用对方的主键是否存在
    select e.employee_id employee_id from Employees e left join Salaries s on e.employee_id=s.employee_id where s.employee_id is null
    union all
    select s.employee_id employee_id from Employees e right join Salaries s on e.employee_id=s.employee_id where e.employee_id is null
    order by employee_id;

3、not exist
select e.employee_id employee_id from Employees e where not exists (select 1 from Salaries s where s.employee_id = e.employee_id)
union all
select s.employee_id employee_id from Salaries s where not exists (select 1 from Employees e where s.employee_id = e.employee_id)
order by employee_id;

4、 in 
select employee_id from Employees where employee_id not in (select employee_id from Salaries)
union all
select employee_id from Salaries where employee_id not in (select employee_id from Employees)
order by employee_id;

```

###### 16、with 创建临时表

创建后，使用一次select就会销毁该临时表。也可以with多个表

增加了sql的易读性，如果构造了多个子查询，结构会更清晰；

```sql
with tmptable as (select * from t1)

with 
tmp1 as (select * from t1)，
tmp2 as (select * from t2)
select tmp1.a tmp2.b from tmp1 join tmp2 on tmp1.id = tmp2.id



```

###### 17、LEAD，LAG

```sql
SELECT ID,Name,Population,
       lag(Population, 1, 0) over(ORDER BY ID) reslag,
       lead(Population, 1, 0) over(ORDER BY ID) reslead
FROM city limit 10;

```

###### 18、窗口函数

[SQL窗口函数_梁萌的博客-CSDN博客_sql窗口函数](https://blog.csdn.net/liangmengbk/article/details/124253806?ops_request_misc=%7B%22request%5Fid%22%3A%22166694235616782390543751%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=166694235616782390543751&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-124253806-null-null.142^v62^control,201^v3^control_2,213^v1^t3_control2&utm_term=sql  窗口函数&spm=1018.2226.3001.4187)

包括：聚合窗口函数：AVG()、SUM()、COUNT()、MAX()以及MIN()等函数。排名窗口函数：ROW_NUMBER()、RANK()、DENSE_RANK()、PERCENT_RANK()、CUME_DIST()以及NTILE()等函数。取值窗口函数：取值窗口函数用于返回指定位置上的数据行。FIRST_VALUE()、LAST_VALUE()、LAG()、LEAD()、NTH_VALUE()等函数。

按照字面意思理解，FIRST_VALUE()获得分组或分组排序后的第一个数值。ROW_NUMBER()则是获得分组或分组排序后的行数。

```sql
#关键字OVER表明SUM()是一个窗口函数。括号内为空，表示将所有数据作为一个分组进行汇总
select  ID,Name,Population ,sum(Population) over() s
from  city
#如果不指定PARTITION BY选项，表示将全部数据作为一个整体进行分析
select  ID,Name,Population, CountryCode ,sum(Population)
over(partition by CountryCode order by Population asc) s
from  city

select  ID,Name,Population, CountryCode ,ROW_NUMBER()
over(partition by CountryCode order by Population asc) s
from  city

select  ID,Name,Population, CountryCode ,
ROW_NUMBER() over(partition by CountryCode order by Population asc) s2,
FIRST_VALUE(Population) over(partition by CountryCode order by Population asc) s
from  city
```

