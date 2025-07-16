# 判断语句

参考：

- [等式运算符](../operator/equality.md) 
- [逻辑运算符](../operator/logical.md) 
- [关系运算符](../operator/comparison.md)
- [布尔值](../datatype/datatype.md#boolean)
- [隐式数据类型转换](../datatype/datatype.md#type-coercion)

## if 语句 <a id="if" href="#if">&#x23;</a>

if 语句包含条件判断部分、执行代码部分。 

执行代码部分可以是一句代码（不能是单个分号表示的空语句），或者一个[语句块](blocks.md)。

而 if 语句将条件表达式返回的结果与布尔值 true 进行比较，比较使用[等式运算符](../operator/equality.md)( 支持自动类型转换、` _eq` 元方法 )。

if 语句可选包含任意多个 elseif 分支判断语句，可选在最后包含一个 else 语句。

if 语句可以嵌套使用。

一个标准的 if 语句示例:  

```aardio
import console;

var a=1;

if(b==1){
	if(a==1)  {
		console.log("if");
	}
}
elseif(a==11){
	console.log("elseif");
}
else{
	console.log("else");
}

console.pause();
```  

> 注意：上面的 `elseif` 如果改为 `else if` 相当于在 `else` 语句嵌套了一个新的 if 语句，这通常是不必要的。

## select case 语句 <a id="select-case" href="#select-case">&#x23;</a>

select case 语句是一种条件分支结构，根据表达式的值执行不同的语句块。

基本结构如下：

```
// test-expression 指定要匹配的条件表达式
select( select-expression ) {

    // 一个或多个 case 语句，用于列举匹配条件表达式的值
    case case-expression {
        // 符合条件则执行这对应的语句或语句块
    } 

    // 可选的 else 语句块
    else {
        // 所有条件都不符合时执行这里的默认语句或语句块
    }   
}
```

`case-expression` 支持以下 3 种格式：

1. 指定单个条件值，例如 `case 123`。默认使用恒等运算符比较，可在开始指定比较使用的运算符，例如 `case != 10 ` 。
2. 指定逗号分隔的多个条件值， 例如 `case 1,2,3`，任意一个值匹配则满足条件。默认使用恒等运算符比较，可在开始指定比较使用的运算符，例如 ` case == 1,2,3` 。
3. 使用分号或 `to` 分隔的范围值。例如 `case 1;10` 表示使用恒等运算符匹配 1 到 10 范围（包含首尾数值 1,10）的任意数值是否相等。使用 `>=` 与 `<=` 运算符比较，不能自定义运算符。

> 恒等运算符要求要求数据类型绝对相等，不会自动转换类型进行比较。参考链接：[恒等运算符](../operator/equality.md)
   
`select case` 语句的执行流程如下：

- 第一个符合条件的 case 语句将会执行，然后退出 select case 语句。不会连续执行后面的 case 语句，没有穿透（ fall-through ）特性，break 没有中断 select 语句的作用。
- 如果所有条件都不符合，则执行可选的 else 语句。`else` 也可以简写为 `case else`。

示例：  
  
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

select case 语句允许嵌套使用，但 select case 语句嵌套写可能影响可读性，应当避免这样做。

case 或 else 也可以执行单个语句，示例：

```aardio
var selectValue = 0;

select( selectValue ) {
    case 1 print("值为 1");
    case 2,3,4 print("值为 2,3,4 其中之一"); 
    case 5;10 print("5 到 10 范围的值"); 
    case !== 0 print("不等于 0"); 
    else print("没有 case 语句符合条件");
}
```

aardio 提供的内置函数 switch 则可以自一个表对象快速检索指定值对象的函数并执行该函数，虽然 switch 函数仅支持表检索方式，但如果有大量需要比对的值， switch 函数检索的速度会快很多。

请参考：[switch 函数](../builtin-function/switch.md)