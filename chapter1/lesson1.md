#2 词法结构

##2.1 字符集
+ javascript程序使用Unicode字符集编写。

+ javascript区分大小写，html不区分大小写

+ 注释：

```
//单行注释
/*一段注释**/
/***
*   多行注释
*
*/
```
+ 标识符：以字母、数字、下划线或者$开始，后续为字母、数字、下划线或$(数字不能开头)
+ 保留字：

```
break   delete  function    return  typeof
case    do  if  switch  var
catch   else    in  this    void
continue    false   instanceof  throw   while
debugger    finally new true    with
default for null    try
```
ECMAScript5 保留字：
```
class   const   enmu    export  extends import  super
```

严格模式下保留字：
```
implements  let private public  yield
interface   package protected   static
```

避免关键字作为标识符：

```

```


##1.3 类型 值和变量

+ javascript的数据类型分为原始类型和对象类型
+ 原始类型：数字 字符串 布尔值及特殊的null 和 underfine
+ 对象类型：对象书属性的集合，每个属性都是名/值对构成
+ 普通对象时命名值得无序集合；数组是带编号的值得有序集合
+ 函数是具有与它相关的可执行代码的对象，通过调用函数来运行可执行代码，并返回结果。也是一直特殊的对象，
+ 构造函数都定义了class对象————由构造函数初始化的对象组成的集合
+ Javascript核心定义特殊类：Array Function Date RegExp Error
+ javascript有自己的内存管理机制
+ Javascript 是一种面向对象语言
+ Javascript只有null和undefined无法拥有方法的值
+ javascript的类型可以分为可变类型和不可变类型
+ 可变类型：数组和对象
+ 不可变类型：数字 布尔值 null undefined 字符串
+ javascript可以自由进行数据转换
+ Javascript 的变量时无类型的，通过var来声明


3.1 javascript的所有数字都是浮点数值来表示