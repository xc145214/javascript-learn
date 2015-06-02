# 7 数组

数组是值得有序集合。

Javascript数组是无类型的：元素可以使任意类型。包括数组

Javascript的索引是基于0的32位数值。第一个索引为0，最大索引为2的32次方-2.

javascript是动态的。无需重新分配空间

Javascript数组可以是稀疏的。不一定连续

Javascript数组有一个length属性。

数组是对象的特殊形式，是经过优化的比一般对象属性要快

数组继承自Array.prototype()的属性.

## 7.1 创建数组

+ 数组直接量：

```
var empty = [];                 // An array with no elements
var primes = [2, 3, 5, 7, 11];  // An array with 5 numeric elements
var misc = [ 1.1, true, "a", ]; // 3 elements of various types + trailing comma

var base = 1024;
var table = [base, base+1, base+2, base+3];

var b = [[1,{x:1, y:2}], [2, {x:3, y:4}]];

var count = [1,,3]; // An array with 3 elements, the middle one undefined.
var undefs = [,,];  // An array with 2 elements, both undefined.
```

+ Array的构造函数：
```
var a = new Array();

var a = new Array(10);

var a = new Array(5, 4, 3, 2, 1, "testing, testing");
```

##7.2 读写元素

+ 使用[]来访问
```
var a = ["world"];     // Start with a one-element array
var value = a[0];      // Read element 0
a[1] = 3.14;           // Write element 1
i = 2;         
a[i] = 3;              // Write element 2
a[i + 1] = "hello";    // Write element 3
a[a[i]] = a[0];        // Read elements 0 and 2, write element 3
```

+ 数组是对象的特殊形式

```
o = {};        // Create a plain object
o[1] = "one";  // Index it with an integer

a[-1.23] = true;  // This creates a property named "-1.23"
a["1000"] = 0;    // This the 1001st element of the array
a[1.000]          // Array index 1.  Same as a[1]
```

## 7.3 稀疏数组

```
a = new Array(5);   // No elements, but a.length is 5.
a = [];             // Create an array with no elements and length = 0.
a[1000] = 0;        // Assignment adds one element but sets length to 1001.
```

```
var a1 = [,,,];         // This array is [undefined, undefined, undefined]
var a2 = new Array(3);  // This array has no values at all
0 in a1                 // => true: a1 has an element with index 0
0 in a2                 // => false: a2 has no element with index 0
```

## 7.4 数组长度

```
[].length             // => 0: the array has no elements
['a','b','c'].length  // => 3: highest index is 2, length is 3

a = [1,2,3,4,5];     // Start with a 5-element array.
a.length = 3;        // a is now [1,2,3].
a.length = 0;        // Delete all elements.  a is [].
a.length = 5;        // Length is 5, but no elements, like new Array(5)

a = [1,2,3];                               // Start with a 3-element array.
Object.defineProperty(a, "length",         // Make the length property
                      {writable: false});  // readonly.
a.length = 0;                              // a is unchanged.

```

## 7.5 数组元素的添加和删除
+ 为新索引赋值

```
a = []           // Start with an empty array.
a[0] = "zero";   // And add elements to it.
a[1] = "one";
```

+ push() 数组摸添加元素

```
a = [];              // Start with an empty array
a.push("zero")       // Add a value at the end.  a = ["zero"]
a.push("one", "two") // Add two more values.  a = ["zero", "one", "two"]
```

+ unshift() 首部插入

+ 删除元素 delete 不改变数组的length属性
```
a = [1,2,3];   
delete a[1];   // a now has no element at index 1
1 in a         // => false: no array index 1 is defined 
a.length       // => 3: delete does not affect array length
```

+ pop() 尾部删除元素。length-1

+ shift() 头部删除元素。length-1

+ splice() 通用方法：插入 删除 替换

## 7.6 数组遍历

```
var keys = Object.keys(o);   // Get an array of property names for object o
var values = []              // Store matching property values in this array
for(var i = 0; i < keys.length; i++) {  // For each index in the array
    var key = keys[i];                  // Get the key at that index
    values[i] = o[key];                 // Store the value in the values array
}
```
优化：
```
for(var i = 0, len = keys.length; i < len; i++) {
   // loop body remains the same
}
```
过滤 null undefined 和不存在的元素

```
for(var i = 0; i < a.length; i++) {
    if (!a[i]) continue;  // Skip null, undefined, and nonexistent elements
    // loop body here
}
```
只过滤undefined+ 不存在的元素
```
for(var i = 0; i < a.length; i++) {
    if (a[i] === undefined) continue; // Skip undefined + nonexistent elements
    // loop body here
}
```

只过滤不存在的元素
```
for(var i = 0; i < a.length; i++) {
    if (!(i in a)) continue ; // Skip nonexistent elements
    // loop body here
}
```

for/in循环处理
```
for(var index in sparseArray) {
   var value = sparseArray[index];
   // Now do something with index and value
}

for(var i in a) {
    if (!a.hasOwnProperty(i)) continue;  // Skip inherited properties
    // loop body here
}

for(var i in a) {
    // Skip i if it is not a non-negative integer
    if (String(Math.floor(Math.abs(Number(i)))) !== i) continue;
}
```

forEach
```
var data = [1,2,3,4,5];     // This is the array we want to iterate
var sumOfSquares = 0;       // We want to compute the sum of the squares of data
data.forEach(function(x) {  // Pass each element of data to this function
                 sumOfSquares += x*x;  // add up the squares
             });
sumOfSquares                // =>55 : 1+4+9+16+25
```

## 7.7 多维数组

Javascript不支持真正的多维数组，通过数组的数组近似。
```
// Create a multidimensional array
var table = new Array(10);               // 10 rows of the table
for(var i = 0; i < table.length; i++)
    table[i] = new Array(10);            // Each row has 10 columns
// Initialize the array
for(var row = 0; row < table.length; row++) {
    for(col = 0; col < table[row].length; col++) {
        table[row][col] = row*col;
    }
}
// Use the multidimensional array to compute 5*7
var product = table[5][7];  // 35
```

## 7.8  数组的方法

+ array.join():元素拼接转化为字符串，并返回字符串。 通过String.split()逆向。

```
var a = [1, 2, 3];     // Create a new array with these three elements
a.join();              // => "1,2,3"
a.join(" ");           // => "1 2 3"
a.join("");            // => "123"
var b = new Array(10); // An array of length 10 with no elements
b.join('-')            // => '---------': a string of 9 hyphens
```

+ array.reverse():逆向数组，重新排列原有元素

```
var a = [1,2,3];
a.reverse().join()  // => "3,2,1" and a is now [3,2,1]
```

+ array.sort(): 元素排序并返回排序后的数组 
不带参数按字母表顺序排序,undefined排在尾部

```
var a = new Array("banana", "cherry", "apple");
a.sort();
var s = a.join(", ");  // s == "apple, banana, cherry"
```

带参数数值大小排序

```
var a = [33, 4, 1111, 222];
a.sort();                 // Alphabetical order:  1111, 222, 33, 4
a.sort(function(a,b) {    // Numerical order: 4, 33, 222, 1111
           return a-b;    // Returns &lt; 0, 0, or &gt; 0, depending on order
       });
a.sort(function(a,b) {return b-a});   // Reverse numerical order
```

带参数 转换为小写排序

```
a = ['ant', 'Bug', 'cat', 'Dog']
a.sort();                // case-sensitive sort: ['Bug','Dog','ant',cat']
a.sort(function(s,t) {   // Case-insensitive sort
           var a = s.toLowerCase();
           var b = t.toLowerCase();
           if (a < b) return -1;
           if (a > b) return 1;
           return 0;
       });               // => ['ant','Bug','cat','Dog']
```

+ array.concat() ：创建并返回新数组，连接的是数组的元素，不会递归扁平化数组的数组。也不会修改调用的数组

```
var a = [1,2,3];
a.concat(4, 5)          // Returns [1,2,3,4,5]
a.concat([4,5]);        // Returns [1,2,3,4,5]
a.concat([4,5],[6,7])   // Returns [1,2,3,4,5,6,7]
a.concat(4, [5,[6,7]])  // Returns [1,2,3,4,5,[6,7]]
```

+ array.slice():返回指定数组的片段或者子数组.

```
var a = [1,2,3,4,5];
a.slice(0,3);    // Returns [1,2,3]
a.slice(3);      // Returns [4,5]
a.slice(1,-1);   // Returns [2,3,4]
a.slice(-3,-2);  // Returns [3]
```

+ array.splice(): 插入或删除数组的方法。会修改指定的数组。

```
var a = [1,2,3,4,5,6,7,8];
a.splice(4);    // Returns [5,6,7,8]; a is [1,2,3,4]
a.splice(1,2);  // Returns [2,3]; a is [1,4]
a.splice(1,1);  // Returns [4]; a is [1]
```
```
var a = [1,2,3,4,5];
a.splice(2,0,'a','b');  // Returns []; a is [1,2,'a','b',3,4,5]
a.splice(2,2,[1,2],3);  // Returns ['a','b']; a is [1,2,[1,2],3,3,4,5]
```

+ array.push() 和 array.pop()

```
var stack = [];       // stack: []
stack.push(1,2);      // stack: [1,2]      Returns 2
stack.pop();          // stack: [1]        Returns 
stack.push(3);        // stack: [1,3]      Returns 2
stack.pop();          // stack: [1]        Returns 3
stack.push([4,5]);    // stack: [1,[4,5]]  Returns 2
stack.pop()           // stack: [1]        Returns [4,5]
stack.pop();          // stack: []         Returns 1
```

+ array.unshift() he array.shift()

```
var a = [];            // a:[]
a.unshift(1);          // a:[1]         Returns: 1
a.unshift(22);         // a:[22,1]      Returns: 2
a.shift();             // a:[1]         Returns: 22
a.unshift(3,[4,5]);    // a:[3,[4,5],1] Returns: 3
a.shift();             // a:[[4,5],1]   Returns: 3
a.shift();             // a:[1]         Returns: [4,5]
a.shift();             // a:[]          Returns: 1
```

+ toString() 和toLocalString()

```
[1,2,3].toString()          // Yields '1,2,3'
["a", "b", "c"].toString()  // Yields 'a,b,c'
[1, [2,'c']].toString()     // Yields '1,2,c'
```

## 7.9 ECMAScript 5中数组的方法

ECMAScript 5定义了9个新的方法来遍历、映射、过滤、检测、简化、和搜索数组。这些方法不会修改他们调用的原始数组。

+ forEach()

```
var data = [1,2,3,4,5];                           // An array to sum
// Compute the sum of the array elements
var sum = 0;                                      // Start at 0
data.forEach(function(value) { sum += value; });  // Add each value to sum
sum                                               // => 15
// Now increment each array element
data.forEach(function(v, i, a) { a[i] = v + 1; });
data                                              // => [2,3,4,5,6]
```

```
function foreach(a,f,t) {
    try { a.forEach(f,t); }
    catch(e) {
    if (e === foreach.break) return; 
    else throw e;
    }
}
foreach.break = new Error("StopIteration");
```

+ map()

```
a = [1, 2, 3];
b = a.map(function(x) { return x*x; });  // b is [1, 4, 9]
```

+ filter()

```
a = [5, 4, 3, 2, 1];
smallvalues = a.filter(function(x) { return x < 3 });   // [2, 1]
everyother = a.filter(function(x,i) { return i%2==0 }); // [5, 3, 1]
```

压缩稀疏数组
```
var dense = sparse.filter(function() { return true; });
```
压缩并删除undefined和null元素

```
a = a.filter(function(x) { return x !== undefined && x != null; });
```

+ every():判断 和 some()：是否存在

```
a = [1,2,3,4,5];
a.every(function(x) { return x < 10; })      // => true: all values < 10.
a.every(function(x) { return x % 2 === 0; }) // => false: not all values even.
```

```
a = [1,2,3,4,5];
a.some(function(x) { return x%2===0; })  // => true a has some even numbers.
a.some(isNaN)                            // => false: a has no non-numbers.
```

+ reduce() reduceRight()

```
var a = [1,2,3,4,5]
var sum = a.reduce(function(x,y) { return x+y }, 0);     // Sum of values
var product = a.reduce(function(x,y) { return x*y }, 1); // Product of values
var max = a.reduce(function(x,y) { return (x>y)?x:y; }); // Largest value
```

```
var a = [2, 3, 4]
// Compute 2^(3^4).  Exponentiation has right-to-left precedence
var big = a.reduceRight(function(accumulator,value) {
                            return Math.pow(value,accumulator);
                        });
```
union()参数
```
var objects = [{x:1}, {y:2}, {z:3}];
var merged = objects.reduce(union);    // => {x:1, y:2, z:3}

var objects = [{x:1,a:1}, {y:2,a:2}, {z:3,a:3}];
var leftunion = objects.reduce(union);       // {x:1, y:2, z:3, a:1}
var rightunion = objects.reduceRight(union); // {x:1, y:2, z:3, a:3}
```

+ indexOf() lastIndexOf()

```
a = [0,1,2,1,0];
a.indexOf(1)       // => 1: a[1] is 1
a.lastIndexOf(1)   // => 3: a[3] is 1
a.indexOf(3)       // => -1: no element has value 3

// Find all occurrences of a value x in an array a and return an array
// of matching indexes
function findall(a, x) {
    var results = [],            // The array of indexes we'll return
        len = a.length,          // The length of the array to be searched
        pos = 0;                 // The position to search from
    while(pos < len) {           // While more elements to search...
        pos = a.indexOf(x, pos); // Search
        if (pos === -1) break;   // If nothing found, we're done.
        results.push(pos);       // Otherwise, store index in array
        pos = pos + 1;           // And start next search at next element
    }
    return results;              // Return array of indexes
}
```

## 7.10 数组类型

检验是否是数组：

```
//EMCAScript 5:
Array.isArray([])     // => true
Array.isArray({})     // => false

//EMCAScript 3:
var isArray = Function.isArray || function(o) {
    return typeof o === "object" &&
    Object.prototype.toString.call(o) === "[object Array]";
};
```

## 7.10 类数组对象

```
var a = {};  // Start with a regular empty object
// Add properties to make it "array-like"
var i = 0;
while(i < 10) {
    a[i] = i * i;
    i++;
}
a.length = i;
// Now iterate through it as if it were a real array
var total = 0;
for(var j = 0; j < a.length; j++)
    total += a[j];
```

 Arguments object ，document.getElementsByTagName(),都是类数组对象
 
判断类数组对象
```
 // Determine if o is an array-like object.
 // Strings and functions have numeric length properties, but are 
 // excluded by the typeof test. In client-side JavaScript, DOM text
 // nodes have a numeric length property, and may need to be excluded 
 // with an additional o.nodeType != 3 test.
 function isArrayLike(o) {
     if (o &&                                // o is not null, undefined, etc.
         typeof o === "object" &&            // o is an object
         isFinite(o.length) &&               // o.length is a finite number
         o.length >= 0 &&                    // o.length is non-negative
         o.length===Math.floor(o.length) &&  // o.length is an integer
         o.length < 4294967296)              // o.length < 2^32
         return true;                        // Then o is array-like
     else
         return false;                       // Otherwise it is not
 }
```

```
var a = {"0":"a", "1":"b", "2":"c", length:3};  // An array-like object
Array.prototype.join.call(a, "+")  // => "a+b+c"
Array.prototype.slice.call(a, 0)   // => ["a","b","c"]: true array copy
Array.prototype.map.call(a, function(x) { 
    return x.toUpperCase();
})                                 // => ["A","B","C"]:
```
```
var a = {"0":"a", "1":"b", "2":"c", length:3};  // An array-like object
Array.join(a, "+")
Array.slice(a, 0)
Array.map(a, function(x) { return x.toUpperCase(); })
```

```
Array.join = Array.join || function(a,sep) {
    return Array.prototype.join.call(a,sep);
};
Array.slice = Array.slice || function(a,from,to) {
    return Array.prototype.slice.call(a,from,to);
};
Array.map = Array.map || function(a, f, thisArg) {
    return Array.prototype.map.call(a, f, thisArg);
}
```

## 7.12 作为数组的字符串

```
var s = test;
s.charAt(0)    // => "t"
s[1]           // => "e"
```
```
s = "JavaScript"
Array.prototype.join.call(s, " ")      // => "J a v a S c r i p t"
Array.prototype.filter.call(s,         // Filter the characters of the string
    function(x) {                      
        return x.match(/[^aeiou]/);    // Only match nonvowels
    }).join("")                        // => "JvScrpt"
```