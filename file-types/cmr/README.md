# com_param.cmr (COM Transformation Recognition)

## Introduction
Originally researched by me and [malachi](https://github.com/malachi1990) in January 2022, and revisited in October 2023.

When **editing transformations** from the ``common_param.dat`` file, the AI **isn't actually aware of the changes** made.

Many would assume such transformation recognition would be *hardcoded*, but fortunately for us, **it is not**.

Incase you are interested in **editing COM parameters in bulk** (no hex involved), [click here](https://github.com/ViveTheModder/bt3-com-param-editor).

## Breakdown
![speak-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/cmr/img/com-1.png)

Granted, we don't know exactly everything, but the gist of it is as follows:
* The bytes in **red** serve as identifiers/descriptors for certain transformation categories, based on their requirements and bonuses;
* The bytes in **teal** aren't as indicative, but they're still worth researching further;
* The **green** byte, if set to 0x0A, will make the AI recognize multiple transformations (if it has any);
* The **purple** byte is the maximum health percentage required for the COM to transform.

Here is a list of values in hex that the short located at byte 15 can take:

```
[Bytes 15-16]   [MAX HP %]   [Byte 264]  [Transformation Category/Candidates]
00 00           00           00          Default value (no transformations)
14 14           1E (30%)     1E          Great Apes, Syn Shenron, Zarbon, Videl
1E 32           28 (40%)     3C          Transformations with Health Bonus
14 28           1E (30%)     32          Transformations with Ki Bonus
28 0A           28 (40%)     0F          Transformations without bonuses (e.g. Super Saiyan)
28 03           3C (60%)     03          Detransformation only
```

## Fun Stats
1. For fusions, the maximum health percentage is set to 60%, at least for Goten and Kid Trunks. Otherwise, it should be 40%.
2. The COM is 14% more likely to transform if:
* it’s a Frieza type transformation (1 stock, 5k HP bonus);
* it’s facing an actual player (making it 14% less likely for COM vs COM battles).