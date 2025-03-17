# AFS (ADX File System)

## Introduction
DBZ Budokai Tenkaichi 3, like any other game for the PS2, PS3 or Wii at the time, uses AFS files.

Regardless of version, a DBZBT3 ISO comes with the following AFS files:
* ``X``ZS3``YY``0.AFS -> Contains files for the language select screen, used for the PAL (European) version of the game.
* `X``ZS3``YY``1.AFS -> Contains almost all the essentials (resident files, menu files, character files and so on).
* ``X``ZS3``YY``2.AFS -> Contains only ADX files (voice lines, jingles, music, a few sound effects).

To understand these file names better, just know that X and YY are meant to be replaced with other letters.

``X`` represents the **console**: ``P`` (for the Sony **PlayStation**) and ``W`` (for the Nintendo **Wii**).

``YY`` on the other hand represents the **region**, which can take the following letters:
* ``EU`` -> **European Union** (PAL);
* ``JP`` -> **Japan** (NTSC-J);
* ``US`` -> **United States** (NTSC-U).

So, for example, when deciphered correctly, ``PZS3US1.AFS`` basically means the following:
 
AFS number 1 of the American version of **P**layStation (Dragon Ball) **Z** **S**parking! **3** (aka Sparking! METEOR).

## Brief Explanation
Back to the file format for a minute, it was originally made to store **ADX files**, hence the **A in AFS**.

Later, it was used to store all kinds of other files, making AFS a **useful file container**.

An AFS file can store **up to 65536 files**, as long as the AFS in question does NOT exceed **2 GBs of space**.

Each file in an AFS can have **up to 2 KBs (2048 bytes) of free/reserved space**.

In case you are interested in a **library that allows interactions/editing of AFS files**, [click here](https://github.com/jagger1407/Afster).

## Breakdown
![afs-1](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-1.png)

To start things off, the file always starts with a 4-byte string saying ``AFS``.

![afs-4](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-4.png)

Next to it, there's the **number of files** within the AFS, stored as a **short** (despite it **occupying 4 bytes** like an **integer**).

![afs-2](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-2.png)

After that, the file contains a series of **addresses** (in orange) and **file sizes** (in blue), in that order.

So, for instance, the first file in the AFS has its **address** stored as an **integer in byte 8**, 
and its **file size** also stored as an **integer in byte 12**.

Say the AFS has 3399 files inside. That would make for 27192 bytes of information in total.

However, there is actually one more address & file size right after all those bytes.

![afs-3](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-3.png)

After going to that address, we can see a series of **file names** followed by **medata**.

If an **AFL (ADX File List)** has been inserted beforehand, then the **new file names will be included** as well.

![afs-5](https://github.com/ViveTheModder/dbzbt3-research/blob/main/file-types/afs/img/afs-5.png)

As for the metadata:
* Red -> File Name (max: 30-31 characters)
* Orange -> Date Modified: Year;
* Green -> Date Modified: Month;
* Blue -> Date Modified: Day;
* Purple -> Date Modified: Hour;
* Pink -> Date Modified: Minute;
* Yellow -> Date Modified: Seconds;
* Brown -> File Size.

If the addresses, file sizes and the remaining metadata is removed from an AFS, that gives us a **VOL**.

Out of the 3 DBZ Budokai Tenkaichi games, only one is used for the first game, containing vital information.

This means of converting AFS to VOL files, mainly to prevent data theft, was [used in the fanmade DBZBT4 mod](https://github.com/ViveTheModder/bt4-research).

Reversing this conversion is impossible without the once-removed information being stored or generated elsewhere (whether it's an **ELF** or some other **binary file**).

In the case of DBZBT1, the game relies on information stored in the **ELF (Executable & Linkable Format)**.