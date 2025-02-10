# Lip-Syncing (.lps files)

## Introduction
Original research by [Nero_149](https://www.youtube.com/@nero_149), but covered more in depth by me in July 2020.

LPS files are used throughout the Tenkaichi games to **simulate mouth movement based on speech**.

For an **automated lip-syncing solution** (no manual work or hex editing), [click here](https://github.com/ViveTheModder/tenkaichi-lps-generator).

## Breakdown
Let's start off with the three key components of an LPS file.

![lps-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/lps/img/lps-1.png)

* **Red** - ``(unsigned int)`` The number of keyframes used by the LPS.

* **Green** - ``(unsigned short)`` The frame itself (which is based on a 60 fps setting, despite the game being rendered in 30 fps).

* **Purple** - ``(unsigned short)`` A boolean parameter that enables (if set to ``01 00``) or disables the mouth movement (if set to ``00 00``).

The first keyframe of any LPS must be either ``00 00 00 00`` or ``00 00 01 00``.

![lps-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/lps/img/lps-2.png)

If set to ``00 00 00 00``, the boolean parameters for each keyframe located in columns 1 and 3 (highlighted in green) must be set to 0. 

The inverse applies if set to ``00 00 01 00``, and must be done up until the last keyframe (in this case, ``86 00 00 00``).

## Demonstration
![wav](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/lps/img/wav.png)

To visualize this LPS better (taken from Early Goku's first voice line, ``Goku_0_500_US.adx``), I did the following:
* Converted said ADX to WAV using [PESSFC](https://www.moddingway.com/file/1640.html);
* Dropped it in the timeline of VEGAS Pro 18.0 (but **any other audio/editor can be used**);
* Used the Region feature (*Insert* -> *Region*) to highlight the chunks of audio in which Goku is talking.
