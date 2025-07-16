# web.view 自动化操作网页

web.view 操作网页应当是最优选，如无必要应优先使用 web.view 而不是 chrome.driver 。

请参考: 

- [web.view 使用指南](_.md)
- [web.view 库参考文档](../../../../library-reference/web/view/_.md)

## 等待网页文档对象 <a id="wait" href="#wait">&#x23;</a>


在浏览器组件打开的网页加载完成以后，仍有可能会动态创建或加载其他内容。
根据需要等待的目标不同，web.view 提供了不同的方法。

web.view 网页加载事件按触发先后顺序列举如下：

- `wb.onDocumentInit` 事件，网页初始化创建文档对象（ 也就是通过 JavaScript 脚本里可以访问 document 对象 ）时触发此事件。
- `wb.onDocumentComplete` 事件，网页 DOM 准备就绪触发此事件，也就是在网页的 document 对象的 DOMContentLoaded 事件触发时执行。
- `wb.onLoad` 事件，此时整个页面及所有依赖资源如样式表和图片都已完成加载，也就是在网页的 window 对象的 load 事件触发时执行。

web.view 还提供了 3 个函数用于判断网页文档对象的加载状态，并等待网页的文档对象准备就绪

- 我们也可以在代码中调用 `wb.waitDoc` 函数同步等待网页文档对象准备就绪。  
- 由于在网页打开过程中还可能会从一个网址跳转到另一个网址，web.view 还提供了更常用的 `wb.wait` 函数用于等待指定网址的文档对象准备就绪。`wb.wait` 函数可指定需要等待的网址参数（可指定网址的部分字符串，支持模式匹配语法）。
- 函数 `wb.waitUrlParam("网址字符串","URL 参数名")` 则会等待网页打开指定的网址并且网页文档对象准备就绪，并且打开的网址参数中出现了指定的参数名。此函数会返回指定参数的值。

示例：

```aardio
import win.ui;
var winform = win.form(text="WebView2");

import web.view;
var wb = web.view(winform);

//网页文档对象已初始化。
wb.onDocumentInit = function(url){
    // url 为 当前打开的网址
}

//打开网页
wb.go("http://www.example.com?q=word")

//仅等待文档对象
wb.waitDoc();

//等待指定的 URL 打开并且网页文档对象准备就绪。	
wb.wait("example.com");

//等待指定的 URL 的指定参数，并返回该参数的值。	
var qValue = wb.waitUrlParam("example.com","q");

//查看 q 参数的值
winform.msgbox(qValue);

winform.show();
win.loopMessage();
```

## 等待并自动化操作网页元素 <a id="waitEle" href="#waitEle">&#x23;</a>

我们可以使用 wb.waitEle 或 wb.waitEle2 等待指定的网页创建指定的元素。
这两个函数的参数用法是相同的，但 wb.waitEle 仅等待单个网址，如果网页中途发生跳转，则 wb.waitEle 会返回 null，但 wb.waitEle2 在等待过程中允许网址跳转，它会继续等待直到成功，除非网页窗口关闭。

wb.waitEle 函数用法：

```aardio
/*
同步等待网页创建指定节点。
网页关闭或当前网址变更也会退出等待。

@selector 参数指定CSS选择器。
可选用参数 @timeout 指定超时，单位毫秒。 

成功返回 CSS 选择器，失败返回 null,错误对象（表对象或字符串）。
*/
wb.waitEle(selector,timeout) 

/*
异步等待网页创建指定节点。

@selector 参数指定CSS选择器。
找到节点则执行参数 @2 指定的 JavaScript 代码。
执行 JS 代码时自动绑定 this 对象为找到的节点对象。 

可选用参数 @timeout 指定超时，单位毫秒。 
*/
wb.waitEle(selector,js,timeout) 

/*
异步等待网页创建指定节点。

@selector 参数指定CSS选择器。
找到节点后异步回调参数 @2 指定的 aardio 函数，参数@1为 CSS选择器。
可选用参数 @timeout 指定超时，单位毫秒。 
*/
wb.waitEle(selector,callback,timeout)
```

`wb.wait` 函数支持[模式匹配语法](https://www.aardio.com/zh-cn/doc/guide/language/pattern-matching.html) 。

wb.waitEle2 函数用法：

```aardio
/*
同步等待网页创建指定节点。
不同于 waitEle 仅在当前网页有效，
waitEle2 支持跨网页等待，网址变更也不会退出等待。
除非关闭窗口，此函数会一直等待直到指定选择器的对象创建成功。

@selector 参数指定CSS选择器。
可选用参数 @timeout 指定超时，单位毫秒。 

成功返回 CSS 选择器，失败返回 null,错误对象（表对象或字符串）。
*/
wb.waitEle2(selector,timeout) 

/*
同步等待网页创建指定节点。

@selector 参数指定CSS选择器。
找到节点则异步执行参数 @2 指定的 JavaScript 代码。
执行 JS 代码时自动绑定 this 对象为找到的节点对象。 

可选用参数 @timeout 指定超时，单位毫秒。 
*/
wb.waitEle2(selector,js,timeout) 

/*
同步等待网页创建指定节点。

@selector 参数指定CSS选择器。
找到节点后异步回调参数 @2 指定的 aardio 函数，参数@1为 CSS选择器。
可选用参数 @timeout 指定超时，单位毫秒。 
*/
wb.waitEle2(selector,callback,timeout)
```

wb.waitEle 与 wb.waitEle2 的区别在于：：

- wb.waitEle2  可跨网页等待，网址变更不会退出。
- wb.waitEle 仅在当前网页有效，当前网页关闭或跳转（ JavaScript 中的 document 文档对象变更）就会自动释放。

如果等待时网页可能发生跳转，务必使用 wb.waitEle2 函数。

示例：
 
```aardio
import win.ui;
var winform = win.form(text="web.view 等待并操作网页元素")

import web.view;
var wb = web.view(winform);

//打开网址
wb.go("https://www.aardio.com/zh-cn/doc/");

//用法 1. 异步等待参数@1指定 CSS 选择器的节点，回调 aardio 函数
wb.waitEle("#search-input",function(ok,err){
    wb.doScript("
        var searchInput = document.querySelector('#search-input');
        searchInput.value='多线程'; 
        searchInput.dispatchEvent(new Event('input', { bubbles: true, }));
    ")
})

//用法 2. 不指定回调函数或回调 JS 脚本则同步等待参数 @1 指定CSS选择器的节点
wb.waitEle("#search-input");

wb.doScript("
var searchInput = document.querySelector('#search-input');
searchInput.value='多线程'; 
searchInput.dispatchEvent(new Event('input', { bubbles: true, }));
")

winform.show(3/*_SW_SHOWMAXIMIZED*/);
win.loopMessage();
```

## 网页动注入 JavaScript 代码 <a id="preloadScript" href="#preloadScript">&#x23;</a>


wb.preloadScript 函数可在网页初始化时注入 JS 代码，这是做网页自动化非常有用的函数，在这个函数里可以修改很多 JS 默认函数实现有趣的功能，下面是一个 禁止刷新缩放的例子：

```aardio
import win.ui;
var winform = win.form(text="禁止按 F5，Ctrl + R 刷新")

import web.view;
var wb = web.view(winform);

//定义字符串 initScript，赋值为需要执行的 JavaScript
var initScript = /****

//禁止页面刷新
document.onkeydown = function (e) {
    if (e.key == "F5" || (e.ctrlKey && e.key == "r") ) {
        e.preventDefault(); 
    }
} 
 
//禁止滚轮缩放
document.addEventListener('wheel', function(e) {
    if(e.ctrlKey) {
        e.preventDefault();
    }
}, { passive: false });

****/

//添加网页默认加载执行的 JavaScript
wb.preloadScript(initScript)

wb.html = /**
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<script >alert("网页每加载一次，显示一次弹框")</script>
</head>
<body>已禁止刷新，禁止 Ctrl + 鼠标滚轮缩放。</body>
</html>
**/

winform.show();
win.loopMessage();
```

拦截弹窗：

```aardio
import win.ui;
var winform = win.form(text="web.view - 拦截网页弹窗")

import web.view; 
var wb = web.view(winform);

//弹出新窗口触发
wb.onNewWindow = function(url){

//耗时操作应返回异步自动执行的函数（提前结束 onNewWindow ）
return function(){ 
    //如果打开的是 file: 前缀网址，例如拖放文件到网页上。
    var filePath = inet.url.getFilePath(url)
    if(filePath){
        winform.msgbox(filePath,"本地文件"); 	
    }
    else {
        //用 wb.location 代替 wb.go 跳转网页则当前页面设为 HTTP referrer 请求头。 
        wb.location = url;
    } 
}
}

wb.html = /**
<!doctype html>
<html><head>
<base target="_blank" />
</head>

<body style="margin:50px">
<a href="http://www.aardio.com">aardio.com</a>
<button onclick="window.open('http://www.aardio.com')" >aardio.com</button>
**/

winform.show();
win.loopMessage();
```

### 4. 使用 CDP 命令 <a id="preloadScript" href="#preloadScript">&#x23;</a>


web.view 提供以下 CDP 有关函数调用 WebView2 内置 CDP 接口：

- `wb.cdp()` 用于执行 CDP 命令。
- `wb.cdpWait()` 执行 CDP 命令并等待返回非 null 值。
- `wb.cdpWaitQuery(selector,parent,timeout)` 接口查询并等待节点，这里的 selector 参数也是指定 CSS 选择器。
- `wb.cdpSubscribe()` 用于订阅 CDP 事件 

更多函数请参考：

- [CDP 范例](../../../../example/WebUI/web.view/DevTools/cdp.html)
- [CDP 文档](https://chromedevtools.github.io/devtools-protocol)

web.view 自动关闭弹框示例:

```aardio
import win.ui;
var winform = win.form(text="CDP 事件 - 自动关闭网页上弹出信息框")

import web.view; 
var wb = web.view(winform);
winform.show();

//允许监听页面事件
wb.cdp("Page.enable");

//订阅 CDP 事件
//https://chromedevtools.github.io/devtools-protocol/tot/Page/#event-javascriptDialogOpening
wb.cdpSubscribe("Page.javascriptDialogOpening",function(dlg){
/*
dlg.message 是对话框文本。
dlg.type 是对话框类型
dlg.url 对话框所在页面网址
*/

//为避免阻塞导致某些网页出现异常，应返回异步执行的函数关闭弹框。
return function(){
    //自动关闭弹框
    wb.cdp("Page.handleJavaScriptDialog",{accept=true})
} 
})

wb.html = /**
<script type="text/javascript">alert("测试弹框")</script>
**/
win.loopMessage();
```