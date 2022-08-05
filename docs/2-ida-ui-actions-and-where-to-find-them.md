[#]: translator: "Always-Y"

2、IDA 用户界面动作及如何找到它们
=======

在上一篇文章中，我们描述了如何使用键盘快速调用 IDA 的一些命令。但有时候，您可能需要多次执行特定的操作，如果没有分配默认的快捷键，重复的点击菜单栏选择相关选项可能会很无聊。即使是快捷键，能做的也十分有限。有时你一开始可能很难发现或找到一个特定的操作（某些动作甚至没有菜单项）。这里有两个 IDA 功能会有所帮助：

### 快捷方式编辑器

通过 Options>Shortcuts... 来查看、添加和修改 IDA 中几乎所有 UI 操作的快捷方式。

![shotcut_editor][1]

对话框是非模式的，并显示了哪些可用于当前视图的操作（当前禁用的视图将被删除），因此您可以尝试单击 IDA 周围，来查看一组可用的操作如何根据上下文进行更改。

要分配快捷方式，请在列表中选择操作，然后在“Shortcut”字段中输入键组合（在 Windows 上您也可以单击“Record”按钮，然后输入所需的快捷键），然后单击“Set”保存为此窗口以及未来的所有 IDA 窗口的新快捷方式。使用“Restore”可以还原此操作，或者使用“Reset”将所有操作重置为默认状态（如`idagui.cfg`中所述）。

### 命令选项板

命令选项板（默认快捷方式为`Ctrl`–`Shift`–`P`）与快捷方式编辑器类似，因为它显示所有 IDA 操作的列表，但您可以简单地调用该操作，而不是更改快捷方式。

![palette_jump][2]

底部的过滤器框使用模糊匹配过滤包含键入文本的操作，并在打开选项板时聚焦，因此您可以只键入操作的近似名称，然后按`Enter`键调用最佳匹配。

--------------------------------------------------------------------------------

via: https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/

作者：Igor Skochinsky
译者：[Always-Y](https://github.com/Always-Y)
校对：[firmianay](https://github.com/firmianay)

[1]: https://www.hex-rays.com/wp-content/uploads/2020/08/shotcut_editor.png
[2]: https://www.hex-rays.com/wp-content/uploads/2020/08/palette_jump.png
