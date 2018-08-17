## 增删改

### 插入数据

INSERT INTO 语句用于向表中插入新记录。

#### 实例

```sql
INSERT INTO 表名 (name, url, alexa, country)
VALUES ('百度','https://www.baidu.com/','4','CN');
```

### 更改

UPDATE 语句用于更新表中的记录。

#### 实例

```sql
UPDATE Websites 
SET alexa='5000', country='USA' 
WHERE name='菜鸟教程';
//将Websites表中 name='菜鸟教程' 的列alexa改为'5000', country改为'USA' 
```

### 删除

DELETE 语句用于删除表中的记录。

#### 实例

```sql
DELETE FROM Websites
WHERE name='百度' AND country='CN';
```

##基础

### 选择

SELECT 语句用于从数据库中选取数据。

实例

```sql
SELECT column_name,column_name
FROM table_name;
与
SELECT * FROM table_name;
```

### 选择（去除重复值）

SELECT DISTINCT 语句用于返回唯一不同的值。

实例

```sql
SELECT DISTINCT country FROM Websites;
```

### 过滤

WHERE 子句用于过滤记录。

实例

下面的 SQL 语句从 "Websites" 表中选取国家为 "CN" 的所有网站：

`SELECT * FROM Websites WHERE country='CN';`

### 过滤（多个条件）

AND & OR 运算符用于基于一个以上的条件对记录进行过滤。

实例

```sql
SELECT * FROM Websites
WHERE alexa > 15
AND (country='CN' OR country='USA');
```

### 排序

ORDER BY 关键字用于对结果集进行排序。

实例

```sql
SELECT * FROM Websites
ORDER BY country desc,alexa;	//默认为升序，country加了关键字desc，所以为降序
```

## 高级

### 规定返回数量

SELECT TOP, LIMIT, ROWNUM 子句用于规定要返回的记录的数目。

MySQL 支持 LIMIT 语句来选取指定的条数数据， Oracle 可以使用 ROWNUM 来选取。其他数据库SELECT TOP

**实例**

```sql
SELECT * FROM Websites LIMIT 2;		//显示前两条数据
```

###where指定模式

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。

**实例**

```sql
SELECT * FROM Websites
WHERE name LIKE 'G%';
//SQL 语句选取 name 以字母 "G" 开始的所有客户：
'%a'    //以a结尾的数据
'a%'    //以a开头的数据
'%a%'    //含有a的数据
‘_a_’    //三位且中间字母是a的
'_a'    //两位且结尾字母是a的
'a_'    //两位且开头字母是a的
```

#### 通配符

| 通配符                       | 描述                       |
| ---------------------------- | -------------------------- |
| %                            | 替代 0 个或多个字符        |
| _                            | 替代一个字符               |
| [*charlist*]                 | 字符列中的任何单一字符     |
| [^*charlist*]或[!*charlist*] | 不在字符列中的任何单一字符 |

**实例**

```sql
SELECT * FROM Websites
WHERE name LIKE '_oogle';
//选取 name 以一个任意字符开始，然后是 "oogle" 的数据
```

正则匹配

```sql
SELECT * FROM Websites
WHERE name REGEXP '^[GFs]';
//选取 name 以 "G"、"F" 或 "s" 开始的数据：
```

#### where规定多个值

IN 操作符允许您在 WHERE 子句中规定多个值。

**实例**

```sql
SELECT * FROM Websites
WHERE name IN ('Google','菜鸟教程');
//句选取 name 为 "Google" 或 "菜鸟教程" 的数据
```

###两个值之间的数据

BETWEEN 操作符选取介于两个值之间的数据范围内的值。这些值可以是数值、文本或者日期。

**实例**

```sql
SELECT * FROM Websites
WHERE alexa BETWEEN 1 AND 20;
//选取 alexa 介于 1 和 20 之间的所有数据
```

NOT BETWEEN 表示不在此范围内

**实例**

```sql
SELECT * FROM Websites
WHERE (alexa BETWEEN 1 AND 20)
AND NOT country IN ('USA', 'IND');

//选取alexa介于 1 和 20 之间但 country 不为 USA 和 IND 的数据
```

```sql
SELECT * FROM Websites
WHERE name NOT BETWEEN 'A' AND 'H';
//选取 name 不介于 'A' 和 'H' 之间字母开始的数据
```

### 别名

可以为表名称或列名称或字段名或函数名指定别名。

```sql
SELECT column_name(s)
FROM table_name AS alias_name;
//表名起别名为'alias_name'

SELECT column_name AS '中文name'
FROM table_name;
//列名起别名为'中文name'
```

### 连接

**join**基于表的共同字段,将多个表连接起来

不同的join

- **INNER JOIN**：如果表中有至少一个匹配，则返回行
- **LEFT JOIN**：即使右表中没有匹配，也从左表返回所有的行
- **RIGHT JOIN**：即使左表中没有匹配，也从右表返回所有的行
- **FULL JOIN**：只要其中一个表中存在匹配，则返回行

**实例**

```sql
SELECT Websites.id, Websites.name, access_log.count, access_log.date
FROM Websites
INNER JOIN access_log
ON Websites.id=access_log.site_id;
//查询 Websites 表的 id,name 和 access_log 表的 count,date
//基于左表 Websites.id 和右表 access_log.site_id
```

​	

###约束

####唯一约束

**UNIQUE** 

对字段加了唯一约束后，字段中数据不可出现重复（当值为null可出现重复）

####非空约束

**NOT NULL**

指示某列不能存储 NULL 值。

####主键约束

**PRIMARY KEY**

非空约束 和 唯一约束的结合。确保某列（或两个列多个列的结合）有唯一标识

####默认值

**DEFAULT** 

规定没有给列赋值时的默认值。

####检查约束

**CHECK** 

保证列中的值符合指定的条件。(mysql不支持)

####外键约束

**FOREIGN KEY**

保证一个表中的数据匹配另一个表中的值的参照完整性。

​	外键需要是其他表的主键

### 自动增长

Auto-increment 会在新记录插入表中时生成一个唯一的数字。

**实例**

```sqlk
CREATE TABLE Persons
(
ID int NOT NULL AUTO_INCREMENT,
LastName varchar(255) NOT NULL,
FirstName varchar(255),
Address varchar(255),
City varchar(255),
PRIMARY KEY (ID)
)

//把 "Persons" 表中的 "ID" 列定义为 auto-increment 主键字段
```

### NULL值

选取列中带有 NULL 值的记录

无法使用比较运算符来测试 NULL 值，比如 =、< 或 <>。

我们必须使用 **IS NULL** 和 **IS NOT NULL** 操作符。

```sql
SELECT LastName,FirstName,Address FROM Persons
WHERE Address IS NULL

//查询Address字段为NULL的数据
```

## 数据类型

在 MySQL 中，有三种主要的类型：Text（文本）、Number（数字）和 Date/Time（日期/时间）类型。

**Text 类型：**

| 数据类型         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| CHAR(size)       | 保存固定长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的长度。最多 255 个字符。 |
| VARCHAR(size)    | 保存可变长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的最大长度。最多 255 个字符。**注释：**如果值的长度大于 255，则被转换为 TEXT 类型。 |
| TINYTEXT         | 存放最大长度为 255 个字符的字符串。                          |
| TEXT             | 存放最大长度为 65,535 个字符的字符串。                       |
| BLOB             | 用于 BLOBs（Binary Large OBjects）。存放最多 65,535 字节的数据。 |
| MEDIUMTEXT       | 存放最大长度为 16,777,215 个字符的字符串。                   |
| MEDIUMBLOB       | 用于 BLOBs（Binary Large OBjects）。存放最多 16,777,215 字节的数据。 |
| LONGTEXT         | 存放最大长度为 4,294,967,295 个字符的字符串。                |
| LONGBLOB         | 用于 BLOBs (Binary Large OBjects)。存放最多 4,294,967,295 字节的数据。 |
| ENUM(x,y,z,etc.) | 允许您输入可能值的列表。可以在 ENUM 列表中列出最大 65535 个值。如果列表中不存在插入的值，则插入空值。**注释：**这些值是按照您输入的顺序排序的。可以按照此格式输入可能的值： ENUM('X','Y','Z') |
| SET              | 与 ENUM 类似，不同的是，SET 最多只能包含 64 个列表项且 SET 可存储一个以上的选择。 |

**Number 类型：**

| 数据类型        | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| TINYINT(size)   | 带符号-128到127 ，无符号0到255。                             |
| SMALLINT(size)  | 带符号范围-32768到32767，无符号0到65535, size 默认为 6。     |
| MEDIUMINT(size) | 带符号范围-8388608到8388607，无符号的范围是0到16777215。 size 默认为9 |
| INT(size)       | 带符号范围-2147483648到2147483647，无符号的范围是0到4294967295。 size 默认为 11 |
| BIGINT(size)    | 带符号的范围是-9223372036854775808到9223372036854775807，无符号的范围是0到18446744073709551615。size 默认为 20 |
| FLOAT(size,d)   | 带有浮动小数点的小数字。在 size 参数中规定显示最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DOUBLE(size,d)  | 带有浮动小数点的大数字。在 size 参数中规显示定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| DECIMAL(size,d) | 作为字符串存储的 DOUBLE 类型，允许固定的小数点。在 size 参数中规定显示最大位数。在 d 参数中规定小数点右侧的最大位数。 |

> **注意：**以上的 size 代表的并不是存储在数据库中的具体的长度，如 int(4) 并不是只能存储4个长度的数字。
>
> 实际上int(size)所占多少存储空间并无任何关系。int(3)、int(4)、int(8) 在磁盘上都是占用 4 btyes 的存储空间。就是在显示给用户的方式有点不同外，int(M) 跟 int 数据类型是相同的。
>
> 例如：
>
> 1、int的值为10 （指定zerofill）
>
> ```
> int（9）显示结果为000000010
> int（3）显示结果为010
> ```
>
> 就是显示的长度不一样而已 都是占用四个字节的空间

**Date 类型：**

| 数据类型    | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| DATE()      | 日期。格式：YYYY-MM-DD**注释：**支持的范围是从 '1000-01-01' 到 '9999-12-31' |
| DATETIME()  | *日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS**注释：**支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' |
| TIMESTAMP() | *时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的秒数来存储。格式：YYYY-MM-DD HH:MM:SS**注释：**支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()      | 时间。格式：HH:MM:SS**注释：**支持的范围是从 '-838:59:59' 到 '838:59:59' |
| YEAR()      | 2 位或 4 位格式的年。**注释：**4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。 |

*即便 DATETIME 和 TIMESTAMP 返回相同的格式，它们的工作方式很不同。在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。

## 函数

参考:http://www.runoob.com/sql/sql-function.html

###聚合函数

- AVG() - 返回平均值
- COUNT() - 返回行数
- FIRST() - 返回第一个记录的值
- LAST() - 返回最后一个记录的值
- MAX() - 返回最大值
- MIN() - 返回最小值
- SUM() - 返回总和

###标准函数

- UCASE() - 将某个字段转换为大写
- LCASE() - 将某个字段转换为小写
- MID() - 从某个文本字段提取字符，MySql 中使用
- SubString(字段，1，end) - 从某个文本字段提取字符
- LEN() - 返回某个文本字段的长度
- ROUND() - 对某个数值字段进行指定小数位数的四舍五入
- NOW() - 返回当前的系统日期和时间
- FORMAT() - 格式化某个字段的显示方式

###分组函数

GROUP BY 语句用于结合聚合函数，根据一个或多个列对结果集进行分组。

