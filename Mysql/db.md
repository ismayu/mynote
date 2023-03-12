## 连接函数
**函数1：初始化库**
```
int mysql_library_init(int argc, char **argv, char **groups)
void mysql_library_end(void)
```
函数mysql_library_init和mysql_library_end是一对用于初始化和释放的函数。
mysql_library_init需要在调用其他函数之前调用，用来初始化MySQL库。而mysql_library_end调用可以释放内存，在调用mysql_close之后调用，以帮助释放内存数据。

**函数2：创建一个数据库对象指针**
```
MYSQL *mysql_init(MYSQL *mysql)
```
该函数有两种用法：一种是传入MySQL对象；另一种是传入nullptr参数。两种调用都会返回MySQL对象指针，如果有传入值，传入返回的对象就是同一个；如果传入的参数为nullptr，就会返回一个实例。这两者的差别在于谁来管理MySQL对象的实例，如果是在外部创建的对象，在调用mysql_close之后就需要手动释放该对象，以免造成内存泄漏。如果是由mysql_init创建的对象，就只需要关闭连接。

**函数3：销毁现有对象指针**
```
void mysql_close(MYSQL *mysql)
```
该函数用于关闭一个连接，如果MySQL实例是库生成的，该函数就会同时释放该对象。

**函数4：连接函数**
```
MYSQL *mysql_real_connect(MYSQL *mysql, const char *host, const char *user, const char *passwd, const char *db, unsigned int port, const char *unix_socket, unsigned long client_flag)
```
在这个函数中，第5个参数为想要连接的数据库的名字，可以为nullptr，表示只是想产生一个连接，但不选择具体数据库，后续调用其他函数选定具体的数据库。
当我们连接一个给定名字的数据库时，如果这个数据库不存在，就会出现错误，而MySQL对象也不可再用，需要关闭。鉴于这种情况，在第一次连接时就可以将数据库名设为nullptr，让它进行一个默认的连接，再调用mysql_select_db函数进行数据库的选择，若选择失败，则说明所需的数据库不存在，这时可以创建需要的数据库。
在连接函数中，最后一个参数client_flag是一系列的常量，在本书的示例程序中使用了CLIENT_FOUND_ROWS，官方的解释如下：
CLIENT_FOUND_ROWS: Return the number of found (matched) rows, not the number of changed rows
即返回匹配的行数，而不是修改行数。
若函数mysql_real_connect的返回值是nullptr，则连接失败；若成功，则返回值为MySQL的对象指针。

**函数5：设置属性**
```
int mysql_options(MYSQL *mysql, enum mysql_option option, const void *arg)
```
该函数是在mysql_init之后、mysql_real_connect之前调用的，对MySQL对象进行一些属性设置。在本书的框架中用到了两个常用的属性：
· MYSQL_OPT_CONNECT_TIMEOUT：设置连接超时。
· MYSQL_OPT_RECONNECT：是否自动连接。
更多的枚举可以在头文件mysql.h中查看。

**函数6：选择数据库**
```
int mysql_select_db(MYSQL *mysql, const char *db)
```
该函数输入一个需要连接的数据库名。返回值0表示成功，非0即为出错编号。该函数指定一个数据库作为当前选中的数据库。在调用该函数时，如果用户没有指定数据库的权限就会出错。

**函数7：错误代码**
```
unsigned int mysql_errno(MYSQL *mysql)
```
在每一个函数调用之后，如果出现错误，就可以通过调用函数mysql_errno得到当前错误的编号。

**函数8：ping函数**
```
int mysql_ping(MYSQL *mysql)
```
该函数检查连接是否处于正常工作中。
在函数mysql_options的设置中，可以打开自动连接的开关，当网络断开时，调用mysql_ping会自动重新连接

## 预处理函数


**函数1：创建一个预处理**
```
MYSQL_STMT *mysql_stmt_init(MYSQL *mysql)
```
MySQL库的预处理使用一个名为MYSQL_STMT的结构，调用该函数即可创建一个MYSQL_STMT指针，返回值不是nullptr时则为成功。

**函数2：销毁一个预处理**
```
my_bool mysql_stmt_close(MYSQL_STMT *stmt)
```
该函数销毁传入的MYSQL_STMT指针。
**函数3：初始化预处理**
```
int mysql_stmt_prepare(MYSQL_STMT *stmt, const char *stmt_str, unsigned long length)
```
该函数将一个SQL语句写入预处理结构中，第二个参数即为SQL语句，该语句不需要有结束符分号，即不需要符号“；”。传入的SQL语句在参数的位置用“？”代替。
调用该函数，返回值为非0时，表示有错误，可以用mysql_stmt_error函数查看错误编码。
**函数4：出错检查**
```
const char *mysql_stmt_error(MYSQL_STMT *stmt)
```
一旦发现某个预处理函数有异常或出错，就可以通过调用该函数来获取错误描述。
**函数5：绑定参数**
```
my_bool mysql_stmt_bind_param(MYSQL_STMT *stmt, MYSQL_BIND *bind)
```
前面传入的SQL语句中，关于动态参数的部分是用“？”来代替的。而函数mysql_stmt_bind_param是专门为这些“？”准备的，利用MYSQL_BIND结构提供参数。像函数一样，一个预处理在实际执行阶段需要绑定实际的参数。
**函数6：执行**
```
int mysql_stmt_execute(MYSQL_STMT *stmt)
```
绑定完参数之后的预处理指针就可以调用执行函数来执行。如果返回结果不为0，就表示有错误。

**函数7：获取执行结果个数**
```
my_ulonglong mysql_stmt_affected_rows(MYSQL_STMT *stmt)
```
预处理执行之后，我们可以通过该函数来获取结果行的个数，即当前执行的预处理结果中有多少行数据

## 查询数据
**函数1：查询函数**
```c++
int mysql_query(MYSQL *mysql, const char *stmt_str)
int mysql_real_query(MYSQL *mysql, const char *stmt_str, unsigned long length)
```
库中提供了两个查询函数。函数mysql_query执行指定的SQL语句，参数stmt_str可以不带SQL语句的结束符“；”，但必须是有结束符的字符串，即最后以'\0'字符结尾。mysql_query函数使用的SQL语句不能带二进制数据，如果需要带二进制数据，就需要使用函数mysql_real_query。从函数定义上能看得出来，mysql_real_query函数执行SQL语句的时候，使用的参数是char*和它的长度。这个char*的字符串是允许存在'\0'这种结束符的。这就是这两个函数本质上的区别。
如果要查看执行该语句后的结果，那么可以在mysql_query调用之后调用函数mysql_field_count查看有多少列数据。如果执行的语句不是一个select，那么mysql_field_count调用的结果可能为0。另外，执行操作也可以由mysql_query来完成，即传入的SQL语句可以不是一个select语句。
**函数2：读取结果**

```c++
MYSQL_RES *mysql_store_result(MYSQL *mysql)
```
调用mysql_query函数之后可以用mysql_store_result得到结果，该函数将全部结果缓存到MYSQL_RES结构中并返回，MYSQL_RES用完之后需要使用mysql_free_result释放数据。
函数mysql_store_result返回为空时，不意味着失败。如果执行语句是insert语句，mysql_store_result就会返回空，因为insert语句并没有集合可以返回。
**函数3：获取结果中有多少列**

```
unsigned int mysql_num_fields(MYSQL_RES *result)
```
调用函数mysql_store_result的结果不为空时，可以调用mysql_num_fields来判断有多少列。
**函数4：读取字段**
```
MYSQL_FIELD *mysql_fetch_field(MYSQL_RES *result)
```
该函数的使用相当于一个迭代器，对MYSQL_RES的列数据进行一个迭代，当返回值为空时表示没有更多的列了。
**函数5：获取行**
```
MYSQL_ROW mysql_fetch_row(MYSQL_RES *result)
```
同理，函数mysql_fetch_row也是一个迭代器，迭代的是MYSQL_RES集合，也就是mysql_store_result得到的集合