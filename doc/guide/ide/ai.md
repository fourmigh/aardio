
# AI 编程指南

使用 aardio 调用 AI 遇到问题，请先思考：

> 如果不限制软件体积大小，没有种种要求限制，用相同的提示词，任选一个编程语言，能否用 AI 完成相同的任务？暂不考虑皮肤是否更华丽和好看，请任选一个 aardio 之外的编辑器，任选一个除 aardio 之外的编程语言，能否替代 aardio 更省时省力高效地开发 GUI（图形界面）的桌面软件？

如果回答是也不能，那么请继续向下阅读，让我们一起学习一些简单的技巧。

> 工具再好也是别人的，技巧与知识才是属于自己的。

## AI 工具

aardio 是一个支持 AI 的可视化桌面软件集成开发工具，可以方便地构建图形用户界面（GUI）。

aardio 自语法层级到文档、范例、开发环境都针对 AI 进行了持续地深度优化，使用 aardio 开发环境内置的 AI 助手，并使用 [aardio 官方针对 aardio 优化的 AI 接口](https://aardio.com/vip/)，可显著提升用 AI 编写 aardio 代码的质量与效率。

> 注意：aardio 官方 AI 接口，带有 `aardio` 前缀的模型 ID 会接入 aardio 知识库（ 忽略联网搜索选项 ），带有 `:online` 后缀的模型 ID 自带联网搜索功能。

-  『 aardio 工具 » 问 AI 』可用于问答对话，点【设置】按钮可添加或修改 AI 接口参数。

    <img alt="设置 AI 接口" src="https://aardio.com/zh-cn/doc/images/api-keys.jpg" width="500">

- 在 aardio 代码编辑器中按 `F1` 键可激活 AI 编码助手，它有四种工作模式：
	
	* 文档查询模式：如果已选中库、库函数、关键字、特殊符号，则会自动打开相关文档
    * 代码分析模式：如果选中其他类型代码，则会自动打开"问 AI"助手会话界面，并自动生成提示词
    * 智能纠错模式：在开发环境中运行代码报错后 30 秒内，回到代码编辑器立即按 `F1` 键，无选区则调用 AI 智能纠错。
    * 代码续写模式：如果按 `F1` 时无选区，AI 编码助手将帮您续写或补全代码

    <img alt="F1 键编码助手" src="https://aardio.com/zh-cn/doc/images/fim.gif" width="500"> 
 

## 去掉幻觉

这里的去掉幻觉不是指去掉 AI 的幻觉，而是使用 AI 时我们自己应当去掉不切实际的幻想。

AI 写 Python 和前端确实不错，除了更多的用量与训练数据以外，人家 Python 是亲生的，前端先天就适合模板式生成。当然这个不错和很好也只是相对的，稍复杂一点、需要特殊定制的程序单靠 AI 完成也会很困难。

对其他编程语言的 AI 支持不要有过高的预期，尤其是原本就极端复杂的桌面开发环境。

能对 AI 的支持做到 aardio 这个程度的其实并不多见。aardio 可能是唯一从语言、标准库、开发环境到文档、范例都针对 AI 主动进行大量优化的编程语言。aardio 的全部文档都针对 AI 重写过一遍。而且 aardio 编写的程序上下文极短、代码量极少，这都是先天对 AI 友好的。

通过 aardio 专用的 AI 接口，AI 也可以掌握完整的、最新的 aardio 知识。

aardio 会继续努力优化 AI 编程体验。但如果只是片面强调工具的因素，而忽视了提升自己使用 AI 的技巧，这是非常可惜的。

> 工具再好也是别人的，技巧与知识才是属于自己的。

## 代码注释更加重要

写代码注释的同时也是在写 AI 提示词。

良好的注释可以：

- 指导 AI 更好地理解你的意图
- 提供上下文信息，帮助 AI 生成更准确的代码
- 作为 F1 键续写功能的重要参考

## 提示词并不总是越多越好。

与 AI 交流时，应避免冗长、模糊的描述，而是使用简洁、明确的指令。

凌乱、带有歧义、思路不清晰的提示词会误导 AI 的方向，并降低回复质量。

提示词中对提升 AI 生成内容没有帮助的内容应当移除。

## 最近优先原则

对于【问 AI】聊天对话助手，最后一次发送的消息最重要。

对于  `F1` 键代码补全助手，前置代码中临近输入光标的最后一行注释最为重要。

## 提供更多可以逼近最佳答案的关键词

我们在使用 AI 时常常会犯一个常见的错误：

AI 不再只是简单地使用关键词去查找知识 —— 让我们过度高估 AI 的智慧 ，并误以为关键词不再重要。  
由此我们错失了一个廉价并且重要的技巧：通过提供更多更好的关键词让 AI 更加逼近正确的答案。

举例有一个这样的用户问题：

> 用浏览器打开网页时， 如何获取 HTTP 请求头 ？

对上面的提示词，AI 可能会给出完全不相关的答案。

我们尝试修改上面的问题，加入更多我们已知的关键词，给 AI 提供更多逼近问题的线索，并尝试更清晰地描述我们的问题并避免歧义：

> aardio 如何实现用浏览器控件自动打开网页的同时用 CDP 抓取网页的请求头等信息，类似在浏览器里按 F12 可以监听到网络请求的 HTTP 请求头。

这时候 AI 给出了高质量的回复，并且直接生成了可运行的代码。

如果我们并不了解 CDP 协议这也没有关系，我们去掉 “用 CDP 抓取” 的提示词如下：

> aardio 如何实现用浏览器控件自动打开网页的同时抓取网页的请求头等信息，类似在浏览器里按 F12 可以监听到网络请求的 HTTP 请求头。

AI 仍然给出了完美的答案，并且在回复中说明了应该使用 CDP 协议。所以我们并不是必须在提示词里包含什么提示词，但你至少应当尽可能地提供正确的关键词与线索。

## 将复杂的问题分解成简单的小问题，分步完成，不要试图一步到位

新手常见的一个错误就是写一大堆要求给 AI 然后直接等答案。  
其实有时候我们可能低估了实现这些需求的复杂度，有时候可能要上千句代码才能完成任务。

AI 只是一个程序，资源总是有限的。  
用户提出的一个问题中包含的要求和任务越多，AI 的注意力就会越分散，回复的质量就会越差。

更好的方法是将大问题分解为多个小问题，逐步解决。  
无论是使用 AI 还是编程，都要有摸石头过河的心态，有耐心地逐步探索前进。

例如问题： `读取 …… 环境变量并且执行 …… 操作` 可能导致 AI 生成答非所问的低质量回复。

我们作如下优化，将一个问题分为两个更小的问题：

1. 先发给 AI 问题：`读取操作系统的环境变量中的 ……` 。  

2. 接上文继续提问：`使用上述环境变最 …… 执行 …… 操作`。

通过分解任务并逐步引导 AI 解决更小的问题，对单个任务提供更具体的描述并补充关键词，可以显著改善 AI 生成的回复与代码质量。

## 新建对话

最好不要在同一个上下文中连续问完全不相关的问题，这会导致 AI 处理与问题完全不相关的上下文，浪费 tokens ，使速度变慢，生成的代码与内容质量下降。还可能会因为上下文堆叠过多，输入超出大模型上限导致请求出错。 

对于全新的问题，最好点击【清除】按钮以清除无关的上下文，以创建新的会话。如果需要保存原对话可右键点击提示词输入框，在弹出的右键菜单内点击【导出对话】。

## 设置合适的 temperature 参数 <a id="temperature" href="#temperature">&#x23;</a>

大模型的 temperature 参数需要指定 0~1 范围的值，这个值越低则越倾向于生成更准确更可靠的内容。然而总是生成最准确最可靠的内容就会失去多样性，通过调高 temperature 的值就可以更多容忍 AI 随机生成可能不是最准确最可靠的内容，这增加了不确定性，幻觉、错误、虚构的概率也会随之增加，但同时也增加了 AI 的自由度和创造力，有利于生成更丰富多样的内容。

对于编程这种严谨并重视精确性的任务，一般建议使用较低的 temperature，例如 0 或者 0.1 。  
如果设置较高的 temperature 则 AI 很可能会更多地胡编乱造，虚构各种库、函数、甚至是天马行空地生造语法。

但是对于一些大模型，或者特定的编程任务，适度调高 temperature 及最大回复长度（ max tokens ）可以生成更具创造力的内容，并不会导致过多的错误和虚构成份。可以尝试不同的 temperature 值以获得更多的可能性。

注意即使 temperature 设为 0，AI 仍然会存在较低的随机性，对于同样的问题并不一定总是会精确地回复完全相同的内容。

要点：

- 如果 AI 生成的代码较多错误，请降低 temperature，或者将 temperature 设为 0 。
- 如果适度调高 temperature 可以让 AI 生成更理想的代码，并且不会带来更多的错误，可尝试调高 temperature 。
- 不要将无法调整 temperature 参数的 AI 应用于编程任务，建议改用开发环境自带的专用于编程的 AI 助手，例如 aardio 自带的 AI 助手。
- 一般不建议改动大模型其他参数的默认值，例如 top-p。

## 使用 Markdown 语法与 AI 交互

AI 对话时默认会使用 Markdown 格式，这种结构化的文本格式对 AI 更友好。Markdown 语法非常简单，也是用好 AI 必备的基本功。

要特别注意不要把编程代码直接发给 AI ，应当将编程代码包含在 Markdown 代码块标记内部，例如：

``````markdown
```aardio
for(i=1;10;1){
	print(i);
}
```
``````

- 代码块以连续 3 个以上的反引号开始，并以相同数目的连续反引号结束。
- 开始与结束的反引号都必须独立一行，前后都有换行。
- 在开始标记后建议注明编程语言的名称。

在 aardio 编辑器内选中代码，  
右键菜单内点击 `复制到文档` 或按 `Ctrl+Insert` 快捷键可快速复制 Markdown 格式代码块。

更简单的方法是在编辑器内选中代码，然后直接按 `F1` 键调用 AI 助手查看选中代码。

## 如何避免 AI 知识断片、答非所问

当用户向 AI 提出问题时，
AI 首先会快速获取与问题有关的知识，然后再生成回复内容。
但有时候用户的问题并不直接与答案有关，这会导致第一次快速获取的知识关联性不大，回复可能会答非所问。

那么为什么我们不让 AI 总是先深度分析提示词，
找出用户的真正意图，再去调用搜索工具，每次都生成更完美的回复呢？

这是因为：

- 让 AI 调用搜索工具实会发生多次请求，对于大多数普通的问题这样很浪费资源而且慢。
- 大多时候 AI 会自动判断是否需要调用搜索工具，而这个判断很多时候都是不调用，原因参见上一条。
除非用户在提示词中明确的给出了“搜索”、“查找”这些指令。
- 不是所有大模型都支持搜索工具。

更好的方法是：

1. 先让 AI 解释一下问题, 或者明确的指示 AI 查找什么知识。
2. 得到更具体的方向以后，再明确地问具体的什么功能用 aardio 如何实现。

请将 AI 当作工具，在 AI 发生知识断片时，只要简单地引导一下让断片的知识发生连接就可以获得满意的答案。

虽然在 AI 时代提问的难度与获得满意答案的难度大幅降低，但是否掌握关键技巧的区别还是很大的。AI 拥有海量的知识，但这些知识是否能为你所用，你提问的步骤与使用的关键词至关重要。

## 避免使用内容空洞的提示词

| 错误提示词 | 正确提示词 |
| --- | --- |
| 代码有问题 | 具体说明代码有什么问题，提供错误信息 | 
| 重新写 | 具体说明你希望改变什么。检查一下提示词，尝试更好地描述需求，补充更多信息 |
| 不行 | 具体说明哪里不行 |
| 还是不行 | 具体说明哪里不行 |
| 出错了 | 具体说明出了什么错误 |
| 又出错了 | 具体说明出了什么错误 |
| 界面不好 | 具体说明哪个控件不对，哪里不好了 |
| 还是没成功 | 在代码中添加 console.log，debug.log 等检查代码运行时状态与相关数据，分析与收集信息并反馈给 AI |

“提示词” —— 是要给 AI “提示”，如果 AI 生成的代码报错或者未达到预期效果：

- 尽可能补充相关信息，收集相关运行时信息，提供更多有意义的关键词。向 AI 反馈具体遇到了什么错误，把错误信息发给 AI 。给 AI 的线索越多，回复质量就会越好。
- 可以尝试在提示词中补充更多可以引导 AI 调整思考路径，逐渐逼近解决方案的信息。例如这样写提示词 “ …… 上面的代码报错了，出错的代码是 …… 错误信息是 …… 我怀疑这个错误可能与多线程有关，你认为呢？是否可以尝试 …… ”

## 在 aardio 中嵌入其他编程语言

aardio 支持大量第三方编程语言，在 aardio 编辑器中也可以调用 AI 编写其他编程语言的代码。

尤其是对于 AI 较擅长的前端代码（ HTML / JS ）、Python、Go 代码等，在 aardio 编辑器中用 AI 写起来都非常方便。但是我们需要注意通过注释或变量命名提示 AI 哪里是其他编程语言的代码。

aardio 调用 Python 示例：

```aardio
import py3; 

//下面是需要执行的 Python 代码
var pyCode = /** 
def getList(a,b):  
    return [a,b,testData] # 返回列表
    #return a,b,testData # Python 多返回值实际是返回一个 tuple
**/

//执行 Python3 的代码
py3.exec( pyCode ) 

//从 py3.main 模块调用 Python 代码定义的函数 
var pyList = py3.main.getList(12,23);
```

在上面的 aardio 示例中，通过变量命名与注释明确了 pyCode 内存放的是待执行的 Python 代码，也说明了 aardio 代码与被调用 Python 函数的关系。这时候如果我们在 pyCode 内部按 F1 键，AI 就会改用 Python 语法写代码，并且会考虑与 aardio 的交互关系。

![aardio 调用 AI 编写 Python 代码](https://aardio.com/zh-cn/doc/images/fim-py.gif)

再例如下面的 aardio 调用网页的示例：

```aardio
import win.ui;
/*DSG{{*/
var winform = win.form(text="WebView2";right=966;bottom=622)
winform.add()
/*}}*/

import web.view;
var wb = web.view(winform);
	
wb.external = {
	log = function(str){ 
		winform.msgbox(str)
		return str;
	};
}

//下面是 HTML 代码
wb.html = /********

********/

winform.show();
win.loopMessage();

```

通过注释与 `wb.html` 这个意图明确的变量命名，让 AI 明确 wb.html 应当写 HTML 代码。

当我们在 wb.html 内按 `F1` 键时，AI 就会写出正确的 HTML 代码，并且会给出调用 aardio 函数 external.log 的 JavaScript 示例代码。

只要了解了上面的方法，在 aardio 里调用 AI 写其他编程语言的代码是非常方便的。

## 更好地投入才能更好地产出

一些新手在使用 AI 时只期望 AI 生成更好的内容，却不明白更好的产出源于更好更多地投入。

去跟 AI 理论“我只是一个小白，完全没有编程基础，你不应当要求我更好地描述需求和问题 …… 你应当生成更好的代码 …… 否则我要你干什么” 是不会有作用的。

任何时候，只有更好地投入才能有更好的产出。所以我们仍然要努力地学习更多的技术知识，更用心地编写提示词。
