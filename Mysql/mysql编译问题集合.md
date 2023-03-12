启动MySQL的方法： 1.  /etc/init.d/mysqld start

​                   2.、usr/local/mysql/bin/mysqld_safe--user=mysql  &

​                   3.，service mysqld start

关闭MySQL的方法： 1./etc/init.d/mysqld stop

​                    2.service mysqld stop

重启MySQL服务的方法： 1.service mysqld restart

终止MySQL进程： killall  mysqld  重复使用这条命令，知道没有MySQL进程可关闭。

MySQL本地连接：mysql  -u  root  -p

##### 解决mysql.h no such file or directory


原因：没有安装mysql开发库:

```
yum install mysql-devel
```

##### 编译报错

```
g++编译后加 `mysql_config --cflags --libs`
```

##### 多次mysql_query()出现错误

解决：每次query之后需要 (MYSQL_RES *)result = mysql_store_result(mysql);

出现如下错误为无权限

```
3248:M 21 Sep 19:00:01.100 * 1 changes in 3600 seconds. Saving...
3248:M 21 Sep 19:00:01.101 * Background saving started by pid 6788
6788:C 21 Sep 19:00:01.101 # Failed opening the RDB file dump.rdb (in server root dir /usr/local/redis) for saving: Permission denied
3248:M 21 Sep 19:00:01.203 # Background saving error
3248:M 21 Sep 19:00:07.041 * 1 changes in 3600 seconds. Saving...
3248:M 21 Sep 19:00:07.041 * Background saving started by pid 6792
6792:C 21 Sep 19:00:07.042 # Failed opening the RDB file dump.rdb (in server root dir /usr/local/redis) for saving: Permission denied
3248:M 21 Sep 19:00:07.142 # Background saving error
```

查看redis文件夹是否有写权限，没有就增加写权限(或者/redis/src文件夹)

```
sudo chmod 777 dir_name
```

###### 字符串函数

| 函数                  | 功能                                                 |
| --------------------- | ---------------------------------------------------- |
| CONCAT(S1, S2, ...Sn) | 连接S1,S2,...Sn为一个字符串                          |
| INSERT(str,x,y,instr) | 将字符串str从x位置开始，y个字符串长的字串替换成instr |
| LOWER(str)            | 将字符串str中所有字符变成小写                        |
| UPPER(str)            | 将字符串str中所有字符变成大写                        |
| LEFT(str, x)          | 返回字符串str最左边的x个字符                         |
| RIGHT(str, x)         | 返回字符串str最右边的x个字符                         |
| LPAD(str, n, pad)     | 用pad对str最左边进行填充，直到长度为n个字符长度      |
| RPAD(str, n, pad)     | 用pad对str最右边进行填充，直到长度为n个字符长度      |
| LTRIM(str)            | 去掉字符串str左侧的空格                              |
| RTRIM(str)            | 去掉字符串str右侧的空格                              |
| REPLACE(str, a, b)    | 用字符串b替换字符串str中所有出现的字符串a            |
| REOEAT(str, x)        | 返回str重复x次的结果                                 |
| STRCMP(s1,s2)         | 比较s1和s2                                           |
| TRIM(str)             | 去掉字符串行尾和航头的空格                           |
| SUBSTRING(str, x, y)  | 返回字符串str第x位置y长度的字串                      |

###### 数值函数

| 函数          | 功能                             |
| ------------- | -------------------------------- |
| ABS(x)        | 返回x绝对值                      |
| CEIL(x)       | 返回大于x的最小整数值            |
| FLOOR(x)      | 返回小于x的最大整数值            |
| MOD(x,y)      | 返回x/y的模                      |
| RAND()        | 返回0~1的随机值                  |
| ROUND(x,y)    | 返回参数x的四舍五入的y位小数的值 |
| TRUNCATE(x,y) | 返回数字x截断为y位小数的结果     |

###### 日期时间函数

| 函数                                | 功能                                           |
| ----------------------------------- | ---------------------------------------------- |
| CURDATE()                           | 返回当前日期                                   |
| CURTIME()                           | 返回当前时间                                   |
| NOW()                               | 返回当前的日期和时间                           |
| UNIX_TIMESTAMP(data)                | 返回日期date的UNIX时间戳                       |
| FROM_UNIXTIME                       | 返回UNIX时间戳的日期值                         |
| WEEK(date)                          | 返回日期date为一年中的第几周                   |
| YEAR(date)                          | 返回日期date的年份                             |
| HOUR(time)                          | 返回time的小时值                               |
| MINUTE(time)                        | 返回time的分钟值                               |
| MONTHNAME(date)                     | 返回time的月份名                               |
| DARE_FORMAT(date,fmt)               | 返回按字符串fmt格式化日期date值                |
| DATE_ADD(date, INTERVAL, expr type) | 返回一个日期或者时间值加上一个时间间隔的时间值 |
| DATEDIFF(expr, expr2)               | 返回起始时间expr和结束时间expr2之间的天数      |

###### 流程函数

| 函数                                                         | 功能                                         |
| ------------------------------------------------------------ | -------------------------------------------- |
| IF(value, t f)                                               | 如果value是真，返回t，否则返回f              |
| IFNULL(value1, value2)                                       | 如果value1不为空，返回value1，否则返回value2 |
| CASE WHEN [value1] THEN [result]...ELSE [default] END        | 如果value1是真，返回result，否则返回default  |
| CASE [expr] WHEN [value1] THEN [result] ...ELSE [default] END | 如果expr为真，返回result, 否则返回default    |

###### 其他函数

| 函数           | 功能                    |
| -------------- | ----------------------- |
| DATAVASE()     | 返回当前数据库名        |
| VERSION()      | 返回当前数据库版本      |
| USER()         | 返回当前登陆用户名      |
| INET_ATON(IP)  | 返回IP地址的数字表示    |
| INET_NTOA(num) | 返回数字代表的IP地址    |
| PASSWORD(str)  | 返回字符串str的加密版本 |
| MD5()          | 返回字符串str的MD5值    |



##### 数据库、表设计规范

- 存储引擎：Innodb

  原因：支持事务、行锁、并发性能好。

- 编码规范：UTF-8

- 表设计规范

  **必须主键：**主键递增，提高写入性能，减少碎片。

  **禁止使用外键：**降低表之间耦合，不涉及更新操作的级联，并发高情况极度影响SQL性能。

- 字段设计规范

  **必须有注释。**

  **必须NOT NULL：**null的列不能使用索引。

  **整形：**默认int(11)0。int(11)表示显示长度，勾选无符号unsigned并且填充零zerofill后，长度不够11位则会自动补零，1->00000000001。

  **字符串：**默认空字符串。

  **时间：**非current_timestamp默认'1970-01-01 08:00:01'，date类型无时分秒。

  **通用字段：**

  **create_time(created_at)：**创建时间，默认current_timestamp。

  **update_time(updated_at)：**更新时间，默认current_timestamp，on update current_timestamp

  **is_deleted：**逻辑删除标志位，视情况选择。

  **禁止使用text\blob：**浪费磁盘和内存空间，影响数据库性能。

  **金额禁止使用小数：**尽量使用分或者更小的单位用整数存储，否则精度问题很麻烦。

- 命名规则

  **表、列：**使用业务模块开头，如tb_order，列名以下划线分割。

  **索引：**create_time、update_time必须包含索引

  **主键索引：**数据库自动

  **唯一索引、组合唯一索引：**uk_colName_colName

  **普通索引、组合普通索引：**idx_colName_colName

  **建表示例：**

  ```sql
  CREATE TABLE `user` (
  `id` int unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(128) NOT NULL DEFAULT '' COMMENT '用户名',
  `ccert_no` varchar(64) NOT NULL DEFAULT '' COMMENT '身份证号',
  `gender` tinyint NOT NULL DEFAULT '0' COMMENT '性别0女1男',
  `active_date` date NOT NULL DEFAULT '1970-01-01 08:00:00' COMMENT '失效时间',
  `create_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'.
  `update_time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_cert_no` (`cert_no`) USING BTREE,
  KEY `idx_username_gander` (`username`, `gender`) USING BTREE
  ) BEGINE=InnoDB DEFAULT CHARSET=utf-8;
  ```

###### TEXT与BLOB

用途：保存较大文本，BLOB可以保存二进制数据，TEXT只能保存字符数据。BLOB和TEXT会引起性能问题，在执行删除操作之后表中会留下很大的"空洞",以后填入这些"空洞"性能会受到影响，所以要定期使用 `OPTIMIZE TABLE`功能对这类表进行碎片化整理。

###### 关键字

`distinct`：返回唯一的某行数据

`limit`：限制语句返回行数，`limit 1, 1`检索出的是第二行，MYSQL 5的limit语法，`LIMIT 3, 4`是从行3开始的4行，`LIMIT 4 OFFSET 3`意为从行3开始取4行。

`ORDER BY`：可将select检索出的数据进行排序，默认为升序；多列排序 `ORDER BY col1, col2`仅在多个行具有相同的col1值时才对col2进行排序。

`DESC`：按降序排序，可以多个列中使用，`ORDER BY col1 DESC, col2`，DESC关键字只应用于直接位于其前面的列名，与DESC相反的关键字时ASC。

> `ORDER BY`和`LIMIT`的组合：再给出`ORDER BY`时应该保证它在`FROM`之后，若使用`LIMIT`它必须位于`ORDER BY`之后。

`WHERE 中 BETWEEN`：可以检索范围值，`WHERE tablename BETWEEN 10 AND 20;

`WHERE 中 AND 和 OR`：and的优先级高于or

`IN`：in操作符来指定条件范围，范围中的每个条件都可以进行匹配，in取合法值得由逗号分隔得清单，全部在圆括号中。`WHERE col  IN (1000, 2000)`.

`NOT`：WHERE子句中得NOT操作符有且只有一个功能，那就是否定它之后所跟得任何条件。

`%`：匹配多个字符，ma% 匹配ma开头的字符串，%ma%表示匹配任何位置包含ma得字符串，%ma匹配ma结尾得字符串

`_`：匹配单个字符，匹配规则和%相似但是仅仅匹配一个字符

###### 聚集函数

`AVG()`:返回某列平均值，会忽略列值为NULL的行
`COUNT()`:返回某列的行数，`COUNT(*) `对表中行中数目进行计数，无论NULL还是非空值，`COUNT(column)`对特定列进行技术，忽略NULL值。
`MAX()`:返回某列最大值，忽略列值为NULL的行，可以找出最大的数值或日期值。
`MIN()`:返回某列的最小值，忽略列值为NULL的行，可以找出最小的数值或日期值。
`SUM()`:返回某列值之和

###### 分组数据

`GROUP BY`: 

```mysql
SELECT id, COUNT(*) AS num_prods
FROM products
GROUP BY id;
```

- `GROUP BY`必须出现在`WHERE`之后，`ORDER BY`之前；
- 若分组列中有NULL值，则NULL将作为一个分组返回，列中有多行NULL值，他们将分为一组；
- `GROUP BY`子句可以包含任意数目的列，这使得能对分组进行嵌套，为数据分组提供更细致的控制；
- 若在`GROUP BY`子句中嵌套了分组，数据将在最后规定的分组上汇总；
- `GROUP BY`子句中列出的每个列都必须是检索列或有效的表达式，如果select中使用表达式，则必须在`GROUP BY`子句中指定相同的表达式。

`HAVING`：WHERE过滤行而HAVING过滤分组；

> HAVING和WHERE的差别 WHERE在数据分组前进行过滤，HAVING在数据分组后进行过滤，这是重要的区别，WHERE排除的行不包括在分组中。

如果需要排序不要依赖`GROUP BY`子句的排序，要使用 `ORDER BY`来限制。

**例子：**

```mysql
SELECT cust_name, 
        cust_state,
        (SELECT COUTN(*)
         FROM orders 
         WHERE orders.cust_id = customers.cust_id) AS orders
FORM customers
ORDER BY cust_name;
```

**分析：**这条语句对customers表中每个客户返回3列：cust_name，cust_state和orders orders是计算字段，他是子查询建立的，该子查询对检索出的每个客户执行一次，在此例子中执行了5次，原因是检索出了5个客户。