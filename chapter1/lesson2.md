#3 类型 值和变量

+ javascript的数据类型分为原始类型(primitive type)和对象类型(Objective type)

1. 原始类型：数字 字符串 布尔值及特殊的null 和 underfine
2. 对象类型：对象（object）是属性（property）的集合，每个属性都是名/值对构成

+ 普通对象时命名值得无序集合；数组是带编号的值得有序集合

+ 函数是具有与它相关的可执行代码的对象，通过调用函数来运行可执行代码，并返回结果。也是一直特殊的对象，

+ 构造函数都定义了class对象————由构造函数初始化的对象组成的集合。类可以看作为对象类型的子类型。

+ Javascript核心定义特殊类：Array Function Date RegExp Error

+ javascript有自己的内存管理机制，可以自动对内存进行垃圾回收

+ Javascript 是一种面向对象语言

+ 技术上只有javascript对象才能拥有方法，但是数字、字符串和布尔值也可以拥有自己的方法。

+ Javascript只有null和undefined无法拥有方法的值

+ javascript的类型可以分为可变类型和不可变类型：

1. 可变类型：数组和对象
2. 不可变类型：数字 布尔值 null undefined 字符串

+ javascript可以自由进行数据转换

+ Javascript 的变量时无类型的，通过var来声明


##3.1 数字
+ javascript的所有数字都是浮点数值来表示

+ javascript的实际操作中(索引)基于32为整数

+ 浮点型数字：
```
[digits][.digits][(E|e)[(+|-)]digits]
```
```
3.14
2345.789
.333333333333333333
6.02e23        // 6.02 × 1023
```

+ 算数运算符：

1. 基本运算符：+ - * / %
2. 复杂运算符：
```
Math.pow(2,53)           // => 9007199254740992: 2 to the power 53
Math.round(.6)           // => 1.0: round to the nearest integer
Math.ceil(.6)            // => 1.0: round up to an integer
Math.floor(.6)           // => 0.0: round down to an integer
Math.abs(-5)             // => 5: absolute value
Math.max(x,y,z)          // Return the largest argument
Math.min(x,y,z)          // Return the smallest argument
Math.random()            // Pseudo-random number x where 0 <= x < 1.0
Math.PI                  // π: circumference of a circle / diameter
Math.E                   // e: The base of the natural logarithm
Math.sqrt(3)             // The square root of 3
Math.pow(3, 1/3)         // The cube root of 3
Math.sin(0)              // Trigonometry: also Math.cos, Math.atan, etc.
Math.log(10)             // Natural logarithm of 10
Math.log(100)/Math.LN10  // Base 10 logarithm of 100
Math.log(512)/Math.LN2   // Base 2 logarithm of 512
Math.exp(3)              // Math.E cubed
```
javascript 算数运算符在溢出、下溢 或者被0除不会报错。
溢出（overflow）：无穷大（Infinity）或者负无穷大（-Infinity）。
下溢（underflow）: 0 或者 -0
被0除： 返回 Infinity 或者 -Infinity

```
Infinity                    // A read/write variable initialized to Infinity.
Number.POSITIVE_INFINITY    // Same value, read-only.
1/0                         // This is also the same value.
Number.MAX_VALUE + 1        // This also evaluates to Infinity.
Number.NEGATIVE_INFINITY    // These expressions are negative infinity.
-Infinity
-1/0                        
-Number.MAX_VALUE - 1
NaN                         // A read/write variable initialized to NaN.
Number.NaN                  // A read-only property holding the same value.
0/0                         // Evaluates to NaN.
Number.MIN_VALUE/2          // Underflow: evaluates to 0
-Number.MIN_VALUE/2         // Negative zero
-1/Infinity                 // Also negative 0
-0
```
> 判断NaN： x!=x only x=NaN时为true 

> isFinite() 在参数部位NaN、Infinity -Infinity时为true

```
var zero = 0;         // Regular zero
var negz = -0;        // Negative zero
zero === negz         // => true: zero and negative zero are equal 
1/zero === 1/negz     // => false: infinity and -infinity are not equal
```