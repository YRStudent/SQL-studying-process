
# 一、SQL结构
## 1. <mark>**顺序**<mark>
**FROM>WHERE>GROUP BY>HAVING>SELECT>ORDER BY>LIMIT**  
![执行顺序](./images/SQL执行顺序.png =600x600)

往往由于WHERE是最先开始的 所以一般后面出现的定义别称、聚合函数等 不应该放在这里面 否则出现找不到该定义的情况  

## 2. HAVING和WHERE的用法区别（结合顺序解释）
HAVING 是WHERE关键字无法和聚合函数一起使用就比如SUM()等  
还需要注意就算有的改了别称的聚合函数也是要用HAVING的。  
先从表里面获取数据，获取到表的信息之后再限定条件WHERE，然后再对这些数据进行整合比如（GROUP BY），HAVING是聚合函数的使用语句，如果要是没有聚合就会报错 所以GROUP BY 和HAVING应该是成对出现的。  
然后剩下的再筛选数据 比如SELECT 然后再排序 排序完了最后是限制LIMIT  
**总的顺序为：**
FROM->WHERE->GROUP BY->HAVING->SELECT->ORDER BY->LIMIT

---
# 二、相关概念
## 1. 视图
**语法**：CREATE VIEW viewname 
SELECT column1,column2... FROM table WHERE XXX
就相当于指定让你看什么东西  
在一定程度上保护了数据的隐秘性  
**视图本身是一个SQL查询语句，只有你打开它时才会查询到相关数据，不打开就没有，就相当于一个调用接口可以理解为**  
此外，**一般将视图视作是检索SELECT而不是增删改，这样会破坏表的基础性**

# 增删改查语句
## 1. DELETE 
**删除单表内容的语法**：DELETE FROM table WHERE xxx  
**删除连表内容的语法**：DELETE T1 FROM T1 JOIN T2 ...
DELETE 不删除表本身而只删除表的内容  
如果想要快速删除表本身 那么使用TRUNCATE table则表示重新建立一个表  
见题：https://leetcode.cn/problems/delete-duplicate-emails/

# <MARK>GROUP BY<MARK>
1. Group by 如果有多列那么将在最后的列上进行汇总
2. 在子句中不能含有聚合类型的函数（详见SQL语句先后顺序）
3. GROUP BY 是对子句中的列建立了分组，分组的意思就是指定列都一起计算，所以呢就不能从单独的列中取出单独的数据 
也就是可以理解为Excel表中的分组显示功能一样
<mark>对于非分组列,必须使用聚合函数（MIN()、MAX()、AVG())<mark>
# 连接
https://blog.csdn.net/ysmintor_/article/details/99694870
## Inner join
取的是交集
## Left join
取的是左边表的所有值
## Right join
同Left join
## Full/Outer join
取的是并集，两个表**互相不存在**的那么就赋值为null
## Union
Union就是将列累加起来 形成一个整体的一列  
但是Union内部的SELECT语句列数一定相同，数据类型也是  
呈现的是不重复的
## Union all
是将所有的值都呈现，不管有没有重复

# 取数 LIMIT OFFSET
## LIMIT的意思就是取出来多少条 数字多少就是代表取多少 
## LIMIT m,n 表示从m+1开始取出n条数据
## LIMIT n: 从0开始取出n条数据(0，n)

查询第多少条数据的时候比如第二条数据，为1，eg:  
SELECT * FROM TABLEA LIMIT 1,1 ——>表示第二条数据  
SELECT * FROM TABLEA LIMIT 1,2 ——>表示第二条、第三条的数据  
...
如果是查询前多少条的时候：  
SELECT * FROM TABLEA LIMIT 0,2 ——>表示查询前两条的数据 所以这里计数应该是从0，取出0和1，相当于python取列表时候的前闭后开  

## OFFSET n 去掉第n个值
跳过n条数据，取n+1条值  
SELECT * FROM TABLEA LIMIT 3 OFFSET 1  
表示offset 1从第二条开始取三条LIMIT 3  

## 这个TOP N 针对的是SQL Server而非Mysql语言 这个要注意

# 函数

## 窗口函数
**是对全局进行操作**
窗口函数一般与函数公用，如排序函数、聚合函数
### 1. 专用窗口函数：rank、dense_rank、row_number等
### 2. 聚合函数：sum、avg、count、max、min
语法：
**<窗口函数> OVER (PARTITION BY <用于分组的列名> ORDER BY <用于排序的列名>)**


### **排序函数**
#### RANKING() 对于分数相同的排序号码会一致 但是下一个排序号码则会累计相同分数的数量
#### DENSE_RANK() 对于分数相同的排序号码也是一致的，但不累计数量
#### ROW_NUM() 不管分数相同与否，都依次按照排序号码排列下来
具体见图所示：
![排序函数](images\1e5f6d319cf74303c9263d57d740fcb0fd6e61a685386347ddc53bf7fc20d035.png) 

### **位移函数**
#### LEAD(column_name,offset,default 0) OVER ()
数据从下往上推
#### LAG(column_name,offset,default 0) OVER ()
数据从上往下推  

相关题目链接：  
180 连续出现的数字  
https://leetcode.cn/problems/consecutive-numbers/

# 条件语句
## CASE WHEN ELSE
CASE XX
    WHEN XXX THEN XXX
    ELSE XXX
    END;

## IF(expression, result_ture,result_false)
## IFNULL(expr1,expr2)
判断表达式1是否为空值，如果是则返回表达式2，否则就返回它自身(null)
## NULLIF（expr1,expr2)
如果expr1 == expr2 则返回null,否则就返回expr1
## ISNULL(expr)
如果expr是null 返回1 否则返回0

