`settable`声明：

```c++
void lua_settable(lua_Value a, lua_Value k, lua_Value v)
```

缺点：lua会做垃圾收集，由于lua语言引擎不知道lua中的一个表可能会被保存在C语言变量中，因此它可能错误的认为这个表是垃圾并将其回收。

###### 压入元素

```c++
void lua_pushnul(lua_State *L);
void lua_pushboolean(lua_State *L, int bool);
void lua_pushnumber(lua_State *L, lua_Number n);  // lua_Number为lua中的浮点数默认double
void lua_pushinteger(lua_State *L, lua_Integer n);  // lua_Integer为lua中整形一般为long long
void lua_pushlstring(lua_State *L, const char *s, size_t len);
void lua_pushstring(lua_State *L, const char *s);
```

###### 查询元素

  检查栈中的一个元素是否为特定的类型，C API提供了名为lua_is*的函数，原型为:

```c++
int lua_is*(lua_State *L, int index);
```

`lua_type`用于返回栈中元素的类型。

函数`lua_to*`用于从栈中获取一个值

```c++
int lua_toboolean(Lua_State *L, int index); // nil和false转换为0，其他所有值转为1
const char *lua_tolstring(lua_State *L, index, size_t *len); //对于类型不正确的值，返回NULL
lua_State *lua_tothread(lua_State *L, int index); //对于类型不正确的值，返回NULL
lua_Number lua_tonumber(lua_State *L, int index);
lua_Integetr lua_tointeger(lua_State *L, int index);
```

数值相关的函数无法提示数值的类型错误，因此只能简单的返回0，Lua5.2引入了如下的新函数：

```c++
lua_Number lua_tonumberx(lua_State *L, int idx, int *isnum);
lua_Integer lua_tointegerx(lua_State *L, int idx, int *isnum);
```

出口参数isnum返回了一个布尔值，来表示lua值是否被强制转为期望的类型。

###### 其他栈操作

```c++
// 返回栈中元素个数
int lua_gettop     (lua_State *L);
// 将栈顶设置为一个指定的值，栈顶>index元素被丢弃，栈顶<index压入nil值
// #define lua_pop(L, n) lua_settop(L, -(n) - 1)
void lua_settop    (lua_State *L, int index);
// 用于将指定索引上的元素的副本压入栈
void lua_pushvalue (lua_State *L, int index);
// 将指定索引的元素向栈顶转动n个位置，n为正表示将元素向栈顶方向转动，n为负则表示向相反的方向转动
void lua_rotate    (lua_State *L, int index, int n);
// 用于删除指定索引的元素，并将该位置之上的所有元素下移填补空缺 
// (lua_rotate(L, (idx), -1), lua_pop(L, 1))
void lua_remove    (lua_State *L, int index);
// 用于将栈顶元素移动到指定位置，并上移指定位置之上的所有元素以开辟出一个元素的空间
// lua_rotate(L, (idx), 1)
void lua_insert    (lua_State *L, int index);
// 将当前栈顶元素设置为指定索引上的值，并且弹出栈顶元素
void lua_replace   (lua_State *L, int index);
// 将fromidx的值覆盖到toidx，原值不受影响
void lua_copy      (lua_State *L, int fromidx, int toidx);
```

###### 操作表

```lua
-- lua file
-- background = { red = 0.30, green = 0.10, blue = 0}
BLUE = { red = 0, gree = 0, blue = 1.0}
background = BLUE
```

```c++
lua_getglobal(L, "background");
if(!lua_istable(L, -1))
    error(L, "'background' is not a table");
red = getcolorfield(L, "red");
green = getcolorfield(L, "green");
blue = getcolorfield(L, "blue");

#define MAX_COLOR 255
int getcolorfield(lua_State *L, const char *key)
{
    int result, isnum;
    lua_pushstring(L, key);
    lua_gettable(L, -2);
    result = (int)(lua_tonumberx(L, -1 &isnum) * MAX_COLOR);
    if(!isnum)
        error(L, "invaliid component '%s' in color", key);
    lua_pop(L, 1);
    return result;
}
```



```c++
lua_getglobal(L, "background");
if(lua_isstring(L, -1))
{
    const char *name = lua_tostring(L, -1);
    int i;
    for(i = 0; colortable[i].name != NULL; i++)
    {
        if(strcmp(colorname, colortable[i].name) == 0)
            break;
    }
    if(colortable[i].name == NULL)
    {
        error(L, "invalid color name (%s)", colorname);
    }
    else
    {
        red = colortable[i].red;
        green = colortable[i].green;
        blue = colortable[i].blue;
    }
}else if(lua_istable(L, -1))
    {
        red = getcolorfield(L, "red");
        green = getcolorfield(L, "green");
        blue = getcolorfield(L, "blue");
    }else
    {
        error(L, "invalid value for 'background'");
    }
```

###### 简便方法

​	由于通过字符串类型的健来检索表是很常见的操作，因此lua语言针对这种情况提供了一个特定版本的lua_gettable函数:`lua_getfield`可以将`getcolorfield`的两行代码

```c++
lua_pushstring(L, key);
lua_gettable(L, -2);
```

重写为:

```c++
lua_getfield(L, -1, key); //获取 background[key]
```

lua还为字符串类型的键提供了一个名为lua_setfield的特殊版本lua_settable

```c++
void setcolorfield(lua_State *L, const char *index, int value)
{
	lua_pushnumber(L, (double)value / MAX_COLOR);
	lua_setfield(L, -2, index);
}
```

###### 调用lua函数

```lua
-- lua file
function f(x, y)
	return (x^2 * math.sin(y)) / (1 - x)
end
```

```c++
// cpp file
double f(lua_State *L, double x, double y){
    int isnum;
    double z;
    lua_getglobal(L, "f");
    lua_pushnumber(L, x);
    lua_pushnumber(L, y);
    
    if(lua_pcall(L, 2, 1, 0) != LUA_OK)
        error(L, "error running function 'f': %s", lua_tosting(L, -1));
    z = lua_tonumberx(L, -1, &isnum);
    if(!isnum)
    	error(L, "function 'f' should return a number");
    lua_pop(L, 1);
    return z;
}
```

###### 数组操作

```c++
void lua_geti(lua_State *L, int index, int key);
void lua_seti(lua_State *L, int index, int key);
```

index表示表在栈中的位置，key表示元素在表中的位置，当t为正数，那么调用`lua_geti(L, t, key)`等价与以下代码:

```c++
lua_pushnumber(L, key);
lua_gettable(L, t);
```

调用`lua_seti(L, t, key) `(t仍为正数)等价于：

```c++
lua_pushnumber(L, key);
lua_insert(L, -2); // 将栈顶元素和栈顶之下的元素进行置换
lua_settable(L, t); // t[key] = v; t是索引t位置的值,v是栈顶元素
```

###### 注册表&&轻量级用户数据(`light userdata`)

注册表(`registry`)是一张只能被C代码访问的全局表，通常我们使用注册表来存储多个模块间共享的数据。注册表总是位于伪索引(pseudo-index)`LUA_REGISTRYINDEX`中。伪索引就像一个栈中的索引，但它关联的值不再栈中。要获取注册表中"Key"的值，可以使用如下调用：

```c++
lua_getfield(L, LUA_REGISTRYINDEX, "Key");
```

注册表是一个普通的Lua表，因此可以使用除nil外的任意Lua值来检索它，不过因为所有的C语言模块共享的用一个注册表，所以必须谨慎的选择作为键的值。**在注册表中不能使用数值类型的健**，因为Lua语言将其用作`引用系统(reference system)`的保留字。

引用系统由辅助库中的一对函数组成，有了这两个函数，我们在表中存储值时不必担心如何创建唯一的键，函数`luaL_ref`用于创建新的引用:

```c++
int ref = luaL_ref(L, LUA_REGISTRYINDEX);
```

上述调用会从栈中弹出一个值，然后分配一个新的整形的键，使用这个键将从栈中弹出的值保存到注册表，最后返回该整形键，这个键称为引用(reference).

要将与引用ref关联的值压入栈中可以这样写

```c++
lua_rawgeti(L, LUA_REGISTRYINDEX, ref);
```

释放值和引用可以调用`luaL_unref`

```c++
luaL_unref(L, LUA_REGISTRYINDEX, ref);
```

这句调用之后，再次调用`luaL_ref`会再次返回相同的引用。

> `lua程序设计 P358`

`lua_rawgetp`和`lua_rawsetp`这两个函数类似于`lua_rawgeti`和`lua_rawseti`但它们使用C语言指针(转化为轻量级用户数据)作为键。

```c++
/* 具有唯一地址的变量 */
statuc char Key = 'k';
/* 保存字符串 */
lua_pushlightuserdata(L, (void *)&Key); // push address
lua_pushstring(L, myStr); // push value 
lua_settable(L, LUA_REGISTRYINDEX); // registry[&Key] = myStr
/* 获取字符串 */
lua_pushlightuserdata(L, (void *)&Key); // push address
lua_gettable(L, LUA_REGISTRYINDEX); // get value
myStr = lua_tostring(L, -1); // transform to string
/* 重写 */
static char Key = 'k';
/* 保存字符串 */
lua_pushstring(L, myStr);
lua_rawsetp(L, LUA_REGISTRYINDEX, (void *)&Key);
/* 获取字符串 */
lua_rawgetp(L, LUA_REGISTRYINDEX, (void *)&Key);
myStr = lua_tostring(L, -1);
```

###### 上值(`upvalue`)

注册表提供了全局变量,而上值则实现了一种类似于C语言静态变量的机制，每一次在Lua中创建新的C函数时，都可以将任意数量的上值与这个函数相关联，每个上值都可以保存一个Lua值，后面调用该函数时，可以通过伪索引来自由的访问这些上值。我们将这种C函数与其上值关联称为闭包，每个闭包都可以拥有不同的上值。