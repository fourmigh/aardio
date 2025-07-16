
# switch 函数

switch 函数类似 select case 语句，可以在一组数据中查找指定的值并执行对应的代码。

switch 利用查表法在指定表中检索值，并通过表的键值对映射获取并执行对应的函数。这种方法也被称为表驱动法（ Table-Driven Methods ）。当待检索的数据较多时，相比使用 select 或 if 语句，表驱动法精简了判断流程，速度也会快很多很多。

但是 switch 仅支持直接检索表中存储的值，如果需要进行更复杂的条件匹配，请参考  [select case 语句](../statements/branching.md#select-case) 或者 [if 语句](../statements/branching.md#if) 。

switch 是内置的保留函数，全局可用。

### 1. 函数原型：

```aardio
switch( switchValue,caseTable,default,...)
```

### 2. 参数说明：

- @switchValue 参数指定任意索引值。  
- @caseTable 参数必须指定一个待检索的表对象。这个参数不能是其他数据类型，否则会抛出异常。
- @default 可省略或指定默认函数对象。 只能省略或指定函数对象，指定其他类型值会抛出异常。

### 3. 执行操作：

- 使用 @switchValue 作为索引获取 @caseTable 表对应的成员函数并执行。如果获取到的不是函数类型则跳过。
- 如果没有找到函数则执行 @default 或 caseTable.default 指定的默认函数。如果没有找到默认函数或 caseTable.default 不是函数对象则忽略不作任何操作。
- @caseTable 表中检索返回的如果不是函数对象，都会跳过，不抛异常不报错。

### 4. 调用参数与返回值：

- switch 函数的第 4 个 @default 参数后面所有参数会作为调用新函数的参数，被调用函数的 owner 参数指定为 @caseTable 表对象。
- 被执行函数的所有返回值将由 switch 函数的原样返回给调用者。
- 如果没有匹配到任何函数或默认函数则忽略不执行任何操作。

### 5. 示例：

```aardio
switch("caseValue1",{  
	caseValue1: function(){
		print("参数值是 caseValue1")
	};
	caseValue2: function(){
		print("参数值是 caseValue2")	
	}; 
	caseValue3: function(){
		print("参数值是 caseValue3")
	};  
	default: function(){
		print("没有找到匹配的值")	
	}; 
})
```

这个函数做的事非常简单：

- 在表里面找到了名字为 "caseValue1" 的函数就执行该函数。
- 如果没有找到就执行 default 函数。default 是可选的，如果没有找到就跳过不会报错。

下面的 aardio 代码模仿了 switch 函数的主要操作：

```aardio
var caseTable = {  
	caseValue1: function(){
		print("参数值是 caseValue1")
	};
	caseValue2: function(){
		print("参数值是 caseValue2")	
	}; 
	caseValue3: function(){
		print("参数值是 caseValue3")
	};  
	default: function(){
		print("没有找到匹配的值")	
	}; 
}

var switchValue = "caseValue1"
var func = caseTable[switchValue] or caseTable.default or default 
func();
```

这种方法的关键是利用了 `caseTable[switchValue]` 这种速度极快的检索表操作以精简流程判断语句。当然，实际的 switch 函数要更复杂一些。

switch 可以用于检索任何可以存储在表索引中的值，例如：

```aardio

//在参数 @2 指定的表中查找索引值为 2 的函数，然后执行
switch(2,{
[1]: λ() print("One");
[2]: λ() print("Two"); //匹配成功，输出 2 。
[3]: λ() print("Three");  
},λ() print("Default") )
```

上面的 `λ` 等价于 `lambda` 关键字， lambda 是匿名函数的简化写法。

参考链接：[lambda 表达式](../function/lambda.md)
