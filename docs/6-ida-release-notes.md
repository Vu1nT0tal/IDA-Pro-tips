[#]: translator: "cease2e"

6、IDA 发行说明
=======

对于每个 IDA 版本，我们都会发布详细的版本说明，描述各种新功能、改进和错误修复。虽然一些因为被高亮显示，看起来非常明显，其他的并不那么明显，但可能仍然需要仔细阅读。仔细查看这些发行说明，您会惊讶地发现通过不同的 IDA 版本添加了许多小而有用的功能。

**几个很好的例子可以是：**

### 寄存器定义和使用

在 IDA 7.5 中添加的这些操作允许您在寄存器的各种用途之间快速跳转。

``` text
UI：添加了用于搜索寄存器定义或寄存器使用的操作（Shift+Alt+Up、Shift+Alt+Down）
来自：[IDA 7.5 的新功能](https://hex-rays.com/products/ida/news/7_5/)
```

`Shift`–`Alt`–`Up`：查找选定寄存​​器的前一个被定义（写入）的位置。
`Shift`–`Alt`–`Down`：查找所选寄存器的下一个被使用（读取或部分覆盖）的位置。

这些操作在以高优化级别编译的大型函数中特别有用，其中定义和使用之间的距离可能很大，因此使用[标准高亮显示](https://hex-rays.com/blog/igor-tip-of-the-week-05-highlight/)跟踪寄存器并不总是可行的。

[regdefuse][1]

在上面的屏幕截图中，您可以看到 `Alt`-`Up` 跳转到最近的高亮子字符串匹配，而 `Shift`-`Alt`-`Up` 查找 rbx 更改的位置（`ebx` 是 `rbx` 的低位，因此 `xor` 指令会更改 `rbx`）。

这些操作目前仅针对有限数量的处理器（x86/x64、ARM、MIPS）实施，但如果我们收到更多请求，可能会扩展到其他处理器。

### 跳转到上一个或下一个函数

``` text
+ ui：添加快捷键 Ctrl+Shift+Up/Ctrl+Shift+Down 跳转到上一个/下一个函数的开头
来自：[IDA 7.2 中的新功能](https://hex-rays.com/products/ida/news/7_2/)
```

在 IDA 7.2 中添加，这些是次要但非常有用的快捷方式，尤其是在具有许多大功能的大型二进制文件中。

顺便说一句，如果标准快捷键难以使用，您始终可以使用您喜欢的组合键[设置自定义快捷键](https://hex-rays.com/blog/igor-tip-of-the-week-02-ida-ui-actions-and-where-to-find-them/)。

--------------------------------------------------------------------------------

via: https://hex-rays.com/blog/igor-tip-of-the-week-06-release-notes/

作者：Igor Skochinsky
译者：[cease2e](https://github.com/cease2e)
校对：[]()

[1]: https://www.hex-rays.com/wp-content/uploads/2020/09/regdefuse.png
