# 裙子

## 介绍
原始研究来自 [Maredzer 关于如何修复裙子的教程](https://www.youtube.com/watch?v=2QISo12MdHg)。

《武道会天下第一》模型不仅在角色模型的**臀部和大腿模型部分**中存储裙子，
还在**它们自己的模型部分**中存储，这通常填补了上述模型部分留下的空洞。

![skirt-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-1.png)

## 详细分析
**裙子模型数据的偏移量**可以在**字节 112 的无符号整数**中找到。

![skirt-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-2.png)

**位置实际上是正确的**，因为我选择的骨骼是**每个模型的最后 3 个骨骼（THROW、CAMERA、UTILITY）**。

这意味着腰部就在这些模型部分旁边。

![skirt-3](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-3.png)

![skirt-4](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-4.png)

``2A 00 00 00`` 和 ``0E 00 00 00`` 的用途未知，但**头部（前 16 字节）**确实包含**两个重要的整数**：

**第一个**，用**红色**高亮显示，总是用来跳过头部，因为那是**网格数据**所在的位置。

![skirt-5](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-5.png)

至于第二个，用**蓝色**高亮显示，它指向**每个网格的纹理和着色器 ID**：

![skirt-6](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-6.png)

![skirt-7](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-7.png)

值得指出的一点是纹理 ID 实际上是如何不同的
* 08004121**26**000020（来自任何其他模型部分）；
* 08004121**22**000020（来自裙子模型部分）。

唯一的变化是 ``0x26`` 被替换为 ``0x22``。

否则，着色器 ID 保持完全相同。

因此，如果**纹理 ID 的第 5 个字节**以 **5** 或 **6** 结尾，对于裙子，它需要被替换为 **2**。