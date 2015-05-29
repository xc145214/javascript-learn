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
break       delete  function    return  typeof
case        do      if          switch  var
catch       else    in          this    void
continue    false   instanceof  throw   while
debugger    finally new         true    with
default     for     null        try
```
ECMAScript5 保留字：
```
class   const   enmu    export  extends import  super
```

严格模式下保留字：
```
implements  let     private     public  yield
interface   package protected   static
```

避免关键字作为标识符：

```
abstract    double  goto        native      static
boolean     enum    implements  package     super
byte        export  import      private     synchronized
char        extends init        protected   throws
class       final   interface   public      transient
const       float   long        short       volatile
```

避免关键字作为函数名与变量名：
```
arguments           encodeURL           Infinity    Number          RegExp
Array               encodeURIComponent  isFinite    Object          String
Boolean             Error               isNaN       parseFloat      SytaxError
Date                eval                JSON        parseInt        TypeError
decodeURI           EvalError           Math        RangeError      underfined
decideURIComponent  Function            NaN         ReferenceError  URIError
```

