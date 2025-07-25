# 等式运算符

等式运算符比较两个操作数是否相等，并返回 boolean 类型的值(true 或 false)。

## 一、全等式运算符

全等式又称为恒等式，要求数据类型绝对相等、并且不会调用 `_eq` 元方法。因此也不能重载恒等操作符。  

| 运算符 | 说明 |
| --- | --- |
| `===` | 恒等运算符 |
| `!==` | 非恒等运算符 |

如果是数字，字符串，指针，布尔值，在值与类型都相等时恒等式返回真，返则返回假．  
如果是其他传址对象，指向同一个对象返回真，否则返回假．  

| 示例 | 结果 |
| --- | --- |
| `"abc" === "abc"` | true |
| `null === false` | false |

## 二、等式运算符

等式运算符判断两个操作数是否相等。  

| 运算符 | 说明 |
| --- | --- |
| `==` | 等式运算符 |
| `!=` | 不等式运算符 |

> 在无歧义时可以使用 `=` 替代 `==` 。  
> 需要注意的是： `func( "键名"="值" )` 这种写法所传的函数参数是一个[省略外层 `{}` 的表参数](../function/parameter.md#table-argument)，而 `func( "字符串1"=="字符串2" )` 这种写法所传的参数则是一个返回布尔值的等式。 

"等式"与"恒等式"最大的不同是"等式"允许类型自动转换。与逻辑值有关的类型自动转换规则：

1. 在条件判断中，非 0. 非 null、非 false 为 true，反之为 false。  
  
2. 使用 [等式运算符](..\operator\equality.md) 比较 2 个值时：  
    - 任意值与 true,false 比较则先转换为布尔值。  
    - 非布尔值与数值比较，则先转换为数值，然后比较数值是否相等。  

        例如 `null == 0` 就属于非布尔值与数值比较，而 null 转换为数值还是 null，null 与 0 不是相等的数值。所以 `null == 0` 会返回 false 。

        再例如  `""== 0` 或 `' \t\r\n'== 0`  同样属于非布尔值与数值比较，空白字符串自动转换为数值时返回 0，所以 `""== 0` 或 `' \t\r\n'== 0` 都会返回 true。

 > 注意：**当等式或不等式的操作数中只有数值而没有出现布尔值，就不应当错误地根据布尔值转换规则去推导结果。**

请参考： [隐式类型转换](../datatype/datatype.md#type-coercion)

### 1. 操作数的数据类型相同之间的等式比较规则

对于数值( type.number )、字符串( type.string )、指针( type.pointer )，布尔值( type.boolean )等传值类型比较值是否相等。  
  
| 示例 | 结果 |
| --- | --- |
| `"abc" == "abc"` | true |
| `123 != 456` | true |

对于 table、cdata、bufffer 等传引用类型，当引用同一个对象时相等。否则检查参考比较的两对象是否存在相同的 `_eq` [元方法 ](../datatype/table/meta.md) ，如果存在就调用 `_eq` 元方法判断是否相等。如果操作数不是同一个对象且没有相同的 `_eq` 元方法则不相等。
  
| 示例 | 结果 | 说明 |
| --- | --- | --- |
| `::User32 != ::Kernel32` | true | 引用了不同的对象 |
| `{} == {}` | false | 引用了不同的对象 |
| `raw.buffer("abc")==raw.buffer("abc")`| false | 引用了不同的对象 |
| `time.now() == time.now() `| true | 调用time.now().\_eq元方法比较 |

### 2. 非布尔值与布尔值之间的等式比较规则

0, null 与 false 相等，而所有非零、非 false、非 null 值与 true 相等。

| 表达式 | 结果 |
| --- | --- |
| `0==false` | true |
| `null==false` | true |
| `1==false` | false |
| `if( 1 ) console.log("true")` | 条件符合，执行代码 console.log("true") |

### 3. 非布尔非数字值与数字值之间的等式比较规则

非布尔非数字值与数值比较，则先转换为数值，然后比较数值是否相等。 要注意字符串在自动转换为数值时，空白字符串会转换为 0，转换忽略 `_tonumber` 元主法。

| 表达式 | 结果 |
| --- | --- |
| `"123"==123` | true |
| `"abc"==123` | false |
| `""==0 `| true |
| `'\r\n\t '==0` | true |
| `null==0` | false |

### 4. 其他不同类型的操作数之间的等式比较规则

如果数据类型不同、会尝试进行类型转换后进行比较。如果类型转换失败、也无适合的 _eq 元方法可以调用则返回 false。

