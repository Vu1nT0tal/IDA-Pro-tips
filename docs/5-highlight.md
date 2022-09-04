[#]: translator: "cease2e"

5、高亮操作
=======

在 IDA 中，高亮是光标下的单词或数字以及屏幕上所有匹配的子字符串进行动态着色。在默认配色方案中，黄色背景色用于高亮显示。

当您点击列表中的非空白位置或使用箭头键移动光标时，高亮显示会更新。在以下情况下，高亮显示不会更新（保持不变）：

- 使用 `PgUp`、`PgDn`、`Home`、`End` 移动光标；
- 使用鼠标滚轮或滚动条滚动列表；
- 使用跳转命令或单击导航带（除非光标恰好落在新位置的单词上）；
- 高亮被 LockHighlight 操作锁定（它是少数几个操作之一，默认情况下只能作为工具栏按钮使用）。

[LockHighlight_action][1]

### 寄存器高亮

对于某些处理器，以特殊方式处理高亮显示的寄存器：不仅高亮显示相同的寄存器，还高亮显示包含它或属于它的一部分的任何寄存器。例如，在 x86_x64 上，如果选择 ax，则 al、ah、eax 和 rax 也会高亮显示。

[register_highlight][2]

### 手动高亮

除了通过点击单词/数字自动高亮显示外，您还可以使用鼠标或键盘选择任意子字符串，它将用于高亮显示屏幕上的所有匹配序列。对于手动高亮显示，只有完全匹配的子字符串才会被高亮——对寄存器没有做特殊处理。

[manual_highlight][3]

### 高亮导航

您可以使用 `Alt`–`Up` 和 `Alt`–`Down` 在高亮显示的匹配项之间快速跳转。即使最接近的匹配不在屏幕上，这也有效——IDA 将在选定的方向上寻找下一个匹配。

高亮显示不仅在反汇编列表中可用，而且在大多数基于文本的 IDA 子视图中可用：伪代码、十六进制视图、结构体和枚举。

--------------------------------------------------------------------------------

via: https://hex-rays.com/blog/igor-tip-of-the-week-05-highlight/

作者：Igor Skochinsky
译者：[cease2e](https://github.com/cease2e)
校对：[]()

[1]: https://www.hex-rays.com/wp-content/uploads/2020/09/highlight_lock.png
[2]: https://www.hex-rays.com/wp-content/uploads/2020/09/highlight_reg.png
[3]: https://www.hex-rays.com/wp-content/uploads/2020/09/highlight_manual.png
