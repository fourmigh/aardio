# 逻辑操作符

逻辑操作符将其操作数视为条件表达式( 可理解为与布尔值 true 比较的等式，请参考：[等式运算符](equality.md) )，首先将操作数求值，并转换为逻辑值( boolean 值 )，0, null 转换为false，而所有非零、非 false、非 null 值转换为 true，如果结果为 false 则条件为假，结果为 true 则条件为真。

> 布尔值（ boolean ）是只有 true, false 两种值的逻辑数据类型。true 表示真，false 表示假。布尔值通常用在条件表达式中。通俗一点说，true 表示是、符合条件，false 表示不是、不符合条件。 

## 一、逻辑操作符

1. 逻辑非 <a id="not" href="#not">&#x23;</a>

    逻辑非运算符首先取得操作数的布尔值( boolean )，然后取反比较。如要操作数为 true 则返回 false,如果操作数为 false 则返回 true.也就是反过来取值的意思。  

    | 运算符 | 说明 |
    | --- | --- |
    | `!` | 逻辑非 |
    | not | 逻辑非 |

    在 aardio 中逻辑操作符有 `!` 与 `not` 两种，作用相同。

2. 逻辑或 <a id="or" href="#or">&#x23;</a>

    逻辑或要求两个操作数其中之一为真( true ),如果第一个操作数为真则直接返回第一个操作数，否则返回第二个操作数。表达式会直接返回操作数的原值（不是转换为逻辑值的true或false）

    | 运算符 | 说明 |
    | --- | --- |
    | &#x7c;&#x7c; | 逻辑或 |
    | `or` | 逻辑或 |
    | `:` | 逻辑或，注意当  `:`  号作为用作表构造器中的键值对分隔符时不表示逻辑或。 |

    `||` 与 `or` 是完全等价的，优先级也相同。而 `:` 的优先级略低。 

    [条件赋值语句](#conditional-assignment) `a := b` 等价于 `a = a : b`，可用于常量赋值语句避免重复赋真值，例如 `::User32 := raw.loadDll("user32.dll");`。

    > 注意: 在构造表或纯数组时， `:` 在标识符、字符串字面量、数值字面量表示的键后面可作为健值分隔符使用。例如： `[123:"稀疏数组成员",1,2,3]` 

3. 逻辑与 <a id="and" href="#and">&#x23;</a>

    逻辑与要求两个操作数取布尔值后都为true,如果第一个操作数为真则返回第二个操作数，如果第一个操作数为假则返回第一个操作数(注意返回的是操作数原值，而不是转换后的布尔值)。  

    | 运算符 | 说明 |
    | --- | --- |
    | `&&` | 逻辑与 |
    | `and` | 逻辑与 |
    | `?` | 逻辑与 |

    `&&` 与 `and` 是完全等价的，优先级也相同。而 `?` 的优先级略低。 

## 二、惰性求值 <a id="short-cut-evaluation" href="#short-cut-evaluation">&#x23;</a>


逻辑与、逻辑或运算符支持惰性求值，当取得第一个操作数的值并满足条件时，即不再计算第二个表达式的值。

示例：

```aardio
import console; 

a = true || console.log("偷懒成功,第一个操作数已经能确定返回值了") 
a = false && console.log("偷懒成功,第一个操作数已经能确定返回值了") 
a = true && console.log("偷懒失败,第一个操作数不能确定返回值") 

console.pause();
```

## 三、条件取值 <a id="conditional-operator" href="#conditional-operator">&#x23;</a>


逻辑或、逻辑运算符返回的不是转换后的布尔值，而是操作的原值。利用此特性，可以有条件的取得操作数的值。  

  
```aardio
import console; 

console.log( true ? 123 ) //显示123
console.log( false ? 123 ) //显示false
console.log( 0 : 123 ) //显示123
console.log( 1 : 123 ) //显示1 

console.log( false ? 2 : 3 ) //显示3
console.log( true ? 2 : 3 ) //显示2 
console.log( true ? false : 3 ) //显示3
console.log( true && false || 3 ) //同上

console.pause();
```  

使用逻辑操作符构建 `a?b:c` 格式的三元表达式时，如果 a 为真但是 b 为false，返回的仍然是 c 的原值， 这一点与其他编程语言的三元操作符不同，请注意区分。<a id="ternary expression" href="#ternary expression">&#x23;</a>


## 四、条件赋值 <a id="conditional-assignment" href="#conditional-assignment">&#x23;</a>


逻辑或、逻辑与操作符可以用于赋值语句，进行有条件赋值。  

示例：

```aardio
a = a : 123;//如果a为false、null、0时赋值为123
a := 123; //等价于上面的语句，通常用于常量赋值，以避免重复赋值。
```  

`a := b` 是复合赋值语句，等价于 `a = a : b` ，也就相当于 `a = a or b`。  

示例：

```aardio
import console; 
str = "abcdefg"
str ?= string.left(str,3); //如果str为null，则不赋值，以避免string.left抛出错误
console.log( str )
```
  
`a ?= b(a)` 等价于 `a = a ? b(a)` ，也就相当于 `a = a and b(a)`。这样就实现了如果 `a` 为真则会调用 `b(a)`。

对于代码 `string.left(str,3)` ，如果 str 为 null 就会出错。使用 `str ?= string.left(str,3)` 就可以实现只有 str 为真（ 自然也就不可能是 null 值了 ）才会执行这句代码。
