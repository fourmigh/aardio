io帮助文档
===========================================
<a id="io"></a>
io 成员列表
-------------------------------------------------------------------------------------------------
文件、文件流相关函数库,  
包含控制台标准输入输出流操作,  
直接操作控制台请使用 console函数库，  
这是自动导入的内核库,  
[使用手册相关文档](chm://libraries/kernel/io/io.html)

<h6 id="io._exedir">io._exedir </h6>
 主程序所在目录,  
返回完整长路径，不会返回8.3短路径，不会返回相对路径  
  
目录路径以反斜杠结束

<h6 id="io._exefile">io._exefile </h6>
 主程序文件名

<h6 id="io._exepath">io._exepath </h6>
 主程序文件路径  
返回完整长路径，不会返回8.3短路径，不会返回相对路径

<h6 id="io.appData">io.appData </h6>
 获取  %LocalAppData% 目录下的绝对路径。  
可选使用指定需要存入的数据

<h6 id="io.appData">io.appData(path,data) </h6>
 将@path指定的相对路径转换为系统 %LocalAppData% 目录下的绝对路径,  
可选使用 @data 指定需要存入的数据,  
存入文件与目标文件长度不同或 PE 时间戳不同则允许替换旧文件,  
指定 @data 参数后如果无法创建文件返回null,  
最后返回转换所得的完整路径

<h6 id="io.close">io.close() </h6>
 关闭控制台窗口。  
程序退出也会自动关闭控制台窗口。  
 调用 execute("pause") 或 console.pause() 可等待用户按键以避免立即退出

<h6 id="io.createDir">io.createDir("/__") </h6>
 创建目录  
如果有多个参数则首先用io.joinpath拼接  
如果父目录尚未创建将自动创建  
成功返回完整路径,失败返回空值

<h6 id="io.curDir">io.curDir </h6>
 获取或修改当前目录

<h6 id="io.curDir">io.curDir() </h6>
 无参数获取当前目录

<h6 id="io.curDir">io.curDir(dir) </h6>
 将 @dir 参数指定的目录路径转换为完整路径并设为当前目录  
成功返回 true

<h6 id="io.exist">io.exist </h6>
 判断文件路径是否存在，  
也可以用于判断文件是否可读写。  
  
包含不可见字符的错误路径可用「工具>文本文件>十六进制编辑器」  
或 string.hex 函数查看

<h6 id="io.exist">io.exist("文件路径") </h6>
 判断文件路径是否存在  
存在则转换为绝对路径并返回该路径,不存在返回null  
参数@1不是有效字符串返回null

<h6 id="io.exist">io.exist("文件路径",2) </h6>
 测试文件是否可写  
成功则转换为绝对路径并返回该路径,失败返回null  
目录或只读文件直接返回null  
参数@1不是有效字符串返回null

<h6 id="io.exist">io.exist("文件路径",4) </h6>
 测试文件是否可读  
成功则转换为绝对路径并返回该路径,失败返回null  
目录返回null  
参数@1不是有效字符串返回null

<h6 id="io.exist">io.exist("文件路径",6) </h6>
 判断文件是否可读写  
成功则转换为绝对路径并返回该路径,失败返回null  
目录返回null  
参数@1不是有效字符串返回null

<h6 id="io.fullpath">io.fullpath("__") </h6>
 将相对路径转换为绝对路径，转换规则如下:  
  
如果路径以 \\ 开始，不作转换直接返回，路径前加 \\?\ 可避免转换并支持畸形路径，  
如果路径以 // 开始，移除第一个斜杠后返回，不作其他转换，用于表示分区根目录,  
如果以单个 \  / 字符开始，作为应用程序根目录下的相对路径转换并返回完整路径,  
如果路径以 ~ 开始,作为当前运行的EXE根目录下的相对路径转换并返回完整路径,  
其他路径按系统规则转换为完整路径,  
传入空字符串或空值返回 null  
  
注意相对路径 ".\" 易被改变，例如打开文件对话框可能改变相对路径。  
而 aardio 提供的应用程序根目录表示确定的位置。

<h6 id="io.getSize">io.getSize(__) </h6>
 获取参数@1指定路径的文件字节长度,  
返回数值

<h6 id="io.getSpecial">io.getSpecial(_CSIDL__) </h6>
 获取特殊文件夹,  
参数@1使用 _CSIDL 开头的常量指定特殊文件夹的 CSIDL,  
不指定参数@1则默认值为 _CSIDL_DESKTOP,  
可选增加任意个拼接到目录后的子路径参数  
这个函数与fsys.getSpecial函数用法接近,  
但支持不定个数子路径参数, 不支持返回PIDL  
  
fsys.knownFolder 可用于获取更多已知的特殊文件夹

<h6 id="io.getText">io.getText(__/*可选指定缓冲区大小*/) </h6>
 读取控制台文本  
不包含尾部的回车换行,  
相比使用io.stdin.read函数,io.getText 可更好的支持 Unicode 字符

<h6 id="io.joinpath">io.joinpath(__,) </h6>
 用于拼接多个路径  
该函数可以避免子目录首尾连接缺少、或多出反斜杠  
此函数首先会将斜杆自动转换为反斜杆,  
如果连接的非空路径之间没有至少一个反斜杆,则添加反斜杆,  
如果连接处出现重复的反斜杆，则去掉其中一个,  
也可指定一个目录,再指定需要按上述规则追加的反斜杠参数

<h6 id="io.libpath">io.libpath("__") </h6>
 将库路径转换为文件路径;  
如果库存在则返回 2 个值:库文件路径,库文件所在目录  
否则库路径返回null空值,按用户库合法格式返回目录路径,  
返回的库目录路径以反斜杠结束.

<h6 id="io.lines">io.lines </h6>
 创建用于for in语句的迭代器,逐行读取文件,  
按行拆分普通字符串请使用 string.lines 函数

<h6 id="io.lines">io.lines() </h6>
 无参数时逐行读取标准输入（控制台输入）

<h6 id="io.lines">io.lines(文件对象） </h6>
 创建用于for in语句的迭代器,逐行读取文件,  
参数请指定一个用io.open打开的文件流对象.  
  
以文本模式打开文件,返回文本不含换行,  
二进制模式仅移除换行分隔符,遇'\0'会丢弃后续字符.  
  
不负责关闭参数传入的文件对象

<h6 id="io.lines">io.lines(文件路径） </h6>
 创建用于for in语句的迭代器,逐行读取文件  
以文本模式打开文件,输入转换为单换行，输出转换为回车换行  
遇到'\x1A（CTRL+Z）、'\0'等终止输入  
返回的文本不包含换行  
  
循环结束立即关闭此函数打开的文件对象。  
但是：如果使用 break,return 等语句中断循环，  
则打开的文件对象可能不会立即关闭（稍后内存回收后关闭）。  
改为传入文件对象作为参数，可自行控制何时关闭文件

<h6 id="io.localpath">io.localpath("__") </h6>
 如果路径使用了aardio专用格式转换为系统支持的完整路径,  
否则返回空值，转换规则如下：  
  
如果路径以 \\ 开始,不作转换,返回 null 空值,  
如果路径以 // 开始，移除第一个斜杠后返回，不作其他转换，用于表示分区根目录,  
如果以单个 \  / 字符开始，作为应用程序根目录下的相对路径转换并返回完整路径,  
如果路径以 ~ 开始,作为当前运行的EXE根目录下的相对路径转换并返回完整路径,  
其他格式路径不作转换返 null 空值

<h6 id="io.open">io.open </h6>
 打开文件,成功返回文件对象,  
失败返回 null,错误信息,错误代码。  
  
此函数创建的文件对象也支持在API调用时  
自动转换为系统文件句柄并返回一个指针

<h6 id="io.open">io.open("/文件路径","读写模式",共享模式) </h6>
 参数@1指定路径,  
路径首字符可用斜杠表示应用程序根目录，用~加斜杠表示EXE根目录。  
如果~\或~/开头的EXE根目录路径不存在，自动转换为应用程序根目录下的路径重试。  
  
参数@2指定打开文件的读写模式，支持以下选项:  
w+ 可读写模式，创建新文件清空源文件  
w 只写模式，清空原文件  
r+ 可读写模式，文件必须存在,不清空文件内容  
r 只读模式,文件必须存在  
a+ 可读写追加模式，可打开原文件移动指针到文件尾，也可创建新文件  
a 只写追加模式，可打开原文件移动指针到文件尾，也可创建新文件  
b二进制模式，不转换回车换行  
t文本模式，输入使用单换行,输出使用回车换行,遇到'\x1A'（CTRL+Z）、'\0'字符终止输入  
R 随机优化 S 连续优化  
D 创建临时文件,关闭对象后删除文件  
注意: r,w,a只能且必须选择其中一个作为读写模式的第一个字符  
  
参数@3 可选以 _SH_前缀常量指定共享模式

<h6 id="io.open">io.open() </h6>
 第一个参数为空时打开控制台窗口,  
可选使用第二个参数指定控制台标题  
  
此函数不会重定向 msvcrt.dll 定义的 stdin,stdout,stderr 到控制台，  
改用 console.open 函数打开控制台可支持该功能

[返回对象:fileObject](#fileObject)

<h6 id="io.open">io.open(系统文件句柄,读写模式) </h6>
 系统文件句柄转换为文件对象  
读写模式为字符串,同样使用"r","w","b"等标记  
必须与原来的文件句柄使用相同的模式

<h6 id="io.popen">io.popen() </h6>
 [返回对象:fileObject](#fileObject)

<h6 id="io.popen">io.popen(命令行,读写模式) </h6>
 执行参数 @1 指定的命令，成功返回进程管道对象。  
失败返回 null,错误信息,错误代码。  
  
参数 @1 如果为完整路径且包含空格，应在首尾加上双引号。  
读写模式为 "r" ( 或省略 )返回绑定目标进程输入流的管道对象，  
读写模式为 "w" 返回绑定绑定目标进程输出流的管道对象。  
  
此函数不隐藏目标程序控制台，建议提前打开控制台。  
建议改用更强大的 process.popen（支持隐藏控制台）

<h6 id="io.print">io.print( __ , ... ) </h6>
 在控制台窗口以字符串形式输出变量的值

<h6 id="io.print">io.print(."__") </h6>
 在控制台窗口输出信息

<h6 id="io.remove">io.remove("__") </h6>
 删除参数@1指定路径的文件，在路径前加上 \\?\ 可支持畸形路径。  
成功返回 true，失败返回 null，错误信息，错误代码 。  
  
此函数仅用于删除文件，  
删除目录请改用 fsys.delete、fsys.deleteEx 或 fsys.remove 函数。

<h6 id="io.rename">io.rename("__","") </h6>
 重命名  
成功返回true,失败返回null

重命名  
成功返回true,失败返回 null，错误信息，错误代码

<h6 id="io.specialData">io.specialData(path,data,csidl) </h6>
 将 @path 指定的相对路径转换为特殊文件夹下的绝对路径,  
可选使用 @data 指定需要存入的数据,  
存入文件与目标文件长度不同或 PE 时间戳不同则允许替换旧文件,  
指定 @data 参数后如果无法创建文件返回null,  
参数@csidl 使用 _CSIDL 开头的常量指定特殊文件夹的 CSIDL,  
不指定@csidl 则默认值为 _CSIDL_DESKTOP,  
最后返回转换所得的完整路径

<h6 id="io.splitpath">io.splitpath() </h6>
 [返回对象:ioSplitFileInfoObject](#ioSplitFileInfoObject)

<h6 id="io.splitpath">io.splitpath(__) </h6>
 拆分文件路径为目录、文件名、后缀名、分区号等，  
返回 io.pathInfo 对象，  
返回对象可修改drive,path,name,ext等字段，  
并可作为 tostring 函数的参数重新合成为文件路径

<h6 id="io.stderr">io.stderr </h6>
 标准错误输出,  
在二进制模式下不做任何转换,  
但在文本模式下始终以 Unicode 编码输出文本,  
  
[返回对象:fileObject](#fileObject)

<h6 id="io.stdin">io.stdin </h6>
 标准输入,  
在二进制模式下不做任何转换,  
但在文本模式下始终以 Unicode 编码读取返回UTF-8编码文本,  
  
自控制台读取字符时,使用io.getText可以更好的支持Unicode字符  
  
[返回对象:fileObject](#fileObject)

<h6 id="io.stdout">io.stdout </h6>
 标准输出,  
在二进制模式下不做任何转换,  
但在文本模式下始终以 Unicode 编码输出文本,  
  
[返回对象:fileObject](#fileObject)

<h6 id="io.tmpname">io.tmpname </h6>
 生成系统临时文件目录下的临时文件路径

<h6 id="io.tmpname">io.tmpname(prefix,ext) </h6>
 生成临时文件路径，  
可选用 @prefix 参数指定前缀名，  
可选用 @ext 参数指定后缀名，后缀名应包含点

<h6 id="io.updateData">io.updateData </h6>
 更新指定文件的数据

<h6 id="io.updateData">io.updateData(data,path,...) </h6>
 更新指定 @path 指定路径的文件为 @data 指定的数据。  
如果添加更多参数，则首先调用 io.joinpath 拼接到 @path 后面。  
存入文件与目标文件长度不同或 PE 时间戳不同则允许替换旧文件。  
替换失败返回 null，否则返回文件路径

<h6 id="io.utf8">io.utf8 </h6>
 设为true允许控制台启用UTF8模式  
所有线程设置必须相同,否则会导致重新打开控制台,  
WIN10 以上系统默认值为 true  
在 WIN10 以下的系统不建议设为 true,  
console 标准库始终使用 unicode 输出文本  
io.stdin,io.stdout,io.stderr 在默认的非二进制模式也使用 unicode 编码

<a id="fileObject"></a>
fileObject 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="fileObject.close">fileObject.close() </h6>
 关闭文件流

<h6 id="fileObject.flush">fileObject.flush() </h6>
 输出缓冲区数据  
成功返回 true，失败返回 null,错误信息,错误代码

<h6 id="fileObject.mode">fileObject.mode() </h6>
 不指定参数时返回表示文件读写模式的数值，  
对于标准输入输出流,aardio在二进制模式不转换编码,在文本模式负责自动转换编码  
  
普通文件对象 aardio 以 UTF8 编码或二进制模式读写,  
不会自动转换文本编码。

<h6 id="fileObject.mode">fileObject.mode(mode) </h6>
 修改文件模式  
参数可设为二进制模式 _O_BINARY 或 文本模式 _O_TEXT,  
  
其他_O_WTEXT,_O_U16TEXT,_O_U8TEXT要求UTF-16读写，  
用于多字节或二进制操作会导致程序异常  
返回表示文件读写模式的数值，  
失败返回 null,错误信息,错误代码。  
指定错误的模式参数时会抛出异常

<h6 id="fileObject.read">fileObject.read </h6>
 自文件读取数据。  
支持不定个数参数，每个参数指定一个读取标志并增加一个返回值。  
  
参数为"%s"读取并返回一行，参数数为 "%d" 读取一个数值。  
参数为数值则读取指定字节长度的数据并返回字符串。  
参数为结构体则读取数据并填充结构体的值。  
  
函数执行失败返回 null,错误信息,错误代码

<h6 id="fileObject.read">fileObject.read("%d") </h6>
 从当前位置,向后读取下一个数值，支持多参数。  
失败返回 null,错误信息,错误代码

<h6 id="fileObject.read">fileObject.read("%s") </h6>
 从当前位置,向后读取下一行，支持多参数。  
失败返回 null,错误信息,错误代码

<h6 id="fileObject.read">fileObject.read() </h6>
 从当前位置,向后读取下一行。  
失败返回 null,错误信息,错误代码

<h6 id="fileObject.read">fileObject.read(-1) </h6>
 向后读取到文件尾部。  
失败返回 null,错误信息,错误代码。  
参数为负数表示从文件尾部倒计数位置,支持多参数  
读取普通文件全部数据使用 string.load 函数更快。  
readBuffer() 函数则拥有最快的读取速度。

<h6 id="fileObject.read">fileObject.read(0) </h6>
 检测是否读取到文件尾

<h6 id="fileObject.readBuffer">fileObject.readBuffer </h6>
 读取数据到buffer,成功返回读取长度,失败返回null

<h6 id="fileObject.readBuffer">fileObject.readBuffer(buffer指针,读取长度) </h6>
 直接读数据到内存  
参数@1可以是 buffer,或内存指针,  
如果是指针则必须指定读取长度,否则长度参数可选  
成功返回读取长度

<h6 id="fileObject.readTo">fileObject.readTo(__) </h6>
 读取到指定结束字符前面的所有字符,  
  
参数@1指定结束字符的字节码（数值）,  
单引号包含字符并加#号取字节码，例如 '\0'#  
如果没有指定参数,则参数默认为'\0'#  
  
返回字符串不包含结束字符,但文件指针会移到该字符后面,  
如果未遇到指定字符,则读取到文件尾

<h6 id="fileObject.readUnicode">fileObject.readUnicode(读取字符数) </h6>
 读取UTF 16编码数据,如果文件头有BOM会自动移除,  
参数按字符计数，不是按字节计数,  
如果已到文件尾读取长度可能小于参数指定的长度

<h6 id="fileObject.readUnicode">fileObject.readUnicode(读取字符数,true) </h6>
 读取Unicode数据,并直接返回UTF 16编码的字符串,  
参数按字符计数，不是按字节计数

<h6 id="fileObject.readback">fileObject.readback() </h6>
 从当前位置,向前读取上一行  
仅支持UTF8或ANSI换行符,不支持UTF16换行符

<h6 id="fileObject.readback">fileObject.readback(-1) </h6>
 向前读取到文件头部  
负数表示从文件头部倒计数位置

<h6 id="fileObject.readback">fileObject.readback(0) </h6>
 检测是否读取到文件头

<h6 id="fileObject.readback">fileObject.readback(__) </h6>
 正数参数表示从当前位置向前读取n个字节

<h6 id="fileObject.reopen">fileObject.reopen("CONOUT$","w__") </h6>
 可使用"CONOUT$"重定向到控制台输出缓冲区,  
使用"CONOUT$"重定向到控制台输入缓冲区,  
重定向到控制台只能使用文本模式  
参数@2不可使用ccs标记指定编码  
成功返回 true，失败返回 null,错误信息,错误代码

<h6 id="fileObject.reopen">fileObject.reopen("__/*文件路径*/","w+") </h6>
 重定向文件流  
成功返回 true，失败返回 null,错误信息,错误代码

<h6 id="fileObject.seek">fileObject.seek("cur",__) </h6>
 移动至相对当前位置的指定偏移量，偏移量应当是一个普通数值。  
1表示向后移动1字节,-1表示向前倒退1字节,  
成功返回当前位置（相对于文件开始计数）,  
失败返回null,错误信息,注意超越文件尾是允许的

<h6 id="fileObject.seek">fileObject.seek("end") </h6>
 移动指针至结束处,并返回文件长度  
获取文件大小推荐使用 size() 函数

<h6 id="fileObject.seek">fileObject.seek("end",__) </h6>
 移动至相对结束处的指定偏移量，  
偏移量应当是一个普通数值。  
成功返回当前位置（相对于文件开始计数）,  
失败返回 null,错误信息,错误代码。注意超越文件尾是允许的

<h6 id="fileObject.seek">fileObject.seek("set") </h6>
 移动指针到开始

<h6 id="fileObject.seek">fileObject.seek("set",__) </h6>
 移动至相对开始处的指定偏移量。  
注意0表示文件开始,1表示第一个字节后面,准备读取的是应该是第2字节,  
成功返回当前位置（相对于文件开始计数）,  
失败返回 null,错误信息,错误代码。注意超越文件尾是允许的。  
  
偏移量应当是一个普通数值。  
普通数值的整数上限为 8PB，没有可能会写这么大的文件。  
所以偏移量不支持、也不必要使用 math.size64 对象

<h6 id="fileObject.seek">fileObject.seek() </h6>
 得到当前位置（相对于文件开始计数）

<h6 id="fileObject.seteof">fileObject.seteof() </h6>
 将当前文件位置设为文件末尾,  
用于快速改变文件大小  
成功返回true

<h6 id="fileObject.setvbuf">fileObject.setvbuf("full",__) </h6>
 设为完全缓冲模式，  
读缓冲区直致为空再从流读入数据，写满缓冲区后向流写入数据  
参数@2使用字节数指定缓冲区大小

<h6 id="fileObject.setvbuf">fileObject.setvbuf("line",) </h6>
 设为行缓冲模式，  
每次从流中读取或写入一行数据  
参数@2使用字节数指定缓冲区大小

<h6 id="fileObject.setvbuf">fileObject.setvbuf("no") </h6>
 禁用缓冲

<h6 id="fileObject.size">fileObject.size() </h6>
 返回文件指针当前位置到文件尾的大小,该函数不会改变文件指针当前位置，  
此函数不需要指定任何参数

<h6 id="fileObject.size64">fileObject.size64() </h6>
 自当前位置计算到文件尾的大小,返回math.size64正整数对象  
该函数不会改变文件指针当前位置  
  
[返回对象:mathSize64Object](#mathSize64Object)

<h6 id="fileObject.type">fileObject.type() </h6>
 获取文件对象的类型  
例如控制台，管道，本地文件....等等  
返回值请参考_FILE_TYPE_前缀的常量

<h6 id="fileObject.write">fileObject.write </h6>
 写数据

<h6 id="fileObject.write">fileObject.write(__, ) </h6>
 写数据,支持一个或多个参数  
参数可以是字符串,数值,或结构体。  
成功返回 true，失败返回 null,错误信息,错误代码

<h6 id="fileObject.writeBuffer">fileObject.writeBuffer </h6>
 写入buffer数据,成功返回写入长度,失败返回null

<h6 id="fileObject.writeBuffer">fileObject.writeBuffer(buffer指针,写入长度) </h6>
 直接写数据到内存  
参数@1可以是 buffer,或内存指针,  
如果是指针则必须指定写入长度,否则长度参数可选  
成功返回写入长度
