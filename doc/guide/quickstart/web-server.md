# aardio 语言 Web 服务端开发指南

## 一. 简介

在 aardio 中可以使用以下方式创建 HTTP 服务端

1. 使用 wsock.tcp.simpleHttpServer 创建多线程 HTTP 服务端。
常用于桌面客户端程序创建嵌入式 HTTP 服务端，体积非常小仅数十 KB，支持发布为独立 EXE 文件，无外部依赖。
以多线程的方式处理 HTTP 请求，服务端 aardio 代码在独立线程中运行，需遵守 aardio 多线程规则。

2. 使用 wsock.tcp.asynHttpServer 创建单线程异步 HTTP 服务端。
用于桌面客户端程序在界面线程内创建嵌入式 HTTP 服务端，体积非常小仅数十 KB，支持发布为独立 EXE 文件，无外部依赖。
以单线程的方式处理 HTTP 请求，通常在界面线程直接处理，需要注意避免在请求中执行耗时代码阻塞界面线程。

可利用继承自 wsock.tcp.asynHttpServer 的 web.socket.server 在同一个端口同时提供 HTTP 与 WebSocket 服务端。

3. 使用 fastcgi.client 创建 CGI 服务端，可在 IIS 中注册该服务端以支持在 IIS 中运行 aardio 代码。
FastCGI 通常用于支持在 IIS 中用 aardio 写网站或者其他 HTTP 服务端 API 等。

上述所有用 aardio 创建的 HTTP 服务端环境都支持完全相同的应用开发接口。
无论是用 aardio 创建本地服务器，还是在 IIS 等Web服务器上用 aardio 编写的 FastCGI 模块，在这些用 aardio 创建的 HTTP 服务端环境内，用 aardio 开网站应用程序的接口都是一致的。


## 二. 用 aardio 开发网站应用 <a id="website" href="#website">&#x23;</a>


### 1. 创建网站工程

创建网站工程的步骤：

- 在 aardio 中点击『 主菜单 » 新建工程 』打开工程向导。
- 在工程向导中点选 『 Web 服务端 » 网站程序 』，然后点击『创建工程』。

一个最简单的网站应用示例：

```aardio
<!doctype html>
<html>
<body>
<?
response.write("你好！")
?>

```

### 2. 使用模板语法

aardio 代码原生支持模板语法。如果 aardio 文件以 HTML 开始则 aardio 代码必须写在 `<? ..... ?>` 内部。aardio 代码中写在  `<? ..... ?>` 外部的部分实际上会被解析为模板输出函数 [print](../../language-reference/builtin-function/print.md) 的参数。print 在不同的环境下可能指向不同的输出函数，在 aardio 编写的网站中，`print` 指向 `response.write` 函数。

模板语法要点：
- 模板开始标记 `<?` 必须独立，不能紧跟英文字母。例如 `<?xml` 不被解析为 aardio 代码段开始标记。
- 可以用 `<?=表达式?>` 输出文本，`=` 号前后允许空白字符与换行，其作用类似于 `print( 表达式 )`，可指定多个用逗号分开的表达式，例如 `<?=表达式1,表达式2 ?>`

参考文档: [aardio 模板语法](../../language-reference/templating/syntax.md) 


### 3. 使用 request,response,session 对象 

使用 aardio 启动的网站应用都提供以下对象：

- request: HTTP 请求对象
- response:  HTTP 响应对象
- session:  HTTP 会话对象

在 aardio 中所有 HTTP 服务端环境上述对象的接口与用法是一样的。

在网站应用中 request,response,session 都是全局对象，在 HTTP 服务端请求处理函数中则应当优先使用参数传过来的 request,response,session 对象而不是全局对象。

request,response,session 对象的详细说明请参考 [fastcgi.client 库](../../library-reference/fastcgi/client/_.md) 文档。

常见用法示例：

```aardio

//取指定的 HTTP 请求头，键名必须小写
var h = request.headers["name"]

//取 URL 请求参数，键名必须小写
var p = request.get["name"] 

//取表单参数，参数必须小写
p = request.post["name"] 

//取 URL 参数或表单参数，参数必须小写
p = request.query("字符串参数") 

//如果请求类型为 application/json 或 text/json 解析提交的 JSON 并返回 aardio 对象
var data = request.postJson()

//获取原始上传数据
data = request.postData()

//取客户端 IP 地址
var ip = request.remoteAddr

//取请求 URL，不带 URL 参数部分
var url = request.url

//请求 URL 中的路径部分，注意请求路径开始于"/config/","/lib/" 目录会被拒绝
var path = request.path


//如果提交的是 multipart/form-data 表单返回 fsys.multipartFormData 对象
var fileData = request.postFileData()

//自定义响应数据类型
response.contentType = "application/json"

//输出错误并终止执行后面的代码
response.error("输出一个或多个 500 错误信息")

//response.write 会自动将表转为 JSON，这个属性决定是否输出缩进格式化的 JSON 以利阅读
response.jsonPrettyPrint = true;

//如果参数指定 aardio 文件则解析并执行，否则请求或下载普通文件
response.loadcode("文件路径") //可添加多个传给被调用 aardio 文件的调用参数

//重定向与 301 重定向
response.redirect("重定向网址")
response.redirect("重定向网址",301)

//发送响应数据，支持输出字符串,bufer,结构体，表对象转为 JSON 输出，其他类型转为字符串输出
response.write(arg1,arg2,...)

//添加 HTTP 响应头，键名里每个单词的首字母必须大写，其他字母必须小写。value 可以是字符串或字符串数组
response.headers["Header-Name"] = stringOrStringArray 

//发送 HTTP 错误代码，可选指定错误信息，必须在页面尚未输出响应头时调用,此函数终止执行页面后续代码
response.errorStatu(code,message)

response.close() //关闭页面输出，终止执行代码
```


## 三、创建 HTTP 服务端 <a id="simpleHttpServer" href="#simpleHttpServer">&#x23;</a>


可以使用 aardio 标准库中的 wsock.tcp.simpleHttpServer 或 wsock.tcp.asynHttpServer 启动微型 HTTP 服务端。这几个服务端非常小并且不依赖外部文件，可以方便地嵌入到程序中，可以作为网页界面的嵌入后端使用。

wsock.tcp.simpleHttpServer 是多线程 HTTP 服务端，不依赖消息循环。  
用 wsock.tcp.simpleHttpServer 创建一个 Web 服务端非常简单，几句代码就可以了，如下：

```aardio
import console;
import wsock.tcp.simpleHttpServer; 
var app = wsock.tcp.simpleHttpServer("127.0.0.1",8081);
 
console.open();

app.run(
     
    function(response,request){
     	response.write("hello!")
    }
);
```

然后用 `http://localhost:8081` 就可以访问该服务端了。

上面的代码在本地IP 127.0.0.1 启动一个 Web 服务器，监听连接的端口为 8081，
用户每一次连接都自动回调 函数  `function(response,request,session){ }`

每一个 aardio 创建的 Web 服务器应用都应该遵守相同的调用约定，使用相同的回调函数格式，并创建相同的 `response`,`request` 对象，无论是创建本地服务器，还是在 IIS 等Web服务器上创建 FastCGI 模块，网站应用程序的入口是一致的。

在界面线程中使用 `wsock.tcp.simpleHttpServer.startUrl("/")` 则会创建一个单实例（多次调用不会重复创建线程）的 HTTP 服务器线程（自动分配空闲端口，在界面线程退出时自动退出），并且返回参数指定子路径的 HTTP 访问网址。 

而 wsock.tcp.asynHttpServer 则是单线程异步的 HTTP 服务端，依赖窗口消息循环（ 指 `win.loopMessage()` ），只能用于界面线程，示例：

```aardio
import wsock.tcp.asynHttpServer;
var httpServer = wsock.tcp.asynHttpServer();

//这里可以指定 IP 和端口，不指定则自动分配空闲端口 
httpServer.start("127.0.0.1");
 
//获取访问网址
var url = httpServer.getUrl("/.www/main.aardio"); 
```

wsock.tcp.asynHttpServer 一个有趣的用法是用于虚拟文件系统：

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form(text="虚拟文件演示")
/*}}*/

import wsock.tcp.asynHttpServer;
var httpServer = wsock.tcp.asynHttpServer();

//自定义处理 HTTP 请求的方式，参数可以是函数，也可以文件路径映射表。
httpServer.run( {
	
	//自定义某个路径返回的数据
	["/index.html"] = "hello"; 
	
	//自定义某个路径的响应程序
	["/hello"] = function(response,request,session){ 
         response.loadcode(request.path);
    }
} );

//启动服务器，随机分配空闲端口
httpServer.start("127.0.0.1");

//获取访问网址
var url = httpServer.getUrl("/index.html"); 

import web.view;
var wb = web.view(winform);

//用浏览器组件打开网页试试
wb.go(url);

winform.show();
win.loopMessage();
```

在使用 [web.view 创建的网页程序](../../library-guide/std/web/view/_.md) 中，只要简单地调用 `import wsock.tcp.simpleHttpServer` 或者 `import wsock.tcp.asynHttpServer` 就可以自动创建默认的 HTTP 服务端，wb.go 函数可自动将参数中基于应用程序根目录访问的路径（ 指路径的首字符是单个斜杆或板斜杆 ）转换为 HTTP 地址，例如：

```aardio
import web.view;
var wb  = web.view(mainForm);  

import wsock.tcp.simpleHttpServer; 
wb.go("\web\index.html");
```

在 aardio 工程向导中选择 『 网页界面 » web.view 』 中的任何工程模板都可以创建包含以上代码的完整示例。

## 四、创建 FastCGI 服务端 <a id="create-project" href="#create-project">&#x23;</a>

在 IIS 这样的服务端中可通过 FastCGI 加载 aardio 编写的 FastCGI 服务端，就可以支持 aardio 编写的网站程序，下面我们介绍具体步骤。 

首先请在主菜单中点击新建工程。

在工程向导中点选【CGI 服务端】，创建 CGI 服务端工程。

CGI 服务端应先生成 EXE 文件、并在 Web 服务器上注册为 FastCGI 模块才能使用，
IIS 服务器可通过在代码中 import fastcgi.iisInstall 自动注册 FastCGI 模块。

### 在 IIS 注册 FastCGI 模块操作步骤 <a id="iisInstall" href="#iisInstall">&#x23;</a>

双击运行通过 aardio 工程向导里创建发布的 AardioCGI.exe 会自动安装 FastCGI 模块到 IIS 服务器。  
此功能由标准库 fastcgi.iisInstall 实现，可以看看源代码了解细节。

手动设置的步骤，以 Win2008 ,IIS7 为例:

1. 桌面右键点【计算机】，弹出菜单中点【管理】，【添加角色/添加IIS】

2. 右键点【Web服务器(IIS)】，弹出菜单中点【添加角色服务】，确认已添加【CGI】

3. 然后打开IIS，到指定的网站点击【处理程序映射】，添加【处理程序映射】
   后缀名设为：*.aardio ( 如果设为 *,取消勾选请求限制到文件或目录则处理所有URL )
   模块选：FastCgiModule 可执行文件：选中使用本工程生成的 aardio-cgi.exe 
   
4. 在资源管理器右键点击 aardio-cgi.exe 所在目录，在目录属性中点【安全】，
添加IUSR,IIS_USER用户组,允许读取和执行、列出目录、写入权限。

5. 右键点击网站所在目录，在目录属性中点【安全】，添加IUSR,IIS_USER用户组,
允许读取和执行、列出目录、写入权限。

6. 如果是 64 位系统，请在应用程序池属性中设置"启用 32 位的应用程序"为 True

### IIS 禁止 FastCGI 输出缓存 <a id="responseBufferLimit" href="#responseBufferLimit">&#x23;</a>

FastCGI 的默认缓冲区大小为 4MB(0x400000)，  
在缓冲区未写满且输出未完成时不会立即发送数据到客户端，  
如果服务端需要执行耗时操作并且希望即时发送数据，那么应当减小输出缓冲或者将其设置为 0。

例如使用 `response.eventStream()` 函数必须将缓冲区大小设为 0 才能实现实时 SSE 推送的效果。

如果你希望全局设置缓冲区大小，或者设置指定网站的 FastCGI 缓冲区大小，  
那么只要直接运行通过 aardio 工程向导里创建发布的 AardioCGI.exe 即可直接设置缓冲区大小。   

下面简单讲一下如何手动修改指定目录的 aardio 的 FastCGI 缓冲区大小。

以 IIS 8.5 为例：

- 先 **备份**  **备份**  **备份**  然后打开 `C:\Windows\System32\inetsrv\config\applicationHost.config` 。

- 找到这个配置文件的最后一句 `</configuration>` 在前面插入：

    ```xml
    <location path="ai.aardio.com/api/v1/chat" overrideMode="Allow">
    <system.webServer>
        <handlers>
            <remove name="aardio" />
            <add name="aardio" path="*.aardio" verb="*" modules="FastCgiModule" scriptProcessor="C:\Project\AardioCGI\AardioCGI.exe" resourceType="File" requireAccess="Script" responseBufferLimit="0"  />
        </handlers>
    </system.webServer> 
    ``` 

    上面的 `ai.aardio.com/api`/v1/chat` 改成实际的域名与目录路径。
    `C:\Project\AardioCGI\AardioCGI.exe` 也改成实际的 AardioCGI.exe 路径。 

要特别注意，新版 IIS 管理器的『处理程序映射』里添加或设置 FastCGI 模块映射实际修改的是 `C:\Windows\System32\inetsrv\config\applicationHost.config` 这个配置文件，而不是网站对应目录下面的 web.config 。

### 常见问题：在 IIS 中 post 到 aardio 服务端时报错不支持的 HTTP 谓词

造成这个问题的一个常见原因是因为提交网址是一个目录，但目录尾部没有加斜杠。这时候 get 请求正常，但 post 请求会报错不支持的 HTTP 谓词。

解决方法：

1. 首先做基本的检查，在 IIS 管理器对应网站的处理映射中查看 *.aardio 对应的 FastCGI 模块是否正常安装，双击 aardio 的  FastCGI 模块查看属性，然后点击『请求限制』检查是否勾选了处理『全部谓词』，所谓的谓词就是指的发送 HTTP 请求的 GET,POST 等 HTTP 请求方法。

2. 检查是否在网站的默认文档中添加了 main.aardio ，以支持请求目录时自动指向 main.aardio 。

2. 如果其他设置都没有问题，网址后面去掉斜杠就不能正常 post，那么在 IIS 管理器打开该目录的上层目录点击『URL 重写』：

    - 右键点空白处，在弹出菜单中点『添加规则』
    - 在弹出向导中点选『搜索引擎优化 » 附加或删除尾部反斜杠符号』
    - 双击添加的规则，双击条件中的 `{REQUEST_FILENAME} / 不是目录` 改为 `{REQUEST_FILENAME} / 是目录`，保留另一个 `不是文件` 不变。
    - 将处理方式中的『操作类型』由『重定向』改为『重写』
    - 保持勾选『附加查询字符串』等其他选项不变
    - 最后点击『应用』新的规则

### 绑定 localhost 查看 500 错误信息

aardio-cgi.exe 内部错误可请查看 `aardio-cgi.exe 目录/config` 下面生成的日志文件。

网页内的 aardio 代码发生 500 内部错误，可查看 `网站目录/config` 下面生成的日志文件。在浏览器或 HTTP 客户端不会会显示为 500 错误的详细信息（显然这样不安全）。

推荐的方法是在开发时将网站域名绑定中临时添加 localhost ，然后通过 localhost 连接与测试，这时候在客户端可以直接获取与查看 500 错误的详细信息，开发与排错效率有较大提升，但程序开发完成后再移除 localhost 绑定即可。

在编写网站时，可以在服务端输出日志文件来排查错误。也可以使用 response.errorStatus() 函数向客户端发送错误信息或错误代码。在开发时客户端应当在  HTTP 服务端应答的状态码异常时处理并及时显示服务端返回的错误信息，不然出错了不知道发生什么。

### 其他 FastCGI 常见问题解答

1. 如果用新版 aardio 编写的代码，在旧版编译的 aardio-cgi.exe 运行报错，那么把旧版 aardio-cgi.exe 重新编译一次就可以。

2. import 导入的库，在一个进程中只会加载一次, 如果网站引用了修改的库，应当杀除CGI.EXE进程重启动，如果在服务器上编译 aardio-cgi.exe，此工程会在发布后自动执行此操作。工程内的发布前触发器, `/.build/default.init.aardio` 会在每次发布前停止已运行的 aardio-cgi.exe 进程，这个操作需要管理权限如果在本机上安装IIS测试，本机测试建议以管理权限启动 aardio 开发环境

3. 如果是 64 位系统，请在应用程序池属性中设置"启用 32 位的应用程序"为 True，双击生成的 aardio-cgi.exe 自动安装 FastCGI 模块时已经自动修改此设置。




