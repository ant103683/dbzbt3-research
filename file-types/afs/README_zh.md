# AFS (ADX 文件系统)

## 介绍
《龙珠Z：武道会天下第一3》，就像当时 PS2、PS3 或 Wii 上的任何其他游戏一样，使用 AFS 文件。

无论版本如何，DBZBT3 ISO 都带有以下 AFS 文件：
* ``X``ZS3``YY``0.AFS -> 包含语言选择屏幕的文件，用于游戏的 PAL（欧洲）版本。
* ``X``ZS3``YY``1.AFS -> 包含几乎所有必需品（常驻文件、菜单文件、角色文件等）。
* ``X``ZS3``YY``2.AFS -> 仅包含 ADX 文件（语音、铃声、音乐、一些音效）。

为了更好地理解这些文件名，只需知道 X 和 YY 应该被其他字母替换。

``X`` 代表**游戏机**：``P``（索尼 **PlayStation**）和 ``W``（任天堂 **Wii**）。

另一方面，``YY`` 代表**地区**，可以使用以下字母：
* ``EU`` -> **欧盟**（PAL）；
* ``JP`` -> **日本**（NTSC-J）；
* ``US`` -> **美国**（NTSC-U）。

因此，例如，当正确解读时，``PZS3US1.AFS`` 基本上意味着以下内容：

**P**layStation（龙珠）**Z** **S**parking! **3**（又名 Sparking! METEOR）美国版本的 AFS 编号 1。

## 简要说明
回到文件格式，它最初是为了存储 **ADX 文件**而制作的，因此 **AFS 中的 A**。

后来，它被用来存储各种其他文件，使 AFS 成为一个**有用的文件容器**。

一个 AFS 文件可以存储**多达 65536 个文件**，只要相关的 AFS 不超过 **2 GB 的空间**。

AFS 中的每个文件可以有**多达 2 KB（2048 字节）的自由/保留空间**。

如果您对**允许与 AFS 文件交互/编辑的库**感兴趣，[请点击这里](https://github.com/jagger1407/Afster)。

## 详细分析
![afs-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-1.png)

首先，文件总是以一个 4 字节的字符串 ``AFS`` 开始。

![afs-4](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-4.png)

在它旁边，有 AFS 内的**文件数量**，存储为**短整型**（尽管它像**整数**一样**占用 4 字节**）。

![afs-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-2.png)

之后，文件包含一系列**地址**（橙色）和**文件大小**（蓝色），按此顺序。

因此，例如，AFS 中的第一个文件的**地址**存储为**第 8 字节的整数**，
其**文件大小**也存储为**第 12 字节的整数**。

假设 AFS 内有 3399 个文件。这总共会产生 27192 字节的信息。

但是，在所有这些字节之后实际上还有一个地址和文件大小。

![afs-3](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-3.png)

转到该地址后，我们可以看到一系列**文件名**，后跟**元数据**。

如果事先插入了 **AFL（ADX 文件列表）**，那么**新文件名也将被包含**。

![afs-5](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-5.png)

至于元数据：
* 红色 -> **文件名**（最大：30-31 个字符）
* 橙色 -> 修改日期：**年**；
* 绿色 -> 修改日期：**月**；
* 蓝色 -> 修改日期：**日**；
* 紫色 -> 修改日期：**小时**；
* 粉色 -> 修改日期：**分钟**；
* 黄色 -> 修改日期：**秒**；
* 棕色 -> **文件大小**。

如果从 AFS 中删除地址、文件大小和剩余的元数据，那就给我们一个 **VOL**。

在 3 个《龙珠Z：武道会天下第一》游戏中，**只有第一个游戏使用了一个**，包含重要信息。

这种将 AFS 转换为 VOL 文件的方法，主要是为了防止数据盗窃，[在粉丝制作的 DBZBT4 模组中使用](https://github.com/ViveTheModder/bt4-research)。

**逆转这种转换是不可能的**，除非曾经删除的信息存储或生成在其他地方（无论是 **ELF** 还是其他**二进制文件**）。

在 DBZBT1 的情况下，游戏依赖于存储在 **ELF（可执行和可链接格式）**中的信息。