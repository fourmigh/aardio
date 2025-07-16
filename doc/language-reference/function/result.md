# 函数返回值


##  `return` 语句

请参考：[定义函数](definitions.md)

在函数体内部可以选用 `return` 语句返回一个或多个值，如果不返回任何值或者不调用 `return` 语句函数会默认返回 null 值。

`return` 语句之后直到函数结束的代码将视为注释注略不执行，

## 使用函数返回值

在 aardio 中函数是纯函数（Pure Function）- 输入输出数据流都是显式（Explicit）的。  

函数从函数外部接受的所有输入信息都通过参数传递到该函数内部；函数输出到函数外部的所有信息都通过返回值传递到该函数外部，即函数的数据只有一个入口( 参数 )，一个出口( 返回值 )。函数可以返回多个值。  
  
示例如下：

  
```aardio
import console; 

//定义一个函数 
var func = function(){ 
	//函数有多个返回值 
	return 1,2,3; 
}

// re==1,re2==2,re3==3,re4==null
var re,re2,re3,re4 = func(); 

// re==1 其他返回值被丢弃
re = func(); 

//把所有的返回值放到表构造器中生成一个 table，
var tab = { func() }; //tab 的值为 {1,2,3}

//这里 func 仅返回一个值
console.log( func() , "123" ); 

//只有在参数列表的尾部才能接收函数的多个返回值
console.log( "123" , func() );

console.pause();
```  

函数可以省略部分返回值，如下：

```aardio
var re1,,re2 = func(arg1,arg2,arg3)
```  

也可以丢试所有返回值，如下：

```aardio
func(arg1,arg2,arg3)
```  

## 使用返回值判断函数是否执行成功

在 aardio 中如果一个函数有返回值，  
通常成功时返回值的逻辑值为真，失败时通常会返回2个值，分别为 null,错误信息 。  
  
有时候也会返回 false,错误信息。  
null,false 作为逻辑值使用时是等价的，不影响判断语句的逻辑。  

但函数失败时不一定会提供错误信息，有些函数会多一个返回值以提供错误代码。  
  
如果未加特别说明，标准库函数都遵守此规则。  
库函数一般不会轻易抛出异常，而是使用返回值来告知调用代码是否出错。

只有在遇到了不应该出现的严重的设计时错误，库函数才会抛出异常。抛出异常通常意谓着该错误是应该在开发期间就必须修正的问题。  
  
示例：  

```aardio
var test = function(a,b){        
	//无法原谅的设计时错误,抛出异常  
	if( a === null ) error("请输入第一个参数");    
	if( b === null ) error("请输入第二个参数");        
	
	if( false ){        
		return null,"函数在执行时遇到了运行时错误,例如网络连接失败"    
	}        
	
	return true;  
}
```

## 使用 rget 拣选返回值

1. 函数原型：   

	`返回值列表 = rget( 开始位置,... )`

	`返回值列表 = rget( 开始位置,函数调用语句 )`

	`返回值列表 = rget( 表对象或函数对象 )`

2. 函数说明： 

	rget 参数 @1 如果用数值指定开始位置则截取并返回后续参数,丢弃开始位置之前的参数。开始位置为正数时表示自前向后计数，1 表示自第 2 个参数开始向后截取，2 表示自 3 个参数开始向后截取 ...... n 表示自 n 个参数开始向后截取。开始位置为负数时从最后一个参数开始倒数到指定位置开始向后截取所有参数。  

	可以使用函数调用语句返回其他参数，然后使用 rget 自返回值列表中截取指定位置之后的全部返回值。

	`rget( array )` 的第一个参数 @array 如果是表则展开并返回表的全部数组成员，等价于调用 `table.unpack( array )` 。

	`rget( func )` 的第一个参数 @func 如果是函数，则调用参数 @func 指定的函数并返回该函数的所有值。如果指定了多个参数则将这些参数作为调用 @func 函数的参数。
  
3. 调用示例： 
  
	```aardio 

	//定义一个函数 
	var func = function(){ 
		//函数有多个返回值 
		return 1,2,3; 
	}

	//从第二个参数开始返回, re==2,re2==3
	var re,re2 = rget(2,func() );  

	//从倒数第一个参数开始返回, re==3,re2==null
	var re,re2 = rget(-1,func() );  

	//展开数组，等价于调用 table.unpack( [1,2,3] )
	var item1,item2,item3 = rget( [1,2,3] ); 
	```

## 多返回值与不定参数

在函数实参尾部调用有多个返回值的函数时，会增加多个调用参数。

示例：

```aardio
import console;

var tab = {}

table.push( tab,tonumber("1") );
table.push( tab,tonumber("123") );

console.log("你觉得tab应当有两个成员吗,下面我们看看 tab 里面是什么:")

// tab 里面实际有四个元素,超出了我们的预期,这是怎么回事?
console.varDump( tab )
```

aardio 支持多个返回值，也支持不定参数,
`tonumber()` 转换字符串为数值，成功则返回两个值，一个是转换后的数值, 一个是该数值在字符串中的字节长度。例如 `tonumber("123")` 返回 `123,3` 。

而 table.push() 函数接受不定个数的参数，它会将所有参数添加到数组的尾部。
`table.push( tab,tonumber("123") )` 在执行时就变成了  `table.push( tab,123,3 )` 。

我们可以向下面这样加一对圆括号：

`table.push( tab,( tonumber("123") ) ); ` 

圆括号建立的单值表达式只会返回一个值。

参考：[table.push](../../library-guide/builtin/table/_.md#push)