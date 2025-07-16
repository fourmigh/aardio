# aardio 窗口绘图基础

对于绘图大致了解一些基础的概念就可以。
学习 aardio 并不是一定要深入学习这些内容，大多时候不需要自己绘图。

## 一. 绘图接口 <a id="gdi" href="#gdi">&#x23;</a>


1. GDI(Graphics Device Interface) 

    GDI 是基础的绘图接口。
    aardio 标准库 `gdi` 用于 GDI 绘图操作，win.ui 库默认会导入 gdi 库。
    传统的窗口控件默认使用 GDI 绘图。

2. GDI+(GDI Plus)

    GDI+ 是新版绘图接口。
    标准库 `gdip` 用于 GDI+ 绘图操作。

    win.ui 默认不会导入 gdip 库。
    但 plus 控件基于 gdip（GDI+），而高级选项卡基于 plus 控件。
    使用了 plus 控件也会自动导入有关的 gdip.bitmap gdip.graphics 等 GDI+ 常用库。

    aardio 程序用得比较多的是 GDI+ 。

    [GDI+ 参考手册](https://learn.microsoft.com/zh-cn/windows/win32/gdiplus/-gdiplus-gdi-start)

## 二. 图像

1. GDI 位图

    GDI 位图与图标与其他系统资源一样通过句柄操作，句柄是一个指针类型的值。 
    使用位图句柄要仔细查看相关函数的文档，了解什么时候需要手动释放位图。
    可使用 ::DeleteObject(hBmp) 释放位图（参数 hBmp 为位图句柄）。
	
2. com.picture 图像
	
    [com.picture](../../../../library-reference/com/picture.md) 可用于加载 JPG,GIF,BMP 等图像， 
    调用 com.picture 对象的 CopyHandle() 函数可以复制位图并返回位图句柄。 
    调用 com.picture.fromBitmap() 函数可将位图句柄转换为 com.picture 对象。
    picturebox 控件使用 com.picture 加载图像并获取位图句柄。
    com.picture 对象会在不需要时自动回收，可选用 com.Release() 函数主动释放。
	
3. gdip.bitmap 位图对象

    [gdip.bitmap](../../../../library-reference/gdip/bitmap.md) 用于创建GDI+ 位图对象。
    可用于加载常用图像格式,例如 JPG,GIF,BMP，并且支持 PNG 以及 PNG 透明度。

    调用 gdip.bitmap 对象的 copyHandle() 函数可以复制位图并返回位图句柄。 
    调用 gdip.bitmap 构造函数可将参数 @1 指定的位图句柄转换为 gdip.bitmap 对象。
    调用 gdip.bitmap 构造函数可将参数 @1 指定的 com.picture 对象转换为 gdip.bitmap 对象。

    gdip.bitmap 对象会在不需要时自动回收，可选用对象的成员函数 dispose() 主动释放

## 三. 颜色格式

1. 用于 GDI 的 RGB 颜色值

    RGB 颜色值一般用于 GDI 接口（aardio 标准库：gdi），以及标准控件。

    RGB 颜色值可用 6 位 16 进制数值 0xBBGGRR 表示。
    BB 表示蓝色（blue）分量，GG 表示绿色（green)分量，RR 表示红色(red)分量。

    例如 0xFF0000 表示蓝色。

    RGB 颜色值在内存存储格式用 aardio 中用单引号包含的编译时转义字符串表示就是 `'\xRR\xGG\xBB'`。
    用 aardio 结构体表示就是 `{ BYTE r;  BYTE g;  BYTE b; } `，存储顺序都是低位在前。

    数值的存储顺序同样是低位在前，也就是小端字节序（这样记：低位在前，小端在前）
    但日常书写数值顺序是高位在前，所以 RGB 颜色值写为数值就是 0xBBGGRR。 

    在 Windows 的 GDI 头文件中表示此颜色值的定义为 COLORREF 与 RGB 。

    aardio 窗体设计器中的所有设计时属性里的颜色值（例如 bgcolor, forecolor,color 等 ）都使用此 0xBBGGRR 格式，这是因为窗体设计器的属性面板使用 GDI 颜色格式。就算是完全由 GDI+ 实现的 plus 控件也是在初始化时将 bgcolor, forecolor, color 自动转换为使用 GDI+ 的颜色格式（0xAARRGGBB）的 background, foreground, argbColor 等控件属性值。
	
2. 用于 GDI+ 的 ARGB 颜色值

    ARGB 颜色值用于 GDI+ 接口（aardio 标准库：gdip），以及基于 GDI+ 的 plus 控件。

    ARGB 颜色可用 8 位 16 进制数值 0xAARRGGBB 表示。
    AA 表示 Alpha 值，Alpha 值影响的是透明度，其他表示 RGB 分量。

    例如 0xFFFF0000 表示不透明红色。

    ARGB 颜色值在内存存储格式用 aardio 中单引号包围的编译时转义字符串表示就是 `'\xBB\xGG\xRR\xAA'`。
    用 aardio 结构体表示就是 `{ BYTE b;BYTE g;BYTE r;BYTE a; }`
    数值的书写顺序是反过来的，所以写为 0xAARRGGBB 。

    在 Windows 的 GDI+ 头文件中对应这种颜色格式的 ARGB 类型被定义为 DWORD 类型的别名 —— 对应 aardio 原生类型中的 INT 类型（ aardio 中大写 INT 为无符号 32 位整数 ）。

    在用 aardio 调用 .NET 时，.NET 里的 System.Drawing.Color 颜色值在 aardio 中也会被自动转换为 ARGB 格式颜色数值。

    在 aardio 中调用 plus 控件、高级选项卡等基于 GDI+ 的对象的 skin 函数调整样式方案时，参数里的颜色值都使用此 0xAARRGGBB 格式。

3. 字符串格式

    aardio 提供 gdi.colorParse() 函数可解析网页兼容的、用字符串表示的颜色代码。
    支持 `#RGB`、`#RRGGBB`、`#RRGGBBAA` 三种格式，`#`号可省略。

    `#RGB`、`#RRGGBB` 返回 GDI 兼容的 RGB 值。
    `#RRGGBBAA` 返回 GDI+ 兼容的 ARGB 格式颜色值。

    现代浏览器组件( 例如 web.view )可支持 `#RRGGBBAA` 格式颜色值，IE 浏览器组（指 web.form ）不支持 `#RRGGBBAA` 。

    也可以用 `gdi.colorStringify()` 将颜色值转换为字符串格式。

    这种字符串格式的颜色值一般用在网页代码中，aardio 代码中一般都使用速度更快的数值格式颜色值。

示例：

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form(text="颜色格式";bgcolor=0xFFFFFF)
winform.add(
static={cls="static";bgcolor=0xFF0000;text="Static";left=192;top=98;right=542;bottom=181;align="center";center=1;font=LOGFONT(h=-32);z=1}
plus={cls="plus";bgcolor=0x0000FF;left=192;top=277;right=542;bottom=360;font=LOGFONT(h=-32);z=2};
)
/*}}*/

//-------------- GDI / 传统控件 --------------

//标准控件的背景色设为 RGB 颜色，格式 0xBBGGRR
winform.static.bgcolor = 0xFF0000;//蓝色

//获取 R,G,B 分量，参数为 RGB 颜色值
var r,g,b = gdi.getRgb(0xFF0000);
//函数名 gdi.getRgb 是 get R,G,B 的缩写。

//R,G,B 分量转换为 RGB 颜色数值（0xBBGGRR）
winform.static.bgcolor = gdi.RGB(r,g,b);

//用字符串转换为 RGB 颜色数值
winform.static.bgcolor = gdi.colorParse("#0000FF");

//颜色值转换为字符串
winform.static.text = gdi.colorStringify(0xFF0000,false);

//字体颜色设为白色
winform.static.color = 0xFFFFFF;

//-------------- GDI+ / plus 控件 --------------

//plus 控件的背景色设为 ARGB 颜色，格式 0xAARRGGBB
winform.plus.background = 0xFFFF0000;

//获取 R,G,B,A 分量，参数为 ARGB 颜色值。 
var r,g,b,a = gdi.getRgba(0xFFFF0000)
//函数名 gdi.getRgba 是 get R,G,B,A 的缩写。

//R,G,B 分量转换为 RGB 颜色数值（0xBBGGRR）
winform.plus.background = gdi.ARGB(r,g,b,a);

//用字符串转换为 ARGB 颜色数值
winform.plus.background = gdi.colorParse("#FF0000FF");

//颜色值转换为字符串
winform.plus.text = gdi.colorStringify(0xFFFF0000,true)

winform.show();
win.loopMessage();
```

## 四. 自绘

基于 GDI+ 实现的 plus 控件自绘是最方便的，
plus 控件提供了一系列的自绘事件，并已经创建好了内存画板，自动支持双缓冲优化，
并提供了创建动画的函数。

🅰 示例：

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form(text="自绘示例 - 鼠标移入太极图试试";right=601;bottom=549)
winform.add(
plus={cls="plus";text="GDI+ 控件";left=53;top=27;right=539;bottom=513;notify=1;z=1}
)
/*}}*/

import gdip;//导入 GDI+ 库

//画背景，graphics 参数是内存画板（ gdip.graphics 对象，已自动支持双缓冲优化）。
winform.plus.onDrawBackground = function(graphics,rc,bkColor,foreColor){
	
	//GDI+ 支持透明背景 PNG 图像
	var bmp = gdip.bitmap("~\extensions\wizard\project2\forms\images\winform.jpg");
		
	//绘图参数：位图,模式（"expand"为九宫格切图）,::RECT 结构体,九宫格切图上,右,下,左
	graphics.drawBackground(bmp,"expand",rc,140,140,100,225);

	//创建画笔，负责描边画线，GDI+ 使用 0xAARRGGBB 格式颜色值，支持 Alpha 透明通道。
	var pen = gdip.pen( 0xFFFF0000, 1, 2/*_GdipUnitPixel*/ );

	//画线
	graphics.drawLine( pen, 10, 10, 200, 100)
	
	//画曲线，可添加任意个坐标数值
	graphics.drawCurve(pen,
		10,10,//x,y
		100,100,//x2,y2
		50,150,//x3,y3
		200,200,//x4,y4
		50,250//x5,y5
	)

	//矩形描边
	graphics.drawRectangle(pen, 30, 30, 100, 100);
	
	//创建实色画刷，画刷负责填充、喷涂颜色。  
	var brush = gdip.solidBrush(0xAA0000FF);//GDI+使用 0xAARRGGBB 格式颜色值

	//用画刷填充矩形内部  
	graphics.fillRectangle(brush, 30, 30, 100, 100)
	
	//GDI+ 使用浮点数坐标结构体
	var p1 = ::POINTF(10,10)
	var p2 = ::POINTF(100,100)  
	
	//创建渐变画刷
	var lineBrush = gdip.lineBrush(p1/*渐变起始坐标*/, p2 /*渐变终止坐标*/ , 0xFF0000FF/*起始颜色*/, 0xFFFF0000/*结束颜色*/, 2/*_GdipWrapModeTileFlipY*/ )
	
	//为了圆形画的平滑自然,指定抗锯齿参数
	graphics.smoothingMode = 4/*_GdipSmoothingModeAntiAlias*/ ; 

	//画圆形、或椭圆
	graphics.fillEllipse(  brush, 150/*x坐标*/, 50/*y坐标*/,150/*宽*/, 120/*高*/);
	
	//用渐变色刷子填充矩形内部
	graphics.fillRectangle( lineBrush, 100, 100, 100, 100)
	
	//创建字体
	var family = gdip.family("Segoe UI");
	var curFont = family.createFont(30,2/*_GdipFontStyleItalic*/, 2/*_GdipUnitPixel*/)
	
	//创建字体格式
	var strformat = gdip.stringformat();  
	strformat.align = 0/*_GdipStringAlignmentNear*/; 
	
	//消除走样,且边作平滑处理
	graphics.textRenderingHint = 3/*_GdipTextRenderingHintAntiAliasGridFit*/;

	//设置文字区域
	var rclayout = ::RECTF(350,15,550,150) 
	
	graphics.drawString( "Hellow world!"  , curFont
	, rclayout,strformat,brush);
	
	//创建路径
	path = gdip.path();  
	path.addArc(0,0,30, 20, -90, 180);
	path.startFigure();
	path.addCurve( 
		5,30, 
		20,40,
		50,30
	)
	path.addPie(260, 10, 40, 40, 40, 110);
	
	//设置文字区域，GDI+ 使用浮点数矩形结构体
	rectfoat = ::RECTF(50,220,550,150); 
	
	//添加文字路径
	path.addstring( "AARDIO", family, 1/*_GdipFontStyleBold*/,60,rectfoat,strformat);
	
	//填充路径 
	graphics.fillPath(brush, path)
		
	//描边
	graphics.drawPath(pen, path)
		
	//自己创建的 GDI+ 对象，要自己释放
	pen.delete();
	brush.delete();
	lineBrush.delete(); 	
	strformat.delete();
	family.delete();
	curFont.delete();
	bmp.dispose();
}

//输出字符串
winform.plus.onDrawString = function(graphics,text,font,rectfoat,strformat,brush){
	
	if(#text){ 
		//回调参数中的 GDI+ 对象，不是自己创建的就不要释放
		//graphics.drawString(text,font,rectfoat,strformat,brush);
	}
}

//在前景里绘制动画：旋转的太极图
winform.plus.onDrawContent = function(graphics,rc,txtColor,rcContent,foreColor){
	
	if(owner.animationState!==null ) {
		//旋转画板 
		graphics.rotateRect(rc,owner.animationState);
	}
	
	//创建画刷
	var brush = gdip.solidBrush(0xFF84FF26);
	var brush2 = gdip.solidBrush(0xFF0080FF);
	
	//画左右半圆
	var w,h = rc.width(),rc.height();
	graphics.fillPie(brush, 0, 0, w, h, 90, 180);
	graphics.fillPie(brush2, 0, 0, w, h, 90, -180);
	
	//画鱼头
	graphics.fillPie(brush, w/4-1, h/2, w/2, h/2, 90, -180);
		graphics.fillPie(brush2, w/4+1, 0, w/2, h/2, 90, 180);
	
	//画鱼眼
	graphics.fillEllipse(brush, w/2-10, h/4-10, 20, 20);
	graphics.fillEllipse(brush2, w/2-10, h/4*3-10, 20, 20);
		
	brush.delete();
	brush2.delete();	 
}

//动画状态控制函数
winform.plus.onAnimation = function(state,beginning,change,timestamp,duration){
	
	if(timestamp < duration){
		var x = timestamp/duration;  
		return beginning+change*(-x*x + 2*x);     	
	}    
}

winform.plus.onMouseEnter = function(wParam,lParam){
	//开始动画，参数：interval,beginning,change,duration
	winform.plus.startAnimation(12,0,360,1200);
}

winform.plus.onMouseLeave = function(wParam,lParam){
	winform.plus.stopAnimation()
}

//窗体与其他控件则使用 GDI 绘图
winform.onDrawBackground = function(hdc,rc){
	//onDrawBackground 事件的 hdc 参数已经是内存绘图设备，不必再去做双缓冲优化。
	
	//GDI 使用 0xBBGGRR 格式的颜色值
	gdi.fillRect(hdc,0x00008C,rc.copy(,150));
	gdi.fillRect(hdc,0x468C00,rc.copy(200));
	
	var bmp = com.picture.loadBitmap("~\extensions\wizard\project2\forms\images\winform.jpg");
	
	//绘图参数：绘图设备,位置句柄,::RECT 结构体,九宫格切图上,右,下,左
	gdi.drawBitmap(hdc,bmp,rc.move(200,150),140,140,100,225);

	var font = ::LOGFONT(weight=800;color=0xFF);
	gdi.drawTextCenter(hdc,font,"改变窗口大小试试,任意位置贴图都可以支持九宫格",rc.move(120,150));
}

winform.show();
win.loopMessage();
```

参考：
- [plus 控件自绘背景](../../../../example/plus/drawBack.html)
- [GDI 自绘 listbox 控件](../../../../example/Windows/Listbox/gdi.html)
- [GDI+ 自绘 listbox 控件](../../../../example/Windows/Listbox/ownerDraw.html)
- [自绘 listview 控件](../../../../example/Windows/ListView/customdraw.html)