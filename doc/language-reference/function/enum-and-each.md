# 枚举与迭代

## 集合对象

集合对象在这里泛指被其他对象管理的一组具有类似行为的对象，例如桌面上的所有窗口, 目录下的所有文件，aardio 语言中表示复合数据的 [table 对象](../datatype/table/_.md) 。

## 枚举、迭代

枚举函数的主要特点：

- 枚举函数名通常以 `enum` 作为前缀。
- 枚举函数需要在参数中指定处理枚举元素的回调函数，回调函数返回 `false` 停止枚举。
- 深度遍历所有子级元素。

迭代器工厂函数的主要特点：

- 创建迭代器的函数名通常以 `each` 作为前缀。
- 迭代器用于 for in 语句，迭代器返回的首个迭代变量为 `null` 时停止迭代。
- 通常不会深度遍历所有子级元素。

注意这只是一种应用于 aardio 标准库的约定，非硬件规定。

## 示例：遍历窗口

调用 winex.enum 枚举所有桌面窗口：

```aardio
import winex;

//枚举所有窗口（包括子窗口）。
winex.enum( 
	
	function(hwnd,depth){
		print( 
			depth/*深度*/,
			hwnd/*窗口句柄*/,
			win.getText(hwnd,30)/*标题*/ 
		)
	} 
)
```

> 请参考：[定义函数](definitions.md)

从上面的示例可以看出：

- 名字以 `enum` 为前缀的枚举函数需要在参数里指定一个处理枚举元素的回调函数。
- 枚举函数是会深度枚举所有向下层级的成员。以枚举窗口为例，枚举范围包含所有顶层窗口与所有子窗口。

调用 winex.each 遍历所有顶层桌面窗口：
  
```aardio
import winex;

for hwnd,title,theadId,processId in winex.each( ) { 
	print( hwnd,title,theadId,processId )
}
```  

> 请参考：[泛型 for 与迭代器](../statements/iterator.md)

从上面的示例可以看出：

- 名字以 `each` 为前缀的迭代器工厂函数会创建一个可用于 `for in` 语句的迭代器。
- 迭代器通常仅遍历集合的直接成员，不会深度遍历所有向下层级的成员。以遍历窗口为例，winex.each 创建的迭代器仅仅会遍历顶层窗口，不会深度向下遍历子窗口。
 
## 示例：遍历目录下的文件

`fsys.enum(path,fileMask,callback,subDirectory)` 可用于遍历目录下的全部文件。
path 指定目录，fileMask 指定通配符或通配符数组，callback 指定回调函数，subDirectory 指定是否递归搜索所有子目录（省略则默认为 `true` ）。

示例：

```aardio 
import fsys;
fsys.enum( "/", "*.*",
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