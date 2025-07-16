# COM 类型与类型转换

## 一、IDispatch 对象

aardio 支持 COM 原生接口，在标准库的 com.interface 名字空间下可找到很多这类原生接口。COM 原生接口使用的参数的数据类型与调用原生 API 相类似，参考 raw 库 [相关文档](../raw/datatype.md) 即可。

我们一般所说的 COM 对象并非上面所述的原生接口对象，而是实现了 COM 动态接口的 IDispatch 对象，com.CreateObject 用于 创建 IDispatch 对象， com.IsObject() 函数用于判断参数是否 IDispatch 对象。

在 aardio 中我们一般提到的 "COM 对象"也专指这种动态接口的 IDispatch 对象。IDispatch 对象在 aardio 中的元类型为 "com.IDispatch"，所以我们也称 IDispatch 对象为 "com.IDispatch" 对象。

## 二、Variant 变体类型

我们在调用 COM 对象时，一般不会直接使用 Variant 变体类型。但是 IDispatch 接口传参在内部主要使用变体类型（Variant），也就是 aardio 中的 com.Variant 对象。

变体类型（Variant）本质上是一个结构体，并且用一个 vt 字段声明了自己的类型，例如 VT_I4 就表示 4 字节的整型数值，而 VT_BSTR 表示一个字符串类型。

例如一个 VT_I4 类型的数值 123，用 aardio [结构体](../raw/datatype.md#struct) 描述就是这样：

```aardio
{
    WORD vt = 3/*_VT_I4*/;
    WORD r1;
    WORD r2;
    WORD r3;
    ptr lVal = 123;
    ptr r4;
};
```

## 三、COM 接口类型转换基本规则

### 有类型信息的自动类型转换

COM 对象通常会提供类型库可以自动获取接口参数的类型信息。如果有这样的类型信息，那么参数与返回值的类型转换可以比较完美地自动完成。aardio 会依据 COM 函数提供的参数类型、以及实际传入的参数自动转换为合适的COM类型，COM 函数的返回值也会自动转换为合适的 aardo 类型。各种 aardio 数据类型能自动支持 COM 对象，例如常见的数值、字符串类型、时间对象、table 数组等等。

### 无类型信息的转换规则

有些 COM 对象不提供类型库，无法自动获取接口的参数类型信息。这种接口有些会比较宽松并自动转换兼容的类型，但有些接口既不提供类型信息，对类型的要求又非常严格一点不对就报错。

这时候我们就先要了解在这种无类型信息的情况下，类型自动转换的规则。并且了解怎样[显式声明指定类型的参数 ](#explicit-typing)。

在无类型信息的情况下调用 COM 函数，类型转换又分为以下三大类：

1. 非数值非数组类型
    非数值非数组类型基本都能自动转换为兼容的 Variant 变体，参数会转换为合适的类型。例如布尔值，buffer 这些直接兼容，字符串转为 _VT_BSTR 类型，时间转换为 _VT_DATE 类型 …… 非数组的普通 table 以及 function 转换为 IDispatch 接口对象，这些类型基本都是相互兼容的，一般也不会出错。

2. 数值类型
    COM 参数类型报错基本都是因为数值类型。我们要特别注意 aardio 对数值的转换规则：单个整数值默认在 COM 函数中会转为 4 字节 VT_I4( int ) 类型变体，如果数值包含小数则默认转为 8 字节 _VT_R8（double) 类型变体。

3. 数组类型

    COM 中的数组则称为安全数组（ SAFEARRAY ）。

    SAFEARRAY 可以是各种类型的数据组成，例如数值数组，字符串数组，或者由 Variant 对象组成的 Variant 数组。因为 Variant 对象本身可以承载不同的数据类型，所以 Variant 数组当然也就可以包含各种类型的数组成员了。

    纯数值 aardio 数组默认会转为 _VT_R8（double) 类型 SAFEARRAY，要特别注意数组的默认转换类型不是整型。

    纯字符串 aardio 数组默认会转为 _VT_BSTR 类型 SAFEARRAY。
    普通 COM 对象（IDispatch 对象） aardio 数组会转为 _VT_DISPATCH 类型 SAFEARRAY。

    其他类型 aardio 数组会转为 _VT_VARIANT 类型 SAFEARRAY。
    
## 四、COM 安全数组（ SAFEARRAY ）

在调用 COM 接口函数时，如果参数是一个 table 对象并且包含数组元组，则转换为 COM 安全数组(SAFEARRAY)。

非空数组转换为 COM 安全数组时遵守以下规则：  

1. 如果数组元素是字符串，则转换为 _VT_BSTR 类型的 SAFEARRAY 。  
2. 如果数组元素是数值，则转换为 _VT_R8 类型的 SAFEARRAY 。  
3. 如果数组元素是普通 COM 对象（IDispatch 对象），则转换为 _VT_DISPATCH 类型的 SAFEARRAY 。 
4. 数组元素是其他类型或混合不同类型时，则转换为 _VT_VARIANT 类型的 SAFEARRAY 。 

上面说的 COM 类型得了是变体类型（ Variant Type ），以 _VT_ 前缀的常量表示。

### com.SafeArray

使用 `com.SafeArray(变体类型)` 可以创建 COM 安全数组（ SAFEARRAY ）。

com.SafeArray 返回的对象是一个标准的 aardio 纯数组，但在 `_safearray_type` 元属性中声明了 COM 元素类型。

我们也可以手动构造相同格式的数组，示例：

```aardio
[ 1,2,3, @{_safearray_type=0x11/*_VT_UI1*/} ]
```  

将这种数组传入 COM 函数， aardio 会将其作为 COM 安全数组（SAFEARRAY）使用。 

COM 对象返回到 aardio 中的安全数组也自动转换为相同格式，一个例外是 COM 字节数组会转换为 aardio 中的 buffer 类型。

### com.toSafeArray

使用 `com.toSafeArray(value,variantType)` 可将参数 @value 转换为 COM 兼容数组。

转换规则：

1. 如果参数 @value 是 `buffer` 类型则直接返回，参数 @variantType 无效。
2. 如果参数 @value 已经是 com.SafeArray 格式的 COM 数组则直接返回，参数 @variantType 无效。
3. 如果参数 @value 是 `字符串` 则返回 `{ _safearray = value;  _type = 0x11/*_VT_UI1*/}` 格式的包装对象，参数 @variantType 无效。 
4. 其他情况下返回 `{ _safearray = value || [];  _type = variantType}` 的包装对象，value 只能指定表对象或者 null 值（将创建一个空的数组作为参数）。这种特定格式的包装对象只应传给 COM 函数，不应当在 aardio 代码中使用。这种格式唯一的作用就是用来告诉 COM 函数这是一个 SAFEARRAY，同时又可以避免复制新的数组或者改变原来的数组。在这种格式的包装对象中如果如果未指定 _type 字段，则 _safearray 为字符串或 buffer 时 COM 元素类型为  _VT_UI1，否则为 _VT_VARIANT。如果将  _type 字段指定为 _VT_ILLEGAL 则自动检测并设定 COM 数组的元素类型（这是 COM 接口处理普通数组的默认规则），纯字符串数组默认元素类型为 _VT_BSTR,纯数值类型默认元素类型为 _VT_R8（double），COM 对象( IDispatch 对象 )数组元素类型为 _VT_DISPATCH，其他数组元素类型默认设置元素类型为 _VT_VARIANT。

> COM 接口仅使用 [直接下标](../../../language-reference/operator/member-access.md#raw-subscript) 读取并检查表对象的 `_safearray` 字段值，不会触发元方法。

### buffer 与字节数组

当 COM 函数返回给 aardio 的值是无符号字节数组（类型为 VT_UI1 ）时会自动转换为 aardio 中的 buffer 类型，反过来 aardio 中的 buffer 类型对象传入COM 函数时也自动表示为 COM 中的字节安全数组（元素类型为VT_UI1)。  

## 五、自动转换为 IDispatch 对象的 table、function 对象

如果传入 COM 函数的表对象不是数组、COM 安全数组、或者 COM 接口支持的 time 对象、变体对象、IDispatch 对象，就是一个普通的表对象，则会自动转换为 IDispatch 接口对象。

如果传入 COM 对象的是一个函数对象，也会自动转换为 IDispatch 接口对象。

关于将 table、function 对象自动转换为 IDispatch 接口的细节请参考 [com.ImplInterface](ImplInterface.md#IDispatch)

# 六、改变 IDispatch 接口类型转换时的默认类型

自动转换类型对于一般的 COM 对象是没有问题的，但仍然会出现一些例外，例如我们用 `com.CreateObject("ScriptControl")` 创建 VBScript 的脚本解释器，
在 VBScript 里就只能识别 Variant 类型的对象，数组也只能为 Variant 类型数组。

当然，我们可以直接调用 `com.Variant({1,2,3})` 这种方式创建 Variant 类型的数组，但这样显然很麻烦，对于这样的 COM 对象，我们要使用 `com.SetPreferredArrayType(comObject,0xC/*_VT_VARIANT*/)` 将其设置为始终使用 Variant 类型转换参数，可查看封装了 ScriptControl 的标准库 web.script 就是这样做的。

`com.SetPreferredArrayType(comObject,0xFFFF/*_VT_ILLEGAL*/)` 则会告知 aardio 使用默认规则自动分析数组类型，这是 COM 对象的默认设置。显然参数指定为 _VT_ILLEGAL 是无意义的，因为默认就是这个值。

注意 `com.SetPreferredArrayType()` 的作用具有传递性，COM 对象会将这个函数设置的值传递给它返回的其他 COM 对象。

# 七、明确声明 COM 参数类型：<a id="explicit-typing" href="#explicit-typing">&#x23;</a>


有一些 COM 对象则恰恰相反，要非常明确的指定数组的类型，
例如函数要求 16位无符号整型的数组，你传 32 位整型数组就会报错，
标准库的 dotNet 基于 COM 接口调用 .Net ，.Net 就有这个特性，需要更严格地匹配参数类型。

aardio 已在 dotNet 支持库实现了比较完美的类型自动转换和自动兼容。
但我们仍然要了解如何在 COM 接口中明确地声明数值或数组的类型。

因为我们可能在其他 COM 对象中遇到这种问题（虽然并不多见），例如 AutoCAD 的接口。

aardio 提供了以下函数可以明确的声明 COM 参数的类型：

- com.byte() 将参数指定的数值或数组声明为 8 位整型数值。
- com.ubyte()  将参数指定的数值或数组声明为 8 位无符号整型数值。
- com.word() 将参数指定的数值或数组声明为 16 位整型数值。
- com.uword() 将参数指定的数值或数组声明为 16 位无符号整型数值。
- com.int() 将参数指定的数值或数组声明为 32 位整型数值。
- com.uint() 将参数指定的数值或数组声明为 32 位无符号整型数值。
- com.long() 将参数指定的数值或数组声明为 64 位整型数值。
- com.ulong() 将参数指定的数值或数组声明为 64 位无符号整型数值。
- com.float() 将参数指定的数值或数组声明为 32 位浮点数值。
- com.double() 将参数指定的数值或数组声明为 64 位浮点数值。
- com.Variant() 将参数指定的值或数组声明为变体类型。

aardio 中原生类型使用大写表示无符号数值类型，
在以上函数名字中我们在小写的类型名前加上"u" 取代大写以表示无符号类型，

要注意不同编程语言之间的差别： 
> VB6/VBA 中 Integer 是16位数值，Long 是32位数值，
而在 C# 中 int 是32位数值, long 是64位数值，
更重要的不是类型名字，而是存储长度。

COM 函数也可以兼容 raw 库以下函数创建的类型化数值：

- raw.byte() 
- raw.ubyte() 
- raw.word() 
- raw.uword()
- raw.int()
- raw.uint()
- raw.long()
- raw.ulong()
- raw.float()
- raw.double() 

# 八、示例

```aardio
import com;

//创建用于 COM 函数的 16位数值
var v = com.word(32)

//创建用于 COM 函数的 16位数组
var v = com.word({1,2,3})

//创建用于 COM 函数的 Variant 类型变体数组（注意有的参数不允许字符串数组，要求传变体数组）
var v = com.Variant({"test","test2"});

//下面这样写与 com.word({1,2,3}) 的作用是一样的。
var v = com.Variant({1,2,3},2/*_VT_I2*/);
 
//创建用于 COM 函数的 Variant 类型变体数组,
var v = com.toSafeArray({1,2,3}); //这个函数返回的不是 Variant 也不是 SAFEARRAY,只是包装对象

//下面这样写与 com.word({1,2,3}) 的作用是一样的，但返回的不是 Variant 对象而是包装对象 
var v = com.toSafeArray({1,2,3},2/*_VT_I2*/); 

//下面这样是创建 SAFEARRAY 数组，可以作为普通 aardio 数组使用，也可以作为 COM 参数用
var v  = com.SafeArray(); //COM 对象返回的所有 SAFEARRAY 数组也是这种格式
table.push(v,1,2,3); //实际上 SAFEARRAY 也是普通数组

//也可以在参数中指明类型，下面代码作用类似 com.word({1,2,3})，但返回的是  SAFEARRAY 而非 Variant 对象。
var v  = com.SafeArray(_VT_I2,1,2,3); 
 
```