# time 库

日期时间函数库，这是自动导入的内置库。

## time 时间对象 

time 结构体用于表示日期时间对象，其声明如下：

  
```aardio
class time{
    WORD year; //年
    WORD month;//月
    WORD dayofweek; //星期
    WORD day; //日期
    WORD hour; //小时
    WORD minute; //分钟
    WORD second; //秒
    WORD milliseconds;//这个字段正常情况下为0，只有在WinAPI函数中会起作用
    format = "%Y/%m/%d %H:%M:%S"; //时间格式字符串 
}
```  

time 结构体兼容 WinAPI 的 SYSTEMTIME 结构体、兼容 COM 接口日期时间对象。 可在 Windows API 函数，COM 接口或支持这些接口的组件（ 例如 web.view ）中直接使用。

## 创建时间对象

函数原型：

`dateTime = time( strOrTimestampOrTableOrTime,format,locale);`

函数说明：

构造并返回时间对象。  
参数@1可以是表示时间的数值、字符串、参数表、或其他 time,time.ole 对象。

参数 @1 可以用一个普通的表指定部分时间字段，也可以传入另外一个需要复制的 time 或 time.ole 对象。time 将会构造一个新的日期时间对象并返回。

不指定参数@1默认初始化为当前时间。

可选用参数@2指定格式化串，格式化串的首字符为`!`表示 UTC 时间。省略格式串时默认值为'%Y/%m/%d %H:%M:%S'，可兼容解析 ISO8601 格式时间。

格式化串有两个作用：

- 将返回的时间对象转换为字符串时会使用格式化串指定的格式与规则。
- time 对象的第一个构造参数为文本时，则依据第二个参数 @format 指定的格式化串解析获取时间。

可在创建时间对象以后使用 format 属性修改格式化串。

请参考：[日期时间格式化语法](#format)

可选用第三个参数 @locale 指定对象文本格式化使用的区域语言。 locale 支持的参数与 setlocale 相同，例如英文为 "enu", 简体中文为"chs" 。也可以在时间对象的 locale 属性指定格式化使用的语法，如果不指定则使用默认的区域设置 - 默认区域设置可使用 setlocale()函数进行设置，例如 `setlocale("all","chs")` 或 `setlocale("time","chs")` 应用简体中文格式化,而使用 `setlocale("time","enu") `应用英文语言格式化时间。

> 格式化串如果将 `!` 作为第一个字符表示使用 UTC 标准时间，否则使用本地区域时间。UTF 时间可以使用时间对象的 local() 函数转为本地时间，而本地时间可以使用对象的 utc() 函数转换为 UTC 时间。  

不同参数用法：

- time() 

    不指定任何参数创建时间对象则默认初始化为当前时间。 

    不指定参数@1 在 milliseconds 字段返回毫秒数，且 dayofyear 字段无效,
    参数@1指定其他参数时 milliseconds 字段总是为 0

- time( timestamp )

    自时间戳创建日期时间对象。

    参数 @1 用一个数值指定时间戳 - 也就是自 UTC 时间 1970年1月1日 00:00:00 到 3000年12月31日23:59:59 之间的秒数。

- time( str,format )

    使用字符串参数 @format 指定格式化串。参数 @format 指定的格式与规则解析参数 @str 指定的字符串解析时间对象，支持自 1900年1月1日 到 9999年12月31日 的时间。
    
    自文本解析时间以尽可能宽松的规则识别时间。
    格式串中不是`%`前导格式化字符、分隔符不要求精确匹配。
    格式化时间时宽松处理所有空格，无须考虑空白字符的严格匹配.

    如果输入文本中的时间数值超出日期范围，则返回 null 。
    但如果出现当月不存在的日期且小于 31 号时会顺推为下月时间。

    如果格式化解析时间时输入文本提前结束，返回 null。
    创建对象后可通过 format 属性单独修改输出时间格式。 

    请参考：[日期时间格式化语法](#format)


返回值说明：

返回的时间对象可传入其他线程使用。 

返以回的时间对象可作为调用 tonumber 的参数转换为自 UTC 时间 1970年1月1日 00:00:00 到 3000年12月31日23:59:59 之间的秒数。需要更宽的运算范围请使用 time.ole 对象。

返以回的时间对象可作为调用 tostring 函数的参数转换为字符串。

可选用参数@2指定格式化串,首字符为!表示 UTC 时间。
参数@1 为文本时，则依据参数@2指定的格式化串解析获取时间。
 

🅰 示例：
  
```aardio
//如果省略所有参数返回当前时间
var tm = time(); 

//自文本解析时间
var tm2 = time("2017-05-27T16:56:01Z",'%Y/%m/%d %H:%M:%S');

//格式化为字符串
var str = tostring(tm2); 
```  

## time 对象类型转换: 转换为字符串或数值

🅰 示例：
  
```aardio
var tm = time();

//返回时间戳数值，以秒为单位
var n = tonumber(tm); 

//修改格式化串
tm.format = "%Y年%m月%d日 %H时%M分%S秒";

//返回格式化的时间字符串
var s = tostring(tm); 

//格式化时可选指定转换格式化串（不改变对象的默认 format 属性），也可以指定区域代码（ "chs" 为简体中文 ）
var s = tostring(tm,"%Y年%m月%d日 %H时%M分%S秒","chs"); 

//也可以调用时间对象的 format 方法返回格式化的字符串。
var s = tm.format()

//同样可以添加格式化串参数，区域代码参数
var s = tm.format("%Y年%m月%d日 %H时%M分%S秒")

print(s);
```  

## 检测时间对象

可以使用 type.eq 函数检查两个时间对象是否相等。  
也可使用 time.istime 函数判断对象是否一个时间对象。  
  
type.eq 是严格判别类型，而 time.istime 是兼容性检测，只要拥有相同的结构体声明都会返回 true。

标准库中的 time.ole 继承自 time 对象，用法与接口与  time 对象基本相同，time 对象或 time.ole 对象传入 time.istime 函数都会返回 true。实际上 aardio 中几乎所有与时间有关的函数都支持并兼容 time 或 time.ole 对象。

time.istime 检测返回为真的对象，同样意谓着可以通用于 COM 函数，API 函数，对于时间类型都会使用 time.istime 进行检测。

🅰 示例：

```aardio
import console; 
import time.ole;

var tm = time();

//输出 true
console.log( time.istime(tm) )

var oleTime = time.ole(); 

//输出 true,time.ole 兼容 time 对象
console.log( time.istime(oleTime) )

//输出 false，不相等
console.log( !! type.eq(oleTime,tm) )

console.pause(); 
```  

## 时间格式化语法 <a id="format" href="#format">&#x23;</a>


创建时间的time对象构造函数的第二个参数可以指定格式化字符串，不指定格式化串时默认值为'%Y/%m/%d %H:%M:%S'。 也可以在创建时间对象以后使用 format 属性修改格式化串。

![](../../../icon/info.gif) 格式化串如果将 `!` 作为第一个字符表示使用 UTC 标准时间，否则使用本地区域时间。UTF 时间可以使用时间对象的 local() 函数转为本地时间，而本地时间可以使用对象的 utc() 函数转换为 UTC 时间。 

 格式化时间时可以使用第三个参数指定格式化使用的语言， 例如 `tm = time(,"%a %B %Y %m %d  %H:%M:%S","enu")` 的最后一个参数"enu"找定了格式化时使用英文语言。中文则为"chs"，也可以在时间对象的 locale 属性指定格式化使用的语法，如果不指定则使用默认的区域设置 - 默认区域设置可使用 setlocale()函数进行设置，例如:setlocale("all","chs") 或 setlocale("time","chs") 应用简体中文格式化,而使用 setlocale("time","enu") 应用英文语言格式化时间。
 
 在格式化串中，每一个 `%` 号声明一个格式化标记，全部可用的格式化标记如下表：   `

- `%%` - 表示原始 `%` 字符。
- `%c` - 输出字符串时按当前区域首选的日期时间格式，因为这个格式具有不确定性，不应使用此格式解析输入字符串。 
- `%x` - 格式化输出字符串时使用当前区域首选的日期表示法，不包括时间，格式化输入字符串时等价于`"%m/%d/%y" `
- `%X` - 格式化输出字符串时当前区域首选的时间表示法，不包括日期，格式化输入字符串时使用`"%H:%M:%S" `
- `%a` - 当前区域星期几的简写（格式化输入字符串时忽略此字段遇到的错误） 
- `%A` - 当前区域星期几的全称（格式化输入字符串时忽略此字段遇到的错误） 
- `%b` - 当前区域月份的简写 %B - 当前区域月份的全称 
- `%d` - 月份中的第几天，十进制数字（范围从 01 到 31） 
- `%H` - 24 小时制的十进制小时数（范围从 00 到 23） 
- `%I` - 12 小时制的十进制小时数（范围从 00 到 12）
- `%m` - 十进制月份（范围从 01 到 12） 
- `%M` - 十进制分钟数 
- `%p` - 根据给定的时间值为 `am' 或 `pm'，或者当前区域设置中的相应字符串 
- `%S` - 十进制秒数 
- `%y` - 没有世纪数的十进制年份（范围从 00 到 99,解析文本时也兼容输入的4位年份） 
- `%Y` - 包括世纪数的十进制年份(解析文本时也兼容输入的2位年份) 
- `%Z` - 时区名或缩写 %w - 星期中的第几天，星期天为 0 
- `%W` - 本年的第几周，从第一周的第一个星期一作为第一天开始，不应使用此格式解析输入字符串生成时间,此标记使用前需要调用 tm.addsecond(0) 函数刷新 tm.dayofyear 字段。 
- `%U` - 本年的第几周，从第一周的第一个星期天作为第一天开始，不应使用此格式解析输入字符串生成时间,此标记使用前需要调用 tm.addsecond(0) 函数刷新 tm.dayofyear 字段。 
- `%j` - 本年的第几天，十进制数（范围从 001 到 366），不应使用此格式解析输入字符串生成时间,此标记使用前需要调用 tm.addsecond(0) 函数刷新 tm.dayofyear 字段。   ``  

使用格式串解析文本并转换为时间时，除上述匹配时间的标记以外， 
其他分隔字符支持宽松的模糊匹配，规则如下：

- 格式串不是使用`%`前导的字符如果没有精确匹配到对应字符,  
则模糊匹配连续的标点、或连续的字母，或连续的宽字符（例如汉字）。  
  
- 忽略目标字符串空格，忽略目标字符串以数值表示的时间字段前的非数值字符。  

- 宽松处理所有空格，无须考虑空白字符的严格匹配.
  
- 支持 ISO8601 兼容的省略间隔符的写法（即使格式串中指定了间隔符）  
省略间隔符时年月日组合的数值必须为8位或6位，其中月、日必须是两个数字，  
时分秒连写时每个部分都必须是两个数字。  
  
如果输入文本中的时间数值超出日期范围，则返回 null 。但如果出现当月不存在的日期且小于 31 号时会顺推为下月时间。 如果格式化时间未完成，但输入文本提前结束则返回 null 。 

如果最后一个格式化标记解析成功以后如果还有剩余的字符串， 首先跳过前面的空白字符，从前面取其他连续的非空白字符存入时间对象的endstr属性内。endstr可用于后续解析 ISO8601 等格式的时区，可参考 builtin 库中 time.iso8601 函数的源码。

下面是一个使用格式化代码的示例：

  
```aardio
import console;

//从字符串创建时间值
var tm = time("2006/6/6 12:56:01","%Y/%m/%d %H:%M:%S");

//改变格式化模式串
tm.format = "%Y年%m月%d日 %H时%M分%S秒";

//从时间值创建字符串
var str = tostring(tm);

//打印结果到控制台窗口
console.log(str);

console.pause();
```  

time 对象支持连接操作符 `++`，在与任意值连接时都会自动转换为字符串并返回连接后的字符串，即使有一个操作数的值为 null 值 aardio 也会忽略该操作数返回格式化时间对象得到的字符串。

## 兼容无 `%` 符号格式化语法

aardio 同样兼容 无 `%` 符号的时间格式化语法，对于 time 对象 aardio 会将其自动转换为有 `%` 符号格式化语法。

具体转换规则如下：

- `yyy,yyyy,YYY,YYYY` 重复字母超过 4 次转换为 %Y 表示四位年份
- `y,yy,YY,YY `转换为 %y 表示两位年份
- `MMMM` 转换为 %B 完整月份名称
- `MMM` 转换为 %b 月份名称简写
- `MM,M` 都转换为 %m 月份数值
- `m` 不分重复次数转换为  %M 分钟数
- `dddd,DDDD` 重复字母超过 4 次转换为 %A 星期几的全称
- `ddd,DDD` 转换为 %a 星期几的简写
- `d,dd,D,DD` 转换为 %d 月份中的第几天
- `HH` 不分重复次数转换为 %H 24 小时制的小时数
- `hh `不分重复次数转换为 %I 12 小时制的小时数
- `S,s` 不分重复次数转换为 %S 秒数
- `Z ` 不分重复次数转换为 %Z 时区名或缩写
- `t,a,A` 不分重复次数转换为 %p 上午/下午 am/pm 

时间格式化语法主要分为有无 `%` 符号这两种风格，但各家的实现有一些小的差别。 
aardio 兼容了一些细微的差异并进行了简化，在无 `%` 符号格式化串中大小写  D Y S 的作用相同。

time.ole 对象则默认使用无 `%` 符号语法进行格式化，但同样会自动兼容有 `%` 符号的格式化串。

建议：
- 如果没有其他特殊原因，aardio 建议总是使用有 `%` 符号格式化串，这可以避免不必要的转换，虽然这个过程很快。
- 对于 time.ole 对象，在构造函数中自文本解析时间则必须转换为有 `%` 符号格式化串并调用 time 构造函数解析文本并获取时间，在 time.old 对象创建以后，则总是使用无 `%` 符号格式串作为参数调用系统 API 函数格式化时间。

## 时间的加减运算

时间对象可以使用 tonumber 函数转换为以秒为单位的数值( time.ole 对象则转换为以天为单位的数值) 进行运算。

我们也可以直接使用 time 对象提供的方法进行加减运算。
  
🅰 示例：

```aardio
import console;
var dateTime = time.now();

dateTime.year += 2;
console.log(dateTime,"增加2年")

dateTime.addsecond(30)
console.log(dateTime,"增加30秒")

dateTime.addminute(180)
console.log(dateTime,"增加180分")

dateTime.addhour(2)
console.log(dateTime,"增加两小时")

dateTime.addday(365)
console.log(dateTime,"增加365天")

dateTime.addmonth(-24)
console.log(dateTime,"倒退24个月")

var tm2 = time.now()

//计算相差时间
console.dumpJson(
	相差月份 = tm2.diffmonth(dateTime);
	相差天数 = tm2.diffday(dateTime);
	相差小时数 = tm2.diffhour(dateTime);
	相差分钟数 = tm2.diffminute(dateTime);
	相差秒数 = tm2.diffsecond(dateTime);
	相差秒数2 = tonumber(dateTime) - tonumber(tm2); 
) 

console.pause();
```  

使用 add... 系列函数修改时间将自动计算并更新时间对象的所有字段。

而直接指定时间对象的部分字段，aardio 不保证时间的合法性，也不会自动更新 dayOfWeek 字段，也可以主动调用 tm.update() 函数重新计算时间并更新该字段。

注意 time 或 time.ole 对象并不直接支持 `+`,`-` 等算术运算符，虽然可以实现这些很简单，但这几个对象都没有重载这几个操作符。这是因为时间对象与普通数值做算术存在一定的歧义，而且这种操作也不常见，更好的方法是调用适合需要的的具体函数。

## datetime 的关系运算

🅰 示例：

```aardio
import console;

var dateTime = time.now();
dateTime.year += 2;

var tm2 = time.now(); 

//关系运算
console.dumpTable( 
	相等 = tm2 == dateTime;
	大于 = tm2 > dateTime;
	小于等于 = tm2 <= dateTime;
)

console.pause()
```  

##  UTC 时间与本地时间

### 1. UTC 时间与本地时间相互转换

格式化串（ 也就是时间对象的 format 属性 ）首字符为  `!` 仅仅是表示这是一个 UTC 时间，单纯增删这个 `!` 标记并不会改变时间的值。而调用 time 对象的 utc()、 local() 方法可以根据时区调整时间的值（同时调整`!` 标记，转换前会检测`!` 标记以判断是否需要转换）。

🅰 示例：

```aardio
//假设当前操作系统为北京时间
var tm = time("2017-08-22 10:00:00")

//转换为 UTC 时间，修改并返回自身，如果参数为 true 则不修改自身而是返回副本
var tmUtc = tm.utc() 

//输出 UTC 时间 "2017-08-22 02:00:00"
print(tmUtc) //print 会自动调用 tostring(tmUtc)

//转换为本地时间（通过格式化串的 "!" 发现是 UTC 时间，转换为本地时间并移除格式化串前面的 "!" ）
var tmLocal = tm.local() //如果参数为 true 则不修改自身而是返回副本

//显示 2017-08-22 10:00:00，时区调整以后时间的值变了
print(tmLocal)

//再次转换为本地时间，格式化串前面没有 "!"，发现已经是本地时间，本次调用不执行任何操作
tm.local()
```

注意同一个时间无论是转换为 UTC 时间，还是转换为本地时间，其时间戳都是不变的。

🅰 示例：

```aardio
var localTime = time();
var utcTime = localTime.utc(true);

//不同时区显示不同的时间
print(localTime) 
print(utcTime) 

//但转换为时间戳是相同的值
print( tonumber(localTime) )
print( tonumber(utcTime) )

//等式、关系运算符内部都使用时间戳进行比较
print(utcTime == localTime,"时间戳是相等的")
```

但是要注意有些接口对时间参数并不是取统一的时间戳而是直接取时间对象本身则会受到时区的影响。
例如 COM 接口虽然多数情况下默认会使用本地时间，但 COM 接口不会自动识别时区并做到自动兼容，
需要明确接口要求的是否本地时间，或者是否有指定时区的参数。


### 2. 创建或解析 UTC 时间对象

可以使用 time.utc() , time.iso8601(), time.gmt() 这几个函数创建 UTC 时间，
这几个函数主要的区别是格式化串不同，相同点则是格式化串的首字符都是  `!`  以表示这是一个 UTC 时间。

🅰 示例：

```aardio
//返回 UTC 时间
var utc = time.utc()

//显示 !%Y/%m/%d %H:%M:%S
print(utc.format)

/*
也返回 UTC 时间，但使用 ISO8601 格时化串。
参数 @1 为字符串时支持 ISO8601 的各种兼容格式，例如：
"20170822 123623 +7" 时区简写
"20170822 123623" 省略分隔符格式
"20170822123623" 14位数字格式
"170822123623" 12位数字格式 
"20170822" 只有日期部分
*/
var iso8601 = time.iso8601("20170822 123623 +0700")

//显示 !%Y-%m-%dT%H:%M:%SZ
print(iso8601.format)
 
/*
返回 RFC1123 格式 GMT 时间，HTTP 协议使用该格式。
如果参数是字符串，可兼容解析 RFC 850，RFC1123 两种格式， 
 字符串中如果使用数值格式指定了时区，将自动补偿时差转换为 GMT 时区
 时区以正负号开始，支持省略冒号分隔符的1位，2位，3位，4位数字缩写
*/
var gmt = time.gmt()

//显示 !%a, %d %b %Y %H:%M:%S GMT
print(gmt.format);

```

time.iso8601() ，time.utc() 都默认会用两种时间格式串解析字符串参数：包含日期时间的格式串、以及仅有日期部分的格式串。

可参考 time.utc 函数的源代码如下：

```aardio
time.utc = function(t){
	return  time(t,"!%Y/%m/%d %H:%M:%S") || time(t,"!%Y-%m-%d");
}
```

## time 对象 在 WinAPI 函数中的运用

```aardio
//创建 UTC 时间
var dateTime = time.utc();

//调用 API 函数
::Kernel32.GetSystemTime(dateTime);
```

注意 `::Kernel32.GetSystemTime` 接收的是 UTC 时间。
