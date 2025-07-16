# 随机数、随机字符串

## 随机字符串

`str = string.random( len [, seed] )  `
  
返回长度为 len 的随机字符串，seed 为可选参数指定供随机选择字符的字符串(默认值为英文字母、数字)。  
seed 参数可以使用中英文字符。

```aardio
import console; 
console.log( string.random(10  ) )
console.log( string.random(10 ,"seed参数可以使用中英文字符。") )
```  

`str = string.random( str [, str2[, ...] ] )  `
  
参数为多个字符串，随机选择其中一个字符串并作为返回值。


```aardio
import console;  
console.log( string.random( "待选字符串","待选字符串2","待选字符串3") );
```  

## 随机数

`n = math.random(min,max)`

指定最小随机数 min，最大随机数 max，返回 \[5,99\] 之间的随机数，如果不指定参数返回(0,1)之间的小数。  

## 随机数发生器

`math.randomize(随机数发生器种子)  `

用于改变随机数队列的函数。

math.randomize 的参数可以是一个任意的数值，省略参数时，则自动生成一个安全的随机数作为随机数种子。

aardio 程序在启动时，主线程会以 time.tick 获得的系统启动毫秒数作为参数调用一次 math.randomize。  

在创建新的线程时也会自动调用 math.randomize，但不会使用系统启动毫秒数作为参数，而是自动生成一个安全的随机数作为随机数种子。

## 安全随机数 <a id="csprng" href="#csprng">&#x23;</a>

math.random 与 string.random 都是基于伪随机数生成器（ PRNG ），生成速度更快但存在可预测性。

标准库 crypt.random 则是基于系统熵源（System Entropy Source）的密码学安全随机数生成器（ CSPRNG ）。
系统熵源来自不可预测的操作系统随机事件，例如鼠标的移动与点击，按键，磁盘读写，网络数据到达等随机性事件。
crypt.random 生成的随机数具有不可预测性。

🅰 示例：

```aardio
import console;
import crypt.random;

//生成长度为 10 的随机 buffer
console.hex(crypt.random(5)) 

//生成 1000 到 2000 范围的随机整数
console.log(crypt.random(1000,2000)) 

//创建随机数生成器
var rng = crypt.random();

//生成 5 个字节的随机 buffer
var buf = rng.buffer(5);

//生成 1 到 9 范围的随机整数
var n = rng.integer(1,9);

//生成 20 个字符长度的随机字符串
var str = rng.string(20)

//生成 0 到 1 之间的随机小数
var num = rng.number();

console.pause()
```

[math.random 库参考文档](../../../library-reference/crypt/random.md)