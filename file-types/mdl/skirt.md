# Skirt

## Introduction
Original research taken from [Maredzer's tutorial on how to fix skirts](https://www.youtube.com/watch?v=2QISo12MdHg).

Budokai Tenkaichi models store the skirt of a character's model not just throughout their **hip & thigh model parts**,
but **also their own model part**, which usually fills up the hole remaining from the aforementioned model parts.

![skirt-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-1.png)

## Breakdown
The **skirt model data's offset** can be found at **byte 112's unsigned int**.

![skirt-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-2.png)

The **location is actually correct**, as the bones I've selected are the **last 3 bones of every model (THROW, CAMERA, UTILITY**).

This means that the waist is right next to those model parts. 

![skirt-3](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-3.png)

![skirt-4](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-4.png)

What the ``2A 00 00 00`` and ``0E 00 00 00`` are for is unknown, but **the header (first 16 bytes)** does contain **two important integers**:

The **first one**, highlighted in **red**, is ALWAYS used to skip the header, because that is where the **mesh data** is at.

![skirt-5](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-5.png)

As for the second one, highlighted in **blue**, it points to the **texture & shader IDs for each mesh**:

![skirt-6](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-6.png)

![skirt-7](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/mdl/img/skirt-7.png)

One thing worth pointing out is how the texture IDs actually differ
* 08004121**26**000020 (from any other model part);
* 08004121**22**000020 (from the skirt model part).

The only change is that ``0x26`` gets replaced with ``0x22``.

Otherwise, shader IDs remain the exact same.

So, if the **5th byte of a texture ID** ends with **5** or **6**, for the skirt, it needs to be replaced with **2**.

