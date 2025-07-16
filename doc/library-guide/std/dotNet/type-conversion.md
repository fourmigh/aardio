# aardio / .NET 类型转换

[✅ .NET 交互入门](_.md) [📄 调试 .NET 程序集](debug.md) [💻 .NET 运行时版本](clr.md)

#  类型自动转换 <a id="coercion" href="#coercion">&#x23;</a>

aardio / C# 常用类型对应关系如下：

aardio 数据类型 | C#  数据类型
--- | --- 
bool   | bool 
string | string 
buffer | byte[]         
number(整数) | int             
number(小数) | double 
number 数组   | `double[] `  
string 数组  | `string[]` 底层是 COM 中的 BSTR 数组
其他数组     | `object[]` 底层是 COM 中的 Variant 变体类型数组
table        | object,System.__ComObject,dynamic
pointer      | 不支持(只能先转为数值)

[C# 内置类型 参考](https://learn.microsoft.com/dotnet/csharp/language-reference/builtin-types/built-in-types

数值转换规则：
- 当 aardio 数值为整数时传入 COM 或 .NET 接口为 32 位整型。
- 当 aardio 数值为小数（存在非 0 小数部分）时传入 COM 或 .NET 接口为 64 位 double 类型。
- COM 或 .NET 数值传入 aardio 都是同一 aardio 类型（ number 类型，等价于  64 位 double ）。

数组转换规则：
- COM 或 .NET 数组传入 aardio 都是 SafeArray 数组，SafeArray 是在数组的元表中声明了 COM 类型的普通纯数组。
- 使用 COM 库函数，或 dotNet 库函数创建的类型化数组传入 COM 或 .NET 接口保持原来的类型。
- 普通数值数组传入 COM 或 .NET 接口为 double 类型数组（无论数组元素是否整数）。
- 其他普通数组按单值转换规则转换为对应数组，例如字符串数组在 COM 或 .NET 接口里仍然是字符串数组。

.NET 与 aardio 特殊类型处理：

1. .NET 枚举（ enum ）：

    在 aardio .NET 的枚举自动转换为数值，
    aardio 数值也可自动转换为 .NET 枚举参数。

2. .NET 颜色数值

    .NET 的 System.Drawing.Color 在 aardio 则会自动转换为 ARGB 格式的颜色数值。
    调用 .NET 时 ARGB 格式的颜色数值也能自动转换为 System.Drawing.Color 对象。

    注意 GDI+ 使用 ARGB 格式颜色值（0xAARRGGBB），与 gdip 库，plus 控件等兼容。
    aardio 提供了 dotNet.color 函数可以显式创建 System.Drawing.Color 对象，并可避免被自动转换为 aardio 数值。

3. System.Char 类型

    Sytem.Char 类型的数组在 aardio 中会自动转换为 16 位无符号整型 COM 数组，
    反过来 aardio 的 16 位无符号整型 COM 数组也可以自动转换为 Sytem.Char 类型数组。

    可以显式调用 dotNet.char 函数创建 System.Char 类型的单值或数组，并可避免被自动转换为 aardio 数组。

4. aardio 函数对象

    aardio 函数可自动转换为 .NET 委托、事件所需要的委托类型。并且自动匹配调用参数与返回值的数据类型。

    但在 .NET 中通过 InvokeMember,dynamic 直接调用 aardio 对象成员时，回调参数中的 .NET 原生对象为原生 COM 对象，需要自己调用 dotNet.object 转换为更易操作的 [dotNet.object](#object) 对象。

5. 指针与句柄

    .NET 中的 `System.IntPtr`,`System.UIntPtr` 类型在 aardio 中会自动转换为整数值，aardio 中的指针类型（`pointer` 类型）必须使用 `tonumber()` 函数转换为数值才能传入 .NET。`HWND` 在 aardio 以整数值表示，可以直接传入 .NET，aardio 中其他`系统句柄`都是`指针类型`。

## dotNet.object  对象封包、解包原理 <a id="object" href="#object">&#x23;</a>

所有原生 .NET 中的值在 aardio 中分为两类：
1. `null`值、数值、字符串、枚举、 `System.Drawing.Color` 等简单值类型，以及这些值类型的数组可以直接交换。 
2. 其他原生 .NET 对象在 aardio 中存为 `com.NETObject` 对象（对应 .NET 中的 `System.__ComObject` 类型）。

com.NETObject 又分为：
1. 普通 .NET 对象，作为参数传入 `com.IsNetObject()` 函数时返回值为 1 。
2. 封包其他原始 .NET 对象的 DispatchableObject .NET 代理对象，传入 `com.IsNetObject()` 函数返回值为 2 。

一些 aardio 无法直接转换的 .NET 对象（例如 struct,ValueTuple,交错数组）会被自动封包到 DispatchableObject 代理对象内 (在传回 .NET 时会自动解包为原始 .NET 对象)。

.NET 与 aardio 底层交互基于 COM 接口。
但 COM 接口 难以兼容 .NET 对象复杂的数据类型与语法特性（例如无法支持函数重载）， aardio 为了解决这个问题，将 com.NETObject  自动封包为了更易使用的 dotNet.object。普通 .NET 对象自动封装为 dotNet.object 以后在 aardio 中就可以直接使用。
 
如果 DispatchableObject 存储的是 Primitive,enum,string 类型或这些类型的普通数组。则在自动封装为 dotNet.object 以后，可以使用 Value 属性读写 .NET 对象原始值。 

在 aardio 中可调用以下函数创建指定 .NET 类型的 dotNet.object 对象：<a id="casting" href="#casting">&#x23;</a>

- `dotNet.object(value,byRef)` 

    将参数 @value 指定值或数组转换为 .NET 对象。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.buffer(size,value)` 或 `dotNet.buffer(value)`

    等价于调用 dotNet.object( raw.buffer(...),true ) 。
    返回封包 buffer 的 dotNet.object，在 .NET 中可作为 byte[] 使用，支持.NET 输出或引用参数。 

    aardio 用  `raw.buffer` 函数创建的 `buffer` 与 .NET 的`byte[]` 类型是等价的，只有 .NET 输出或引用参数才需要使用 dotNet.buffer 创建 .NET 字节数组，其他情况可直接使用  `raw.buffer` 会更简单一些。

    aardio 中的 `buffer` 适用于大部分 aardio 字符串函数，区别是字符串的字节是只读的，是 `buffer` 的字节是可读写的，例如 `buffer[1]=0xFF`。

- `dotNet.byte(value,byRef) `

    将参数 @value 指定的数值或数组转换为 8 位整型数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.ubyte(value,byRef) `

    将参数 @value 指定的数值或数组转换为 8 位无符号整型数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.word(value,byRef) `

    将参数 @value 指定的数值或数组转换为 16 位整型数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.uword(value,byRef) `

    将参数 @value 指定的数值或数组转换为 16 位无符号整型数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.int(value,byRef) `

    将参数 @value 指定的数值或数组转换为 32 位整型数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.uint(value,byRef) `

    将参数 @value 指定的数值或数组转换为 32 位无符号整型数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.long(value,byRef) `

    将参数 @value 指定的数值或数组转换为 64 位整型数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.ulong(value,byRef) `

    将参数 @value 指定的数值或数组转换为 64 位无符号整型数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.float(value,byRef) `

    将参数 @value 指定的数值或数组转换为 32 位浮点数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.double(value,byRef) `

    将参数 @value 指定的数值或数组转换为 64 位浮点数值。@byRef 参数值为 true 则支持.NET 输出或引用参数。

- `dotNet.char(value,byRef) `

    将参数 @value 指定的数值、数组（只能是普通数值数组、double 或 uword 类型数组）、字符串（不能用 buffer 替代）转换为 System.Char 类型的 .NET 对象。@byRef 参数值为 true 则支持.NET 输出或引用参数。

以上函数会将所有对应的参数值存为 .NET 代理对象 DispatchableObject 以后，
再封包为 aardio 中的 dotNet.object 对象。即使简单的值类型使用上面的函数也可以封装转换为 dotNet.object 对象，这不但可以让 aardio 直接引用 .NET 中的对象，也可以方便地实现 .NET 的 ref,out 的引用与输出参数。  

无论是 aardio 值还是无法转换的 .NET 对象，封装为 DispatchableObject 代理对象以后，作为参数传入 .NET 函数总是会自动解包为原始值。而 DispatchableObject 自身也是一个 .NET 对象，在传入 aardio 时又会被 aardio 自动封包为 dotNet.Object 对象。如果 DispatchableObject 对象里实际存储的是 aardio 可转换的 .NET 基础类型或 .NET 里的 System.Object 数组，可直接使用 Value 属性读写值（读取的值是纯 aardio 对象，写入值也可以使用纯 aardio 对象），注意如果 System.Object 数组里实际存储的是 aardio 无法转换的类型则使用 Value 属性也无法读写转换。

## 使用 COM 库函数声明调用参数类型

因为 aardio 与 .NET 交互底层使用的是 COM 接口，
所以同样可以支持声明 COM 类型的 COM 库函数。

### 1. com.Variant

创建适用于 COM 接口的 VARIANT 变体对象。可用于普通 COM 对象 传值或传址参数。
可用于 .NET 普通输入参数，不支持 .NET 输出参数（可改用 dotNet.object）。
可用于 API 函数中 `VARIANT*` 指针类型参数。

参数用法：

```aardio
com.Variant(初始值,变体类型,是否输出引用参数) 
com.Variant(初始值,变体类型,输出引用) 
```

[请参考： com.Variant 文档](../../../library-reference/com/_.md#com.Variant)

### 2. 创建指定类型 com.Variant 对象

com 库提供以下函数可创建指定类型的  com.Variant 对象：

- com.int(value)
- com.uint(value) 
- com.long(value) 
- com.ulong(value) 
- com.byte(value) 
- com.ubyte(value) 
- com.word(value) 
- com.uword(value) 
- com.double(value)

返回的对象在 .NET 函数中使用时表示的 .NET 类型与 dotNet 库同名的函数创建的对象相同，
例如 com.int 返回的对象传入 .NET 函数与 dotNet.int 创建的对象相同。

这些函数可以用第二个参数指定是否创建输出引用类型的 com.Variant 对象，但引用类型的 com.Variant 对象不适用于 .NET 函数，只能使用 dotNet 库函数创建可作为引用参数的 .NET 对象。