# 14 Window对象

## 14.1 计时器

setTimeout(): 指定毫秒数之后运行，返回一个值，这个值可以传递给clearTimeout()用于取消函数的执行
setInterval()：函数会在指定毫秒数的间隔里重复执行，返回一个值，传递给clearInterval()用于取消函数的执行

定时器函数

```
/*
 * Schedule an invocation or invocations of f() in the future.
 * Wait start milliseconds, then call f() every interval milliseconds, 
 * stopping after a total of start+end milliseconds.
 * If interval is specified but end is omitted, then never stop invoking f.
 * If interval and end are omitted, then just invoke f once after start ms.
 * If only f is specified, behave as if start was 0.
 * Note that the call to invoke() does not block: it returns right away.
 */
function invoke(f, start, interval, end) {
    if (!start) start = 0;          // Default to 0 ms
    if (arguments.length <= 2)      // Single-invocation case
        setTimeout(f, start);       // Single invocation after start ms.
    else {                          // Multiple invocation case
        setTimeout(repeat, start);  // Repetitions begin in start ms
        function repeat() {         // Invoked by the timeout above
            var h = setInterval(f, interval); // Invoke f every interval ms.
            // And stop invoking after end ms, if end is defined
            if (end) setTimeout(function() { clearInterval(h); }, end);
        }
    }
}
```

## 14.2 浏览器的定位与导航

Window.location属性：引用的是Location对象，表示窗口中当前显示文本的URL，并定义了方法使窗口载入新的文档。
Document.location属性：引用Location对象，是文档首次载入后保存该文当的URL的静态字符串。

对过定位到文档的锚点标记`#content`,Window.location会更新，而document.location不会。

+ 解析URL

location的属性：
```
location.href (location.toString()方法返回href的值)
location.protocol
location.host
location.hostname
location.port
location.pathname
location.search
location.hash
```

提取URL的搜索字符串中的参数
```
/*
 * This function parses ampersand-separated name=value argument pairs from
 * the query string of the URL. It stores the name=value pairs in
 * properties of an object and returns that object. Use it like this:
 *
 * var args = urlArgs();  // Parse args from URL
 * var q = args.q || "";  // Use argument, if defined, or a default value
 * var n = args.n ? parseInt(args.n) : 10;
 */
function urlArgs() {
    var args = {};                             // Start with an empty object
    var query = location.search.substring(1);  // Get query string, minus '?'
    var pairs = query.split("&");              // Split at ampersands
    for(var i = 0; i < pairs.length; i++) {    // For each fragment
        var pos = pairs[i].indexOf('=');       // Look for "name=value"
        if (pos == -1) continue;               // If not found, skip it
        var name = pairs[i].substring(0,pos);  // Extract the name
        var value = pairs[i].substring(pos+1); // Extract the value
        value = decodeURIComponent(value);     // Decode the value
        args[name] = value;                    // Store as a property
    }
    return args;                               // Return the parsed arguments
}
```

+ 载入新的文档
location.assign(): 载入指定URL的文档
location.replace()：载入指定URL的文档，并删除当前文档。**常用**
location.reload()：重新加载当前文档
```
// If the browser does not support the XMLHttpRequest object
// redirect to a static page that does not require it.
if (!XMLHttpRequest) location.replace("staticpage.html");
```

浏览器跳转：
```
location = "http://www.oreilly.com"; // Go buy some books!
```

```
location = "page2.html";             // Load the next pag

location = "#top";                   // Jump to the top of the document

location.search = "?page=" + (pagenum+1);  // load the next page
```

## 14.3 浏览历史
Window.history属性引用的是History对象。不能访问浏览历史URL列表

history.forward()：前进
history.back():后退
history.go(-2):后退2格
```
history.go(-2);   // Go back 2, like clicking the Back button twice
```

## 14.4 浏览器和屏幕信息

+ Navigator对象：
以chrome为例：
```
navigator.appName
=> "Netscape"
navigator.appCodeName
=> "Mozilla"
navigator.appVersion
=> "5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/43.0.2357.81 Safari/537.36"
navigator.userAgent
=> "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/43.0.2357.81 Safari/537.36"
navigator.platform
=> "Win32"
navigator.onLine
=> true
navigator.geolocation
=> Geolocation {}
navigator.javaEnabled
=> javaEnabled()
navigator.cookieEnabled
=> true
```

navigatoru.userAgent来进行浏览器嗅探
```
// Define browser.name and browser.version for client sniffing, using code
// derived from jQuery 1.4.1. Both the name and number are strings, and both
// may differ from the public browser name and version. Detected names are:
//
//   "webkit": Safari or Chrome; version is WebKit build number
//   "opera": the Opera browser; version is the public version number
//   "mozilla": Firefox or other gecko-based browsers; version is Gecko version
//   "msie": IE; version is public version number
//
// Firefox 3.6, for example, returns: { name: "mozilla", version: "1.9.2" }.
var browser = (function() {
     var s = navigator.userAgent.toLowerCase();
     var match = /(webkit)[ \/]([\w.]+)/.exec(s) ||
     /(opera)(?:.*version)?[ \/]([\w.]+)/.exec(s) ||
     /(msie) ([\w.]+)/.exec(s) ||
     !/compatible/.test(s) && /(mozilla)(?:.*? rv:([\w.]+))?/.exec(s) ||
     [];
     return { name: match[1] || "", version: match[2] || "0" };
}());     
```

+ Screen对象
Window.screen对象引用Screen对象。提供窗口显示大小等信息
```
screen.width
=> 1440
screen.height
=> 900
screen.availHeight
=> 860
screen.availWidth
=> 1440
screen.colorDepth
=> 24
```

## 14.5 对话框
Window.alert():弹出警告框
Window.confirm()：确认
Window.prompt()：返回用户输入字符串
尽量限制使用
```
do {
    var name = prompt("What is your name?");                  // Get a string
    var correct = confirm("You entered '" + name + "'.\n" +   // Get a boolean
                          "Click Okay to proceed or Cancel to re-enter.");
} while(!correct)
alert("Hello, " + name);  // Display a plain message
```

Window.showModalDialog()
```
<!--
  This is not a stand-alone HTML file. It must be invoked by showModalDialog().
  It expects window.dialogArguments to be an array of strings.
  The first element of the array is displayed at the top of the dialog.
  Each remaining element is a label for a single-line text input field.
  Returns an array of input field values when the user clicks Okay.
  Use this file with code like this:
  var p = showModalDialog("multiprompt.html",
                          ["Enter 3D point coordinates", "x", "y", "z"],
                          "dialogwidth:400; dialogheight:300; resizable:yes");
  -->
<form>
<fieldset id="fields"></fieldset> <!-- Dialog body filled in by script below -->
<div style="text-align:center">   <!-- Buttons to dismiss the dialog -->
<button onclick="okay()">Okay</button>     <!-- Set return value and close -->
<button onclick="cancel()">Cancel</button> <!-- Close with no return value -->
</div>
<script>
// Create the HTML for the dialog body and display it in the fieldset
var args = dialogArguments;
var text = "<legend>" + args[0] + "</legend>";
for(var i = 1; i < args.length; i++) 
    text += "<label>" + args[i] + ": <input id='f" + i + "'></label><br>";
document.getElementById("fields").innerHTML = text;
// Close the dialog without setting a return value
function cancel() { window.close(); }
// Read the input field values and set a return value, then close
function okay() {
    window.returnValue = [];             // Return an array
    for(var i = 1; i < args.length; i++) // Set elements from input fields
        window.returnValue[i-1] = document.getElementById("f" + i).value;
    window.close();  // Close the dialog. This makes showModalDialog() return.
}
</script>
</form>
```

## 14.6 错误处理
window.onerror()

```
// Display error messages in a dialog box, but never more than 3
window.onerror = function(msg, url, line) {
    if (onerror.num++ < onerror.max) {
        alert("ERROR: " + msg + "\n" + url + ":" + line);
        return true;
    }
}
onerror.max = 3;
onerror.num = 0;
```

## 14.7 作为window对象属性的文档元素

```
var ui = ["input","prompt","heading"];     // An array of element ids
ui.forEach(function(id) {                  // For each id look up the element
    ui[id] = document.getElementById(id);  // and store it in a property
});


var $ = function(id) { return document.getElementById(id); };
ui.prompt = $("prompt");
```

## 14.8 多窗口和窗体

+ 打开和关闭窗口 

window.open() 
window.close()

```
var w = window.open("smallwin.html", "smallwin",
                    "width=400,height=350,status=yes,resizable=yes");
```

```
var w = window.open();                         // Open a new, blank window.
w.alert("About to visit http://example.com");  // Call its alert() method
w.location = "http://example.com";             // Set its location property

w.opener !== null;      // True for any window w created by open()
w.open().opener === w;  // True for any window w
```

+ 多窗口关联
```
parent.history.back();
```

```
parent == self;  // For any top-level window
```

```
var elt = document.getElementById("f1");
var win = elt.contentWindow;
win.frameElement === elt     // Always true for frames
window.frameElement === null // For toplevel windows
```