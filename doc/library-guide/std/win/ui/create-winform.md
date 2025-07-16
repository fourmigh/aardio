---
url: https://www.aardio.com/zh-cn/doc/library-guide/std/win/ui/create-winform.html.md
---

# win.form 使用指南 - 创建窗口并添加控件

## 一. 窗体设计器 <a id="new" href="#new">&#x23;</a>

在 aardio 中创建窗口有以下方法：

- 在开发环境左上角点击"创建窗体设计器"的快捷图标，或者直接按 `Ctrl + Shift + N` 快捷键，可新建一个空白窗体。
- 打开工程向导，选择窗口程序，然后任务一个窗口工程模板，点击创建工程。
在创建工程以后，右键点选一个工程目录，点击"新建文件 » 新建窗体设计器"就可以创建窗口。

默认将在窗体设计器中以"设计视图"打开窗体，  
我们可以在开发环境左下方的"界面控件"选择和拖放控件到窗体上。  
点击窗体或控件，在右侧属性面板可以设置控件的属性。  

我们可以点击"代码视图"切换到代码编辑器模式，也可以直接按相同的 `Ctrl + U` 快捷键来回切换视图模式。在窗体设计器上直接双击控件也可以切换到代码模式并且为控件添加事件函数（默认会添加 oncommand 事件，不同的控件有所不同 ）。

## 二. 窗体设计器生成的代码结构 <a id="dsg" href="#dsg">&#x23;</a>


我们创建窗口，添加一个按钮与一个文本框，切换到代码模式，我们看到生成了以下代码：

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form({ text="窗口标题";right=759;bottom=469 })
winform.add({
button1={cls="button";text="Button";left=550;top=414;right=701;bottom=458;z=1};
edit1={cls="edit";text="Edit";left=22;top=16;right=735;bottom=398;edge=1;multiline=1;z=2}
})
/*}}*/

winform.show();
win.loopMessage();
```

`/*DSG{{*/` 到 `/*}}*/` 之间的代码由窗体设计器自动生成，这部分代码默认会折叠并显示为『窗体设计器生成代码(请勿修改)』，通常直接修改这部分代码是不必要的，应当通过窗体设计器修改相关属性。

同一个代码文件不应当包含多个 `/*DSG{{*/ ... /*}}*/` 代码块。建议使用不同的代码文件创建不同的窗口，请参考: [创建多窗口程序](multi-window.md) 。

> 注意：win.form 类并不在 win.ui 名字空间下面，但必须使用 win.ui 库导入 win.form 类。这在 aardio 标准库中是一个特例，其他标准库导入的类或者名字空间与库路径会保持一致，例如 web.view 库会导入 web.view 类。

## 三. 调用 win.form 类创建窗体对象 <a id="win.form" href="#win.form">&#x23;</a>

原型：

```aardio
var winform = win.form(formPropertiesTable)  
```

说明：

参数 formPropertiesTable 是一个表对象。  
formPropertiesTable 包含的键值对定义窗口的属性。  
实际上 formPropertiesTable 就是我们在窗体设计器上设置的窗体属性认集合。  

win.form 返回 winform 对象，在 aardio 中一般窗体通常默认命名为 winform,只有程序的第一个主窗口会命名为全局变量 mainForm。

在 aardio 的文档一般会用 winform 泛指所有 win.form 创建的窗体对象。

示例：

```aardio
var winform = win.form({ text="窗口标题";right=759;bottom=469 })
```

在 aardio 中如果函数参数是一个表对象的构造器，  
并且第一个出现的元素是用等号分隔的名值对，则可以省略外层的括号，  
例如上面的代码可以简写为：

```aardio
var winform = win.form( text="窗口标题";right=759;bottom=469 )
```

注意上面这种写法是构造了一个表对象作为参数，不是命名参数（ aardio 里没有命名参数 ）。

win.form 的构造参数表里不指定 left,top 坐标时则使用默认值 -1，这会让窗口显示在屏幕居中的默认位置。这时候窗口的宽度与高度分别为 right ＋ １,bottom ＋ １。如果 left,top 为小于等于 -2 的值则表示以窗口显示在屏幕右下角以后取得的左上角坐标作为原点计数。如果 left,top 为大于等于 0 的值则表示自屏幕左上角正常计数。如果 right, botom 字段也省略，则会自动指定一个默认值。

text 字段则指定了窗口标题，如果不指定 text 则会自动指定 title 字段为 false，这会使窗口不显示标题栏也就无法使用标题栏的最大化、最小化、关系这些常用按钮。只要是需要显示的窗口建议总是指定 text 字段，即使不显示标题栏的无边框窗口也应当指定窗口标题（方便在系统任务栏查看标题）。

## 四. 调用 winform.add 函数添加控件 <a id="winform.add" href="#winform.add">&#x23;</a>

### 1. 函数原型：
 
```aardio
winform.add = function(controlsPropertiesTable) {
    /*
	controlsPropertiesTable 参数是一个表。
	这个表包含了定义一个或多个控件的名值对: 键名为为控件的名称，名字对应的值则是控件初始化属性集合。 
	*/
}
```

### 2. 参数说明：

controlsPropertiesTable 参数是一个表对象。
controlsPropertiesTable 包含的键值对定义了一个或多个控件：
键名为该控件的名称，键名对应的值刚是描述每个控件初始化属性的集合（也是一个表对象）。 

这些控件的初始化属性可以在窗体设计器里在设计时修改，所以又称为【控件设计时属性】，与控件对象创建成功后可在运行时访问的【控件运行时属性】相区别。

### 3. 使用示例：

```aardio
import win.ui; 
var winform = win.form({ text="窗口标题";right=759;bottom=469;bgcolor=0xFFFFFF });

winform.add({
	button1={//指定访问名称为  winform.button1
		cls="button";//指定用 win.ui.ctrl.button 类构造控件
		text="添加第一个按钮 button1 到  winform.button1";//控件显示的标题
		left=264;top=410;right=469;bottom=454;//控件位置
		z=1 //Z 序
	};
	button2={//指定访问名称为  winform.button2
		cls="button";//指定用 win.ui.ctrl.button 类构造控件
		text="添加第二个按钮 button2 到 winform.button2";//控件显示的标题
		left=499;top=410;right=688;bottom=454;//控件位置
		z=2 //Z 序
	};
	edit1={//指定访问名称为  winform.edit1
		cls="edit";//指定用 win.ui.ctrl.edit 类构造控件
		text="添加第一个编辑框 edit1 到  winform.edit1";//控件文本
		left=386;top=16;right=739;bottom=398;
		edge=true;multiline=true;z=3 //边框，多行，Z 序
	};
	edit2={//指定访问名称为  winform.edit2
		cls="edit";//指定用 win.ui.ctrl.edit 类构造控件
		text="添加第一个编辑框 edit2  到  winform.button2";//控件文本
		left=22;top=16;right=375;bottom=398;//控件位置
		edge=true;multiline=true;z=4 //边框，多行，Z 序
	}
});
```

上面的代码创建了 4 个控件：

- winform.button1
- winform.button2
- winform.edit1
- winform.edit2

controlsPropertiesTable 里的控件名称也就是添加到 winform 对象的成员名字，例如控件名为 "button1" ，则之后的代码就要通过 `winform.button1` 访问该控件。

可以在窗体设计器视图中设计好控件的初始化属性，然后切换到代码视图就可以查看细节。控件的很多设计时属性与运行时属性是相同的，但有些设计时属性仅在控件初始化参数中有效，例如 plus 控件只有在设计时属性中才会使用 bgcolor,forecolor 这些 GDI 格式的颜色值，运行时则由 background, foreground 这些运行时属性支持 0xAARRGGBB （ AA 的值越小，颜色透明度越大）这种 GDI+ 格式的颜色值。

窗口控件的属性分为两种：

- 设计时属性：在创建控件以前指定的初始化属性，必须写在控件的构造参数里，IDE 的窗体设计器可以识别这些属性。
- 运行时属性：在创建控件以后可以使用的控件属性，仅在创建控件以后的代码里使用，窗体设计器并不一定支持运行时属性（很多设计时属性也是运行时属性）。

这里介绍几个常用的控件设计时属性（初始化属性）：

- `z`: 表示 Z 序的正整数，Z 序较小的控件先被创建且在 Z 轴上靠后。bk,bkplus 等背景控件，或 plus 控件用作静态背景控件时 Z 序应小于前面的控件。在窗体设计器上可以在控件右键菜单中点击“前置”、“后置”、“最前面”、“最后面”等菜单项调整 Z 序，按 <kbd>Ctrl+Shift+鼠标左键</kbd> 也可将控件移动到最前面。
- `bgcolor`,`color`,`forecolor`,`iconColor`: 
	在控件的设计时属性（也就是创建控件的初始化参数表里的属性）里可选用 `bgcolor` 自定义窗口背景颜色； 可选用 `color` 自定义字体颜色；plus 控件还可以用 `forecolor` 自定义前景颜色，使用 `iconColor` 自定义图标字体颜色。
	
	* 设计时属性里的所颜色值都使用 GDI 的颜色格式 `0xBBGGRR` （不能指定透明度）。
	* 运行时属性 `bgcolor`,`color` 也使用`0xBBGGRR` 格式颜色。
	
	plus 控件则略有不同：

	* plus 控件在创建控件以后不再使用 `bgcolor` 属性。
	* plus 控件的运行时属性 `iconColor` 使用`0xAARRGGBB` 格式颜色值，设计时属性  `iconColor` 则使用 `0xBBGGRR` 格式颜色值。
	- 调用 plus 控件的 skin 函数时，参数表内的 `color`与 `iconColor` 都使用 `0xAARRGGBB` 颜色格式。

	plus 控件也支持以下运行时属性：
	
	- `background` 背景颜色，使用 `0xAARRGGBB` 格式颜色。plus 控件在运行时不支持 `bgcolor`属性。
	- `foreground` 前景颜色，使用 `0xAARRGGBB` 格式颜色。
	- `argbColor` 字体颜色，使用 `0xAARRGGBB` 格式颜色。plus 控件的运行时属性 `color` 仍然使用  `0xBBGGRR`  颜色格式。
	- `iconColor` 图标字体颜色，使用 `0xAARRGGBB` 格式颜色。

	通过 plus 控件，以及基于 plus 控件的 win.ui.tabs 的 skin 方法（或属性）设置控件样式时，参数里的所有颜色值都只能使用 GDI+ 颜色格式，也就是 0xAARRGGBB ， AA 部分指定不透明度。

- `left`: 相对于父窗口客户区的左坐标，设计时坐标。 所以控件的创建参数中指定的是设计时坐标，运行时坐标会根据设计时坐标计算出来。aardio 窗口支持自动缩放与自动调整坐标，默认会根据系统 DPI 缩放设置自动调整窗口与控件的位置和大小，
- `top`: 相对于父窗口客户区的上坐标，设计时坐标
- `right`: 相对于父窗口客户区的右坐标，设计时坐标
- `bottom`: 相对于父窗口客户区的下坐标 ，设计时坐标
- 固定边距字段：
	* `dl`: 是否固定与父窗口左侧的边距并保持不变。
	* `dt`: 是否固定与父窗口上侧的边距并保持不变。
	* `dr`: 是否固定与父窗口右侧的边距并保持不变。
	* `db`: 是否固定与父窗口底侧的边距并保持不变。

	aardio 窗口上的控件在默认会在窗体缩放时自动调整坐标位置（默认并不会调整控件大小），因此在默认情况下控件的边距并不是固定不变的。

	如果将控件的属性 `dl`，`dt`，`dr`，`db` 指定为 `1` 表示固定边距，指定为 0 到 1 范围的小数表示保持百分比的相对固定，不指定则不固定边距,注意这几个字段的值必须是数值（不能指定为布尔值）。

	可在窗体设计器的右键菜单中点击「九宫格缩放布局」自动设置所有控件的 dl,dt,dr,db 字段的值。「九宫格缩放布局」是指将界面划分为「4 角 + 4 边 + 中间」共计九个方形格子，控件哪侧的边位于哪则的方格内则该边固定边距，例如控件的左侧边位于左侧的格子内则左边距固定，其余类推，使靠边的内容固定，中间的内容自由拉伸，缩放自然。

	也可以在初始化或运行时使用 `anchors` 属性获取或设置固定边距，例如 `winform.edit.anchors = {left=1;top=1;right=1;bottom=1}` 等价于将  winform.edit 控件的 `dl`，`dt`，`dr`，`db` 都设为 1 。

- 自适应大小字段： 
	* `aw`: 是否跟随窗体缩放自动缩放控件宽度。
	* `ah`: 是否跟随窗体缩放自动缩放控件高度。

	`aw` 或 `ah` 指定为 `1` 或 `true` 则启用缩放，不指定则默认不会跟随窗体缩放改变控件大小。
- `notify` 是否响应事件。static,plus,picturebox 这 3 个控件默认为静态控件不响应事件，在初始化属性中指定　`notify=true` 可允许这些控件响应事件，在窗体设计器中双击这些控件也会自动设置 notify 属性的值为 true 。plus 控件在调用 skin 函数时也会自动启用此属性。
- `cls`: 所有控件都必须指定 cls，cls 对应的是 win.ui.ctrl 名字空间的类名。例如 `cls="button"` 就指定调用 win.ui.ctrl.button 控件类创建一个按钮控件。cls 被称为设计时类名，控件在创建以后会生成一个 className 属性用于记录运行时类名，运行时类名通常不一样，例如 win.ui.ctrl.tab 创建以后运行时类名为 "SysTabControl32"，"SysTabControl32" 实际上是操作系统提供的系统控件类名。

	aardio 标准库在 win.ui.ctrl 名字空间提供了以下常用控件类，这些控件类的名称可作为控件设计时类名指定为控件设计时属性 cls 的值：

	* "custom": 自定义控件，可用于加载其他窗体或者作为其他窗窗体的容器，也是在窗体设计器中是唯一可更改 cls 属性为其他控件类的控件。[使用指南](ctrl/custom.md)
	* "plus" 高级图像控件，此控件在 aardio 中最为常用。除了可以显示 jpg、透前背景的 png 图像、动画 gif，还可以用 plus 控件创建静态图片框、动画播放控件、按钮、透明按钮、不规则按钮、复选框、超链接、组合框、进度条、扇形进度条、滑块跟踪条、选项卡、弹出菜单、下拉框...... 并且 plus 控件可以调用 skin 函数更方便地自定义外观样式。aardio 中常用的高级选项卡（ win.ui.tabs ） 也是基于 plus 控件。 要注意 plus 控件除了在创建控件的初始化参数中的设计时属性 `bgcolor`,`color`,`forecolor` 字段使用  `0xBBGGRR` 格式的颜色值，其他属性和方法（例如配置外观样式的 skin 函数）里都只支持 GDI+ 的颜色格式 `0xAARRGGBB`（ AA 为不透明度）。[指南](ctrl/plus.md)
	* "tab": 简单选项卡控件，样式比较简单。 [范例](../../../../example/Windows/Tab/TabControl.html) 。如有更多需求可改用高级选项卡（  [win.ui.tabs](tabs/_.md) ）。
	* "edit": 文本编辑框，可通过 text 属性读写文本。
	* "richedit": 富文本编辑框，支持透明背景，可通过 text 属性读写普通文本，rtf 属性读写 RTF 格式文本。 [范例](../../../../example/Windows/edit/CHARFORMAT2.html)
	* "static": 静态文本控件
	* "button": 控钮按件
	* "radiobutton": 单选控件 ， checked 属性表示是否选中。
	* "checkbox": 复选控件 ，checked 属性表示是否选中。 
	* "combobox": 组合框控件，items 属性可获取或设置一个字符串数组（列表数据）。[范例：自动完成输入效果](../../../../example/Windows/Dropdown/autoComplete.html)
	* "listbox" 列表框，items 属性可获取或设置一个字符串数组（列表数据）。[范例](example/Windows/Listbox/ListboxDemo.html)
	* "listview": 列表视图，items 属性返回或设置包含所有列表项的数组，数组的每个项表示一行，每行也必须是包含列文本的数组，返回数组有一个可选字段 checked 包含了所有勾选的行。[范例](../../../../example/Windows/ListView/listview.html)
	* "checklist": 这是基于 listview 控件实现的带复选框的列表框。 [范例](../../../../example/Windows/ListView/checklist.html)
	* "treeview": 树视图控件 [范例](../../../../example/Windows/TreeView/treeview.html)
	* "splitter": 拆分条控件。使用控件的 split 函数拆分多个控件，参数也可以是控件数组，每个控件数组必须包含位于拆分条同一侧的控件。 [范例](../../../../example/Windows/Controls/splitter.html)
	
	* "syslink": 这个控件的 text 文件属性支持 HTML 的超链接语法，可以包含普通文本或多个超链接。 [范例](../../../../example/Windows/Controls/syslink.html)
	* "atlax": 用于创建 COM 控件，创建时在参数表的 text 字段指定类名。 [范例](../../../../example/COM/Advanced/InkEdit.html)
	* "calendar": 日历控件，time 属性用于获取或设置 time 对象。
	* "datetimepick": 日期时间控件，time 属性用于获取或设置 time 对象。
	* "ipaddress": IP 地址控件，text 属性为 IP 地址文本格式，address 属性为 IP 地址数值格式。
	* "hotkey": 热键控件，value 属性用于获取或设置设置，值为表示热键配置的数组。
	* "picturebox": 图像控件，image 属性设置图像路径或图像数据，不支持 png 或动画 gif 。
	* "progress": 进度条控件。 [范例](../../../../example/Windows/Controls/scroll.html)
	* "spin": 微调按钮，可滚动选择数值，buddy 属性设置、获取伙伴 edit 控件。 [范例](../../../../example/Windows/Controls/scroll.html)
	* "vlistview": 虚表控件。 [范例](../../../../example/Windows/ListView/vlistview.html)
	* "trackbar": 跟踪条控件。 [范例](../../../../example/Windows/Controls/scroll.html)
	* "thread": 线程控件，[范例](../../../../example/aardio/Thread/threadCtrl.html)
	* "close": 这是一个由 plus 控件实现的悬浮（orphanWindow）关闭按钮。
	* "bk": 纯静态背景控件，这是不创建实际的窗口的无句柄控件。仅用于填充所在区域的窗体背景，使用 GDI 绘图。适合放在其他控件后面作为装饰。
	* "bkplus": 高级背景控件，无句柄不创建实际的窗口。作用与 bk 控件相同，但是使用 GDI+ 接口绘图。 [范例](../../../../example/Graphics/drawBackground.html)

	aardio 会自动导入以上由标准库提供的控件类。在将程序发布为 EXE 执行文件时，aardio 会检测所有创建窗口控件时指定的 cls 属性，并自动移除未使用的控件类库。

	除此之外的其他控件类则必须先导入才能使用。例如调用 `import win.ui.ctrl.pick` 导入[取色或调色控件](../../../../example/System/Settings/sysColor.html) 以后就可以在创建控件时将 `cls` 的值指定为 "pick" 以创建取色或调色控件。

### 4. winform.add 函数的返回值：

虽然 winform.add 有返回值，但是我们通常不使用 winform.add 函数的返回值。

winform.add 的返回值与传入参数的结构相同，如果传入的是 controlsPropertiesTable 则返回的与是包含控件名值对的表，只不过对应控件名字的值变成了创建好的控件对象。

应当通过 winform 上的控件名称去访问控件，例如 `winform.button1`，`winform.edit1` ，这样对开发环境更友好，可以获得更好的智能提示与代码补全帮助。

## 五. 创建窗口的高级用法与隐藏参数

win.form 构造参数与 winform.add 有一些不常见的用法与隐藏参数，可参考 aardio 自带的「范例 » Windowns 窗口 » 基础知识」 ：

- [创建窗口隐藏参数](doc://example/Windows/Basics/win.form.aardio)
- [运行时动态创建控件](doc://example/Windows/Basics/win.form.add.aardio)

以下与位置有关的属性仅适用于运行时创建控件，不能用于窗体设计器（`/*DSG{{*/ ... /*}}*/` 标签包围的代码）:

- width: 指定控件的设计时宽度（缩放前宽度）。
- height: 指定控件的设计时高度（缩放前高度）。
- marginLeft: 控件的设计时固定左边距（随 DPI 缩放，但不会随窗体大小缩放），小数表示父窗口宽度的百分比。
- marginRight: 控件的设计时固定右边距（随 DPI 缩放，但不会随窗体大小缩放），小数表示父窗口宽度的百分比。
- marginTop: 控件的设计时固定顶边距（随 DPI 缩放，但不会随窗体大小缩放），小数表示父窗口高度的百分。
- marginBottom: 控件的设计时固定底边距（随 DPI 缩放，但不会随窗体大小缩放），小数表示窗口高度的百分比。

创建控件的所有位置参数都是可选的，任何一边的位置未指定则固定边距为 0, 4 边都不指定则铺满父窗口的客户区。

🅰 示例代码：

```aardio
import fonts.fontAwesome;
import win.ui.mask;
import win.ui;

var winform = win.form(text="示例"); 
var frmMask = win.ui.mask(winform);

//默认铺满客户区
frmMask.add({
	title={cls="plus";font=LOGFONT(h=-35;name="FontAwesome")}
})

//显示沙漏动画
frmMask.title.disabledText = [text="请稍候 ……",'\uF254','\uF251','\uF252','\uF253','\uF250'];

//显示遮罩
frmMask.show(); 

winform.show();
win.loopMessage();

```

## 六. 创建窗口示例 <a id="example" href="#example">&#x23;</a>


```aardio
//自 win.ui 库中导入 win.form 窗口类
import win.ui;

/*DSG{{*/
	
//创建窗口
var winform = win.form(text="窗口标题不要省略";right=763;bottom=425)

//创建多个控件应当仅调用 winform.add 一次
winform.add({//如果函数的参数只有一个表对象，可省略名值对外层的 {}  。
	
	//指定访问控件的名字为 winform.button1 ，不可省略
	button1={
		cls="button";// 指定用类 win.ui.ctrl.button 创建按钮控件，不可省略
		text="点击我";// 按钮上的文字
		left=556;top=367;// 按钮左上角坐标
		right=689;bottom=407; // 按钮右下角坐标
		z=1 // 文本框左上角坐标
	};
	
	//指定访问控件的名字为 winform.edit1 ，不可省略
	edit1={
		cls="edit";// 指定用类 win.ui.ctrl.edit 创建文本框控件，不可省略
		text="输入文本";// 文本框内的文字
		left=9;top=10;// 文本框左上角坐标
		right=752;bottom=351;// 文本框右下角坐标
		multiline=1;//多行文本框
		z=2 //Z序，可省略
	}
})

/*}}*/

// 设置按钮的点击事件处理程序，winform.button1 也就是 winform.add 的参数中创建的名为 "button1" 的控件。
winform.button1.oncommand = function(id, event) {
    // 从 1 循环到 10 的数字输出到 edit 控件
    for(i=1; 10; 1) {
        //注意 edit 以 '\r\n' 换行，而 richedit 以 '\n' 换行
        winform.edit1.appendText(i,'\r\n'); //winform..edit1 也就是 winform.add 的参数中创建的名为 ".edit1" 的控件。
    }

	//禁用按钮并显示等待动画（在按钮文本前面循环显示以下字符图标）
	winform.button1.disabledText = ["✶","✸","✹","✺","✹","✷"]

	//延时 3000 毫秒（3 秒）秒后执行下面的函数
	winform.setTimeout( 
	
		function(){
			
			//取消禁用，并恢复禁用前的外观与按钮文本
			winform.button1.disabledText = null;
		},1000)
};
	
//显示窗口
winform.show();

//启动界面线程消息循环
win.loopMessage();
```

这个代码示例首先自 `win.ui` 库导入了 `win.form` 类，然后创建了一个带有指定标题的 winform 窗口。接着，它在该窗口上添加了一个按钮和一个文本框。当按钮被点击时，会从 1 循环到 10 的数字，每个数字后跟一个换行符，然后输出到文本框中。最后，显示窗口并进入消息循环，以便响应用户的操作。

用户在点击按钮时会回调 winform.button1.oncommand 事件函数，请参考 [aardio 范例：响应控件命令](/example/Windows/Basics/command.md)


## 七. 事件处理：命令、通知、消息回调

- 控件的 oncommand 事件用于处理控件自身发送给父窗体的 _WWM_COMMAND 命令消息。
edit，combobox，listbox，button 等 Windows 标准控件使用 oncommand 处理事件。
- 控件的 onnotify 事件用于处理控件自身发给父窗体的 _WM_NOTIFY 通知消息。
treeview,listview 等 Windows 通用控件使用 onnotify 处理事件。
- 所有窗体或窗口控件都可以在添加 wndproc 处理窗口消息回调，wndproc 可以指定函数，也可以指定表对象（表中的为消息 ID，对应的值为函数对象）。如果指定函数，则多次赋值 wndproc 只会添加消息回调不会覆盖之前添加的消息回调函数。

	示例：

	```aardio
	import win.ui;
	/*DSG{{*/
	var winform = win.form(text="aardio form")
	/*}}*/

	winform.wndproc = function(hwnd,message,wParam,lParam){
		select( message ) { 
			case 0x205/*_WM_RBUTTONUP*/{
				//鼠标右键弹起,下面获取坐标
				var x,y = win.getMessagePos(lParam);
				
			}
			else{
				
			}
		} 
		
		//如果没有返回值就继续调用默认函数
	}

	winform.show();
	win.loopMessage();
	```

另外，不同的控件也定义了不同的事件，例如定义了 listview,treeview 等定义了 onSelChanged 等事件。而 plus 控件则通过标准库 win.ui.tracker 支持 "onMouse" 前缀的一系列鼠标事件。

## 八. 自定义控件与扩展控件

### 1. 自定义控件

在窗体设计器上拖放一个自定义控件（ custom 控件 ），然后可以在 IDE 右侧的窗体属性面板中修改控件类名。

custom 控件的类名可以修改为两种类型的值：

- 修改为 win.ui.ctrl 名字空间的控件类名。  
例如我们创建了一个 win.ui.ctrl.myControl 的控件类，那么控件类名就可以修改为 "myControl"。
- 修改为加载其他 aardio 子窗口的代码文件路径。
例如 `/.res/frmChild1.aardio` 。

请参考 [自定义控件使用指南](ctrl/custom.md)

### 2. 扩展控件

以添加到 custom, static 的控件窗口作为宿主窗口，使用其他类库扩展功能。

下面是使用 web.view 库扩展 static 控件使其显示 Lottie 的例子：

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form(text="Lottie 动画";right=757;bottom=467;bgcolor=0xFFFFFF)
winform.add(
static={cls="static";left=42;top=42;right=451;bottom=273;bgcolor=0xFFFFFF;z=1}
)
/*}}*/

import web.form;
var wb = web.form(winform.static);

wb.html = /**
<!doctype html>
<html>
<head> 
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <style>html,body{ height:100%; margin:0; overflow:hidden } </style> 
    <script src="https://lib.baomitu.com/lottie-web/5.10.0/lottie.min.js"></script> 
</head><body><script>
lottie.loadAnimation({
  	container: document.body,
  	renderer: 'svg',
  	loop: true,
  	autoplay: true,
  	path: "https://download.aardio.com/demo/lottie/1.json" 
});
</script>
**/

winform.show();
win.loopMessage();
```

web.view 基于强大的 WebView2 浏览器控件。如果较为简单的网页也可以使用 web.form，基于更轻量的进程内 COM 控件,但 web.form 仅支持兼容 IE 的网页。

传统控件的样式较为简单。用 aardio 提供的 plus 控件则可以代替很多传统控件的功能，并且可以使用 siin 函数更方便的配置更好看的外观样式。如果需要更丰富的外观定义功能，则可以使用 web.view 等浏览器组件扩展 custom, static 等控件窗口使其支持加载并显示网页，在 aardio 中网页中的 JavaScript 与本地 aardio 代码交互非常方便。

## 九. 无边框窗口 <a id="frameless" href="#frameless">&#x23;</a>

win.form 的构造参数可使用 border 字段指定边框，示例：

```aardio
var winform = win.form(text="创建无边框（无标题栏）窗口";border="none")
```

border 可指定的值如下：

- "none"  创建无边框窗口，这种窗口不会有默认的边框，也不会有标题栏。
- "resizable" 创建可拖动改变窗口大小的边框，这是默认设置，不需要显式指定。
- "dialog frame" 创建对话框窗口边框，不能拖动改变窗口大小。此选项仅适用于包含标题栏的窗口。
- "thin" 窗口显示细边框，不能拖动改变窗口大小。此选项仅适用于无标题栏的窗口。

创建无边框窗口的目的通常是为了自己定制标题栏与边框，aardio 提供了一个简单的示例库 win.ui.simpleWindow 。

用法演示：

```aardio
import win.ui;

//创建无边框（无标题栏）窗口，省略了宽高与位置参数并使用默认值
var winform = win.form(text="aardio form";border="none";bgcolor=0xFFFFFF)

//添加深色的标题栏背景（ bk 是仅用作背景绘图的无句柄窗口）, marginRight 字段指定控件的右边距为 0 以代替固定的 right 字段值。
winform.add(
	bk={cls="bk";bgcolor=0xA4A0A0;left=0;top=0;bottom=32;marginRight=0;dl=1;dr=1;dt=1;z=1}
)

//添加阴影边框、标题栏等，默认为透背背景，标题栏上最大化、最小化、关闭按钮等默认字体为白色
import win.ui.simpleWindow;
win.ui.simpleWindow(winform);

winform.show();
win.loopMessage();
```

win.ui.simpleWindow 创建了一个简洁美观的标题栏，并创建了最大化、最小化、关闭窗口的按钮，按住标题栏可能拖动。同时也会在窗体周围创建阴影边框，支持拖动窗口边框改变窗体大小。默认情况下 win.ui.simpleWindow 创建的标题栏背景是透明的，标题栏按钮默设也是透明背景与白色字体。我们可以用 bk 或 bkplus 控件绘制深色的标题栏背景。bk 或 bkplus 控件并不会创建窗口，只是在父窗体上绘图，所以性能较好，也更适合在上面重叠放置其他控件。

也可以通过 win.ui.simpleWindow 对象自定义标题栏按钮配色，示例：

```aardio
import win.ui;
var winform = win.form(text="aardio form";border="none";bgcolor=0xFFFFFF)

import win.ui.simpleWindow;
var sw = win.ui.simpleWindow(winform);

//标题栏按钮都是 plus 控件，skin 函数用法与 plus 控件相同，颜色使用 GDI+( 0xAARRGGBB ) 格式的值。
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

**无边框窗口都应当添加自定义的标题栏和边框，最简单的方法就是直接使用标准库写好的 win.ui.simpleWindow**。

需要注意的是，win.ui.simpleWindow 是直接在父窗口上添加控件，标题栏下面可以放静态控件（例如 static,bk,bkplus ）控件。
但如果放会抢占输入焦点的控件则标题栏可能无法操作，典型的例如垂直选项卡界面在右侧放一个填满窗口的 custom 控件就会有这样的问题。这时候应当将 win.ui.simpleWindow 改为 win.ui.simpleWindow3 , 其他用法不变。win.ui.simpleWindow3 会创建 orphanWindow 悬浮窗口来放标题栏（实际是伪装为子窗口的独立浮窗），不受底部控件的影响。而且 win.ui.simpleWindow3 默认会显示渐变色背景（可选用 background 属性指定线程渐变的顶部颜色，foreground 指定线形渐变的底部颜色， 0XAARRGGBB 格式）。

请参考： 

- [无边框窗口范例](../../../../example/Windows/Basics/frameless.html)
- [web.view - 网页界面无边框窗口范例](../../../../example/WebUI/web.view/frameless.html)