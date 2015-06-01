# 5. 语句


声明语句：声明变量或定义新函数

Javascript默认为顺序执行。

##5.1 表达式语句
```
greeting = "Hello " + name;
i *= 3;

counter++;

delete o.x;

alert(greeting);
window.close();

cx = Math.cos(x);
```

## 5.2 符合语句
+ 语句块：
```
{
    x = Math.PI;
    cx = Math.cos(x);
    console.log("cos(π) = " + cx);
}
```
注意： 语句块的结尾不需要分号；语句块的行都有缩进，方便阅读；语句块内的变量不是私有的。

```
// Initialize an array a
for(i = 0; i < a.length; a[i++] = 0) ;

if ((a == 0) || (b == 0));   // Oops! This line does nothing...
    o = null;                // and this line is always executed.
    
for(i = 0; i < a.length; a[i++] = 0) /* empty */ ;
```

+ var

var声明变量：
```
var name_1 [ = value_1] [ ,..., name_n [= value_n]]
```

```
var i;                                        // One simple variable
var j = 0;                                    // One var, one value
var p, q;                                     // Two variables
var greeting = "hello" + name;                // A complex initializer
var x = 2.34, y = Math.cos(0.75), r, theta;   // Many variables
var x = 2, y = x*x;                           // Second var uses the first
var x = 2,                                    // Multiple variables...
    f = function(x) { return x*x },           // each on its own line
    y = f(x);
```

```
for(var i = 0; i < 10; i++) console.log(i);
for(var i = 0, j=10; i < 10; i++,j--) console.log(i*j);
for(var i in o) console.log(i);
```

+ function

定义函数
```
function funcname([arg1 [, arg2 [..., argn]]]) {
      statements
}
```
example:
```
var f = function(x) { return x+1; }  // Expression assigned to a variable
function f(x) { return x+1; }        // Statement includes variable name


function hypotenuse(x, y) {
    return Math.sqrt(x*x + y*y);  // return is documented in the next section
}
function factorial(n) {           // A recursive function
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```
##5.4 条件语句
+ if

```
if (expression)
    statement
```
example:
```
// If username is null, undefined, false, 0, "", or NaN, give it a new value
if (!username) username = "John Doe";
```

```
if (expression)
      statement1
else
      statement2
```

example
```
if (n == 1) 
    console.log("You have 1 new message.");
else
    console.log("You have " + n + " new messages.");
```
+ else if
```
if (n == 1) {
    // Execute code block #1
}
else if (n == 2) {
    // Execute code block #2
}
else if (n == 3) {
    // Execute code block #3
}
else {
    // If all else fails, execute block #4
}
```

+ switch
使用`===`比较
```
switch(expression) {
      statements
}
```

```
switch(n) {
  case 1:                      // Start here if n == 1
    // Execute code block #1.
    break; 
                     // Stop here
  case 2:                      // Start here if n == 2
    // Execute code block #2.
    break;                     // Stop here
  case 3:                      // Start here if n == 3
    // Execute code block #3.
    break;                     // Stop here
  default:                     // If all else fails...
    // Execute code block #4.
    break;                     // stop here
}


function convert(x) {
    switch(typeof x) {
      case 'number':            // Convert the number to a hexadecimal integer
        return x.toString(16);
      case 'string':            // Return the string enclosed in quotes
        return '"' + x + '"';
      default:                  // Convert any other type in the usual way
        return String(x);
    }
}

```

##5.5 循环

+ while
```
while (expression)
      statement
```
example:
```
var count = 0;
while (count < 10) {
    console.log(count);
    count++;
}
```

+ do/while
```
do
      statement
  while (expression);
```
example:
```
function printArray(a) {
    var len = a.length, i = 0;
    if (len == 0)
        console.log("Empty Array");
    else {
        do {
            console.log(a[i]);
        } while (++i < len);
    }
}
```

+ for
```
// statement 1
for(initialize ; test ; increment)
      statement
      
// statement 1     
initialize;
  while(test) {
      statement
      increment;
  }
```

example:
```
for(var count = 0; count < 10; count++)
    console.log(count);
    
var i,j;
for(i = 0, j = 10 ; i < 10 ; i++, j--)
    sum += i * j;    

function tail(o) {                          // Return the tail of linked list o
    for(; o.next; o = o.next) /* empty */ ; // Traverse while o.next is truthy
    return o;
}
```
+ for/in

```
for (variable in object)
      statement
```
example:
```
for(var i = 0; i < a.length; i++)  // Assign array indexes to variable i
    console.log(a[i]);             // Print the value of each array element
    
for(var p in o)        // Assign property names of o to variable p
    console.log(o[p]); // Print the value of each property
    
var o = {x:1, y:2, z:3};
var a = [], i = 0;
for(a[i++] in o) /* empty */;

for(i in a) console.log(i);
```

##5.6 跳转语句
+ 标签语句

```
//statement
identifier: statement

mainloop: while(token != null) {
    // Code omitted...
    continue mainloop;  // Jump to the next iteration of the named loop
    // More code omitted...
}
```

+ break
立即退出最内层的循环或者switch语句。

```
for(var i = 0; i < a.length; i++) {
    if (a[i] == target) break;
}


var matrix = getData();  // Get a 2D array of numbers from somewhere
// Now sum all the numbers in the matrix.
var sum = 0, success = false;
// Start with a labeled statement that we can break out of if errors occur
compute_sum: if (matrix) {
   for(var x = 0; x < matrix.length; x++) {
        var row = matrix[x];
        if (!row) break compute_sum;
         for(var y = 0; y < row.length; y++) {
                    var cell = row[y];
                    if (isNaN(cell)) break compute_sum;
                    sum += cell;
                }
            }
            success = true;
}
// The break statements jump here. If we arrive here with success == false
// then there was something wrong with the matrix we were given.
// Otherwise sum contains the sum of all cells of the matrix.
```

+ continue
退出本次循环进入下次循环。

continue的情景：
 + 在while循环中，在循环开始出指定的expression会重复检测，如果检测结果为true，循环体会从头开始执行
 + 在do/while循环中，程序的执行直接跳到循环的结尾处。这时会重新判断循环条件，之后才会继续下一次循环
 + 在 for循环中，首先计算自增表达式，砸次检测test表达式。泳衣判断是否执行循环体。
 + 在 for/in 循环中，循环开始遍历下一个属性名，这个属性名赋给指定的变量。
 
 ```
 for(i = 0; i < data.length; i++) {
     if (!data[i]) continue;  // Can't proceed with undefined data
     total += data[i];
 }
 ```
 
+ return

```
return expression;
```

```
function square(x) { return x*x; }   // A function that has a return statement
square(2)                            // This invocation evaluates to 4

function display_object(o) {
    // Return immediately if the argument is null or undefined.
    if (!o) return;
    // Rest of function goes here...
}
```

+ throw

```
throw expression;
```
demos
```
function factorial(x) {
    // If the input argument is invalid, throw an exception!
    if (x < 0) throw new Error("x must not be negative");
    // Otherwise, compute a value and return normally
    for(var f = 1; x > 1; f *= x, x--) /* empty */ ;
    return f;
}
```

+ try/catch/finally

通常解释器执行到try块的尾部，然后开始执行finally的逻辑，以便于进行必要的清理工作。当由于return，continue或者break语句使得解释器跳出try语句是，解释器在执行新的代码之前有限执行finally中的逻辑。

如果finally块中使用了return，continue、break 或者throw语句是程序发生跳转，或者通过调用了抛出异常的方法改变了程序的执行流程，不管这个跳转是程序挂起还是继续执行，解释器都会将其忽略。
```
try {
  // Normally, this code runs from the top of the block to the bottom
  // without problems. But it can sometimes throw an exception,
  // either directly, with a throw statement, or indirectly, by calling
  // a method that throws an exception.
}
catch (e) {
  // The statements in this block are executed if, and only if, the try
  // block throws an exception. These statements can use the local variable
  // e to refer to the Error object or other value that was thrown.
  // This block may handle the exception somehow, may ignore the
  // exception by doing nothing, or may rethrow the exception with throw.
}
finally {
  // This block contains statements that are always executed, regardless of
  // what happens in the try block. They are executed whether the try
  // block terminates:
  //   1) normally, after reaching the bottom of the block
  //   2) because of a break, continue, or return statement
  //   3) with an exception that is handled by a catch clause above
  //   4) with an uncaught exception that is still propagating
}
```
demos
```
try {
    // Ask the user to enter a number
    var n = Number(prompt("Please enter a positive integer", ""));
    // Compute the factorial of the number, assuming the input is valid
    var f = factorial(n);
    // Display the result
    alert(n + "! = " + f);  
}
catch (ex) {    // If the user's input was not valid, we end up here
    alert(ex);  // Tell the user what the error is
}
```

```
// Simulate for( initialize ; test ; increment ) body;
initialize ;
while( test ) {
    try { body ; }
    finally { increment ; }
}
```
## 5.7 其他语句

+ with 

```
with (object)
    statement
```
在严格模式中，禁止使用with语句。非严格模式也不推荐，尽量避免使用with语句。
```
with(document.forms[0]) {
    // Access form elements directly here. For example:
    name.value = "";
    address.value = "";
    email.value = "";
}
```
+ debugger语句

debugger语句，以调试模式运行，产生一个断点，代码的执行会停止在断点的位置。这时可以输出断点的值或者价差调用栈等。

```
function f(o) {
  if (o === undefined) debugger;  // Temporary line for debugging purposes
  ...                             // The rest of the function goes here.
}
```

+ "use strict" 指令

1. 严格模式禁止使用with语句
2. 严格模式下，所有的变量都要先声明。如果给一个未声明的变量、函数、函数参数、catch 从句参数或者全局对象的属性赋值，会抛出一个引用错误异常。非严格模式下，隐式声明的全局变量的方法是给全局对象添加一个新的属性。
3. 严格模式下，调用函数的this值是undefined。非严格模式，调用this是全局对象。
判断Javascript实现是否支持严格模式：
```
var hasStrictMode = (function() { "use strict"; return this===undefined}());
```

##5.8 总结

|语句  | 语法 | 用途|
|:--------|:--------|:--------|
| break |   break [label]; | Exit from the innermost loop or switch or from named enclosing statement |
|case | case expression: | Label a statement within a switch|
|continue | continue [label]; | Begin next iteration of the innermost loop or the named loop|
|debugger | debugger; | Debugger breakpoint|
|default |  default: |  Label the default statement within a switch|
|do/while | do statement while (expression); | An alternative to the while loop|
|empty |    ;   |   Do nothing|
|for |  for(init; test; incr) statement |   An easy-to-use loop|
|for/in     |   for (var in object) statement | Enumerate the properties of object|
| function |    function name([param[,...]]) { body } |  Declare a function named name|
|if/else |  if (expr) statement1 [else statement2] |    Execute statement1 or statement2|
|label |    label: statement |  Give statement the name label|
|return |   return [expression];|    Return a value from a function|
|switch |   switch (expression) { statements } |    Multiway branch to case or default: labels|
|throw |    throw expression; |  Throw an exception|
|try |  try { statements  } [catch { handler statements }] [finally { cleanup statements }] |Handle exceptions|
|use strict|     "use strict";| Apply strict mode restrictions to script or function|
|var |  var name [ = expr] [ ,... ];| Declare and initialize one or more variables|
|while |while (expression) statement |  A basic loop construct|
|with |with (object) statement |     Extend the scope chain (forbidden in strict mode)|

