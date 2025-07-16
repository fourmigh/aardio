# 无边框窗口开发指南

## 创建无边框窗口

无边框窗口：指的是在窗体设计器中将窗体的初始化属性 border(边框) 的值设为 "none"，
这种窗口不再有默认的边框、阴影、标题栏。示例：

```aardio
import win.ui;
/*DSG{{*/  
var winform = win.form(text="无边框窗口";border="none";bgcolor=0xFFFFFF)
/*}}*/

winform.show();
win.loopMessage();

```

注意隐藏标题栏、并将边框属性设为 "resizable" 不是无边框窗口，
这种在不同操作系统上显示效果都很差，要么是多出一圈，要么顶上多出一块。
只有边框 为 "none" 的才是真正的无边框窗口。

上面这种无边框窗口，同时也就没有了默认的标题栏按钮，我们就只能按 `Alt+F4` 关闭窗口。  
好处是我们可以自行定义标题栏也边框了。

## 使用 win.ui.simpleWindow 为无边框窗口添加阴影边框与标题栏

为无边框窗口添加阴影边框、标题栏（包含标题栏按钮）最简单的方法就是使用标准库的 win.ui.simpleWindow 。

示例：

```aardio
/*
为无边框窗口添加阴影边框、可拖动边框，自定义的窗口标题栏（包含标题栏按钮）。
如果窗口设计时属性中最大化按钮设为隐藏(false)，则最大化按钮以及可拖动边框不会出现。
*/
import win.ui.simpleWindow;
win.ui.simpleWindow(winform);
```

win.ui.simpleWindow 创建的标题栏按钮默认使用透明背景，
可选在标题栏的位置先放置一个件 bk 或者 bkplus 这样的背景贴图控件自定义背景颜色，
并将该 bk 或 bkplus 控件的固定上边距、固定左边距、固定右边距都设为 true 以适应窗口缩放。

bk 或 bkplus 控件并不会创建窗口，只是在父窗体背景上绘图，所以性能较好，也更适合在上面重叠放置其他控件。

🅰 示例：

```aardio
import win.ui;

//创建无边框（无标题栏）窗口
var winform = win.form(text="aardio form";border="none";bgcolor=0xFFFFFF)

//添加深色的标题栏背景，bgcolor 使用 0xBBGGRR 格式颜色值。
winform.add(
    bk={cls="bk";bgcolor=0xA4A0A0;left=0;top=0;bottom=32;marginRight=0;dl=1;dr=1;dt=1;z=1}
)

//添加阴影边框、标题栏等，默认为透背背景，标题栏按钮等默认字体为白色
import win.ui.simpleWindow;
win.ui.simpleWindow(winform);

winform.show();
win.loopMessage();
```

也可以调用 skin 函数自定义标题栏按钮配色，  
标题栏按钮都是 plus 控件创，win.ui.simpleWindow 的 skin 函数会自动调用 plus 控件的 skin 函数，
所以参数用法与 plus 控件相同。

🅰 示例：

```aardio
import win.ui;
var winform = win.form(text="aardio form";border="none";bgcolor=0xFFFFFF)

import win.ui.simpleWindow;
var sw = win.ui.simpleWindow(winform);

//标题栏按钮都是 plus 控件，skin 函数用法与 plus 控件相同，颜色使用 0xAARRGGBB 格式
sw.skin(
	background = { 
		hover = 0x44B0C4DE; 
		active = 0x664682B4; 
		default = 0x00FFFFFF; 
	}
	color = { 
		hover = 0xFF191970;
		active = 0xFFFFFFFF; 
		default = 0xFF2F4F4F; 
	}
)

winform.show();
win.loopMessage();
```

如果调用代码未显式调用 skin 函数，
win.ui.simpleWindow 将在 100 毫秒后根据窗口背景的颜色（如果窗口此时仍未显示则只能取到白色）自动设置为浅色或深色字体方案。 

标准库提供了以下的几个类可以在无边框窗口上创建虚拟的标题栏以及可拖动边框:

- win.ui.simpleWindow 实现最普通的标题栏。
- win.ui.simpleWindow2 实现的标题栏没有最大化按钮，不允许拖动窗口边框改变大小。其他用法与 win.ui.simpleWindow 相同。
- win.ui.simpleWindow3 标题栏使用分层窗口实现，并使用 orphanWindow 悬浮在父窗口上面，此标题头使用渐变色背景。

下面简单介绍一下 win.ui.simpleWindow 的主要功能是如何实现的，细节请查看这些库的源代码。

模拟窗口标题栏功能的主要函数：

`winform.hitMax() //模拟点击最大化按钮`
`winform.hitMin() //模拟点击最小化按钮`
`winform.hitClose() //模拟点击关闭按钮`
`winform.hitCaption() //拖动标题栏`

例如鼠标左键按住窗口时允许拖动改变窗口位置：

```aardio
winform.onMouseDown  = function(wParam,lParam){
	winform.hitCaption()	
}
```

为窗口添加阴影以及可拖动边框：

```aardio
import win.ui.shadow;
win.ui.shadow(winform);
```

也可以用 win.ui.resizeBorder 单纯添加可拖动边框，但如果窗口四边由浏览器组件接管则 win.ui.resizeBorder 有可能不起作用，win.ui.shadow 则可以解决问题（因为阴影边框在窗口之外）。

如果只想添加阴影，不想支持可拖动边框，可以下面这样写：

```aardio
import win.ui.shadow;
win.ui.shadow(winform).setResizeBorder(false)
```

