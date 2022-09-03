[#]: translator: "spwpun"

4、更多的选择操作
=======

在上一篇文章中我们介绍了 IDA 中的选择操作的基本用法。这周我们将介绍一些选择操作相关的例子。

### 固件/原始二进制分析

当反汇编一个原始二进制文件时，IDA 并不总是可以检测出代码片段，你必须得使用试错法来在整个加载范围中查找代码片段，这可能是一个耗时的过程。在这种情况下，下面的方法可用来做初始化的分析：

1. 使用快捷键定位到数据库起始地址（`Ctrl`–`PgUp`）；
2. 开始选择（`Alt`–`L`）；
3. 直到结束（`Ctrl`–`PgDn`）。你也可以选择到具体的某一个地方，如果你认为这个地方是代码块结束的地方的话（例如：一大块 `\x00` 或 `\xFF` 字节前面的地方）；
4. 选择“Edit->Code”或者按下 `c`，会出现一个会话框询问你要做什么具体操作：

![select_code][1]

5. 如果你确定在选择的区间内确实存在大量指令的话就点击“Force”，如果在指令间可能存在数据的话就点击“Analyze”。
6. IDA 会遍历选择的区域并且尝试将任何未定义的字节转换为指令。如果在选择区域确实存在合法代码，你可能会看到一些函数被添加到函数窗口中去（可能会包括一些误报）。

### 结构体偏移

选择操作的另外一个有用的用法是将结构体便宜应用到多条指令中去。举个例子，我们来看看 UEFI 模块中的这个函数：

```
.text:0000000000001A64 sub_1A64        proc near               ; CODE XREF: sub_15A4+EB↑p
.text:0000000000001A64                                         ; sub_15A4+10E↑p
.text:0000000000001A64
.text:0000000000001A64 var_28          = qword ptr -28h
.text:0000000000001A64 var_18          = qword ptr -18h
.text:0000000000001A64 arg_20          = qword ptr  28h
.text:0000000000001A64
.text:0000000000001A64                 push    rbx
.text:0000000000001A66                 sub     rsp, 40h
.text:0000000000001A6A                 lea     rax, [rsp+48h+var_18]
.text:0000000000001A6F                 xor     r9d, r9d
.text:0000000000001A72                 mov     rbx, rcx
.text:0000000000001A75                 mov     [rsp+48h+var_28], rax
.text:0000000000001A7A                 mov     rax, cs:gBS
.text:0000000000001A81                 lea     edx, [r9+8]
.text:0000000000001A85                 mov     ecx, 200h
.text:0000000000001A8A                 call    qword ptr [rax+50h]
.text:0000000000001A8D                 mov     rax, cs:gBS
.text:0000000000001A94                 mov     r8, [rsp+48h+arg_20]
.text:0000000000001A99                 mov     rdx, [rsp+48h+var_18]
.text:0000000000001A9E                 mov     rcx, rbx
.text:0000000000001AA1                 call    qword ptr [rax+0A8h]
.text:0000000000001AA7                 mov     rax, cs:gBS
.text:0000000000001AAE                 mov     rcx, [rsp+48h+var_18]
.text:0000000000001AB3                 call    qword ptr [rax+68h]
.text:0000000000001AB6                 mov     rax, [rsp+48h+var_18]
.text:0000000000001ABB                 add     rsp, 40h
.text:0000000000001ABF                 pop     rbx
.text:0000000000001AC0                 retn
.text:0000000000001AC0 sub_1A64        endp
```

如果我们知道 `gBS` 是指向 `EFI_BOOT_SERVICES` 的一个指针，我们可以将对它的访问（在 call 指令中）转换为结构体的偏移。这可以通过手工对每个访问进行转换来完成，但却很繁琐。这种情况下选择操作会很有用。如果我们选择了访问结构体的这些指令然后按下 `T`（structure offset），会弹出一个新的会话框：

![sel_stroff][2]

你可以选择哪一个寄存器用作基址，哪一个结构体将会被应用，甚至可以选择你想转换哪一条具体的指令。

选择了 `rax` 和 `EFI_BOOT_SERVICES` 之后，我们会得到一个好看的列表：

```
.text:0000000000001A64 sub_1A64        proc near               ; CODE XREF: sub_15A4+EB↑p
.text:0000000000001A64                                         ; sub_15A4+10E↑p
.text:0000000000001A64
.text:0000000000001A64 Event           = qword ptr -28h
.text:0000000000001A64 var_18          = qword ptr -18h
.text:0000000000001A64 Registration    = qword ptr  28h
.text:0000000000001A64
.text:0000000000001A64                 push    rbx
.text:0000000000001A66                 sub     rsp, 40h
.text:0000000000001A6A                 lea     rax, [rsp+48h+var_18]
.text:0000000000001A6F                 xor     r9d, r9d        ; NotifyContext
.text:0000000000001A72                 mov     rbx, rcx
.text:0000000000001A75                 mov     [rsp+48h+Event], rax ; Event
.text:0000000000001A7A                 mov     rax, cs:gBS
.text:0000000000001A81                 lea     edx, [r9+8]     ; NotifyTpl
.text:0000000000001A85                 mov     ecx, 200h       ; Type
.text:0000000000001A8A                 call    [rax+EFI_BOOT_SERVICES.CreateEvent]
.text:0000000000001A8D                 mov     rax, cs:gBS
.text:0000000000001A94                 mov     r8, [rsp+48h+Registration] ; Registration
.text:0000000000001A99                 mov     rdx, [rsp+48h+var_18] ; Event
.text:0000000000001A9E                 mov     rcx, rbx        ; Protocol
.text:0000000000001AA1                 call    [rax+EFI_BOOT_SERVICES.RegisterProtocolNotify]
.text:0000000000001AA7                 mov     rax, cs:gBS
.text:0000000000001AAE                 mov     rcx, [rsp+48h+var_18] ; Event
.text:0000000000001AB3                 call    [rax+EFI_BOOT_SERVICES.SignalEvent]
.text:0000000000001AB6                 mov     rax, [rsp+48h+var_18]
.text:0000000000001ABB                 add     rsp, 40h
.text:0000000000001ABF                 pop     rbx
.text:0000000000001AC0                 retn
.text:0000000000001AC0 sub_1A64        endp
```

### 强制转换字符串

当一些代码引用一个字符串时，IDA 通常足够聪明，能够检测到它并将引用的字节转换为文字项。然而，在某些情况下，自动转换不起作用，例如：

- 字符串包括非 ASCII 字符
- 字符串不是以 null 字节结尾的

第一种情况的一个常见示例是 Linux 内核，它使用特殊的字节序列来标记不同类别的内核消息。例如，`joydev.ko` 模块中的这个函数：

![sel_joydev][3]

IDA 不会在 `1BC8` 处自动创建一个字符串，因为它是以非 ASCII 字符开头的。然而，如果我们选择了字符串的字节然后按下 `A`（Convert to string），无论如何字符串都会被创建：

![sel_joydev2][4]

### 由数据创建结构体

处理二进制文件中的结构化数据时，此操作也非常有用。让我们考虑一个具有大致如下条目布局的表：

```
struct copyentry {
 void *source;
 void *dest;
 int size;
 void* copyfunc;
};
```

虽然这种结构总是可以在 Structures 窗口中手动创建，但通常需要先格式化数据，然后创建描述数据的结构才会更容易。创建四个数据项后，选择它们，并从上下文菜单中选择“Create struct from selection”：

![sel_struct1][5]

IDA 将创建一个表示所选数据项的结构，然后可用于格式化程序或反汇编中的其他条目，以更好地理解处理这些数据的代码。

![sel_struct2][6]

--------------------------------------------------------------------------------

via: https://hex-rays.com/blog/igor-tip-of-the-week-04-more-selection/

作者：Igor Skochinsky
译者：[spwpun](https://github.com/spwpun)
校对：[firmianay](https://github.com/firmianay)

[1]: https://www.hex-rays.com/wp-content/uploads/2020/08/select_code.png
[2]: https://www.hex-rays.com/wp-content/uploads/2020/08/sel_stroff.png
[3]: https://www.hex-rays.com/wp-content/uploads/2020/08/sel_joydev.png
[4]: https://www.hex-rays.com/wp-content/uploads/2020/08/sel_joydev2.png
[5]: https://www.hex-rays.com/wp-content/uploads/2020/08/sel_struct1.png
[6]: https://www.hex-rays.com/wp-content/uploads/2020/08/sel_struct2.png
