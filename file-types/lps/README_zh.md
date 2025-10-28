# 唇形同步（.lps 文件）

## 介绍
原始研究由 [Nero_149](https://www.youtube.com/@nero_149) 进行，但我在 2020 年 7 月进行了更深入的研究。

LPS 文件在天下第一系列游戏中用于**基于语音模拟嘴部动作**。

对于**自动唇形同步解决方案**（无需手动工作或十六进制编辑），[请点击这里](https://github.com/ViveTheModder/tenkaichi-lps-generator)。

## 详细分析
让我们从 LPS 文件的三个关键组件开始。

![lps-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/lps/img/lps-1.png)

* **红色** - ``(unsigned int)`` LPS 使用的关键帧数量。

* **绿色** - ``(unsigned short)`` 帧本身（基于 60 fps 设置，尽管游戏以 30 fps 渲染）。

* **紫色** - ``(unsigned short)`` 一个布尔参数，启用（如果设置为 ``01 00``）或禁用嘴部动作（如果设置为 ``00 00``）。

任何 LPS 的第一个关键帧必须是 ``00 00 00 00`` 或 ``00 00 01 00``。

![lps-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/lps/img/lps-2.png)

如果设置为 ``00 00 00 00``，位于第 1 列和第 3 列（绿色高亮）的每个关键帧的布尔参数必须设置为 0。

如果设置为 ``00 00 01 00``，则适用相反的情况，并且必须一直执行到最后一个关键帧（在这种情况下，``86 00 00 00``）。

## 演示
![wav](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/lps/img/wav.png)

为了更好地可视化这个 LPS（取自早期悟空的第一条语音线，``Goku_0_500_US.adx``），我做了以下操作：
* 使用 [PESSFC](https://www.moddingway.com/file/1640.html) 将所述 ADX 转换为 WAV；
* 将其放入 VEGAS Pro 18.0 的时间线中（但**可以使用任何其他音频/编辑器**）；
* 使用区域功能（*插入* -> *区域*）来突出显示悟空说话的音频块。