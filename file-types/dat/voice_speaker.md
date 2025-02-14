# voice_speaker.dat (Special Quote Handler)

## Introduction
Original research by [Nero_149](https://www.youtube.com/@nero_149), but covered more in depth by me in July 2020.

## Breakdown
![speak-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-1.png)

The selection highlighted in red consists of 644 bytes, representing the only bytes that are ever used out of this file.

This is important, because when you divide this selection by 4, you get 161, the total amount of characters in Budokai Tenkaichi 3.
Simply put, **this selection is made out of 161 integers**, **one for each character slot** the opponent can pick.
 
Remember, ``voice_speaker.dat`` is not resident/common for all characters: **each character costume has their own file**.

So, to get to any character slot, you multiply their ID by 4 and go to that address.

![speak-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-2.png)

Each integer is split into two shorts, the first one (in red) being used for the **pre-battle interaction**, 
and the second one (in blue) being used for the **victory interaction**.

The first byte of each short (in this case, ``0xFF``, which prevents any voice line from being played) 
represents the **voice line ID of the Special Quote**, whose count **starts from voice line 506**.

![speak-3](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-3.png)

Normally, this value ranges from ``0x00`` (0) to ``0x17`` (23) for pre-battle quotes, and ``0x00`` (0) to ``0x09`` (9) for victory quotes.

You **can use higher values**, but it is **not recommended to exceed** ``0x63`` (99) for pre-battle quotes and ``0x45`` (69) for victory quotes.

The second byte is much simpler to grasp, since it's just a boolean parameter. It only takes two values:
* 00 (false) -  character's Special Quote is played first;
* 01 (true) - opponent's Special Quote is played first.

### Example

#### Gohan's voice_speaker.dat file
![speak-4](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-4.png)

Krillin's ID is 25 in decimal. 25*4=100, aka the address that the cursor is currently at. 
* Byte 100 - pre-battle voice line ID (set to 08, actual voice line: Gohan_4_514.adx);
* Byte 101 - boolean param (Gohan talks first);
* Byte 102 - victory voice line ID (set to 02, actual voice line: Gohan_4_532.adx);
* Byte 103 - boolean param (unused, Gohan talks first anyway).

#### Krillin's voice_speaker.dat file
![speak-5](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/dat/img/speak-5.png)

Adult Gohan's ID is 17 in decimal. 17*4=68.
* Byte 68 - pre-battle voice line ID (set to 08, actual voice line: Krillin_514.adx);
* Byte 69 - boolean param (Gohan talks first);
* Byte 70 - victory voice line ID (set t0 04, actual voice line: Krillin_534.adx);
* Byte 71 - boolean param (unused, Krillin talks first anyway).