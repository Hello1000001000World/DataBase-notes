### 函数

#### 匹配的个数

**COUNT(列名)**

##### 实例

```sql
SELECT COUNT(a) FROM 表名
WHERE site_id=3;					//返回 site_id=3 且列名为 a 的数据个数

SELECT COUNT(*) FROM 表名;			//返回表中的记录数
```

#### 指定列第一个值

**LIMIT 1**

##### 实例

```sql
SELECT 列名 FROM 表名 LIMIT 2;		  //返回前两行数据
```

#### 最大值,最小值,求和,平均值

**max(列名)**  **min(列名)**  **sum(列名)**  **AVG(列名)**

##### 实例

```sql
SELECT max(列名) FROM 表名;		  //返回该列最大值
SELECT min(列名) FROM 表名;		  //返回该列最小值
SELECT sum(列名) FROM 表名;		  //返回该列数值总和
SELECT avg(列名) FROM 表名;		  //返回该列所有数据的平均值
```

#### 大小写

**UCASE()**  **LCASE()**

##### 实例

```sql
SELECT UCASE(列) FROM 表名;		//将该列数据输出为大写,不会改变数据库
SELECT LCASE(列) FROM 表名;		//将该列数据输出为小写,不会改变数据库
```

#### 字符串截取输出

**MID(列名,起始位置,长度)**

##### 实例

```sql
SELECT MID(列名,start[,length]) FROM 表名;	
//start	 必需。规定开始位置（起始值是 1）。
//length 可选。要返回的字符数。如果省略，则 MID() 函数返回剩余文本。
例:mid(列名,1,4)		//该列值为'GOOGLE'的数据,输出为'GOOG'
```

#### 数据字符个数

**LENGTH(列名)**

##### 实例

```sql
SELECT LENGTH(列名) FROM 表名;
SELECT id,LENGTH(url) FROM 表;	//返回该id的url字符长度
```

#### 四舍五入

**ROUND(列名,小数位数)**

##### 实例

```sql
SELECT ROUND(列名,小数位数) FROM 表名;	//返回该列数据并四舍五入
```

#### 当前时间

**NOW()**

------------

#### 格式化字段

**format(列名,格式)**

##### 实例

```sql
DATE_FORMAT(Now(),'%Y-%m-%d') as date	//返回2018-08-18
```



#### 多表连接

**GROUP BY**

---------------------

##### having





## 数据类型

### Text 类型：

| 数据类型         | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| char(size)       | 保存固定长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的长度。最多 255 个字符。 |
| varchar(size)    | 保存可变长度的字符串（可包含字母、数字以及特殊字符）。在括号中指定字符串的最大长度。最多 255 个字符。**注释：**如果值的长度大于 255，则被转换为 TEXT 类型。 |
| tinytext         | 存放最大长度为 255 个字符的字符串。                          |
| text             | 存放最大长度为 65,535 个字符的字符串。                       |
| blob             | 用于 BLOBs（Binary Large OBjects）。存放最多 65,535 字节的数据。 |
| MEDIUMTEXT       | 存放最大长度为 16,777,215 个字符的字符串。(16MB)             |
| MEDIUMBLOB       | 用于 BLOBs（Binary Large OBjects）。存放最多 16,777,215 字节的数据。 |
| LONGTEXT         | 存放最大长度为 4,294,967,295 个字符的字符串。(4GB)           |
| LONGBLOB         | 用于 BLOBs (Binary Large OBjects)。存放最多 4,294,967,295 字节的数据。 |
| ENUM(x,y,z,etc.) | 允许您输入可能值的列表。可以在 ENUM 列表中列出最大 65535 个值。如果列表中不存在插入的值，则插入空值。**注释：**这些值是按照您输入的顺序排序的。可以按照此格式输入可能的值： ENUM('X','Y','Z') |
| SET              | 与 ENUM 类似，不同的是，SET 最多只能包含 64 个列表项且 SET 可存储一个以上的选择。 |

### Number 类型：

| 数据类型        | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| tinyint(size)   | 带符号-128到127 ，无符号0到255。                             |
| smallint(size)  | 带符号范围-32768到32767，无符号0到65535, size 默认为 6。     |
| mediumint(size) | 带符号范围-8388608到8388607，无符号的范围是0到16777215。 size 默认为9 |
| int(size)       | 带符号范围-2147483648到2147483647，无符号的范围是0到4294967295。 size 默认为 11 |
| bigint(size)    | 带符号的范围是-9223372036854775808到9223372036854775807，无符号的范围是0到18446744073709551615。size 默认为 20 |
| float(size,d)   | 带有浮动小数点的小数字。在 size 参数中规定显示最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| double(size,d)  | 带有浮动小数点的大数字。在 size 参数中规显示定最大位数。在 d 参数中规定小数点右侧的最大位数。 |
| decimal(size,d) | 作为字符串存储的 DOUBLE 类型，允许固定的小数点。在 size 参数中规定显示最大位数。在 d 参数中规定小数点右侧的最大位数。 |

int(M) 跟 int 数据类型是相同的,只是显示不同

int的值为10 （指定zerofill）

```
int（9）显示结果为000000010
int（3）显示结果为010
```

### Date 类型:

| 数据类型    | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| DATE()      | 日期。格式：YYYY-MM-DD**注释：**支持的范围是从 '1000-01-01' 到 '9999-12-31' |
| DATETIME()  | *日期和时间的组合。格式：YYYY-MM-DD HH:MM:SS**注释：**支持的范围是从 '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59' |
| TIMESTAMP() | *时间戳。TIMESTAMP 值使用 Unix 纪元('1970-01-01 00:00:00' UTC) 至今的秒数来存储。格式：YYYY-MM-DD HH:MM:SS**注释：**支持的范围是从 '1970-01-01 00:00:01' UTC 到 '2038-01-09 03:14:07' UTC |
| TIME()      | 时间。格式：HH:MM:SS**注释：**支持的范围是从 '-838:59:59' 到 '838:59:59' |
| YEAR()      | 2 位或 4 位格式的年。**注释：**4 位格式所允许的值：1901 到 2155。2 位格式所允许的值：70 到 69，表示从 1970 到 2069。 |

*即便 DATETIME 和 TIMESTAMP 返回相同的格式，它们的工作方式很不同。在 INSERT 或 UPDATE 查询中，TIMESTAMP 自动把自身设置为当前的日期和时间。TIMESTAMP 也接受不同的格式，比如 YYYYMMDDHHMMSS、YYMMDDHHMMSS、YYYYMMDD 或 YYMMDD。

## 插入

**insert into**

### 实例

```sql
INSERT INTO 表名 (列名1, 列名2, 列名3, ...)
VALUES ('值1','值2','值3',...);
```



## 删除

**Delete** from 表名  Where 条件语句

### 实例

```sql
DELETE FROM Websites
WHERE name='百度' AND country='CN';
```



## 更改

**UPDATE 表名 Set** 语句用于更新表中的记录。

### 实例

```sql
UPDATE Websites 
SET alexa='5000', country='USA' 
WHERE name='菜鸟教程';
//将Websites表中 name='菜鸟教程' 的列alexa改为'5000', country改为'USA' 
```



## 查询

**SELECT 列名 FROM 表名 [where]\[LIMIT N][offset M]**

- 使用星号（*），SELECT语句会返回表的所有字段数据
- WHERE 语句来包含任何条件。
  - 使用 AND 或者 OR 指定一个或多个条件。
  - 操作符 "<="  ">="  "="  "!=" "<" ">"
  - WHERE BINARY 区分大小写
- LIMIT 属性来设定返回的记录数。
- OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。

### where子句

#### 区分大小写

**WHERE BINARY** 

##### 实例

```sql
SELECT * from runoob_tbl WHERE BINARY runoob_author='runoob.com';
//无数据
SELECT * from runoob_tbl WHERE BINARY runoob_author='RUNOOB.COM';
+-----------+---------------+---------------+-----------------+
| runoob_id | runoob_title  | runoob_author | submission_date |
+-----------+---------------+---------------+-----------------+
| 3         | JAVA 教程   | RUNOOB.COM    | 2016-05-06      |
| 4         | 学习 Python | RUNOOB.COM    | 2016-03-06      |
+-----------+---------------+---------------+-----------------+
```

#### like 模糊匹配

##### 实例

```sql
SELECT * from runoob_tbl  WHERE runoob_author LIKE '%COM';
+-----------+---------------+---------------+-----------------+
| runoob_id | runoob_title  | runoob_author | submission_date |
+-----------+---------------+---------------+-----------------+
| 3         | 学习 Java   | RUNOOB.COM    | 2015-05-01      |
| 4         | 学习 Python | RUNOOB.COM    | 2016-03-06      |
+-----------+---------------+---------------+-----------------+
'%a'     //以a结尾的数据
'a%'     //以a开头的数据
'%a%'    //含有a的数据
'_a_'    //三位且中间字母是a的
'_a'     //两位且结尾字母是a的
'a_'     //两位且开头字母是a的
```



### 结果集操作

#### 多个结果集合成一个

**union**  		结果集去掉重复数据

**union all** 	结果集全部数据

##### 实例

```sql
SELECT country FROM Websites
UNION
SELECT country FROM apps

//返回Websites表和apps表的country列
```

#### 结果集排序

**order by**

##### 实例

```sql
SELECT * from tab ORDER BY a_data ASC;	//升序
SELECT * from tab ORDER BY a_data DESC;	//降序
```

#### 相同字段分组

SELECT  列名,[函数()]  FROM  表名 **group by**  列名

+ 结果集将列名相同的数据分组
+ 函数为可选参数 , 可以操作分组结果集数据
+ 汇总数据WITH ROLLUP (可选)

##### 实例

```sql
+----+--------+---------------------+--------+
| id | name   | date                | singin |
+----+--------+---------------------+--------+
|  1 | 小明 | 2016-04-22 15:25:33 |      1 |
|  2 | 小王 | 2016-04-20 15:25:47 |      3 |
|  3 | 小丽 | 2016-04-19 15:26:02 |      2 |
|  4 | 小王 | 2016-04-07 15:26:14 |      4 |
|  5 | 小明 | 2016-04-11 15:26:40 |      4 |
|  6 | 小明 | 2016-04-04 15:26:54 |      2 |
+----+--------+---------------------+--------+
SELECT name, COUNT(*) FROM  tb group by name;
//结果为
+--------+----------+
| name   | COUNT(*) |
+--------+----------+
| 小丽 |        1 |
| 小明 |        3 |
| 小王 |        2 |
+--------+----------+
SELECT name, COUNT(*) FROM  tb group by name WITH ROLLUP;	//加可选参数WITH ROLLUP
//结果为
+--------+----------+
| name   | COUNT(*) |
+--------+----------+
| 小丽 |        1 |
| 小明 |        3 |
| 小王 |        2 |
| NULL |	   6 |
+--------+----------+
SELECT coalesce(name, '总数'), COUNT(*) FROM  tb group by name WITH ROLLUP;	//给NULL起个名字
//结果为
+--------+----------+
| name   | COUNT(*) |
+--------+----------+
| 小丽 |        1 |
| 小明 |        3 |
| 小王 |        2 |
| 总数 |	      6	|
+--------+----------+

```



