# voice_speaker.dat（特殊台词处理器）

## 介绍
原始研究由 [Nero_149](https://www.youtube.com/@nero_149) 进行，但我在 2020 年 7 月进行了更深入的研究。

## 详细分析
![speak-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-1.png)

红色高亮的选择包含 644 字节，代表此文件中唯一使用的字节。

这很重要，因为当您将此选择除以 4 时，您得到 161，这是《武道会天下第一3》中角色的总数。
简单地说，**此选择由 161 个整数组成**，**对手可以选择的每个角色槽位一个**。
 
请记住，``voice_speaker.dat`` 不是所有角色的常驻/通用文件：**每个角色服装都有自己的文件**。

因此，要到达任何角色槽位，您将其 ID 乘以 4 并转到该地址。

![speak-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-2.png)

每个整数分为两个短整型，第一个（红色）用于**战前互动**，
第二个（蓝色）用于**胜利互动**。

每个短整型的第一个字节（在这种情况下，``0xFF``，防止播放任何语音线）
代表**特殊台词的语音线 ID**，其计数**从语音线 506 开始**。

![speak-3](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-3.png)

通常，此值对于战前台词范围从 ``0x00``（0）到 ``0x17``（23），对于胜利台词范围从 ``0x00``（0）到 ``0x09``（9）。

您**可以使用更高的值**，但**不建议超过**战前台词的 ``0x63``（99）和胜利台词的 ``0x45``（69）。

第二个字节更容易理解，因为它只是一个布尔参数。它只有两个值：
* 00（false）- 角色的特殊台词首先播放；
* 01（true）- 对手的特殊台词首先播放。

### 示例

#### 悟饭的 voice_speaker.dat 文件
![speak-4](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-4.png)

克林的 ID 是十进制的 25。25*4=100，即光标当前所在的地址。
* 字节 100 - 战前语音线 ID（设置为 08，实际语音线：Gohan_4_514.adx）；
* 字节 101 - 布尔参数（悟饭先说话）；
* 字节 102 - 胜利语音线 ID（设置为 02，实际语音线：Gohan_4_532.adx）；
* 字节 103 - 布尔参数（未使用，悟饭无论如何都先说话）。

#### 克林的 voice_speaker.dat 文件
![speak-5](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-5.png)

成年悟饭的 ID 是十进制的 17。17*4=68。
* 字节 68 - 战前语音线 ID（设置为 08，实际语音线：Krillin_514.adx）；
* 字节 69 - 布尔参数（悟饭先说话）；
* 字节 70 - 胜利语音线 ID（设置为 04，实际语音线：Krillin_534.adx）；
* 字节 71 - 布尔参数（未使用，克林无论如何都先说话）。