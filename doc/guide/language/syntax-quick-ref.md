---
url: https://www.aardio.com/zh-cn/doc/guide/language/syntax-quick-ref.html.md
---

# aardio 语法速览

## 第一个对话框

```aardio
import win;
win.msgbox("Hello, World!");
```

win.msgbox 可将任意类型参数（支持表对象、数组）自动转为字符串后显示。

##  第一个控制台程序

一句代码写的控制台程序：

```aardio
print("Hello, World!");
```

print 是 aardio 自带模板语法的的输出函数，可以被重写并指向不同的实际函数，在没有用到模板语法的默认情况下 print 函数会向控制台窗口输出内容，此函数可自动创建控制台窗口，并在退出线程时自动暂停等待用户按键。

console 库提供了更多控制台函数：

```aardio
//导入库
import console; 

//加载动画
console.showLoading("加载动画测试");
thread.delay(1000); //暂停 1 秒

//输出字符串
console.log('测试字符串');

//输出变量的值，如果是表或数组会输出所有元素。
console.dump({a=123;b=456});

//以 JSON 格式输出变量的值
console.dumpJson({a=123;b=456});

if(console.askYesNo("按 Y 键继续,按 N 键取消")){
	
	//输出错误信息,开发环境内会打印调用栈
	console.error("红色字体错误信息")	
}

//暂停并等待按键
console.pause(true);
```

##  第一个窗口程序  

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form(text="第一个 aardio 窗口程序")
winform.add(
button={cls="button";text="点这里";note="这是一个很酷的按钮";left=435;top=395;right=680;bottom=450;color=14120960;z=2};
edit={cls="edit";left=18;top=16;right=741;bottom=361;edge=1;multiline=1;z=1}
)
/*}}*/

//按钮回调事件
winform.button.oncommand = function(){

	//修改控件属性
	winform.edit.text = "Hello, World!";

	//输出字符串或对象，自动添加换行
	winform.edit.print(" 你好！");
	
    //改用 winform.msgboxErr 则显示错误图标
	winform.msgbox("你好");
}

winform.onClose = function(){
	if( ! winform.msgboxTest("【 确定 】要退出程序吗？") ){
		return false;
	}
} 

//显示窗口
winform.show();

//启动界面线程的消息循环（消息泵）
win.loopMessage();
```

**【窗口程序必须使用 `import win.ui` 导入 `win.form` 窗口类**。

请参考文档：[创建窗口与控件](../../library-guide/std/win/ui/create-winform.md)

## 第一个网页应用程序

```aardio
import win.ui; 
var winform = win.form(text="窗口标题")

import web.view;
var wb = web.view(winform);//创建 WebView2 浏览器控件

// 导出 aardio 函数到 JavaScript 中，在打开网页前定义才会生效
wb.external = {
	add = function(a, b) {
		return a + b;
	}	
} 

// 写入网页 HTML 代码
wb.html = /******
<div id="result"></div>

<script> 
(async ()=>{
	
	//调用 aardio 导出的 wb.external.add 函数。
	var num = await aardio.add(1, 2)

	//显示结果
	document.getElementById('result').innerText = num;
})()
</script>
******/;

//显示窗口
winform.show();

//启动界面消息循环
win.loopMessage();
```

也可以用 wb.go 函数打开网址或本地文件，例如：

```aardio
wb.go("https://www.example.com/");
```

请参考文档：[web.view 入门指南](../../library-guide/std/web/view/_.md)

## import 语句

aardio 默认会将 raw,string,table,math,time,thread,io（包含 io.file） 这些内置库自动导入全局名字空间，其他所有库（标准库或扩展库）都必须先用 `import` 语句导入后才能使用。

注意：

- import 语句可导入库模块创建的同名的名字空间或类，不能用于导入单个函数对象。
- 内置库 io 库时包含 io.file，不用再导入。
- win.form 由 win.ui 库导入。

##  名字空间

默认名字空间为全局名字空间，也就是 global 名字空间。在其他名字空间访问 global 名字空间的对象请在对象名字前加上两点 `..` 。例如 `..console` 等价于 `global.console` 。

示例：

```aardio

//导入库到当前名字空间（默认为全局名字空间）
import console;

//打开名字空间
namespace test.a.b{
   
    //定义当前名字空间变量
    console = 123;
   
    //加上 .. 前缀访问全局名字空间对象
    ..console.log(console); //等价于 global.console(console)

    //在非全局名字空间访问内置库也要加上 .. 前缀
    var str = ..string.trim(" test ")
}

console.pause(true);
```

**在非全局名字空间访问全局名字空间成员(包含内置库成员)必须要加 `..` 前缀，例如 `..string.trim`，这是 aardio 与其他编程语言较大的一个区别**

##  注释语句

```aardio
// 行注释，直到行尾，不包含换行


/*  
块注释（可包含换行），首尾标记里的星号数目必须相同。
*/
```

## 变量与赋值

```aardio

//使用 var 语句声明局部变量
var n = null;
var str = "局部变量";

//使用 self 访问当前名字空间成员
self.str = "当前名字空间变量";

// 未声明为局部变量时可省略 self 
myStr = "当前名字空间变量"; 

//使用 global 访问全局名字空间成员
global.str = "全局变量"

//可用..str 代替 global.str
..gstr = "全局变量"
```

在 aardio 中，默认情况下变量属于当前名字空间。使用 `var` 语句可以定义局部变量，使其作用域仅限于当前语句块。

##  语句与语句块

```aardio
{
    var n = null;
    var str = "字符串";
}
```

语句末尾可以加分号，也可以不加。

aardio 使用 `{` 和 `}` 来标记语句块。

使用 `var` 语句定义的局部变量具有块级作用域，即在语句块内定义的局部变量，其作用域仅限于该语句块内。

##  定义函数

```aardio

//定义函数，在括号内声明参数名 a,b （形参）。
var add = function(a, b, ...) {

    //可用三连点表示不定个数参数，只能用下面的方法才能转为数组
    var args = [...]

    return a + b,"支持多个返回值";
}

//调用函数，在括号内指定调用参数 1,2（实参）。
var num,str = add(1, 2);

/*
要特别注意，函数可以返回多个值。
可以用 () 将多个返回值转换为单个调用参数。
*/
var str = tostring( ( add(1, 2) )  );
```

- 必须用 `{}` 包围函数体
- 默认参数的值只能指定为布尔值字面量、字符串字面量或数值字面量。

##  null 值  

未定义的变量值默认为 null 。  
  
如果对象本身是 null 值，对其使用点号、普通下标会报错，对基使用直接下标 `[[]]` 则不会报错而是会返回 null 值。

用 # 操作符获取 null 值长度返回 0 而不是报错。

将表对象的成员赋值为 null 就可以删除该成员。

在条件判断中，null 值为 false。

##  数值

```aardio
var num = 123.01; //10 进制数值
var hex = 0xEFEF; //16 进制数值
var num2 = 123_456; //可用下划线作为分隔符
var num3 = 2#010; //可在 # 号前自定义进制
```

##  算术运算

```aardio
var num = 3 + 2; //加，值为 5
var num = 3 - 2; //减，值为 1
var num = 3 * 2; //乘，值为 6
var num = 3 / 2; //除，值为 1.5
var num = 3 % 2; //取模，值为 1
var num = 3 ** 2; //乘方，值为 9
```

##  位运算

```aardio
import console;


//按位取反
var n = ~2#101;
//显示2#11111111111111111111111111111010
console.printf("2#%b", n)


//按位与
var n = 2#101 & 2#100;
//显示2#100
console.printf("2#%b", n)


//按位或
var n = 2#101 | 2#110;
//显示2#111
console.printf("2#%b", n)


//按位异或
var n = 2#101 ^ 2#110;
//显示2#011
console.printf("2#%b", n);


//按位左移
var n = 2#101 << 1;
//显示2#1010
console.printf("2#%b", n);


//按位右移
var n = 2#101 >> 1;
//显示2#10
console.printf("2#%b", n);


//按位无符号右移
var n = -2 >>> 18;
//显示 16383
console.log(n);


console.pause();
```

##  字符串

```aardio
var rawString = "双引号包含的是原始字符串，编译时不处理转义符。
回车换行、单换行、单回车都会被范例化为单个换行符（不含回车符）。
这里可以用 "" 表示一个双引号。";


var rawString2 = `反引号的包含的也是原始字符串，编译时不处理转义符。
回车换行、单换行、单回车都会被范例化为单个换行符（不含回车符）。
这里可以用 `` 表示一个反引号。`;


var escapedString = '单引号包含的是转义字符串，
在编译代码时处理转义，反斜杠作为编译时转义符。
忽略字面的回车换行符，可用 \n 表示换行符';

//在文件路径前加 $ 符号，可将该文件在编译时编译为字符串（支持二进制数据）
var fileContent = $"~/aardio.exe"
```

> 单引号包含单个字符并在后面加 `#` 号表示该字符的编码（数值）,例如 `'A'#` 等价于数值 65 。

字符串操作：

```aardio
var str = "abc中文"

//从 1 截取到 3，按单字节计数,返回 "abc"
var s = string.slice(str,1,3);

//位置参数为负数自尾部倒计数，参数 @4 为 true 则按字符计数,返回 "中文"
s = string.slice(str,-2,-1,true);

//取左侧 2 个字节
s = string.left(str,2)

//取右侧 2 个字符，参数 @3 为 true 则以字符单位计数
s = string.right(str,2,true)

//右侧截取，负数表示自左侧计数，-2 表示自左侧第 2 个字节开始向右截取
s = string.right(str,-2) //返回 "bc中文"

//遍历所有字节（byte）
for(i=1;#str;1){
	print("字节码：",str[i]);
	print("字符：",str[[i]]);
}

//拆分字符串为数组，如果参数 @2 i 不指定分隔符，则按字符为单位拆分
var arr = string.split(str)

//遍历所有字符
for(i=1;#chars;1){
	var chr = chars[i]
}

//也可以用下面的模式匹配遍历所有字符，冒号 : 匹配多字节字符（例如汉字）
for chr in string.gmatch(str,":|."){ 
    print(chr)
}

//取字符串的第一个字节码（数值）
var byte1 = str[1] 

//取第一个字符的 Unicode 码点（可大于 0x10000），按字符计数（支持任意个字节表示的字符）。
var charCodePoint = string.charCodeAt(str,1);
```

拆分与合并字符串：

```aardio
var str = "hello,,,world"

//拆分，不支持模式匹配，返回 ["hello","","","world"]
var arr = string.split(str,",")

//拆分，支持模式匹配，返回 ["hello","world"]
var arr = string.splitEx(str,",+")

//合并字符符，参数 @2 指定换行符作为分隔符，单引号包含转义字符串
var str = string.join( ["字符串1","字符串2"] ,'\n')
```

找出数组中最长的字符串：

```aardio 
var longest = reduce(["apple", "banana", "g"], lambda(a, b) #a > #b ? a : b); 
```

## 用注释表示字符串

aardio 允许使用赋值语句中紧邻赋值操作符 `=` 侧的注释表示原始字符串（不处理转义符）。

示例：

```aardio 
var longRawString = /***
- 块注释首尾标记包含的星号数目必须相同。
- 忽略块紧邻注释首尾标记的一个空行（可选）。
- 其他换行总是会规范化为回车换行符 '\r\n'。 
***/
```

只有紧邻赋值操作符 `=` 符号右侧的注释才表示字符串，其他情况下注释仅仅是注释，例如 `print( /*非赋值语句中注释仅仅是注释，不是字符串*/ + "" )` 会报语法错误。

## 字符串的文本编码

```aardio
var utf8 = "字符串可以包含文本,aardio 字符串默认使用 UTF-8 编码"

var utf16 = '单引号包含的编译时转义字符串后加 u 后缀表示 UTF-16 编码字符串'u
```

aardio 在很多地方都支持自动编码转换，例如调用 Unicode(UTF-16) 版本的原生 API 函数（例如 C++ 的 Unicode 函数，操作系统 Unicode 版本的 Windows API ）时，UTF-8 字符串可自动转换为UTF-16 编码字符串（ 支持双向自动转换），例如以下两句代码的作用是相同的：

```aardio
::User32.MessageBox(0,"内容","标题",0);


::User32.MessageBox(0,'内容'u,'标题'u,0);
```

在调用 `::User32.MessageBox` 时，aardio 会自动检测并优先使用 Unicode 版本的 `::User32.MessageBoxW` 函数。当然，也可以主动在 API 函数名后加上大写的 `W` 尾标声明这是 Unicode 版本函数（即使该函数名尾部并没有 `W`，也可以添加 `W` 尾标以声明使用 UTF-16 编码的字符串参数 ）。


##  二进制字符串

aardio 字符串既可以包含纯文本，也可以包含二进制数据。

```aardio
var bin = 'aardio 字符串可以包含任意二进制数据，例如 \0 这后面不会被阶段' 

//字符串可以包含任何二进制文件，在文件路径前加 $ 可将文件编译为二进制字符串。
var imageBytes = $"/images/test.jpg"

```

aardio 字符串也可以作为只读的字节数组使用，可以用下标读取字节码（只能读不能写）。

```aardio
var str = "abc"

//普通下标输出 a 的字节码 97
print( str[1] ) //下标访问的是单个字节

//双层中括号表示的直接下标输出字符 "a"
print( str[[1]] ) 

for(i=1;#str;1){
	print( str[[i]] + "的字节码是：" + str[i]);
}
```

对字符串使用下标，索引超出范围返回 null 而不会报错。

对字符串使用 `#` 操作符、下标 `[]`  或直接下标 `[[]]` 都是按二进制字节计数，例如中文字计为多个字节。

也可以调用 `string.unpack( str,startIndex,endIndex )` 将字符串拆分为字节码数组（返回一个或多个数值，不指定位置参数时默认返回所有字节）, 例如 `var b1,b2,b3,b4 = string.unpack('\x01\x02\x03\x04')` 返回 1,2,3,4 。

## UTF-16 字符串

对于 UTF-16 字符串，下标或直接下标按双字节表示的宽字节码或宽字符计数（ 4 字节代理对计为 2 个单位 ），示例：

```aardio
var utf16Str = '你好'u
 
//下标一次取 2 个字节，所以要用 #utf16Str/2 取字符个数。
for(i=1;#utf16Str/2;1){
	print( utf16Str[[i]] , utf16Str[i]);
}
```

使用 ustring 库获取与转换 Unicode 代码：

```aardio 
import ustring;

var utf16String = '💻字符串'u

//将字符串参数转换为 Unicode 码点数组，参数可以是 UTF-8 或 UTF-16 字符串。
var charCodePoints = ustring.toCharCodes(utf16String);

print( charCodePoints[1] == 0x1F4BB );// 输出 true

//转换为 UTF-16 字符串，可指定一个或多个码点参数，也可以指定一个码点数组
utf16String = ustring.fromCharCode(charCodePoints)

//转换为 UTF-8 字符串，可指定一个或多个码点参数，也可以指定一个码点数组
var utf8tring = string.fromCharCode(charCodePoints)
```

## buffer 二进制字节数组

buffer 类型用于存储可读写的二进制字节数组。
在 aardio 的字符串函数中，如无特别说明，buffer 与 字符串类型通常可以相互替代。

```aardio
//创建指定长度的数组
var buffer = raw.buffer(20)

//可选用参数 @2 指定初始值（默认为 0，可指定数值指字节码、字符串、结构体、指针）.
var buffer = raw.buffer(20,"abc")

//直指转换初始值为 buffer，参数 @1 可指定 字符串、结构体、或需要复制的源 buffer
var buffer = raw.buffer("abc")

//结构体转 buffer
var buffer = raw.buffer({
    int x=1;
    int y=2；
    double array=[1,2,3];
})

//可用下标读写字节，起始索引为 1
buffer[1] = 0
```

## 十六进制编码

```aardio
//创建指定长度的数组
var str = "abc"

//字符串转 16 进制，可选用参数 @2 指定前缀符号。
var hex = string.hex(str,"%") //返回 "%61%62%63"

//16 进制转为字符串，可选用参数 @2 指定前缀符号
var str = string.unhex(hex,"%") //返回 "abc"

str = string.unhex("0x61,0x62,0x63","0x") //返回 "a,b,c"

//不指定分隔符则前缀为空，无前缀参数时不能混杂非 16 进制编码字符
var hex = string.hex(str) //返回 "616263"
```

##  字符串连接

```aardio
// aardio 的字符串连接操作符是两个加号： ++ 
var str = "字符串1" ++ "字符串2";


//如果 ++ 前后紧邻引号，可略写为单个 + 号。
var str = "字符串1" + "字符串2";

//调用内置函数拼接字符串，支持 2 ~ n 个参数，支持 null 值参数
var str = string.concat("字符串1","字符串2")
```

##  表 (table)

table（表）是 aardio 中唯一的复合数据类型，除了非复合的基础数据类型以外，aardio 中几乎所有的复合对象本质上都是表，即使是变量的命名空间也是表。表的本质是一个集合，可以用于容纳其他的数据成员，并且表也可以嵌套包含其他的表（table），在 aardio 里表几乎可以容纳一切其他对象。

  
1.  表可以包含键值对

    ```aardio
    tab = {
        a = 123;
        str = "字符串";
        [123] = "不符合变量命名规则的键应放在下标内。";
        ["键 名"] = "不符合变量命名规则的键可以放在下标内。";
        键名 = {
            test = "表也可以包含表";
        }
    }
    ```

2.  表可以包含有序数组

    如果表中不定个数的成员的“键”是从1开始、有序、连续的数值，那么这些成员构成一个有序的稠密数组。aardio 中如果不加特别说明，数组一般特指稠密数组，aardio 中操作数组的函数一般都是用于操作稠密数组。

    ```aardio
    //在表中创建数组
    var array = {
        [1] = 123;
        [2] = "数组的值可以是任何其他对象";
        [3] = { "也可以嵌套包含其他表或数组"}
    }
    ```

    稠密数组的键可以省略，下面这样写也可以（并且建议省略）

    ```aardio
    var array = {
        123,
        "数组的值可以是任何其他对象";
        {"也可以嵌套包含其他表或数组"}
    }
    ```

    要特别注意 aardio 的数组起始索引为 1 。

    也可以使用 `[]` 构造纯数组（ pure-array ），例如：

    ```aardio
    var array = [
        123,
        "数组的值可以是任何其他对象";
        ["也可以嵌套包含其他表或数组"]
    ]
    ```

    纯数组内部如果出现 `[]` 包含的成员也表示数据，不会解析为下标包含的键名。纯数组的数据类型也是表（类型名为 "table"），但只有纯数组传入 table.isArray 函数会返回 true 。table.isArrayLike 函数的检测条件则更宽松，参数是纯数组或包含稠密数组成员、或者使用元属性声明为数组、来自外部接口可识别处理为稠密数组的表都会返回 true 。
    
3. 表可以包含稀疏数组

    如果表中包含的成员使用了数值作为键，但是多个成员的键并不是从1开始的连续数值 - 则构成稀疏数组。在 aardio 一般我们提到的数组 - 如果未加特别说明则是特指有序的稠密数组（不包含稀疏数组）。
    
    如果表中包含了稀疏数组 - 也就是说成员的数值键（索引）包含不连续的、中断的数值，那么不应当作为有序数组使用。 aardio 中几乎所有针对数组操作的函数或操作符 - 如果未加特别说明都要求参数是有序数组。

    下面的数组就包含了 null 值，属于数值键（索引）不连续的稀疏数组：  
    `var array = { "值：1", null, "值：2", "值：4", null, null,"其他值" }`

4. 表的常量字段

    如果表成员的键名以下划线开始，则为常量字段（一旦赋`非 null 值`后就不可再修改） —— 可通过设置表对象的元属性 `_readonly` 禁止在该表中使用此规则。

5. 表的类 JavaScript 写法

    如果表不是一个声明原生类型的结构体，那么在表构造器中允许使用类 JavaScript / JSON 语法，可用冒号代替等号分隔键值，用逗号代替分号分隔表元素，并允许用引号包含键名。

    示例：

    ```aardio
    var tag ={"name1":123,"name2":456,arr=[1,2,e]}
    ```

    上面包含键名的双引号也可以省略，这种写法的表作为函数参数使用时不可省略外面的大括号 `{}` 。

6. 表作为函数参数的简写法

    如果函数的参数是一个普通的表构造器（ 构造表对象的字面值 ），并且第一个元素是以等号分隔的名值对（并且键名不在 `[]` 内），则可省略最外层的 `{}` 。例如 `console.dump({a = 123,b=456})` 可以简写为 `console.dump(a = 123,b=456)`。

    这种写法很像命名参数，但 aardio 里并没有命名参数。
    
    - 如果参数表是一个结构体则不能省略外层的 `{}`。
    - 如果参数表使用类 JavaScript 语法用冒号分隔键值对，则不能省略外层的外层的 `{}` 。

##  成员操作符

```aardio
var tab = {a = 123; b = 456};

//用点号访问表成员，点号后面必须跟合法的标识符
var item = tab.a;

//用下标操作符 [] 访问表成员，[] 里可以放任何表达式。
var item = tab["a"];

/*
用直接下标操作符 [[]] 访问表成员，[[]] 里可以放任何表达式。
直接下标不会触发元方法，忽略重载的运算符。
*/
var item = tab[["a"]];

tab = null;

//无论操作对象存储的是什么值，直接下标 [[]]  失败返回 null 而不是报错。
item = tab[["a"]];
```

## 判断表是否包含指定键

无论表对象里一个键是否存在，都可以使用下标操作符`[]`直接检索，访问不存在的键仅返回 null ，不会报错。。

```aardio
var tab = {
	key1 = "value1";
	key2 = "value2";
	array = [1,2,3]
}

var keyName = "key2"
if(tab[keyName]){
	print("tab 里存在指定键： key1 ")
}

if(tab.array[3]){
	print("tab.array 存在指定数组索引： 3 ")
}
```

任意类型对象（不包含字面值）使用由双层中括号表示的`直接下标`操作符 `[[]]` 检索键值都不会报错（失败仅返回 null 值 ）：

```aardio
var anyObj = null;

//使用直接下标操作符不报错
if( ! anyObj[["key2"]] ){
	print("没找到")
}

//报错，对 null 值不能使用普通下标操作符
if( anyObj["key2"] ){
	
}
```

## 表的只读成员

表对象（ table ）的成员名称如果首字符为下划线，并且长度大于 1 个字节并小于 256 个字节，则是一个只读成员（ readonly member ）。

示例：

```aardio
var tab = {
    _readonlyMember = "赋为非 null 值后就不可修改";
    member = "值可以修改";
}
```

可以在表的元表中如下禁用表的只读功能：

```aardio

// @ 操作符用于访问对象的元表
tab@ = { _readonly = false}
```

使用 `tab@` 可以获取或设置 `tab` 的元表，对象的元表可以设置对象的行为或者用于重载操作符。

一个对象被设置元表后默认不允许再替换或移除元表， 除非元表内指定了 _float 字段的值为 true，例如 `tab@ = { _float = true }` 就创建了一个非锁定的元表，可以被移除或替换。


## 表操作


##  取数组或字符串长度

```aardio
var array = {123;456};
print("数组长度",#array);

var str  = "abcd";
print("字符串长度",#str);

var n  = null;
print("null 值长度为 0",#n);
```

## 数组操作

```aardio
//创建长度为 3 的数组（指定多个长度参数创建多维数组），最后一个参数指定元素的初始值为 0
var arr = table.array(3,0); //等价于 [0,0,0]

//创建数组，添加从 1 循环到 10 步进为 2 的数值。
var arr = table.range(1,10,2); //等价于 [1,3,5,7,9]

//创建数组
var arr = [1, 2, 3, "hello"];

//取第一个元素
var firstItem = arr[1]

//取最后一个元素
var lastItem = arr[ #arr ]

//在末尾添加元素
table.push(arr, "world");

//删除末尾元素
var last = table.pop(arr);

// 在开头添加元素
table.unshift(arr, 0);

// 删除开头元素
var first = table.shift(arr);

// 在索引 2 处插入元素
table.insert(arr,"aardio", 2);

// 删除索引 3 处的元素 
table.remove(arr, 3);  

// 查找元素
var index = table.find(arr, "hello"); 

// 遍历数组
for(i=1;#arr;1){
	print( arr[i] );
}

//自参数 2 指定的位置截取到参数 3 指定的位置（负数表示自尾部倒计数），返回新数组
var arr2 = table.slice(arr,2,-1)

//参数(数组,删除位置,删除个数,任意个添加项)
table.splice(arr,1,2,"newItem1","newItem2")

//将一到多个数组参数追加拼接到参数 1 指定的数组
table.append([1,2,3],[4,5,6]) //返回增加了数组元素的的参数 1，新的值为： [1,2,3,4,5,6]
```
## 数组排序

table.sort 函数用于排序数组，可选用第二个参数自定义排序函数。

```aardio
var arr = [789,123,456];

//默认排序（较小的元素在前）
table.sort(arr)

//自定义排序，隐式传递的 owner 参数表示当前元素，回调参数 next 表示下一个元素
table.sort(arr,lambda(next) owner > next ); //不要用 <= 或 >= 比较，相等不能返回 true

print( arr );
```

## 调用标准库解析 JSON

```aardio
import JSON;

var jsonString = `{
    a: 123,
    b: 456,
    c: [1,2,3]
}`

// JSON 解码为表对象
var tabObject = JSON.parse(jsonString);

tabObject = {
    a: 123,
    b: 456,
    c: [1,2,3]
}

// 表对象编码为 JSON 
jsonString = JSON.stringify(tabObject); 

// 将参数 2 指定的对象转为 JSON 并保存到文件，参数 3 为 true 则格式化 JSON 以添加缩进排版以增强可读性。
JSON.save("/config.json",{ name = "value"},true)

// 自文件加载并解析 JSON，返回解析后的对象
tabObject = JSON.load("/config.json")
```

aardio 对象与数组的基本写法与 JSON 是相似的。

请参考文档：[JSON 库参考](../../library-reference/JSON/_.md)

##  调用原生 API

```aardio
//加载 DLL 模块
::Msvcrt := ..raw.loadDll("Msvcrt.dll",,"cdecl");

//调用原生 API 函数。
var result = ::Msvcrt.memcmp("abc",raw.buffer("abc"),3);

//也可如下先声明 API 函数的参数与返回值类型
memcmp = ::Msvcrt.api( "memcmp", "int(ptr buffer1,ptr buffer2,INT size)") 

//调用已声明的 API 函数
result2 = memcmp("ABC","abc",3);
```

> 在合法标识符前加上 :: 前缀，可将其转换为全局有效的`保留常量`。 `保留常量`首字母不能是小写字母或者下划线。如果是需要全局重用的 DLL 模块通常使用 `::` 声明为全局保留常量（并且大写首字母）。初始化赋值语句 `::ReservedName := initialExpression` 等价于 `::ReservedName = ::ReservedName : initialExpression` , 这样写的目的是当 `::ReservedName` 已经是真值时可避免重新计算 `initialExpression` 并且保留原来的值。

aardio 已经默认加载了一些常用的系统 DLL 对象，例如 `::User32`, `::Kernel32`, `::Shell32`，`::Ntdll` 等。  示例：

```aardio
::User32.MessageBox(0,"测试","标题",0);
```

##  原生结构体

```aardio
var info = {
    INT size = 8;
    INT tick;
}
::User32.GetLastInputInfo( info )
```

结构体也是一个表对象（table）, 但在字段名前面可以添加原生数据类型声明。
这不会改变对象在 aardio 中存储的 [aardio 数据类型](../../language-reference/datatype/datatype.md)，仅在调用原生 API （ 例如 Windows API ）或 raw 库函数时 [原生类型](../../library-guide/builtin/raw/datatype.md) 才会起作用。

aardio 结构体与原生 API 可声明的原生数据类型如下：  
`BYTE, byte, WORD, word, INT, int, LONG, long, double, bool, POINTER, pointer, STRING, string, ustring, struct, union`

- int 为 32 位，long 为 64 位。
- 大写整型表示无符号数，整型加 u 或 U 前缀也表示无符号数。例如 `INT`,`UINT`,`uint` 都表示 32 位无符号数。
- int / INT 可添加 ８,16,32,64 后缀以指定位长，例如 `int8`,`int16`,`int32`,`int64`,`uint8`,`uint16`,`uint32`,`uint64`。
- 大写 `POINTER`，`STRING`，`USTRING` 的类型不支持 `null` 值，小写时支持 `null` 值。
- `ustring` 适用宽字符串类型，在调用原生接口时自动进行 UTF-8 / UTF-16 编码双向转换，调用者不需要再转换。
- 单个类型名必须完全小写或完全大写所有字符。

调用原生 API 时结构体参数总是传址（传指针）的输出参数， 
输出参数会自动添加到返回值中（ aardio 支持多返回值 ）。示例：

```aardio
var ok,info = ::User32.GetLastInputInfo( {
    INT size = 8;
    INT tick;
} );
```

参考：[使用结构体](../../library-guide/builtin/raw/struct.md)

🅰 示例：

```aardio
//定义结构体
var struct = {
	int8 bytes[5] = [1,2,3,4,5];
	int16 w = 1;
	double d = 1.1;
	UINT16 wstr[] = "UTF-16 字符串"; 
}

//结构体打包为 buffer
var buffer = raw.buffer(struct);

//结构体打包为字符串
var str = raw.tostring(struct);

//解包数据到结构体，参数 1 可指定字符串、buffer、指针
var newStruct = raw.convert(str,{
	int8 bytes[5];
	int16 w = 1;
	double d = 1.1;
	UINT16 wstr[10]; 
});

print(newStruct.wstr);

//打开文件
var file = io.file("/example.bin","w+b")

//写结构体到文件
file.write( {
	int x = 12;
	int y = 23;
	float arr = [1.1,2.2,3.3];
	_struct_aligned = 1; //自定义结构体对齐字节
})

//移到文件开始
file.seek("set",0)

//自文件读取结构体
var st = file.read({
	int x;
	int y;
})

print(st.x, st.y);

```

🅰 示例：

```aardio
//将字符串（或 buffer ，其他结构体）转换为参数 2 指定的结构体。
var struct = raw.convert('\x00\x01\x02\x03\x04',{
	INT num; 
},1/*字节偏移量，不指定则默认为 0*/)

//输出数值 0x04030201
print( struct.num  )

//输出反转字节序的数值 0x01020304
print( raw.swap(struct.num,"INT") )
```

## 日期时间

```aardio
//获取系统运行毫秒数
var tk = time.tick();

//获取当前时间
var now = time()

/*
创建时间对象。
- 参数 @1 可以是数值时间戳，字符串
- 参数 @2 指定时间格式化串，省略则默认为'%Y/%m/%d %H:%M:%S'，字符串解析为时间的规则宽松，空白、标点、字母、中文等都可以匹配任意个数同类字符。忽略首尾不匹配的字符以及完整日期后不匹与的字符。
- 可选用参数 @3 指定格式化语言，例如 "enu"
*/
var tm = time("2017-05-27T16:56:01Z",'%Y/%m/%d %H:%M:%S') 

//自时间戳（秒）创建时间对象
var tm2 = time(123456)

//转换为 UTC 时间
var tm3 = tm.utc(true)

//解析 RFC 1123 格式，RFC 850 格式时间
var tm3 = time.gmt("Sun,07Feb2016 081122 +7")

//解析 ISO8601 时间
var tm4 = time.iso8601("20170822 123623 +0700")

//解析 ISO8601 时间（14 位数字或 12 位数字）
var tm5 = time("20170822123623")

//时间对象格式化为字符串
var str  = tostring( time() )

//时间对象转字符串，并在参数 2 中指定新的格式化串 
var str = tostring( time(),"%Y/%m/%d %H:%M:%S")

//时间对象支持字符串连接操作符，在连接时自动格式化
var str = "时间：" ++ time()

//时间转为时间戳数值，以秒为单位
var stamp  = tonumber( time() )

//时间对象必须转换为数值才能参与算术运算。
var num = tonumber( time.now() ) % 30

//返回以毫秒为单位的时间戳
var stamp = time.stamp()

//参数 1 为 true 则返回字符串，参数 2 为 1 则返回时间戳以秒为单位
var stamp = time.stamp(false,1);

print(tm5);
```

##  逻辑操作符

逻辑非操作符可以使用 `not` 或 `!` 。

逻辑与操作符可以使用 `and`  、`&&` 或者 `?` 。

逻辑或操作符可以使用 `or` 、 `||` 或者 `:`  。

为了兼容其他语言的习惯,aardio 提供了多种逻辑操作符的写法。

##  三元运算符

```aardio
var n = 1;

// ret 值为 true
var ret = n ? true : 0;
```

要特别注意 `?` 实际上是逻辑与操作符，`:` 实际上是逻辑或操作符。如果 `a ? b : c` 这个表达式里 `b` 为 `false`，则该表达式总是返回 `c`。这与其他类 C 语言的三元操作符不同，请注意区分。

##  if 语句

```aardio
if ( _AARDIO_VERSION >= 40 ) {
	print('内核主版本号 >= ' + _AARDIO_VERSION);
}
```

注意在条件判断中，非 `0`、非 `null`、非 `false` 为 `true`，反之为 `false`。

如果要准确判断一个变量的值是否为 `true` 或 `false` ，则应使用恒等操作符，如下：

```aardio
import console;
var enabled = false;

if (enabled === false ) {
  console.log('enabled 的值为 false，不是 0，也不是 null');
}
elseif( enabled ){
  console.log('enabled 的值为真');
}
else{
  console.log('enabled 值为：',enabled);
}

console.pause(true);
```

上面的 `elseif` 也可以改为 `else if`。

## for 循环语句（ Numeric for ）

aardio 使用基于数值范围的 for 循环语法（Range-based for）。

`for` 循环语句基本结构：  
  
```aardio
for(i = initialValue;finalValue;incrementValue){
    //循环体
}
```  

- 可选用 `()` 包含循环条件，可选用分号或逗号分隔循环参数。
- 循环体可以是用 `{}` 包含的语句块，也可以是一个单独的语句。
- 所有循环参数都仅在循环开始前计算一次，循环过程中不会重新计算。
- 循环变量 `i` 的值从 `initialValue` 开始，到 `finalValue` 结束（包含 `finalValue` ），每次递增 `incrementValue`。
- 循环增量 `incrementValue` 可使用负数表示递减循环。 省略 `incrementValue`  则默认设为 1 。

示例：  

```aardio
import console;

// 循环变量 i 从起始值 1 循环到终止值 10 ，每次递增步长为 2。
for(i=1;10;2){ 
    
    // 在控制台输出 1,3,5,7,9
    console.log(i); 
}

// 循环变量 i 从起始值 1 循环到终止值 10 ，每次递增步长为 1。
for(i=1;10;1){
        
    i++; // 修改循环变量的值，使步长变为 2
    
    // 循环变量设为无效数值，下次循环以前自动修复为计数器值
    i = "无效数值" 
}

// 循环变量 i 从起始值 10 循环到终止值 1 ，每次递减步长为 1。
for(i=10;1;-1){  
    
    // 在控制台输出 10,9,8,7,6,5,4,3,2,1
    console.log(i); 
}

console.pause()
```  

##  for in 泛型循环语句（Generic for）

```aardio
var tab = {
	a = 123;
	b = 456;
}

//遍历表对象的所有键值
for(k,v in tab){
	print(k,v)
}
```

`in` 后可以指定：
- 表对象（或数组）：循环遍历该表
- 函数对象（迭代器）：循环调用该函数直到返回 null 
- 函数调用语句（迭代器工厂）：循环调用返回的迭代器函数直到返回 null 
- 其他任何值：忽略不报错

aardio 中所有创建迭代器的工厂函数名字都以 `each` 开始，示例：

```aardio
import process

//遍历已运行进程，参数支持模式匹配
for processEntry in process.each( ".*.exe" ) {
    print( processEntry.szExeFile,processEntry.th32ProcessID );
}

import winex;

//遍历顶层窗口，参数可选指定要搜索的类名与标题，支持模式匹配。
for hwnd,title,threadId,processId in winex.each("","") { 	
    print(hwnd,title)
}

var tab = { a = 123;b = 456 }

//按字典序输出表中的名值对
for k,v in table.eachName(tab){
    print(k,v)
}
```


##  while 循环语句

```aardio
var i = 0;

//i 的值小于 100 时循环执行代码
while (i < 100) {  
    i++;
    print(i);
   
    if(i==10){
        break; //i 的值等于 10 时中步循环
    }
}
```

## select case 语句

select 语句选定一个值，并执行首个匹配选定值的 case 语句。
case 语句不会向下穿透（ fall-through ）, 不能用 break 退出 。
如果所有 case 都不符合条件则执行可选的 else 语句。

```aardio
var selectValue = 0;

select( selectValue ) {

	case 1 { 
		print("selectValue 为 1")
	}
	case 2,3,4{
		print("selectValue 为 2,3,4 之一")
	}
	case 5;10 {  
		print("selectValue 为 5 到 10 范围的值")
	}
	case !== 0 { 
		print("selectValue 不等于 0")
	}
	else{ 
		print("以上 case 都不符合条件")
	}
}
```

##  类

```aardio
import console;

//定义类
class className{
	
	//可选定义类的构造函数，必须写在最前面。
	ctor(name,...){ 
		//在类主体内部通过 this 对象访问当前实例成员，this 仅在类主体内有效。
		this.property = staticProperty; //无 this 前缀的 staticProperty 是类的静态成员。
		
		this.args = [...]; 

        /*
        类内部有四种不同的变量：
        1. 在类构造函数内用 var 定义的变量是在整个类主体范围内有效的私有变量，类外不能访问。
        2. 在类主体范围内没有用 var 定义的变量默认是在类的独立名字空间内有效的静态变量，也可以通过 self 前缀访问，例如 self.staticProperty ，类的所有实例共享。
        3. 在类主体范围内通过 this 前缀访问的变量是类的实例对象的成员变量，例如 this.property 。
        4. 使用 .. 前缀访问的全局变量，例如 ..console.log。
        */

        var privateVariableScopedToTheClassBody = "私有变量"

        staticProperty = "类的静态变量值"; //等价于  self.staticProperty = "类的静态变量值"

        this.member = " 实例对象的成员 "

        //必须通过 .. 访问全局名字空间成员
        ..console.log( ..string.trim(this.member) )

	};
	
	//定义属性，必须写为名值对格式，类的成员必须使用分号隔开
	property = "value";
	
	//定义方法(成员函数)，必须写为名值对格式，不能省略等号或 function 关键字。
	method = function(v){
		if(v===null){
			//在类内部可以直接访问类名字空间的静态成员
			v = staticProperty; //等价于 self.staticProperty
		}
		
		//访问外部全局名字空间必须加上 .. 前缀
		..console.log("method",v);
		
		this.property = v; 
	};
}

//打开类的名字空间（类的所有实例共享）定义静态成员
namespace className{
	
	staticProperty = "类的静态变量值";
	
	staticMethod = function(){
		..console.log("staticMethod",staticProperty);
	} 
}
	
//调用类创建对象
var object = className();

//调用对象的方法（成员函数）
var v = object.method("新的属性值");

//在类主体外添加实例方法。
object.onSomeEvent  = function(){
    //在类主体外应当使用 owner 参数引用当前对象，在类主体外部 this 对象默认是 null 值。
    ..console.log( owner.property )
}

object.onSomeEvent();

//通过类名访问类的名字空间，通过类名字空间访问类的静态成员
className.staticMethod();

console.pause();
```

- 仅在类主体内部可以用 `this` 访问当前实例对象， `this` 实际是类主体内自动定义的一个闭包变量。"""类主体"""指的是定义类的 `class {}` 这个主体部分，包含在类内部定义的构造函数、方法、属性等，而 `namespace className{}` 则不属于类主体。
- 类总是先调用可选的构造函数 `ctor` 然后再初始化其他成员。
- 每个类都有独立的的名字空间，类创建的所有实例共享同一名字空间，类名字空间的成员也就是类的静态成员。

> **使用类名作为名字空间时一定要先定义类：**
> 
> 虽然类名也是名字空间，但普通的名字空间不是一个类。
> aardio 里用 `namespace` 语句打开名字空间时总会检测同名的名字空间是否已存在，如果存在则打开相同的名字空间。
> 如果先定义类，后打开同名的名字空间则不会有问题，因为类也是名字空间，namespace 语句会重用类的名字空间。
> 但是如果反过来先创建同名的名字空间，后定义同名的类，那么通过类名就只能访问类的名字空间，而不是指向之前的名字空间。

## self,this,owner 对象

`self` 表示当前名字空间。

`this` 仅在定义类的主体内部表示当前构造的实例对象。

每个函数都有一个隐式传递的 `owner` 参数。如果用点号 `.` 作为成员操作符获取并调用对象的成员函数（ ownerCall 方式 ），则点号前面的对象是被调用成员函数的 `owner` 参数，否则被调用函数的 `owner` 参数为 `null`。 

示例：

```aardio
object = {
    property = "value";

	method = function(a,b){
		print("我的 property：",owner.property);
        return a+b;
	}
}

//下面这样调用（ ownerCall 方式 ）时 owner 参数指向 object
object.method(123,456); //owner 是隐式传递的参数，且不占用实参位置

//通过 invoke 或 call 调用函数对象时，可用参数 2 自定义 onwer 参数
invoke(object["method"],object,123,456);

//下面这样调用时 owner 参数为 null （一般不应当这样写）。
object["method"](123,456);
```

独立的 aardio 代码文件编译后也相当于一个匿名的函数，其 `owner` 参数默认为 `null` 。使用 `import` 语句加载库文件时， `owner` 参数为库路径或资源文件数据（ 如果是编译后的内嵌库则 owner 指向资源文件 ）。

`owner` 在元方法中表示左操作数。

在迭代器函数中， `owner` 表示迭代的集合对象。

用 `call`, `callex`, `invoke` 等函数调用其他函数时可显式指定 `owner` 参数的值。

## 使用 aardio 模式匹配

请参考：[模式匹配快速入门](pattern-matching.md)

aardio 与查找替换字符串有关的函数基本都支持专有的模式匹配语法。

**要点：**

- 模式匹配的语法比正则表达式简单、速度更快。
- 模式匹配只能对子模式使用量词等运算符，`()` 包含的捕获组不是子模式也不能对其使用任何运算符（这是与正则表达式最主要的区别）。
- 模式匹配必须使用尖括号`<>`包含非捕获组，非捕获组都是原子分组。在 aardio 模式匹配里非捕获组又被称为元序列。非捕获组可以将连续的字符序列转换为单个子模式，可以对非捕获组使用量词等模式运算符。

**模式转义符：**

模式匹配使用 `\` 作为转义符，例如 `\<\>` 表示普通的尖括号而不是表示非捕获组，`\n` 则表示换行。更多转义规则请查看模式匹配语法。如果将模式串写在用单引号包围的转义字符串内则因为 aardio 会先处理编译期转义符，就需要用两个反斜杆表示模式转义符中的单个反斜杆。。在不处理编译转义符的原始字符串里写模式串则可以直接用单个反斜杆 `\` 作为模式转义符。

对比：

- 模式串 `"\n"` 或者 `'\\n'` 包含两个字符（ 一个反斜杆，一个字母 n ），使用模式转义符匹配换行符。
- 而单引号包围的转义字符串 `'\n'` 会被编译为单个换行符，这是真正的换行符。

**查找字符串示例：**

```aardio
var str = "abc ||| 123";

/*
`(\a+)` 匹配连续的字母，此捕获组增加一个返回值
`<\s+>|<\s*\|+\s*>` 匹配连续的 "|||" 或者空白分隔符
`(\d+)` 匹配连续的数字，此捕获组增加一个返回值
*/
var letters, numbers = string.match(str, `^(\a+)<\s+>|<\s*\|+\s*>(\d+)$`);
print(letters, numbers)
```

**查找字符串示例：**

```aardio
var idNumber = "sfz612323198608110000fgd"  

/*
匹配 15 或 18 个连续数字，最后一个字符也可以是 x 或 X。
只有 `<\d\d\d>*` 这样的非捕获分组才能加 `*` 这样的量词运算符。
如果是 `(\d\d\d)*` 这样写就是错的，对捕获组不能使用任何运算符。

`!\d` 是一个边界断言，边界断言是在子模式前面加一个表示否定的感叹号，表示从不匹配（左侧）到匹配（右侧）的边界，这是一个前后双向的零宽断言。
*/
idNumber = string.match(idNumber,"!\d(\d{14}<\d\d\d>*[\dxX])![^\dxX]")  
print( idNumber );
```

**查找字符串示例:**

```aardio
var str = "<title>标题</title>"

//使用 `\<` 转义  `<` 以表示其字面意义，避免被解释为模式匹配的非捕获组
var title = string.match(str,"\<title\>(.+)\</title\>")
print(title);
```

**替换字符串示例：**

```aardio
var str = "1hello 2world";
//将捕获组 2 移到 捕获组 1 前面
str = string.replace(str, "(\d)(\a+)","\2\1");
print(str)
```

注意在替换字符串参数中用 `\1` 到 `\9` 表示向前引用捕获组。

**非捕获组示例：**

```aardio
/*
用尖括号创建元序列（也是非捕获组、原子分组）,
将连续的字符变成单个子模式，只对子模式使用量词等运算符
*/
var s = string.match(`中文 abc`, `<中文\s+[a-z]+>+`)  
```

**匹配成对的字符串：**

```aardio
//对称匹配，% 紧根的两个子模式必须首尾成对出现
var s = string.match(`func(a(b)cd)`, `%()`) //返回 `(a(b)cd)`

//最近对称匹配
var s = string.match(`(a(b)cd)`, `%()?`) //返回最里层的 `(b)
```

**先排除其他模式串参数匹配的部分然后再查找替换字符串：**

```aardio
var str = "123(456)789"

//参数 @2 匹配的部分替换为参数 @3，参数 @4 以及之后的参数匹配的部分被事先排除（这里用对称模式 % 排除了成对括号包围的部分）
str = string.replaceUnmatched(str,"\d","-","%()")
print(str);
```

**连续匹配多个参数示例：**

```aardio
var str = string.reduce("print(a,(1+2))",
	"\w+(%())",//先匹配对称括号
	"^\((.+)\)$",//进一步细分匹配
	"(\d)\)" //更多模式参数
);
```

**连续匹配并替换示例：**

```aardio
var str = string.reduceReplace("print(a,(1+2))",
	"\w+(%())",
	"\((.+)\)",
	"\1" //替换为捕获组 1
);
```

##  文件路径

使用文件路径：

```aardio
/*
双引号或反引号包含的原始字符内内反斜杠不是编译转义符，
因此不需要用两个反斜表示单个反斜杠。

文件路径开始第一个字符可用单个斜杠（或反斜杠）表示应用程序根目录。
应用程序根目录在开发时指工程目录，发布后指 exe 文件所在目录。
*/
var str = string.load("\res\test.txt");


//文件路径中的正斜杠可自动转换为反斜杠
var str = string.load("/res/test.txt");

/*
文件路径开始用波浪线表示 exe 所在目录。
aardio 不需要生成 exe 就可以运行调试，此时 "~/" aardio 开发环境根目录。
*/
var str = string.load("~/res/test.txt");


/*
aardio 很多读文件的函数都兼容工程内嵌资源目录。
"\res\test.txt" 可以是资源目录也可以是文件。
*/
var str = string.load("\res\test.txt");

//文件路径前加 $ 操作符可将文件编译为字符串（可包含二进制）
var fileData = $"/include/example.jpg"

/*
将文件转换为完整路径。
将路径传给第三方组件时，建议这样转换一下。
*/
var path = io.fullpath("\res\test.txt");

//文件是否存在
var path = io.exist(path)

import fsys;
var isDir = fsys.isDir(path); //是否已存在的目录
var isFile = fsys.isFile(path); //是否已存在的文件
```

拆分文件路径：

```aardio
/*
拆分路径，返回的 pathInfo 包含以下字段的表对象：

filename: 文件名（含后缀名）
name: 文件名（不含后缀名）
ext: 后缀名，含 . 号，保留大小写
dir: 目录路径
*/
var pathInfo = io.splitpath(path)
```

获取常用路径：

```aardio
var desktopDir = io.getSpecial(0/*_CSIDL_DESKTOP*/);
print( desktopDir );

//获取 %localappdata%\aardio\std 目录,可增加任意个子目录参数，返回正确拼接后的路径
var dataDir = io.getSpecial(0x1c/*_CSIDL_LOCAL_APPDATA*/,"aardio\std");
print( dataDir );

//展开字符串内的环境变量
var path = string.expand("%localappdata%\aardio\std") 

//获取 %localappdata% 目录下的路径，可选用参数 2 指定文件的更新数据（未更新时不写文件）
var path = io.appData("aardio\std\test.txt")

//获取应用程序根目录，aardio 函数基本都可以直接写 "/" 或 "\"，调用外部接口才需要如下转换为完整路径
var appDir  = io.fullpath("/")

//获取当前 EXE 目录，注意 aardio 在开发调试时不需要先生成 EXE 文件而是以 aardio.exe 创建进程直接运行 aardio 代码。
var exeDir = io.fullpath("~/")

//生成临时文件路径，可选用参数指定前缀名与后缀名
var tempPath = io.tmpname("prefix",".txt")

//获取当前目录，等价于 io.fullpath("./")
var cd = io.curDir()

//修改当前目录
io.curDir("/")
```


## 文件对话框

```aardio
import fsys.dlg;

//打开
var path = fsys.dlg.open("图像|*.jpg;*.png|","标题","/默认目录",winform/*父窗口*/);

//保存
var path2 = fsys.dlg.save("图像|*.jpg;*.png|","标题","/默认目录",winform/*父窗口*/,,"默认文件名.png");
```

## 遍历文件

`fsys.enum(path,fileMask,callback,subDirectory)` 可用于遍历目录下的全部文件。
path 指定目录，fileMask 指定通配符或通配符数组，callback 指定回调函数，subDirectory 指定是否递归搜索所有子目录（省略则默认为 `true` ）。

```aardio
import fsys.dlg.dir;

var dir = fsys.dlg.dir("/可指定目录",winform,"对话框标题");
if(!dir) return;

import fsys;
fsys.enum( dir, "*.*",
	function(dirname,filename,fullpath,findData){ 
		if(filename) print("文件名：" + filename); 
		else print( "目录名：" + dirname);
	} 
);
```

`fsys.each(path,pattern,fileMask,findOption)` 用于创建遍历目录的迭代器，遍历目录时不会递归搜索子目录。
path 指定目录。可选用 pattern 指定匹配模式串。可选用 fileMask 指定通配符或通配符数组。findOption 为 "file" 则仅限查找匹配的文件，为 "dir" 则仅限查找匹配的目录，不指定则不作限制。

```aardio
import fsys;

for i,filename in fsys.each("/"){
	print(filename);
}
```

> aardio 中名字以 `enum` 为前缀的通常是深度遍历集合的枚举函数，而名字以 `each` 为前缀的通常是创建迭代器的工厂函数。

##  读写文件

直接读写文件全部数据：

```aardio
//写文件，二进制模式，参数可以是字符串或 buffer 
string.save("/test.txt","要保存在文件的字符串");


//读文件，二进制模式，返回字符串
var str = string.load("/test.txt");

//读文件，返回二进制字节数组（ buffer 类型）
var str = string.loadBuffer("/test.txt");
```

使用 io.file 创建文件对象，然后读写：

```aardio
//创建文件对象
var file = io.file("/example.txt","w+b");

//写入结构体，成功返回 true，失败返回 null,错误信息
file.write({int x=1,int y=2});

//写入文本
file.write("写入内容",'\r\n');

//移到文件指针
file.seek("set",0);

//读取结构体
var struct = file.read({int x=1,int y=2});
print(struct.x,struct.y);

//读取文本行
var line = file.read("%s");
print(line);

//关闭文件
file.close();
```

注意 `io.file("/example.txt","w+b").write("写入内容").close()` 这样写是错的，write 函数返回的是布尔值而非文件对象自身。

一次性写入文件可以使用 `io.file.write("/example.txt","写入内容")` ，支持写入不定个数的字符串、buffer、数值、结构体。如果只是简单保存字符串或 buffer 到文件也可以使用 string.save 函数（不支持结构体或多个写入参数）。

```aardio
io.file.write("/test.zip",{
    INT sign = 0x06054B50;
    BYTE bytes[22]
}) 
```

io.file,fsys.file,fsys.stream 提供的主要方法基本相同， 这些对象作为函数 `type.isFile( file )` 的参数时都会返回 true 。

主要区别是 io.file 更轻快，而 fsys.file 支持文件、管道等系统句柄参数，fsys.stream 则实现了  COM 接口 IStream ，用于操作内存文件流。

##  嵌入文件到字符串

在文件路径前加上 `$` 操作符可将该文件编译为字符串对象，程序发布后就不再需要这个文件了。

示例：

```aardio
var str = $"\dir\test.txt";
```

不要用这个方法重复包含资源目录下的已经内嵌到 EXE 的资源文件。

## 操作剪贴板

```aardio
import win.clip;

//写入文本到剪贴板
win.clip.write("文本");

//写入表（对象、数组）到剪贴板
win.clip.write({ arr =[1,2,3]})
```

## WMI 查询

```aardio
import com.wmi;

//WQL 参数化查询，返回首个查询结果（COM 对象）
var user = com.wmi.get("SELECT * FROM Win32_UserAccount WHERE Status=@status",{ 
	status = "OK"
})
//user.PasswordExpires = false; //修改 COM 对象的属性
//user.Put_(); //调用 COM 对象的方法

//WQL 参数化查询，返回所有查询结果并转换为表（table）
var dataTable = com.wmi.getTable("SELECT * FROM Win32_Process WHERE CreationDate >= @creationDate",{ 
	creationDate = time().addhour(-1)
})

//dataTable 是包含全部查询结果的数组，数组元素都是包含属性名值对的表
print(dataTable[1].ExecutablePath)
 
//单独指定类名时可使用 wmic 别名，指定属性名则返回单个值（仅用此方法取单个值时会自解析表示 CIM 时间的字符串值）。
var date = com.wmi.get("os","installDate");
print( date.format("%Y-%m-%d") );

```

## 调用 .NET(C#) 程序集

```aardio
import dotNet;
dotNet.import("System.Xml");

var xmlDoc = System.Xml.XmlDocument();
xmlDoc.LoadXml(`<?xml version="1.0" ?>
<Country id="china">
    <Province><Details id="shanxi">Shaanxi</Details></Province>
</Country>`)

// XPath 查询
var ele = xmlDoc.SelectSingleNode(`//Province/Details[@id="shanxi"]`);
print("",ele.OuterXml);
```

## 网站应用

在 aardio 可以 3 种方式创建 HTTP 服务端：
- 用 wsock.tcp.simpleHttpServer 创建多线程 HTTP 服务端。
- 用 wsock.tcp.asynHttpServer 创建单线程异步 HTTP 服务端，依赖窗口消息循环，仅适用于界面线程。
- 用 fastcgi.client 创建的 FastCGI 服务端，适用于 IIS 服务器环境。

这些服务端都兼容相同的接口，并提供相同的 `request`, `response`,`seesion` 对象。

一个最简单的 HTTP 服务端：

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


如果使用 web.view 还支持自动识别导入的 HTTP 服务器模块，并自动将单个斜杆或反斜杆开始的路径转换为 HTTP 服务端路径，例如：

```aardio
import wsock.tcp.simpleHttpServer; 
wb.go("/res/index.html") //自动调用 wsock.tcp.simpleHttpServer.startUrl("/res/index.html") 获取网址
```

一个最简单的网站应用示例：

```aardio
<!doctype html>
<html>
<body>
<?
response.write("你好！")
?>

```

如果 aardio 文件以 HTML 开始则 aardio 代码必须写在 `<? ..... ?>` 内部。

request,response 对象的常见用法：

```aardio

//取请求头，键名必须小写
var h = request.headers["name"]

//取 URL 请求参数，键名必须小写
var p = request.get["name"] 

//取表单参数，键名必须小写
p = request.post["name"] 

//取 URL 参数或表单参数
p = request.query("小写名称") 

//获取请求 JSON 并解析为对象
var data = request.postJson()

//获取原始上传数据
data = request.postData()

//取客户端 IP 地址
var ip = request.remoteAddr

//取请求 URL，不带 URL 参数
var url = request.url

//取请求路径
var path = request.path

//自定义 MIME 类型
response.contentType = "application/json"

//执行 aardio 文件，或下载普通文件（支持断点续传）
response.loadcode("文件路径") 

//重定向，参数 2 可指定 301
response.redirect("重定向网址")

//发送响应数据，可输出一个或多个参数，表参数自动转为 JSON 输出
response.write(arg1,arg2,...)

//添加 HTTP 响应头，键名里每个单词的首字母必须大写。
response.headers["Header-Name"] = stringOrStringArray 

//发送 HTTP 错误代码，可选指定错误信息，终止执行页面后续代码
response.errorStatu(code,message)

//关闭页面输出，终止执行代码
response.close() 
```