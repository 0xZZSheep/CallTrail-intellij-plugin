# CallTrail

[English](README.md) · **简体中文**

**CallTrail** 是一款面向 IntelliJ IDEA、WebStorm、PyCharm、GoLand、PhpStorm、RubyMine、CLion、Rider 和 Android Studio 的**调用图（call graph）**与**调用层次（call hierarchy）**可视化插件。在编辑器里右键函数或方法并选择 *Call Graph…*，即可在弹出的窗口中同时查看向上三层调用者和向下三层被调用者——窗口会停在代码旁边，而不是占用工具窗口。

图里的调用者与被调用者，就是 *Navigate | Call Hierarchy* 里显示的那一批，因此它和 IDE 自己的判断一致。

## 打开调用图

在编辑器里右键点击元素，选择 **Call Graph…**，弹窗就出现在鼠标点击的位置。

## 怎么用这张图

- 调用者画在根元素上方，被调用者画在下方，每一行都标注层号。递归节点只在第一次出现的位置显示一次，递归边直接连回该节点；节点会显示 `Recursion` 标记并使用虚线边框。遍历会在该节点停止，不再沿递归方向逐层重复展开。
- **单击**节点聚焦它——它的邻居保持高亮，其余部分变暗。
- **右键**节点可执行 `Jump to Source`、`Copy Element Name`、`Copy as Mermaid Diagram` 和 `Copy as DOT Diagram`。`Jump to Source`（或选中节点后按 <kbd>Enter</kbd>）不会关闭弹窗，所以可以连续访问多个节点，并保留 IDE 的正常光标位置。
- 复制出来的图是纯文本，可直接贴进 Markdown 文档、PR 描述或 Graphviz 工具；各层会保持成行，画出来的形状和弹窗里一致。
- 想看得比配置的层数更深时，先跳到感兴趣的节点，再在它上面重新打开调用图。
- 弹窗是贴在光标或点击位置旁的浮层。一个节点大约占弹窗宽度的五分之一，所以层级很大时是滚动而不是把整张图缩小：**滚轮**滚动，<kbd>Shift</kbd>+**滚轮**横向滚动，<kbd>Ctrl</kbd>+**滚轮**缩放，拖拽平移，`Fit` 把整张图缩放进弹窗。
- 工具栏上有 `-`/`+`、`Fit`、设置、把弹窗放大到所在屏幕（再按一次还原）的**最大化**按钮，以及关闭按钮。<kbd>R</kbd> 重新计算调用图。
- `Center Selected` 把当前选中节点移回可视区域中央，没有选中节点时回退到根节点；`Filter` 可以隐藏文件节点，或排除 `test`、`src/test`、`**/generated/**` 这类项目目录。过滤规则按项目保存，应用规则时执行与 <kbd>R</kbd> 相同的重新计算。
- 点击弹窗外部不会关闭它，因此可以把它停在编辑器旁边，或拖到另一块屏幕；关闭用工具栏的 ✕（或 <kbd>Esc</kbd>）。*Close the graph popup when clicking outside it* 设置可以恢复"点击外部即关闭"。

## 双屏使用与 Material Theme

弹窗是一个真正的窗口，而不是编辑器里的浮层：按住标题栏就能把它拖到第二块屏幕，然后一边看着图一边继续在 IDE 里干活；点击弹窗外部不关闭它，就是为了这个用法。

**Material Theme UI** 与这个行为冲突。只要它的遮罩开着，*Show Overlays* 功能就会给整个 IDE 盖上一层半透明遮罩，所以 IDE 看起来变暗了。这层遮罩贴在 IDE 的 glass pane 上，因此会把鼠标点击一起挡掉：编辑器不再响应点击，<kbd>Ctrl</kbd>+Click 也无法跳转，直到弹窗关闭——而一个本来就打算常开的弹窗受不了这个。

所以 CallTrail 会在弹窗显示之前，把主题的两个遮罩开关关掉，弹窗关闭时再按原样放回去：*Settings | Tools | CallTrail* 里的 *Turn off Material Theme overlays while the graph popup is open*，默认勾选。开关是通过主题自己的设置读写的，没装主题的 IDE 完全不受影响。如果你把这层保险关掉了，或者仍然残留了一层遮罩（主题自带一个 **Clean Overlay** 动作专门清这个），也可以直接去主题里关：打开 *Settings | Appearance | Material Theme*，在向导（或 **Features** 页）里取消勾选 **Show Overlays**；不想进设置的话，按 <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>A</kbd> 搜索 Overlays，运行 **Toggle Overlays** 或 **Toggle More Overlays**。

## 构图过程中的进度

弹窗页脚会实时显示这次扫描：一条进度条表示已经展开的工作量，还有队列里仍在等待的条目数、目前为止发现了多少、以及每秒处理多少个。耗时很长的图因此能看到它是怎么收敛的，而不是干转一个圈；队列数字还告诉你层次是在继续扩张还是已经开始排空。

## 设置

*Settings | Tools | CallTrail* 可以配置：

- 调用者与被调用者的层数。用户尚未自定义时，Ruby 默认 2 和 2，其他语言默认 3 和 3；在支持 Ruby 的 IDE 中把任一项调到 2 以上会显示性能提示，但不会阻止应用；
- 单张图的节点上限（默认 250）；
- 是否把库（JDK、依赖、`node_modules`……）里的元素纳入图中。默认不纳入，这样图只落在项目源码内，同时让 JavaScript 项目 caller 一侧快很多。作为起点的元素永远都会显示；
- 点击弹窗外部是否关闭它；
- 弹窗打开期间是否关闭 Material Theme UI 的遮罩（默认勾选，也就是默认把遮罩关掉）。

## AI 摘要

单击节点会打开持久、可调整宽度的详情面板。AI 用途摘要显示为文档注释，下面是只读的节点源码；稀疏的 AI 行内说明直接放在对应代码行之后。源码使用平台代码编辑器，带语法高亮、行号和横向滚动；文档注释会在面板宽度内换行，面板宽度会被记住。参数、返回值和实现细节等旧章节已由这种源码局部视图替代。

- 启用 AI 辅助代码理解后，**全景**会先给出整张图的简短概览，再按职责列出语义目录。全景可以由设置控制为打开图时自动生成，也可以从 popup 工具栏按需生成；自动生成默认关闭。点击目录章节会高亮真实子图，并列出相关节点的源码证据；点击“显示完整调用图”可清除高亮。
- 点击图中节点，或点击全景中的节点证据，会切换到 **节点详情**，继续显示该节点的 AI 文档注释和源码行内解释。
- 摘要来自你自己选择的对话 API，任何 OpenAI 兼容端点都行：在设置里填 base URL、API key 和模型（默认 `https://api.openai.com/v1` 与 `gpt-4o-mini`），旁边的 `Verify` 会发一个最小请求验证这些配置。摘要语言也可以配置。
- 打开调用图时，根元素及其直接邻居会立刻生成摘要，所以最先看到的部分先有解释；更深的节点在点击打开详情时才请求。鼠标悬停只显示简短身份信息，不会请求 AI 内容。
- 节点的源码会发送给你配置的那个端点，因为摘要就是根据它写出来的。除此之外不会有任何数据离开 IDE；功能关闭时也不会发任何请求。
- API key 保存在 IDE 设置里（`callHierarchyGraph.xml`），并且不会写入插件日志。

## 环境要求与语言支持

CallTrail 可以在 **2025.2 或更新版本** 的任意 IntelliJ 系 IDE 中运行（如 IntelliJ IDEA、WebStorm、PyCharm、GoLand、PhpStorm、RubyMine 等）。插件仅使用 IDE 的通用 API，无特定产品限制。

根据不同语言，CallTrail 采用两种方式获取调用图（Call Graph）：

- **快速模式（Fast Mode）**
    - **支持语言**：Java、Kotlin、JavaScript、TypeScript、Python、PHP、Go、Ruby。
    - **特点**：使用各语言优化过的调用层次快速构图。调用层次走不到的元素改为走搜索引用（Find Usages）。
- **兼容模式（Compatibility Mode）**
    - **支持语言**：C/C++、Rust、C# 以及 IDE 支持的其他任意语言。
    - **特点**：使用 IDE 的语言调用层次，并以搜索引用（Find Usages）作为后备。Rider C# 使用专用集成，性能接近快速模式。

插件的构建、测试与调试见 [CONTRIBUTING.md](CONTRIBUTING.md)（英文）。

## 许可证

本项目基于 [CallTrail 最终用户许可协议（EULA）](LICENSE.txt) 发布，创建自 [IntelliJ Platform Plugin Template][gh:template]。
