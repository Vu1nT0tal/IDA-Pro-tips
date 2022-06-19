[#]: translator: "firmianay"

1、IDA 中鲜为人知的快捷键
=======

今天，Hex-Rays 很高兴地推出了一个特别的博客系列，由 IDA 专家 lgor，来为大家介绍 IDA 的一些有用的小技巧和功能，这些东西对于用户来说可能不太容易知道。

系列博客的第一篇将介绍最有用的快捷键，它们将提高你的 IDA 使用体验和工作效率。

本周的小技巧是关于在 IDA 中使用键盘。现在，虽然大多数操作都可以使用鼠标来完成，但使用键盘还可以更快更高效。IDA 最初就是个 DOS 程序，那时 GUI 和鼠标还没有普及，这就是为什么你仍然可以在不碰鼠标的情况下完成大部分的工作。大多数常用的快捷键都能在 cheat sheet（[HTML][1], [PDF][2]）中找到，但还有一些平时不太知道但很有用的快捷键。

### 文本输入对话框（例如，输入注释、编辑本地类型）

![dlg_textedit][3]

在对话框中，你可以使用 `Ctrl`-`Enter` 来确认（OK），或者使用 `Esc` 来取消（Cancel）。该操作与按钮的排列方式无关（可能受不同平台或主题的影响）。

### 快速导航菜单

如果你在 Windows 上按住 `Alt` 键（或启用一个系统选项），应该能看到菜单选项名称下面的下划线。

![menu_accel][4]

你可以在按住 `Alt` 的同时按带下划线的字母（也称为加速器“accelerator”）来打开该菜单，然后再按下特定菜单项的划线字母来触发它。即使你松开 `Alt`，第二步也会起作用。例如，要执行“Search->Not function”（它没有默认的热键），你可以按 `Alt`-`H`, `F`。虽然在 Linux 或 Mac 上可能没有下划线，但同样的按键序列应该仍然有效。如果你不能使用 Windows IDA，又不想手动挨个试出加速器，则可以查看 `cfg/idagui.cfg` 文件，其中描述了 IDA 的默认菜单布局和所有分配的加速器（前缀为 &）

### 对话框导航

除了 OK/Cancel 按钮，IDA 的许多对话框都有复选框、单选按钮或编辑栏。你可以用标准的 Tab 键在它们之间进行导航，并用空格键进行切换。另外，与菜单类似，IDA 的大多数对话框控件都有加速器快捷键。你可以在 Windows 上使用 Alt 来显示它们，但与菜单不同的是，即使没有 Alt，它们也能工作。例如，要快速退出 IDA，放弃打开数据库后所做的任何修改，可以使用这个键序：

- `Alt`-`X`（或 `Alt`-`F4`）来显示“保存数据库”对话框；
- `D` 切换到“DON'T SAVE the database”复选框；
- `Enter` 或 `Alt`-`K`（或 `K`）来确定（OK）。

![dlg_exit][5]

注意：有几个对话框不能使用此功能，例如 Options->General... 对话框，还有脚本命令（`Shift`-`F2`）或其他带文本编辑控件的对话框。在这些对话框中，你必须按住 `Alt` 键来使用加速器。

--------------------------------------------------------------------------------

via: https://hex-rays.com/blog/igor-tip-of-the-week-01-lesser-known-keyboard-shortcuts-in-ida/

作者：Igor Skochinsky
译者：[firmianay](https://github.com/firmianay)
校对：[firmianay](https://github.com/firmianay)

[1]: https://www.hex-rays.com/wp-content/static/products/ida/idapro_cheatsheet.html
[2]: https://www.hex-rays.com/wp-content/static/products/ida/support/freefiles/IDA_Pro_Shortcuts.pdf
[3]: https://www.hex-rays.com/wp-content/uploads/2020/07/dlg_textedit.png
[4]: https://www.hex-rays.com/wp-content/uploads/2020/07/menu_accel.png
[5]: https://www.hex-rays.com/wp-content/uploads/2020/07/dlg_exit.png
