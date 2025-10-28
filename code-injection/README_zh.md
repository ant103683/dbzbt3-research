# 如何为《龙珠Z：武道会天下第一3》(PS2) 进行简单代码注入

## 前置条件
* 汇编语言基础知识
* C语言基础知识

## 所需工具
* 任何十六进制编辑器（本例中使用 [010 Editor](https://www.sweetscape.com/download/010editor/)）
* [Cheat Engine](https://github.com/cheat-engine/cheat-engine)
* [DBZBT3 未使用函数 CSV](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/csv/dbzbt3-unused-functions.csv)
* [DBZBT3 未使用方法 CSV](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/csv/dbzbt3-unused-methods.csv)
* [Ghidra](https://github.com/NationalSecurityAgency/ghidra/releases)
* [PCSX2](https://github.com/PCSX2/pcsx2/releases)

# 设置指南
1. 下载 [Ghidra Emotion Engine: Reloaded](https://github.com/chaoticgd/ghidra-emotionengine-reloaded/releases/tag/v2.1.12) 扩展
并将其 ZIP 文件放在 ``{Ghidra 路径}/Extensions/Ghidra`` 中。
2. 打开 Ghidra，转到 File -> Install Extensions 并勾选 ``ghidra-emotionengine-reloaded`` 旁边的复选框来安装扩展。
3. 然后，转到 File -> New Project (CTRL+N) 并指定项目的目录和名称。
4. 接着，转到 File -> Import File (I) 并导入 [这个 ELF](https://github.com/HiroTex/dbz3_sc/blob/main/ghidra/slps_258.15.gzf)。
5. 之后，双击 ``slps_258.15`` 来查看反编译的源代码。

# 简要说明
这种代码注入方法不会添加额外的二进制文件（更多 ELF）或代码到主 ELF（可执行和可链接格式，在本例中为 ``slps_258.15``）。
相反，它利用几个未使用的函数和方法作为注入代码的自由空间。

这就是为什么我提供了每个未使用函数/方法的原型/签名、地址和大小（以字节为单位）。
需要澄清的是，这些地址属于游戏的 NTSC-J（日本）版本，因为只有该版本已被反编译。

不同版本的游戏可能包含不同的函数/方法，所有这些都可能在代码中的不同位置。
因此，跳转指令（``j``、``jal``）在不同版本之间是不同的，因为它们以绝对位置作为参数。

分支指令（``b``、``beq``、``bne`` 等）也可能受到影响，但只有在添加更多代码时才会如此。
这是因为分支以相对位置作为参数，由分支将跳过的下面指令数量表示。

如果该函数/方法的指令数量保持不变，那么无论游戏版本如何，分支也保持不变。

# 如何在其他 DBZBT3 版本中找到等效函数/方法
本教程的其余部分将使用 ``BattleActor::CurrentStageIsTournament(void)`` 方法作为示例。
这是因为我想要一些简单的东西，特别是对于我第一次尝试代码注入。

![ghidra-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/ghidra-1.png)

该方法位于地址 0x001DC3E8，其指令可以直接从游戏内存中查看。
为此，可以使用 Cheat Engine 或 PCSX2 的内置调试器。

现在，要在游戏的 NTSC-U（美国）版本中找到此方法，PCSX2 调试器将无法帮助，这就是为什么 Cheat Engine 是必需的。

在内存中找到方法（或在 Ghidra 中查看）后必须遵循以下步骤：
1. 记下此方法的初始字节，即最初几条指令的内容，
直到遇到 ``j``（跳转）或 ``jal``（跳转并链接）。

在这种情况下，初始字节是 ``F0 FF BD 27 00 00 BF FF``。这显然不够，因为有 1062 个其他函数/方法以完全相同的序列开始。

2. 如果字节搜索导致超过 1 个结果，则包含相邻函数/方法的最后指令。

在这里，我从 ``BattleActor::FUN_001DC390`` 中取了最后 3 条指令并将它们添加到搜索中：

``03 08 00 46 08 00 E0 03 10 00 BD 27 F0 FF BD 27 00 00 BF FF``

3. 在 Cheat Engine 的内存查看器中，使用 CTRL+F 快捷键，选择 ``(Array of) byte`` 选项并粘贴字节搜索。

![ce-5](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/ce-5.png)

4. 查看内存查看器的左下角并复制地址。

![ce-6](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/ce-6.png)

结果，在 NTSC-U 版本中，``BattleActor::CurrentStageIsTournament(void)`` 方法位于 0x201DC480。

Cheat Engine 最初将我带到 0x201DC474，但因为我包含了 3 条不相关的指令，我将地址移动了 12 字节，使其成为 0x201DC480。

相同的逻辑可以应用于直接在 ELF 文件中找到函数/方法。在 NTSC-U 版本的 ELF slus_216.78 中，该方法位于 0x000DD480。

## 如何在内存中查看函数/方法 - Cheat Engine 方式
1. 转到 File -> Open Process 并从进程列表中选择 PCSX2。
2. 点击 ``Memory View`` 按钮。
3. 在内存查看器中，使用 CTRL+G 键盘快捷键并简单地粘贴地址，但将其最左边的数字替换为 2。

![ce-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/ce-1.png)

![ce-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/ce-2.png)

![ce-3](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/ce-3.png)

![ce-4](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/ce-4.png)

## 如何在内存中查看函数/方法 - PCSX2 调试器方式
1. 这可能因 PCSX2 的后续版本（1.6 之后）而有所不同，但转到 Debug -> Open Debug Window...
2. 确保游戏正在运行。否则，调试器的内存查看器将显示问号。
3. 使用 CTRL+G 键盘快捷键并简单地粘贴地址，但将其最左边的数字替换为 2。

![pcsx2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/pcsx2.png)

# 代码注入示例
## 试试看！
1. [为 1 个地图添加环出（山路 - 傍晚）](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/mods/add-ringout-to-maps/SLUS_216%20(v1).78)
2. [为 4 个地图添加环出（所有沙漠地图、行星 - 夜晚和山路 - 傍晚）](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/mods/add-ringout-to-maps/SLUS_216%20(v2).78)
3. [在角色参考中添加查看损坏服装的能力；从 DBZBT4 导入](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/mods/view-damaged-costumes)

## 演示
![example-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/example-1.png)

![example-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/example-2.png)

![example-3](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/example-3.png)

## 详细分析
俗话说，条条大路通罗马。我的方法可能不是最好的，但它确实有效。

因此，为了对 ``BattleActor::CurrentStageIsTournament(void)`` 方法有一个大致的了解，我根据每条指令将其分为几个部分：
1. 初始化（前 6 条指令）
2. 地图检查 1（接下来的 2 条指令）
3. 地图检查 2（接下来的 2 条指令）
4. 将结果变量设置为 true（接下来的 3 条指令）
5. 返回结果变量，默认为 false（最后 2 条指令）

实际代码本身非常简单：获取当前地图 ID，将其与其他两个地图 ID 进行比较，如果 ID 匹配则返回 true。

![ghidra-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/code-injection/img/ghidra-2.png)

为了向代码添加更多地图检查，我找到了三个相邻的未使用方法，并将它们的指令清零以腾出一些空间。
我还复制了上述方法的剩余 12 条指令，将它们清零并移动到那个空白空间。

一个问题仍然存在：如何将一个指令集指向另一个？答案是：通过调试编写新的跳转指令...或记住确切的字节序列。

更好的是，为什么不通过学习更多关于 [MIPS 汇编指令格式](https://en.wikibooks.org/wiki/MIPS_Assembly/Instruction_Formats) 来"自己汇编指令"？

之前拆分代码确保只使用一个跳转指令，而不是几个，但多个跳转仍然可以是一个有效的方法。

回到地图检查，每个检查都包括将常量加载到寄存器，然后将其与具有当前地图 ID 的寄存器进行比较。
检查的放置方式是，如果一个检查失败，则执行下一条指令，即另一个地图检查。
如果地图检查成功，其相应的分支简单地重定向到 ``jr``（跳转寄存器）指令，终止方法。

添加更多地图检查时，重要的是确保检查后的每个分支都指向正确的指令。

## 指令和代码行的示例字节序列
* ``jr ra`` ----------------> ``08 00 E0 03``（跳转到返回地址寄存器）
* ``return 0`` -------------> ``2D 10 00 00``（0 = false；将 ``zero`` 寄存器的内容移动到 ``v0``）
* ``return 1`` -------------> ``01 00 02 24``（1 = true；立即加载到 ``v0``）
* 向下移动 X 条指令 -> ``XX 00 00 10``（无条件分支；如果对 ``zero`` 有条件，则对于 ``beq`` 更接近 ``40 10``，对于 ``bne`` 更接近 ``40 14``）

## 如何编写新指令
1. 这可能因 PCSX2 的后续版本（1.6 之后）而有所不同，但转到 Debug -> Open Debug Window...
2. 启动游戏，但也要确保游戏已暂停而不转到 System -> Pause。
3. 为此，按 F12 或转到 Capture -> Video -> Start recording。它应该弹出一个捕获系统窗口，稍后应该关闭。
4. 回到调试器，它带有一个反汇编器。右键单击那里的任何指令，然后转到 Assemble Opcode。
5. 然后，输入您要编译的指令。
6. 极其重要：确保使用的寄存器没有被同一函数/方法或另一个并发函数占用。
7. 之后，记下从该新指令获得的字节。
8. 对 ELF 所做的更改也会更改 CRC，这是 PCSX2 用于保存状态和正确游戏识别的校验和。
   因此，建议 [使用此工具](https://en.wikibooks.org/wiki/MIPS_Assembly/Instruction_Formats) 来修复 CRC。