process帮助文档
===========================================

<details>  <summary>相关范例</summary>  <p>
范例程序 > 进程
</p></details>


<a id="process"></a>
process 成员列表
-------------------------------------------------------------------------------------------------
运行执行文件或关联文档。  
如果省略所有参数则打开当前进程  
失败则返回 null,错误信息,错误代码  
成功返回进程对象

<h6 id="process( ,系统命令行)">process( ,系统命令行) </h6>
 如果省略第一个参数,并仅指定命令行,则作为系统命令行启动运行

<h6 id="process()">process() </h6>
 [返回对象:processObject](#processObject)

<h6 id="process(执行文件,命令行参数,STARTUPINFO)">process(执行文件,命令行参数,STARTUPINFO) </h6>
 命令行参数可以是字符串或由多个字符串组成的数组,  
数组参数调用 process.joinArguments 合并,  
不在双引号内、且包含空白或需要转义的参数转义处理后首尾添加双引号,  
命令参数最大长度为 32765 个字符。  
STARTUPINFO参数为process.STARTUPINFO 结构体,可选参数  
如果该参数是普通table对象.将自动创建为STARTUPINFO结构体

<h6 id="process(执行文件,命令行参数,更多命令行参数,...)">process(执行文件,命令行参数,更多命令行参数,...) </h6>
 命令行参数可以是一个数组、一个或多个字符串参数,  
  
数组或多个命令行参数调用 process.joinArguments 合并,  
不在双引号内、且包含空白或需要转义的参数转义处理后首尾添加双引号,  
命令参数最大长度为 32765 个字符

<h6 id="process(进程ID,权限)">process(进程ID,权限) </h6>
 打开指定ID的进程,  
参数@2可选用一个数值指定请求的权限，  
不指定权限时默认尝试 _PROCESS_ALL_ACCESS 权限,  
如果失败则尝试以 _SYNCHRONIZE 权限打开进程,  
_SYNCHRONIZE权限打开时只能使用 wait,waitOne函数,  
非管理权限进程创建管理权限进程时只能以_SYNCHRONIZE权限打开进程

<h6 id="process(进程句柄,负责释放进程句柄)">process(进程句柄,负责释放进程句柄) </h6>
 使用指定的进程句柄创建进程对象,  
参数@2为可选参数,默认为true

<h6 id="process.		ReadProcessMemory64">process.		ReadProcessMemory64 </h6>
 
    ::Ntdll.api("NtWow64ReadVirtualMemory64","int(POINTER hProcess,LONG base,struct &buf,long size,long & retSize)" )

<h6 id="process.	GetModuleFileNameEx">process.	GetModuleFileNameEx </h6>
 
    Psapi.api("GetModuleFileNameEx","INT(pointer hProcess,pointer hModule,ustring& lpFilename,INT size)" )

<h6 id="process.	MEMORY_BASIC_INFORMATION">process.	MEMORY_BASIC_INFORMATION </h6>
 
    class {
     		addr BaseAddress;
     		int AllocationBase;
     		INT AllocationProtect;
     		INT RegionSize;
     		INT State;
     		INT Protect;
     		INT Type;
    	}

<h6 id="process.	THREADENTRY32">process.	THREADENTRY32 </h6>
 
    class {
     	INT dwSize;
     	INT cntUsage;
     	INT th32ThreadID; // this thread
     	INT th32OwnerProcessID; // Process this thread is associated with
     	int tpBasePri;
     	int tpDeltaPri;
     	INT dwFlags;
    	}

<h6 id="process.::Advapi32 :">process.::Advapi32 : </h6>
 
    ..raw.loadDll("Advapi32.dll")

<h6 id="process.CreateProcess">process.CreateProcess </h6>
 
    ::Kernel32.api("CreateProcess","int(ustring app, ustring &cmd, pointer processAttributes,pointer threadAttributes, bool inheritHandles, INT creationFlags,ustring environment, ustring lpCurrentDirectory, struct lpStartupInfo, struct& lpProcessInformation )");

<h6 id="process.CreateProcessWithLogonW">process.CreateProcessWithLogonW </h6>
 
    ::Advapi32.api("CreateProcessWithLogonW","int(ustring user,ustring domain,ustring pwd,INT flags,ustring app, ustring &cmd, INT creationFlags,ustring environment, ustring lpCurrentDirectory, struct lpStartupInfo, struct& lpProcessInformation )");

<h6 id="process.FindExecutable">process.FindExecutable </h6>
 
    ::Shell32.api("FindExecutable","int(ustring file,ustring directory,ustring &result)")

<h6 id="process.GetExitCodeProcess">process.GetExitCodeProcess </h6>
 
    ::Kernel32.api("GetExitCodeProcess","bool(POINTER hProcess,INT &code)");

<h6 id="process.GetPriorityClass">process.GetPriorityClass </h6>
 
    ::Kernel32.api("GetPriorityClass","INT(POINTER hProcess");

<h6 id="process.IsWow64Process">process.IsWow64Process </h6>
 
    ::Kernel32.api( "IsWow64Process", "bool(pointer hProcess,bool &Wow64Process)");

<h6 id="process.OpenProcess">process.OpenProcess </h6>
 
    ::Kernel32.api("OpenProcess","pointer(INT desiredAccess,bool inherit,INT pid)")

<h6 id="process.PROCESS_INFORMATION">process.PROCESS_INFORMATION </h6>
 
    class {
     		pointer hProcess;
     		pointer hThread;
     		INT dwProcessId;
     		INT dwThreadId;
    	}

<h6 id="process.ReadProcessMemory">process.ReadProcessMemory </h6>
 
    ::Kernel32.api("ReadProcessMemory","int(POINTER hProcess,addr base,struct &buf,INT size,INT & retSize)" )

<h6 id="process.ReadProcessMemoryByString">process.ReadProcessMemoryByString </h6>
 
    ::Kernel32.api("ReadProcessMemory","int(POINTER hProcess,addr base,string &buf,INT size,INT & retSize)" )

<h6 id="process.ReadProcessMemoryByString64">process.ReadProcessMemoryByString64 </h6>
 
    ::Ntdll.api("NtWow64ReadVirtualMemory64","int(POINTER hProcess,LONG base,string &buf,long size,long & retSize)" )

<h6 id="process.STARTUPINFO">process.STARTUPINFO </h6>
 
    class {
     		INT cb = 68;
     		ustring reserved;
     		ustring desktop;
     		ustring title;
     		INT x;
     		INT y;
     		INT xSize;
     		INT ySize;
     		INT xCountChars;
     		INT yCountChars;
     		INT fillAttribute;
     		INT flags;
     		WORD showWindow;
     		WORD cbReserved2;
     		ustring lpReserved2;
     		pointer stdInput;
     		pointer stdOutput;
     		pointer stdError; 
     		creationFlag = 0;
     		inheritHandles;
    	};

<h6 id="process.STARTUPINFO">process.STARTUPINFO() </h6>
 创建进程启动参数  
  
[返回对象:startinfoObject](#startinfoObject)

<h6 id="process.SetPriorityClass">process.SetPriorityClass </h6>
 
    ::Kernel32.api("SetPriorityClass","bool(POINTER hProcess,INT priorityClass)");

<h6 id="process.SetProcessAffinityMask">process.SetProcessAffinityMask </h6>
 
    ::Kernel32.api("SetProcessAffinityMask","INT(pointer hProcess,INT dwProcessAffinityMask)" )

<h6 id="process.TerminateProcess">process.TerminateProcess </h6>
 
    ::Kernel32.api("TerminateProcess","int(pointer hProcess,INT exitCode)")

<h6 id="process.Thread32First">process.Thread32First </h6>
 
    ::Kernel32.api("Thread32First","int(pointer hsnap,struct& lppe)")

<h6 id="process.Thread32Next">process.Thread32Next </h6>
 
    ::Kernel32.api("Thread32Next","int(pointer hsnap,struct& lppe)")

<h6 id="process.VirtualAllocEx">process.VirtualAllocEx </h6>
 
    ::Kernel32.api("VirtualAllocEx","addr(POINTER hProcess ,addr addr,int dwSize,int flAllocationType,int flProtect)")

<h6 id="process.VirtualFreeEx">process.VirtualFreeEx </h6>
 
    ::Kernel32.api("VirtualFreeEx","int(POINTER hProcess,addr addr,int dwSize,int dwFreeType)")

<h6 id="process.VirtualProtectEx">process.VirtualProtectEx </h6>
 
    ::Kernel32.api("VirtualProtectEx","bool(POINTER hProcess,addr addr, INT dwSize, INT flNewProtect, INT &lpflOldProtect )");

<h6 id="process.VirtualQueryEx">process.VirtualQueryEx </h6>
 
    ::Kernel32.api("VirtualQueryEx","INT(pointer hProcess,addr addr,struct& buf,INT dwLength)" )

<h6 id="process.WaitForInputIdle">process.WaitForInputIdle </h6>
 
    ::User32.api("WaitForInputIdle","INT(pointer hProcess,INT dwMilliseconds)");

<h6 id="process.WriteProcessMemory">process.WriteProcessMemory </h6>
 
    ::Kernel32.api("WriteProcessMemory","int(POINTER hProcess,addr base,struct buf,INT size,INT & retSize)" )

<h6 id="process.WriteProcessMemory64">process.WriteProcessMemory64 </h6>
 
    ::Ntdll.api("NtWow64WriteVirtualMemory64","int(POINTER hProcess,LONG base,struct buf,long size,long & retSize)" )

<h6 id="process.WriteProcessMemoryByString">process.WriteProcessMemoryByString </h6>
 
    ::Kernel32.api("WriteProcessMemory","int(POINTER hProcess,addr base,string buf,INT size,INT & retSize)" )

<h6 id="process.WriteProcessMemoryByString64">process.WriteProcessMemoryByString64 </h6>
 
    ::Ntdll.api("NtWow64WriteVirtualMemory64","int(POINTER hProcess,LONG base,string buf,long size,long & retSize)" )

<h6 id="process.arguments">process.arguments() </h6>
 返回一个命令行参数表对象。  
或将参数指定的表（只能是未指定元表的纯表）转换为命令行参数表对象。  
此参数表可用于指定 process 库所有启动进程函数的命令行参数。  
表中无 / 或 - 前导字符的参数名将由小驼峰转换为连字符风格并加上 -- 前缀。  
名值对参数将用空格分开参数名与参数值。  
注意 -- 前缀的参数用等号和空格分隔参数值通常是等价的。  
参数表的其他数组成员也会直接转换为命令行参数

<h6 id="process.dup">process.dup(句柄,,目标进程句柄) </h6>
 复制句柄到目标进程句柄

<h6 id="process.dup">process.dup(句柄,源进程句柄) </h6>
 从指定进程复制句柄到当前进程

<h6 id="process.dup">process.dup(句柄,源进程句柄,目标进程句柄) </h6>
 进程句柄参数省略则为当前进程句柄  
函数支持更多可选参数如下:  
(句柄,源进程,目标进程,是否可继承,选项,安全访问级别)  
默认可继承,选项默认为_DUPLICATE_CLOSE_SOURCE | _DUPLICATE_SAME_ACCESS  
如果不指定最后一个参数

<h6 id="process.each">process.each </h6>
 
    for processEntry in process.each( ".*.exe" ) {   
    //遍历所有进程  
    	//io.print( processEntry.szExeFile  )  
       
    }

<h6 id="process.eachModule">process.eachModule </h6>
 
    for moduleEntry in process.eachModule(/*进程ID*/) {   
    //io.print( moduleEntry.szExePath  )  
       
    }

<h6 id="process.eachThread">process.eachThread </h6>
 
    for threadEntry in process.eachThread(/*进程ID*/) {   
    //io.print( threadEntry.th32ThreadID  )  
       
    }

<h6 id="process.emptyWorkingSet">process.emptyWorkingSet() </h6>
 将工作集中的内存尽可能移动到页面文件中,  
应发在最小化或程序空闲内存确实暂不需要使用时调用,  
不应频繁调用此函数

<h6 id="process.environment">process.environment() </h6>
 返回当前进程的所有环境变量组成的字符串  
键与值之间使用等号分隔,每个键值对中间以'\0'分隔  
尾部没有'\0'

<h6 id="process.escapeArgument">process.escapeArgument("命令行参数") </h6>
 转义命令行参数  
关于命令行参数转义规则,请参考 string.cmdline 的说明

<h6 id="process.execute">process.execute </h6>
 运行exe应用程序,成功返回进程ID,  
参数详细用法请参考本函数源码以及 WINAPI 中 ShellExecuteEx 函数用法  
运行 UWP 应用请使用 com.shell.activateApp 函数  
  
raw.execute 提供了与本函数类似的功能

<h6 id="process.execute">process.execute("__", parameters="",operation="open",showCmd,workDir,hwnd) </h6>
 参数 @1 为程序路径或系统命令。  
  
可选参数 @parameters 可以是字符串或字符串数组，用于指定启动参数。  
如果指定数组则由 process.joinArguments 自动处理转义并支持命名参数。  
如果启动参数只指定一个文件路径，为避免可能包含空格或以反斜杆结尾等需要转义的情况，  
建议写为 process.execute(exePath,{path}) 这种格式，让 aardio 自动处理转义。  
  
可选参数 @operation 为启动模式  
可选参数 @showCmd 使用 _SW_ 前缀常量，与win.show参数用法相同。  
可选参数 @workdir 为工作目录  
可选参数 @hwnd 可指定父窗口句柄

<h6 id="process.executeEx">process.executeEx("__", parameters="",operation="open",showCmd,workDir,hwnd,fmask) </h6>
 运行 EXE 应用程序,返回 SHELLEXECUTEINFO 结构体,  
参数 @1 指定要运行的执行程序路径,  
参数 @2 可用一个字符串或字符串数组指定启动参数。  
所有参数用法与 process.execute 函数相同。  
关于 @fmask 详细用法请参考本函数源码（一般用不到）。  
除参数 @1 以外所有参数可选

<h6 id="process.executeInvoke">process.executeInvoke(path, parameters,operation,showCmd,workDir,hwnd) </h6>
 创建临时的后台线程运行应用程序  
在打开程序前退出主线程可能无法执行操作,  
参数与 process.execute 函数用法一样,除指定参数@1或参数@2,其他所有参数可选

<h6 id="process.executeWait">process.executeWait("__", parameters="",operation="open",showCmd,workDir=",hwnd=0) </h6>
 运行exe应用程序  
并等待应用程序关闭  
除参数@1以外所有参数可选，所有参数用法与 process.execute 相同

<h6 id="process.executeWaitInput">process.executeWaitInput("__", parameters="",operation="open",showCmd,workDir=",hwnd=0) </h6>
 运行exe应用程序  
并等待进程初始化完成并接受输入  
除参数@1以外所有参数可，所有参数用法与 process.execute 相同

<h6 id="process.explore">process.explore("__/*目录路径*/") </h6>
 使用资源管理器打开目录  
打开WIN10应用这样写:process.explore("shell:appsFolder\appPath")  
使用 com.shell.eachApp 可列出WIN10所有appPath

<h6 id="process.exploreSelect">process.exploreSelect("__/*文件路径*/") </h6>
 打开资源管理器,选定该文件

<h6 id="process.find">process.find("__/*exe 文件名*/") </h6>
 查找进程并返回进程对象,  
参数@1指定要查找的进程启动文件名,注意应指定文件名而非文件路径,  
文件名参数支持模式匹配语法,忽略大小写,

<h6 id="process.find">process.find() </h6>
 [返回对象:processObject](#processObject)

<h6 id="process.findExe">process.findExe("__/*文件路径*/") </h6>
 查找文件关联的可执行程序

<h6 id="process.findId">process.findId("__/*exe 文件名*/") </h6>
 查找进程并返回进程 ID,  
参数@1指定要查找的进程启动文件名,注意应指定文件名而非文件路径,  
文件名参数支持模式匹配语法,忽略大小写

<h6 id="process.firstThreadId">process.firstThreadId(__/*进程ID*/) </h6>
 返回进程的首个线程 ID

<h6 id="process.getHandle">process.getHandle() </h6>
 获取当前进程伪句柄

<h6 id="process.getId">process.getId() </h6>
 获取当前进程 ID

<h6 id="process.getInfo">process.getInfo </h6>
 获取进程信息

<h6 id="process.getInfo">process.getInfo() </h6>
 [返回对象:ProcessInfoObject](#ProcessInfoObject)

<h6 id="process.getInfo">process.getInfo(handle) </h6>
 获取进程信息，参数 @1 指定进程句柄

<h6 id="process.getInfo">process.getInfo(handle,infoClass,infoStruct) </h6>
 参数 @1 指定进程句柄。  
如果参数 infoClass 指定非 null 数值，  
并且 infoStruct 指定结构体。  
在 Win8 以及之后系统获取信息到该结构体。  
成功返回原结构体

<h6 id="process.getParent">process.getParent() </h6>
 获取父进程对象  
  
[返回对象:processObject](#processObject)

<h6 id="process.getParentId">process.getParentId() </h6>
 获取父进程 ID

<h6 id="process.getPath">process.getPath(__/*进程ID*/) </h6>
 返回执行程序文件完整路径

<h6 id="process.is">process.is(__) </h6>
 传入参数是否 process 对象

<h6 id="process.isExe">process.isExe("__/*文件路径*/") </h6>
 检测目标文件是否可执行文件  
如果是可执行文件返回"PE32"或"PE64"  
第二个返回值为子系统,GUI为2,CUI为3  
失败或参数为 null 返回 null

<h6 id="process.joinArguments">process.joinArguments(参数表) </h6>
 参数可以是一个数组或多个非 null 参数,  
调用 tostring 转换参数项为字符串,合并为单个命令行参数并返回,  
不在双引号内、且包含空白字符或 ^ | & 等字符 的参数转义处理后首尾添加双引号  
  
如果参数是一个表，  
则表中以键名以 - 或 / 开头的键值对自动合并为命令行参数，  
键值对参数总是置于数组参数之前

<h6 id="process.kill">process.kill </h6>
 查找并关闭进程,  
注意有些进程需要管理权限才能找到,  
例如资源管理器进程 "explorer.exe" 无管理权限有时会失败,  
在代码第一行添加//RUNAS//可申请管理权限

<h6 id="process.kill">process.kill(exePath,restart) </h6>
 查找所有同名 exe 文件的进程，并关闭进程。  
参数 @exePath 支持模式匹配语法,忽略大小写。  
返回进程的完整路径。  
  
如果 @restart 参数为 true，  
则杀进程成功后立即重新启动该进程

<h6 id="process.kill">process.kill(pid) </h6>
 使用参数@pid指定进程ID,关闭该进程

<h6 id="process.openUrl">process.openUrl(__) </h6>
 调用默认浏览器打开网址,用于窗口程序,  
如果不用这个方法创建线程去打开网址,可能会出现界面卡顿不流畅的现象,  
在打开网址前退出主线程可能无法执行操作  
控制台程序应调用 process.execute 以避免后台线程不能阻止控制台关闭

<h6 id="process.regAs">process.regAs(`__/*命令参数*/`) </h6>
 以管理权限执行 reg 命令

<h6 id="process.shell">process.shell </h6>
 运行exe应用程序,返回进程对象

<h6 id="process.shell">process.shell("__", parameters="",operation="open",showCmd,workDir,hwnd,fmask) </h6>
 参数 @1 指定要运行的执行程序路径,  
参数 @2 可用一个字符串或字符串数组指定启动参数,  
所有参数用法与 process.execute 函数相同。  
关于 @fmask 详细用法请参考本函数源码（一般用不到）。  
除参数 @1 以外所有参数可选

<h6 id="process.shell">process.shell() </h6>
 [返回对象:processObject](#processObject)

<h6 id="process.shellAs">process.shellAs </h6>
 以管理权限运行 EXE 应用程序,返回进程对象

<h6 id="process.shellAs">process.shellAs("__", parameters="",showCmd,workDir,hwnd,fmask) </h6>
 参数 @1 指定要运行的执行程序路径,  
参数 @2 可用一个字符串或字符串数组指定启动参数。  
所有参数用法与 process.execute 函数相同。  
关于 @fmask 详细用法请参考本函数源码（一般用不到）。  
除参数 @1 以外所有参数可选

<h6 id="process.shellAs">process.shellAs() </h6>
 [返回对象:processObject](#processObject)

<h6 id="process.workDir">process.workDir </h6>
 创建进程默认工作目录，  
默认值为"/"，也即应用程序根目录。  
  
启动程序路径可直接访问时默认工作目录为应用程序所在目录，  
反之启动程序路径传入 io.exist 返回 false 则默认工作目录为 process.workDir，  
一般不建议改变默认工作目录，  
更好的选择是在创建进程的选项参数中指定 workDir

<a id="processObject"></a>
processObject 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="processObject.asm">processObject.asm(机器码数组,函数原型,调用约定) </h6>
 使用table数组指定任意个机器码参数,使用分号隔开,  
机器码可以是字符串,结构体,数值或指针,  
函数原型可省略,调用约定默认为"cdecl"

<h6 id="processObject.asmCdecl">processObject.asmCdecl(函数原型,任意多个机器码参数) </h6>
 写入机器码返回函数对象  
请参考:aardio工具->其他编译器->INTEL汇编语言->汇编转机器码

<h6 id="processObject.asmStdcall">processObject.asmStdcall(函数原型,任意多个机器码参数) </h6>
 写入机器码返回函数对象  
请参考:aardio工具->其他编译器->INTEL汇编语言->汇编转机器码

<h6 id="processObject.asmThiscall">processObject.asmThiscall(函数原型,任意多个机器码参数) </h6>
 写入机器码返回函数对象  
请参考:aardio工具->其他编译器->INTEL汇编语言->汇编转机器码

<h6 id="processObject.assignToJobObject">processObject.assignToJobObject(process.job.limitKill) </h6>
 绑定到作业对象,成功返回 true  
作业对象示例请参考标准库 process.job.limitKill 的源码。  
也可直接调用 killOnExit 函数绑定 process.job.limitKill

<h6 id="processObject.closeMainWindow">processObject.closeMainWindow() </h6>
 关闭进程的主窗口，忽略隐藏窗口

<h6 id="processObject.ctrlEvent">processObject.ctrlEvent(0) </h6>
 发送Ctrl+C(SIGINT信号)  
信号将传递到与目标进程控制台连接的所有非分离控制台进程  
64位目标进程会导致当前控制台暂时关闭

<h6 id="processObject.ctrlEvent">processObject.ctrlEvent(1) </h6>
 发送Ctrl+Break(SIGBREAK信号)  
信号将传递到与目标进程控制台连接的所有非分离控制台进程  
64位目标进程会导致当前控制台暂时关闭

<h6 id="processObject.eachModule">processObject.eachModule </h6>
 
    for moduleEntry in processObject.eachModule() {   
    //io.print( moduleEntry.szExePath  )  
       
    }

<h6 id="processObject.eachQuery">processObject.eachQuery(开始地址,结束地址,搜索数据,保护类型,访问类型) </h6>
 
    for( addr,len,str,i,j,pattern,protect,mtype  
    	in processObject.eachQuery(  , ,"/*搜索模式*/" ) ){  
    	  
    }

<h6 id="processObject.eachThread">processObject.eachThread </h6>
 
    for threadEntry in processObject.eachThread() {   
    //io.print( threadEntry.th32ThreadID  )  
       
    }

<h6 id="processObject.emptyWorkingSet">processObject.emptyWorkingSet() </h6>
 将工作集中的内存尽可能移动到页面文件中,  
应发在最小化或程序空闲内存确实暂不需要使用时调用,  
不应频繁调用此函数

<h6 id="processObject.free">processObject.free() </h6>
 释放进程对象。  
不是关闭进程,仅仅是释放对进程的控制句柄。

<h6 id="processObject.getExitCode">processObject.getExitCode() </h6>
 该函数调用成功有两个返回值:进程退出代码,进程是否已退出

<h6 id="processObject.getInfo">processObject.getInfo </h6>
 获取进程信息

<h6 id="processObject.getInfo">processObject.getInfo() </h6>
 获取进程信息

[返回对象:ProcessInfoObject](#ProcessInfoObject)

<h6 id="processObject.getInfo">processObject.getInfo(infoClass,infoStruct) </h6>
 如果参数 infoClass 指定非 null 数值，  
并且 infoStruct 指定结构体。  
在 Win8 以及之后系统获取信息到该结构体。  
成功返回原结构体。  
  
此用法内部调用 ::Kernel32.GetProcessInformation  
细节请参考该 API 文档

<h6 id="processObject.getMainWindow">processObject.getMainWindow() </h6>
 返回进程的主窗口以及窗口进程ID，找不到则搜寻子进程主窗口。  
查找时忽略隐藏窗口。  
  
也可以调用 winex.mainWindows 获取主窗口。  
winex.mainWindows 查找规则略有不同，请参考源码

<h6 id="processObject.getMainWindow">processObject.getMainWindow(类名) </h6>
 返回进程的指定类名的主窗口以及窗口进程ID，找不到则搜寻子进程主窗口。  
类名参数支持模式匹配语法

<h6 id="processObject.getModuleBaseAddress">processObject.getModuleBaseAddress(模块名) </h6>
 返回模块基址,  
模块名忽略大小写,  
不指定模块名则返回应用程序基址

<h6 id="processObject.getParentId">processObject.getParentId() </h6>
 获取父进程 ID

<h6 id="processObject.getPath">processObject.getPath() </h6>
 返回执行程序文件完整路径。  
如果该进程以管理权限运行，  
则调用函数的进程也必须以管理权限运行  
才能获取到路径

<h6 id="processObject.getPriorityClass">processObject.getPriorityClass() </h6>
 返回进程优先级

<h6 id="processObject.getUiInfo">processObject.getUiInfo() </h6>
 获取UI线程窗口焦点,光标等信息  
  
[返回对象:guithreadinfoObject](#guithreadinfoObject)

<h6 id="processObject.handle">processObject.handle </h6>
 进程句柄

<h6 id="processObject.id">processObject.id </h6>
 进程 ID

<h6 id="processObject.isWow64">processObject.isWow64() </h6>
 进程是否在64位系统上运行的32进程

<h6 id="processObject.isX64">processObject.isX64() </h6>
 是否64位进程

<h6 id="processObject.kill">processObject.kill() </h6>
 杀除进程

<h6 id="processObject.killOnExit">processObject.killOnExit() </h6>
 主进程退出时自动退出此进程

<h6 id="processObject.malloc">processObject.malloc(长度) </h6>
 在目标进程分配内存空间

<h6 id="processObject.malloc">processObject.malloc(长度,访问类型) </h6>
 在目标进程分配内存空间

<h6 id="processObject.malloc">processObject.malloc(长度,访问类型,分配类型) </h6>
 在目标进程分配内存空间

<h6 id="processObject.mfree">processObject.mfree(指针) </h6>
 释放malloc成员函数分配的内存指针

<h6 id="processObject.mfree">processObject.mfree(指针,释放长度,0x4000) </h6>
 释放malloc成员函数分配的内存指针  
不建议手工指定长度

<h6 id="processObject.protect">processObject.protect(__/*内存地址*/,4/*_PAGE_READWRITE*/,1) </h6>
 修改内存保护属性,返回原来的保护属性,  
第三个参数指定内存长度

<h6 id="processObject.query">processObject.query(开始地址,结束地址,搜索数据,保护类型,访问类型) </h6>
 查找下一个有效内存地址,所有参数可选,  
搜索数据可以是字符串或结构体  
返回值: addr,len,str,i,j,pattern,protect,mtype

<h6 id="processObject.readNumber">processObject.readNumber(__/*内存地址*/) </h6>
 读取一个int整数,32位  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readNumber">processObject.readNumber(__/*内存地址*/,"BYTE") </h6>
 读取一个字节,8位无符号  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readNumber">processObject.readNumber(__/*内存地址*/,"INT") </h6>
 读取一个int整数,32位无符号  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readNumber">processObject.readNumber(__/*内存地址*/,"LONG") </h6>
 读取一个long类型整数,64位无符号  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readNumber">processObject.readNumber(__/*内存地址*/,"WORD") </h6>
 读取一个word类型整数,16位无符号  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readNumber">processObject.readNumber(__/*内存地址*/,"byte") </h6>
 读取一个字节,8位  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readNumber">processObject.readNumber(__/*内存地址*/,"long") </h6>
 读取一个long类型整数,64位  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readNumber">processObject.readNumber(__/*内存地址*/,"word") </h6>
 读取一个word类型整数,16位  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readString">processObject.readString(内存地址,长度) </h6>
 读取定长字符串  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readStringUtf16">processObject.readStringUtf16(内存地址,长度) </h6>
 读取定长Unicode字符串  
转换为UTF8编码  
注意长度以字符为单位  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.readStruct">processObject.readStruct(内存地址,结构体) </h6>
 读取定义的结构体  
打开进程需要指定 _PROCESS_VM_READ 权限

<h6 id="processObject.remoteApi">processObject.remoteApi </h6>
 在外部进程内创建远程调用函数

<h6 id="processObject.remoteApi("void">processObject.remoteApi("void()","dll名","函数名") </h6>
 参数(函数原型,加载DLL模块名,函数名,调用约定)   
不指定调用约定时默认使用stdcall调用约定  
不会在API函数名字后面自动添加或移除"A","W"编码声明后缀,  
并且要注意搜索DLL时默认搜索路径包含目标EXE所在目录,而非当前EXE目录

<h6 id="processObject.remoteApi("void">processObject.remoteApi("void()","dll名","函数名","cdecl,borland") </h6>
 参数(函数原型,加载DLL模块名,函数名,调用约定)   
不会在API函数名字后面自动添加或移除"A","W"编码声明后缀,  
并且要注意搜索DLL时默认搜索路径包含目标EXE所在目录,而非当前EXE目录

<h6 id="processObject.remoteApi("void">processObject.remoteApi("void()",CALL地址,调用约定) </h6>
 参数(函数原型,CALL地址,调用约定)   
不指定调用约定在数时默认使用stdcall调用约定

<h6 id="processObject.remoteApi("void">processObject.remoteApi("void(INT thisAddr)","dll名","函数名","thiscall") </h6>
 参数(函数原型,加载DLL模块名,函数名,调用约定)  
thiscall使用第一个参数指定this指针地址  
不会在API函数名字后面自动添加或移除"A","W"编码声明后缀,  
并且要注意搜索DLL时默认搜索路径包含目标EXE所在目录,而非当前EXE目录

<h6 id="processObject.resume">processObject.resume() </h6>
 恢复运行

<h6 id="processObject.sendMessage">processObject.sendMessage(hwnd,message,wParam,lParam) </h6>
 向外部进程窗口发送消息  
lParam如果是结构体则复制到目标进程内存,  
结构体如果包含指针应当自行调用 process.malloc 分配内存并复制  
发送消息涉及的用法太多,尤其是涉及到访问外部进程内存,  
所涉及的知识量不能通过看几句函数说明获得,  
普通用户请不要学习或使用此函数

<h6 id="processObject.setAffinity">processObject.setAffinity(1) </h6>
 指定进程运行的CPU内核

<h6 id="processObject.setInfo">processObject.setInfo(infoClass,infoStruct) </h6>
 设置进程信息，成功返回 true。  
infoClass 指定数值，infoStruct 指定结构体。  
此函数内部调用 ::Kernel32.SetProcessInformation 。  
详细用法请参考 API 文档。  
在低于 Win8 的系统不执行操作

<h6 id="processObject.setPriorityClass">processObject.setPriorityClass(0x80/*_HIGH_PRIORITY_CLASS*/) </h6>
 设置进程优先级

<h6 id="processObject.stillActive">processObject.stillActive() </h6>
 进程是否仍在运行

<h6 id="processObject.suspend">processObject.suspend() </h6>
 暂停进程

<h6 id="processObject.terminate">processObject.terminate() </h6>
 强行终止进程  
可在参数中指定退出代码

<h6 id="processObject.tid">processObject.tid </h6>
 返回进程的主线程 ID

<h6 id="processObject.wait">processObject.wait() </h6>
 等待进程关闭,  
可选使用一个毫秒值参数设定超时  
超时或失败返回 false,  
进程已退出则返回值1为true,返回值2为退出代码

<h6 id="processObject.waitMainWindow">processObject.waitMainWindow </h6>
 等待并返回进程主窗口以及窗口进程ID。  
也可调用 winex.mainWindows 且指定参数 @2 为 true 以等待主窗口。  
winex.mainWindows 查找规则略有不同，请参考源码

<h6 id="processObject.waitMainWindow">processObject.waitMainWindow(类名,等待窗口句柄) </h6>
 等待并返回进程主窗口以及窗口进程ID。  
所有参数可选。  
可选指定要等待的类名,类名参数支持模式匹配语法,  
不指定类名时忽略隐藏窗口,  
可选指定等待窗口句柄,该窗口关闭时些函数不再等待并直接返回结果

<h6 id="processObject.waitOne">processObject.waitOne() </h6>
 等待进程关闭,不阻塞UI消息循环,  
可选使用一个毫秒值参数设定超时  
超时或失败返回 false,  
进程已退出则返回值1为true,返回值2为退出代码

<h6 id="processObject.write">processObject.write(内存地址,任意个字符串或结构体参数) </h6>
 写入数据,成功返回写入尾部内存地址,  
失败返回空

<h6 id="processObject.writeNumber">processObject.writeNumber(__/*内存地址*/,0) </h6>
 写入一个int整数,32位

<h6 id="processObject.writeNumber">processObject.writeNumber(__/*内存地址*/,0,"BYTE") </h6>
 写入一个字节,8位无符号

<h6 id="processObject.writeNumber">processObject.writeNumber(__/*内存地址*/,0,"INT") </h6>
 写入一个int整数,32位无符号

<h6 id="processObject.writeNumber">processObject.writeNumber(__/*内存地址*/,0,"LONG") </h6>
 写入一个long类型整数,64位无符号

<h6 id="processObject.writeNumber">processObject.writeNumber(__/*内存地址*/,0,"WORD") </h6>
 写入一个word类型整数,16位无符号

<h6 id="processObject.writeNumber">processObject.writeNumber(__/*内存地址*/,0,"byte") </h6>
 写入一个字节,8位

<h6 id="processObject.writeNumber">processObject.writeNumber(__/*内存地址*/,0,"long") </h6>
 写入一个long类型整数,64位

<h6 id="processObject.writeNumber">processObject.writeNumber(__/*内存地址*/,0,"word") </h6>
 写入一个word类型整数,16位

<h6 id="processObject.writeString">processObject.writeString(内存地址,字符串,长度) </h6>
 写入字符串,长度为可选参数,  
省略内存地址参数则自动分配内存,  
该函数返回写入内存地址,写入长度

<h6 id="processObject.writeStringUtf16">processObject.writeStringUtf16(内存地址,字符串) </h6>
 写入Unicode字符串  
参数可以为默认的UTF8编码文本

<h6 id="processObject.writeStruct">processObject.writeStruct(内存地址,结构体) </h6>
 写入定义的结构体,  
省略内存地址参数则自动分配内存,  
该函数返回写入内存地址,写入长度

<a id="ProcessInfoObject"></a>
ProcessInfoObject 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="ProcessInfoObject.exitStatus">ProcessInfoObject.exitStatus </h6>
 进程退出代码

<h6 id="ProcessInfoObject.pebBaseAddress">ProcessInfoObject.pebBaseAddress </h6>
 PEB 地址,  
注意 64 位进程这里返回 math.size64 对象,  
32 位进程返回数值

<h6 id="ProcessInfoObject.prarentId">ProcessInfoObject.prarentId </h6>
 父进程ID

<a id="heapEntry"></a>
heapEntry 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="heapEntry.dwAddress">heapEntry.dwAddress </h6>
 
    Linear address of start of block

<h6 id="heapEntry.dwBlockSize">heapEntry.dwBlockSize </h6>
 
    Size of block in bytes

<h6 id="heapEntry.dwFlags">heapEntry.dwFlags </h6>
 
    dwLockCount =

<h6 id="heapEntry.dwResvd">heapEntry.dwResvd </h6>
 
    th32ProcessID = owning process

<h6 id="heapEntry.dwSize">heapEntry.dwSize </h6>
 结构体大小;

<h6 id="heapEntry.hHandle">heapEntry.hHandle </h6>
 
    Handle of this heap block

<h6 id="heapEntry.th32HeapID">heapEntry.th32HeapID </h6>
 
    heap block is in

<a id="heapList"></a>
heapList 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="heapList.dwFlags">heapList.dwFlags </h6>
 
    

<h6 id="heapList.dwSize">heapList.dwSize </h6>
 结构体大小;

<h6 id="heapList.th32HeapID">heapList.th32HeapID </h6>
 
    heap (in owning process's context!)

<h6 id="heapList.th32ProcessID">heapList.th32ProcessID </h6>
 
    owning process

<a id="moduleEntry"></a>
moduleEntry 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="moduleEntry.GlblcntUsage">moduleEntry.GlblcntUsage </h6>
 
    ProccntUsage =

<h6 id="moduleEntry.dwSize">moduleEntry.dwSize </h6>
 结构体大小

<h6 id="moduleEntry.modBaseAddr">moduleEntry.modBaseAddr </h6>
 模块基址;

<h6 id="moduleEntry.modBaseSize">moduleEntry.modBaseSize </h6>
 hModule = 模块句柄

<h6 id="moduleEntry.szExePath">moduleEntry.szExePath </h6>
 
    

<h6 id="moduleEntry.szModule">moduleEntry.szModule </h6>
 
    0;

<h6 id="moduleEntry.th32ModuleID">moduleEntry.th32ModuleID </h6>
 模块ID;

<h6 id="moduleEntry.th32ProcessID">moduleEntry.th32ProcessID </h6>
 进程ID,INT数据类型

<a id="processEntryObject"></a>
processEntryObject 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="processEntryObject.cntThreads">processEntryObject.cntThreads </h6>
 此进程开启的线程计数

<h6 id="processEntryObject.dwSize">processEntryObject.dwSize </h6>
 结构体长度，以字节为单位

<h6 id="processEntryObject.pcPriClassBase">processEntryObject.pcPriClassBase </h6>
 进程优先级,INT数据类型

<h6 id="processEntryObject.szExeFile">processEntryObject.szExeFile </h6>
 进程启动文件名,不是文件完整路径

<h6 id="processEntryObject.th32ParentProcessID">processEntryObject.th32ParentProcessID </h6>
 父进程的 ID

<h6 id="processEntryObject.th32ProcessID">processEntryObject.th32ProcessID </h6>
 进程ID,INT数据类型

<a id="startinfoObject"></a>
startinfoObject 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="startinfoObject.createNoWindow">startinfoObject.createNoWindow </h6>
 应用程序不创建控制台窗口

<h6 id="startinfoObject.creationFlag">startinfoObject.creationFlag </h6>
 
    startinfoObject.creationFlag = CREATE //创建进程的参数,参考API CreateProcess的说明

<h6 id="startinfoObject.desktop">startinfoObject.desktop </h6>
 标识启动应用程序所在的桌面的名字

<h6 id="startinfoObject.domain">startinfoObject.domain </h6>
 域名

<h6 id="startinfoObject.environment">startinfoObject.environment </h6>
 新进程的环境变量  
以键值对组成的字符串,多个键值对间请以'\0'分隔  
键与值之间以=号分隔  
也可以传入包含键值对的表对象

<h6 id="startinfoObject.fillAttribute">startinfoObject.fillAttribute </h6>
 控制台窗口使用的文本和背景颜色

<h6 id="startinfoObject.flags">startinfoObject.flags </h6>
 
    startinfoObject.flags = _STARTF_USE //指定结构体中哪些成员生效

<h6 id="startinfoObject.inheritEnvironment">startinfoObject.inheritEnvironment </h6>
 如果此属性的值恒等于false,且同时指定了environment的值,  
那么创建的子进程不会继承父进程的环境变量  
此属性不指定值时默认值为true

<h6 id="startinfoObject.inheritHandles">startinfoObject.inheritHandles </h6>
 默认值为真,所有有可被继承属性的内核对象都会被复制到子进程(实际上是内核对象引用计数加一)

<h6 id="startinfoObject.logonFlags">startinfoObject.logonFlags </h6>
 登录选项,默认为 _LOGON_WITH_PROFILE

<h6 id="startinfoObject.password">startinfoObject.password </h6>
 登录密码

<h6 id="startinfoObject.processAttributes">startinfoObject.processAttributes </h6>
 SECURITY_ATTRIBUTES结构体指针,一般不建议设置  
如需设置请使用raw.malloc将结构体转换为指针

<h6 id="startinfoObject.showWindow">startinfoObject.showWindow </h6>
 显示参数，  
支持以_SW_ 前缀的常量  
_SW_HIDE 表示隐藏窗口（默认值）。  
此属性用于指定是否显示控制台以外的窗口，  
flags 字段必须指定 _STARTF_USESHOWWINDOW 才会生效

<h6 id="startinfoObject.stdError">startinfoObject.stdError </h6>
 标准错误输出(可用于创建管道)

<h6 id="startinfoObject.stdInput">startinfoObject.stdInput </h6>
 标准输入（可用于创建管道）

<h6 id="startinfoObject.stdOutput">startinfoObject.stdOutput </h6>
 标准输出（可用于创建管道）

<h6 id="startinfoObject.suspended">startinfoObject.suspended </h6>
 是否休眠创建进程的主线程  
如果为真自动添加_CREATE_SUSPENDED参数

<h6 id="startinfoObject.threadAttributess">startinfoObject.threadAttributess </h6>
 SECURITY_ATTRIBUTES结构体指针,一般不建议设置  
如需设置请使用raw.malloc将结构体转换为指针

<h6 id="startinfoObject.title">startinfoObject.title </h6>
 控制台标题

<h6 id="startinfoObject.username">startinfoObject.username </h6>
 登录用户名

<h6 id="startinfoObject.waitInputTimeout">startinfoObject.waitInputTimeout </h6>
 进程启动后等待初始化完成的最大超时  
默认为0xFFFFFFFF(无限等待),设为0则不等待

<h6 id="startinfoObject.workDir">startinfoObject.workDir </h6>
 进程工作目录,  
默认值为 process.workDir

<h6 id="startinfoObject.x">startinfoObject.x </h6>
 x坐标(子进程使用默认坐标时、或控制台窗口支持)

<h6 id="startinfoObject.xCountChars">startinfoObject.xCountChars </h6>
 控制台宽度(字符单位)

<h6 id="startinfoObject.xSize">startinfoObject.xSize </h6>
 窗口宽(子进程使用默认坐标时、或控制台窗口支持)

<h6 id="startinfoObject.y">startinfoObject.y </h6>
 y坐标(子进程使用默认坐标时、或控制台窗口支持)

<h6 id="startinfoObject.yCountChars">startinfoObject.yCountChars </h6>
 控制台高度(字符单位)

<h6 id="startinfoObject.ySize">startinfoObject.ySize </h6>
 窗口高(子进程使用默认坐标时、或控制台窗口支持)

<a id="threadEntry"></a>
threadEntry 成员列表
-------------------------------------------------------------------------------------------------

<h6 id="threadEntry.cntUsage">threadEntry.cntUsage </h6>
 引用计数

<h6 id="threadEntry.dwFlags">threadEntry.dwFlags </h6>
 th32OwnerProcessID = 进程ID

<h6 id="threadEntry.dwSize">threadEntry.dwSize </h6>
 结构体大小

<h6 id="threadEntry.th32OwnerProcessID">threadEntry.th32OwnerProcessID </h6>
 
    Process this thread is associated with

<h6 id="threadEntry.th32ThreadID">threadEntry.th32ThreadID </h6>
 线程ID

<h6 id="threadEntry.tpBasePri">threadEntry.tpBasePri </h6>
 
    tpDeltaPri =


自动完成常量
-------------------------------------------------------------------------------------------------
_CREATE_NEW_CONSOLE=0x10  
_CREATE_NEW_PROCESS_GROUP=0x200  
_CREATE_NO_WINDOW=0x8000000  
_CREATE_PROCESS_DEBUG_EVENT=3  
_CREATE_SUSPENDED=4  
_MEM_4MB_PAGES=0x80000000  
_MEM_COMMIT=0x1000  
_MEM_DECOMMIT=0x4000  
_MEM_FREE=0x10000  
_MEM_LARGE_PAGES=0x20000000  
_MEM_MAPPED=0x40000  
_MEM_PHYSICAL=0x400000  
_MEM_PRIVATE=0x20000  
_MEM_RELEASE=0x8000  
_MEM_RESERVE=0x2000  
_MEM_RESET=0x80000  
_MEM_ROTATE=0x800000  
_MEM_TOP_DOWN=0x100000  
_MEM_WRITE_WATCH=0x200000  
_PAGE_EXECUTE=0x10  
_PAGE_EXECUTE_READ=0x20  
_PAGE_EXECUTE_READWRITE=0x40  
_PAGE_EXECUTE_WRITECOPY=0x80  
_PAGE_GUARD=0x100  
_PAGE_NOACCESS=1  
_PAGE_NOCACHE=0x200  
_PAGE_READONLY=2  
_PAGE_READWRITE=4  
_PAGE_WRITECOMBINE=0x400  
_PAGE_WRITECOPY=8  
_PROCESS_ALL_ACCESS=0x1FFFFF  
_PROCESS_CREATE_PROCESS=0x80  
_PROCESS_CREATE_THREAD=2  
_PROCESS_DUP_HANDLE=0x40  
_PROCESS_QUERY_INFORMATION=0x400  
_PROCESS_QUERY_LIMITED_INFORMATION=0x1000  
_PROCESS_SET_INFORMATION=0x200  
_PROCESS_SET_QUOTA=0x100  
_PROCESS_SET_SESSIONID=4  
_PROCESS_SUSPEND_RESUME=0x800  
_PROCESS_TERMINATE=1  
_PROCESS_VM_OPERATION=8  
_PROCESS_VM_READ=0x10  
_PROCESS_VM_WRITE=0x20  
_STANDARD_RIGHTS_REQUIRED=0xF0000  
_SYNCHRONIZE=0x100000  
_TH32CS_INHERIT=0x80000000  
_TH32CS_SNAPALL=0xF  
_TH32CS_SNAPHEAPLIST=1  
_TH32CS_SNAPMODULE=8  
_TH32CS_SNAPMODULE32=0x10  
_TH32CS_SNAPPROCESS=2  
_TH32CS_SNAPTHREAD=4  
