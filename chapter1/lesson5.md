# 6. 对象

+ 对象是javaScript的基本数据类型。对象是一种复合值。可以看作为属性的无序集合。
+ 对象不仅是字符串到值得映射，可以保持自有的属性，还可以从一个称为原型的对象继承属性。
+ 对象的方法通常是继承的属性。
+ 对象时动态的。除了字符串、数字、Boolean、null\undefined之外。所有的Javascript值都是对象
+ 对象常见的用法：create ,set,query,delete,test,enumerate.

+ 三类Javascript对象的区分：

1. 内置对象：ECMAScript规范定义的对象或者类。数组、函数、日期、正则
2. 宿主对象：Javascript解释器的宿主环境。比如web浏览器的window
3. 自定义对象：Javascript代码创建的对象

+ 两类属性：

1. 自有属性：直接在对象中定义的属性。
2. 继承属性：对象的原型对象中定义的属性.

## 6.1 创建对象

创建方式：对象直接量，关键字new , Object.create()函数

+ 对象直接量：

```
var empty = {};                           // An object with no properties
var point = { x:0, y:0 };                 // Two properties
var point2 = { x:point.x, y:point.y+1 };  // More complex values
var book = {                      
    "main title": "JavaScript",           // Property names include spaces,
    'sub-title': "The Definitive Guide",  // and hyphens, so use string literals
    "for": "all audiences",               // for is a reserved word, so quote
    author: {                             // The value of this property is
        firstname: "David",               // itself an object.  Note that
        surname: "Flanagan"               // these property names are unquoted.
    }
};
```

+ new

```
var o = new Object();     // Create an empty object: same as {}.
var a = new Array();      // Create an empty array: same as [].
var d = new Date();       // Create a Date object representing the current time
var r = new RegExp("js"); // Create a RegExp object for pattern matching.
```

+ 原型

每一个Javascript对象（null除外）都和另一个对象（原型）所关联。每一个对象都是从原型继承属性

对象直接量创建的对象都具有同一个原型对象。通过Object.prototype获取引用。

new和构造函数调用创建的对象原型就是构造函数的Prototype属性的值。new Date()的原型：Date.prototype

Object.prototype没有原型的对象。
所有内置对象都具有继承自Object.prototype的原型：new Date()的对象同时继承自`Date.prototype` 和`Object.prototype`

+ Object.create()

```
var o1 = Object.create({x:1, y:2});       // o1 inherits properties x and y.

var o2 = Object.create(null);             // o2 inherits no props or methods.

var o3 = Object.create(Object.prototype); // o3 is like {} or new Object().
```

```
// inherit() returns a newly created object that inherits properties from the
// prototype object p.  It uses the ECMAScript 5 function Object.create() if
// it is defined, and otherwise falls back to an older technique.
function inherit(p) {
    if (p == null) throw TypeError(); // p must be a non-null object
    if (Object.create)                // If Object.create() is defined...
        return Object.create(p);      //    then just use it.
    var t = typeof p;                 // Otherwise do some more type checking
    if (t !== "object" && t !== "function") throw TypeError();
    function f() {};                  // Define a dummy constructor function.
    f.prototype = p;                  // Set its prototype property to p.
    return new f();                   // Use f() to create an "heir" of p.
}
```
inherit()函数方式库函数修改对象。
```
var o = { x: "don't change this value" };
library_function(inherit(o));  // Guard against accidental modifications of o
```

## 6.2 属性

```
var author = book.author;      // Get the "author" property of the book.
var name = author.surname      // Get the "surname" property of the author.
var title = book["main title"] // Get the "main title" property of the book.

book.edition = 6;                   // Create an "edition" property of book.
book["main title"] = "ECMAScript";  // Set the "main title" property.
```

+ 作为关联数组的对象

```
var addr = "";
for(i = 0; i < 4; i++) {
    addr += customer["address" + i] + '\n';
    
function addstock(portfolio, stockname, shares) {
    portfolio[stockname] = shares;
}

function getvalue(portfolio) {
    var total = 0.0;
    for(stock in portfolio) {           // For each stock in the portfolio:
        var shares = portfolio[stock];  //   get the number of shares
        var price = getquote(stock);    //   look up share price
        total += shares * price;        //   add stock value to total value
    }
    return total;                       // Return total value.
}

```

+ 继承

```
var o = {}            // o inherits object methods from Object.prototype
o.x = 1;              // and has an own property x.
var p = inherit(o);   // p inherits properties from o and Object.prototype
p.y = 2;              // and has an own property y.
var q = inherit(p);   // q inherits properties from p, o, and Object.prototype
q.z = 3;              // and has an own property z.
var s = q.toString(); // toString is inherited from Object.prototype    
q.x + q.y             // => 3: x and y are inherited from o and p


var unitcircle = { r:1 };     // An object to inherit from
var c = inherit(unitcircle);  // c inherits the property r
c.x = 1; c.y = 1;             // c defines two properties of its own
c.r = 2;                      // c overrides its inherited property
unitcircle.r;                 // => 1: the prototype object is not affected
```

+ 属性访问错误

```
book.subtitle;    // => undefined: property doesn't exist

// Raises a TypeError exception. undefined doesn't have a length property
var len = book.subtitle.length;

// A verbose and explicit technique
var len = undefined;
if (book) {
    if (book.subtitle) len = book.subtitle.length;
}
// A concise and idiomatic alternative to get subtitle length or undefined
var len = book && book.subtitle && book.subtitle.length;

// The prototype properties of built-in constructors are read-only.
Object.prototype = 0;  // Assignment fails silently; Object.prototype unchanged
```

给对象o设置属性p会失败的规律：

1. o的属性p是只读的：不能给只读属性来重新赋值。（defineProperty()方法有一个例外可以对配置为只读属性赋值）
2. o的属性p是继承属性，且它是它是只读的：不能通过同名自有属性覆盖只读的继承属性
3. o不存在自有属性p:o 没有使用setter方法继承属性p.并且o的可扩展性是false.如果o中 不存在p,且没有setter方法可调用，则p一定会添加到o中，但如果o是不可扩展的，则不能定义新属性。

## 6.3 删除属性
delete 只是断开属性与宿主对象的联系，不会去操作属性中的属性。
```
delete book.author;          // The book object now has no author property.
delete book["main title"];   // Now it doesn't have "main title", either.
```
delete 只能删除自有属性，不能删除继承属性。
delete 删除成功时返回true. delete后不是一个属性方位表达式,也返回true。

```
o = {x:1};          // o has own property x and inherits property toString
delete o.x;         // Delete x, and return true
delete o.x;         // Do nothing (x doesn't exist), and return true
delete o.toString;  // Do nothing (toString isn't an own property), return true
delete 1;           // Nonsense, but evaluates to true
```
delete 不能删除可配置性为false的属性。以下返回false：

```
delete Object.prototype;  // Can't delete; property is non-configurable
var x = 1;                // Declare a global variable
delete this.x;            // Can't delete this property
function f() {}           // Declare a global function
delete this.f;            // Can't delete this property either
```

非严格模式：
```
this.x = 1;      // Create a configurable global property (no var)
delete x;        // And delete it
```
严格模式：
```
delete x;        // SyntaxError in strict mode
delete this.x;   // This works
```

## 6.4 检测属性

检测属性的方式：in, hasOwnProperty和 PropertyIsEnumerable()

+ in 

```
var o = { x: 1 }
"x" in o;         // true: o has an own property "x"
"y" in o;         // false: o doesn't have a property "y"
"toString" in o;  // true: o inherits a toString property
```

只能使用in的场景：in 可以区分不存在的属性 和存在但是值为undfined的属性、
```
var o = { x: undefined }   // Property is explicitly set to undefined
o.x !== undefined          // false: property exists but is undefined
o.y !== undefined          // false: property doesn't even exist
"x" in o                   // true: the property exists
"y" in o                   // false: the property doesn't exists
delete o.x;                // Delete the property x
"x" in o                   // false: it doesn't exist anymore
```

+ hasOwnProperty()

```
var o = { x: 1 }
o.hasOwnProperty("x");        // true: o has an own property x
o.hasOwnProperty("y");        // false: o doesn't have a property y
o.hasOwnProperty("toString"); // false: toString is an inherited property
```

+ PropertyIsEnumerable()

```
var o = inherit({ y: 2 });
o.x = 1;
o.propertyIsEnumerable("x");  // true: o has an own enumerable property x
o.propertyIsEnumerable("y");  // false: y is inherited, not own
Object.prototype.propertyIsEnumerable("toString"); // false: not enumerable
```

+ !== undefined

```
var o = { x: 1 }
o.x !== undefined;        // true: o has a property x
o.y !== undefined;        // false: o doesn't have a property y
o.toString !== undefined; // true: o inherits a toString property
```

```
// If o has a property x whose value is not null or undefined, double it.
if (o.x != null) o.x *= 2;
// If o has a property x whose value does not convert to false, double it.
// If x is undefined, null, false, "", 0, or NaN, leave it alone.
if (o.x) o.x *= 2;
```

## 6.5 枚举属性

```
var o = {x:1, y:2, z:3};             // Three enumerable own properties
o.propertyIsEnumerable("toString")   // => false: not enumerable
for(p in o)                          // Loop through the properties
    console.log(p);                  // Prints x, y, and z, but not toString
    
    
for(p in o) {
    if (!o.hasOwnProperty(p)) continue;       // Skip inherited properties
}
for(p in o) {
    if (typeof o[p] === "function") continue; // Skip methods
}
```

用来枚举属性的对象工具函数
```
/*
 * Copy the enumerable properties of p to o, and return o.
 * If o and p have a property by the same name, o's property is overwritten.
 * This function does not handle getters and setters or copy attributes.
 */
function extend(o, p) {
    for(prop in p) {                         // For all props in p.
        o[prop] = p[prop];                   // Add the property to o.
    }
    return o;
}
/*
 * Copy the enumerable properties of p to o, and return o.
 * If o and p have a property by the same name, o's property is left alone.
 * This function does not handle getters and setters or copy attributes.
 */
function merge(o, p) {
    for(prop in p) {                           // For all props in p.
        if (o.hasOwnProperty[prop]) continue;  // Except those already in o.
        o[prop] = p[prop];                     // Add the property to o.
    }
    return o;
}

/*
 * Remove properties from o if there is not a property with the same name in p.
 * Return o.
 */
function restrict(o, p) {
    for(prop in o) {                         // For all props in o
        if (!(prop in p)) delete o[prop];    // Delete if not in p
    }
    return o;
}
/*
 * For each property of p, delete the property with the same name from o.
 * Return o.
 */
function subtract(o, p) {
    for(prop in p) {                         // For all props in p
        delete o[prop];                      // Delete from o (deleting a
                                             // nonexistent prop is harmless)
    }
    return o;
}
/*
 * Return a new object that holds the properties of both o and p.
 * If o and p have properties by the same name, the values from o are used.
 */
function union(o,p) { return extend(extend({},o), p); }
/*
 * Return a new object that holds only the properties of o that also appear
 * in p. This is something like the intersection of o and p, but the values of
 * the properties in p are discarded
 */
function intersection(o,p) { return restrict(extend({}, o), p); }
/*
 * Return an array that holds the names of the enumerable own properties of o.
 */
function keys(o) {
    if (typeof o !== "object") throw TypeError();  // Object argument required
    var result = [];                 // The array we will return
    for(var prop in o) {             // For all enumerable properties
        if (o.hasOwnProperty(prop))  // If it is an own property
            result.push(prop);       // add it to the array.
    }
    return result;                   // Return the array.
}
```

## 6.6 属性getter和setter

只有setter方法，只写属性。只有getter方法，只读属性。同时又set get方法，读写属性

```
var o = {
    // An ordinary data property 
    data_prop: value,
    // An accessor property defined as a pair of functions
    get accessor_prop() { /* function body here */ },
    set accessor_prop(value) { /* function body here */ }
};
```

```
var p = {
   // x and y are regular read-write data properties.
   x: 1.0,
   y: 1.0,
   // r is a read-write accessor property with getter and setter.
   // Don't forget to put a comma after accessor methods.
   get r() { return Math.sqrt(this.x*this.x + this.y*this.y); },
   set r(newvalue) {
     var oldvalue = Math.sqrt(this.x*this.x + this.y*this.y);
     var ratio = newvalue/oldvalue;
     this.x *= ratio;
     this.y *= ratio;
   },
   // theta is a read-only accessor property with getter only.
   get theta() { return Math.atan2(this.y, this.x); }
};
```
```
var q = inherit(p);  // Create a new object that inherits getters and setters
q.x = 0, q.y = 0;    // Create q's own data properties
console.log(q.r);    // And use the inherited accessor properties
console.log(q.theta);
```
```
// This object generates strictly increasing serial numbers
var serialnum = {
    // This data property holds the next serial number.
    // The $ in the property name hints that it is a private property.
    $n: 0,
    // Return the current value and increment it
    get next() { return this.$n++; },
    // Set a new value of n, but only if it is larger than current
    set next(n) {
        if (n >= this.$n) this.$n = n;
        else throw "serial number can only be set to a larger value";
    }
};
```

```
// This object has accessor properties that return random numbers.
// The expression "random.octet", for example, yields a random number
// between 0 and 255 each time it is evaluated.
var random = {
    get octet() { return Math.floor(Math.random()*256); },
    get uint16() { return Math.floor(Math.random()*65536); },
    get int16() { return Math.floor(Math.random()*65536)-32768; }
};
```

## 6.7 属性的特性

存取器属性的4个属性：读取get ,写入set ,可枚举性和可配置性。

+ 通过Object.getOwnPropertyDescriptor()可以获取某个对象自有属性的属性描述符：
```
// Returns {value: 1, writable:true, enumerable:true, configurable:true}
Object.getOwnPropertyDescriptor({x:1}, "x");
// Now query the octet property of the random object defined above.
// Returns { get: /*func*/, set:undefined, enumerable:true, configurable:true}
Object.getOwnPropertyDescriptor(random, "octet");
// Returns undefined for inherited properties and properties that don't exist.
Object.getOwnPropertyDescriptor({}, "x");         // undefined, no such prop
Object.getOwnPropertyDescriptor({}, "toString");  // undefined, inherited
```
获取继承属性的特性需要Object.getPrototypeOf()方法

设置属性的特性：Object.defineProperty()
```
var o = {};  // Start with no properties at all
// Add a nonenumerable data property x with value 1.
Object.defineProperty(o, "x", { value : 1, 
                                writable: true,
                                enumerable: false,
                                configurable: true});
// Check that the property is there but is nonenumerable
o.x;           // => 1
Object.keys(o) // => []
// Now modify the property x so that it is read-only
Object.defineProperty(o, "x", { writable: false });
// Try to change the value of the property
o.x = 2;      // Fails silently or throws TypeError in strict mode
o.x           // => 1
// The property is still configurable, so we can change its value like this:
Object.defineProperty(o, "x", { value: 2 });
o.x           // => 2
// Now change x from a data property to an accessor property
Object.defineProperty(o, "x", { get: function() { return 0; } });
o.x           // => 0
```

修改多个属性：Object.defineProperties()
```
var p = Object.defineProperties({}, {
    x: { value: 1, writable: true, enumerable:true, configurable:true },  
    y: { value: 1, writable: true, enumerable:true, configurable:true },
    r: { 
        get: function() { return Math.sqrt(this.x*this.x + this.y*this.y) },
        enumerable:true,
        configurable:true
    }
});
```

使用Object.defineProperty()抛出异常的规则：
1. 如果对象时不可扩展的，则可以编辑已有的自有属性，但是不能给它添加新属性
2. 如果属性是不可配置的，则不能修改它的可配置性与可枚举性
3. 如果存取器属性是不可配置的，则不能修改其getter和setter方法，也不能将它转换为数据属性
4. 如果数据属性是不可配置的，则不能将它转换为存取器属性
5. 如果数据属性是不可配置的，则不能将其可写性属性从false修改为true,但是可以从true修改为false
6. 如果数据属性是不可配置且不可写的，则不能修改它的值。然而可配置但是不可写属性的值是可以修改的。

复制属性的特性：
```
/*
 * Add a nonenumerable extend() method to Object.prototype.
 * This method extends the object on which it is called by copying properties
 * from the object passed as its argument.  All property attributes are
 * copied, not just the property value.  All own properties (even non-
 * enumerable ones) of the argument object are copied unless a property
 * with the same name already exists in the target object.
 */
Object.defineProperty(Object.prototype,
    "extend",                  // Define Object.prototype.extend
    {
        writable: true,
        enumerable: false,     // Make it nonenumerable
        configurable: true,
        value: function(o) {   // Its value is this function
            // Get all own props, even nonenumerable ones
            var names = Object.getOwnPropertyNames(o);
            // Loop through them
            for(var i = 0; i < names.length; i++) {
                // Skip props already in this object
                if (names[i] in this) continue;
                // Get property description from o
                var desc = Object.getOwnPropertyDescriptor(o,names[i]);
                // Use it to create property on this
                Object.defineProperty(this, names[i], desc);
            }
        }
    });

```
## 对象的三个属性
原型属性、类、可扩展性

+ 原型属性

原型属性是实例对象创建之初就设置好的。是用来继承属性的
通过Object.getPrototypeOf() 可以查询它的原型.

通过isPrototypeOf()方法检测一个对象是否是另一个对象的原型
```
var p = {x:1};                    // Define a prototype object.
var o = Object.create(p);         // Create an object with that prototype.
p.isPrototypeOf(o)                // => true: o inherits from p
Object.prototype.isPrototypeOf(o) // => true: p inherits from Object.prototype
```

+ 类属性

类属性是一个字符串，用以表示对象的类型信息。

classof()函数：返回类
```
function classof(o) {
    if (o === null) return "Null";
    if (o === undefined) return "Undefined";
    return Object.prototype.toString.call(o).slice(8,-1);
}
```
```
classof(null)       // => "Null"
classof(1)          // => "Number"
classof("")         // => "String"
classof(false)      // => "Boolean"
classof({})         // => "Object"
classof([])         // => "Array"
classof(/./)        // => "Regexp"
classof(new Date()) // => "Date"
classof(window)     // => "Window" (a client-side host object)
function f() {};    // Define a custom constructor
classof(new f());   // => "Object"
```

+ 可扩展性

对象的可扩展性用以表示是否可一个对象添加新的属性。
所有的内置对象和自定义对象都是显式可扩展的，宿主对象的可扩展性是Javascript定义的
判断对象是否可扩展： Object.isExtensible()
将对象转换为不可扩展的： Object.preventExtensions()  不可逆
将对象的自有属性设置为不可配置：Object.seal()
检测对象是都封闭：Object.sealed()
将对象设置为冻结：Object.freeze() 所有属性设置为只读
检测对象是否冻结：Object。isFrozen()

```
// Create a sealed object with a frozen prototype and a nonenumerable property
var o = Object.seal(Object.create(Object.freeze({x:1}),
                                  {y: {value: 2, writable: true}}));
```

## 6.9 对象序列化

JSON.stringfy() 和 JSON.parse()

```
o = {x:1, y:{z:[false,null,""]}}; // Define a test object
s = JSON.stringify(o);            // s is '{"x":1,"y":{"z":[false,null,""]}}'
p = JSON.parse(s);                // p is a deep copy of o
```

JSON的语法支持对象、数组、字符串、无穷大数字、true 和null。并且可以把他们还原
NaN Infinity -Infinity的序列化结果是null
日期对象的序列化是日期字符串，JSON.parse()依然是字符串形态。
函数、ReExp、Error、undefined都不能序列化和还原。
JSON.stringfy()只能序列化可枚举的自有属性。

## 6.10 对象方法
所有的Javascript对象都是从Object.prototype继承属性。
主要方法有：
```
Object.hasOwnProperty();
Object.propertyIsEnumerable();
Object.isPrototypeOf();
Object.create();
Object.getPrototypeOf();
```
+ toString()

```
var s = { x:1, y:1 }.toString();
```

+ toLocaleString()
返回本地化字符串。数字 日期 时间对象有意义

+ toJSON()

+ valueOf()s