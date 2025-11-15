# Retro Recovery

Difficulty: :material-star::material-star::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Join Mark in the retro shop. Analyze his disk image for a blast from the retro past and recover some classic treasures.

??? quote "Mark DeVito"

    I am an avid collector of things of the past. I love old technology. I love how it connects us to the past. Some remind me of my dad, who was an engineer on the Apollo 11 mission and helped develop the rendezvous and altimeter radar on the space craft.

    If you ever get into collecting things like vintage computers, here is a tip. Never forget to remove the RIFA capacitors from vintage computer power supplies when restoring a system. If not they can pop and fill the room with nasty smoke.

    I love vintage computing, it’s the very core of where and when it all began. I still enjoy writing programs in BASIC and have started re-learning Apple II assembly language. I started writing code in 1982 on a Commodore CBM.

    Sometimes it is the people no one can imagine anything of who do the things no one can imagine. - Alan Turing

    You never forget your first 8-bit system.

    There's something in the works. Come see me later and we'll talk.

    While Kevin and I were cleaning up the Retro Store, we found this FAT12 floppy disk image, must have been under this arcade machine for years. These disks were the heart of machines like the Commodore 64. I am so glad you can still mount them on a modern PC.

    When I was a kid we shared warez by hiding things as deleted files.

    I remember writing programs in BASIC. So much fun! My favorite was Star Trek.

    The beauty of file systems is that 'deleted' doesn't always mean gone forever.

    Ready to dive into some digital archaeology and see what secrets this old disk is hiding?

    Go to Items in your badge, download the floppy disk image, and see what you can find!

## Hints

??? tip "Retro Recovery"

    Wow! A disk from the 1980s! I remember delivering those computer disks to the good boys and girls. Games were their favorite, but they weren't like they are now.

??? tip "Retro Recovery"

    I know there are still tools available that can help you find deleted files. Maybe that might help. Ya know, one of my favorite games was a Quick Basic game called Star Trek.

??? tip "Retro Recovery"

    I miss old school games. I wonder if there is anything on this disk? I remember, when kids would accidentlly delete things.......... it wasn't to hard to recover files. I wonder if you can still mount these disks?

## Solution

??? success "Solution to question 1"

```
sudo mkdir /mnt/floppy
sudo mount -o ro floppy.img /mnt/floppy
/mnt/floppy/qb45$ ls -la
total 579
drwxr-xr-x. 2 root root    512 Mar 18  2025 .
drwxr-xr-x. 3 root root   7168 Jan  1  1970 ..
-rwxr-xr-x. 1 root root  97481 Mar 18  2025 BC.EXE
-rwxr-xr-x. 1 root root  77440 Mar 18  2025 BRUN45.EXE
-rwxr-xr-x. 1 root root  35643 Mar 18  2025 LIB.EXE
-rwxr-xr-x. 1 root root  69133 Mar 18  2025 LINK.EXE
-rwxr-xr-x. 1 root root  14674 Mar 18  2025 MOUSE.COM
-rwxr-xr-x. 1 root root   9506 Mar 18  2025 PACKING.LST.txt
-rwxr-xr-x. 1 root root 278804 Mar 18  2025 QB.EXE
-rwxr-xr-x. 1 root root     69 Mar 18  2025 QB.INI


qphotorec
load floppy.img
found 1 txt file
startrek game
contains base64 line in beginning commented
bWVycnkgY2hyaXN0bWFzIHRvIGFsbCBhbmQgdG8gYWxsIGEgZ29vZCBuaWdodAo=
merry christmas to all and to all a good night

1 REM original file superstartrek.bas dated 2/13/2009 from bcg.tar.gz
2 REM QBasic conversion by WTN...
3 REM 2/16/2021 - uncrunched, changed B9=2 to B9=0, added RANDOMIZE and SLEEP
4 REM incorporated instructions from superstartrekins.bas
5 REM 2/19/2021 - changed the code after DO YOU NEED INSTRUCTIONS?
10 REM SUPER STARTREK - MAY 16,1978 - REQUIRES 24K MEMORY
30 REM
40 REM ****        **** STAR TREK ****        ****
50 REM **** SIMULATION OF A MISSION OF THE STARSHIP ENTERPRISE,
60 REM **** AS SEEN ON THE STAR TREK TV SHOW.
70 REM **** ORIGIONAL PROGRAM BY MIKE MAYFIELD, MODIFIED VERSION
80 REM **** PUBLISHED IN DEC'S "101 BASIC GAMES", BY DAVE AHL.
90 REM **** MODIFICATIONS TO THE LATTER (PLUS DEBUGGING) BY BOB
100 REM *** LEEDOM - APRIL & DECEMBER 1974,
110 REM *** WITH A LITTLE HELP FROM HIS FRIENDS . . .
120 REM *** COMMENTS, EPITHETS, AND SUGGESTIONS SOLICITED --
130 REM *** SEND TO:  R. C. LEEDOM
140 REM ***           WESTINGHOUSE DEFENSE & ELECTRONICS SYSTEMS CNTR.
150 REM ***           BOX 746, M.S. 338
160 REM ***           BALTIMORE, MD  21203
170 REM ***
180 REM *** CONVERTED TO MICROSOFT 8 K BASIC 3/16/78 BY JOHN GORDERS
190 REM *** LINE NUMBERS FROM VERSION STREK7 OF 1/12/75 PRESERVED AS
200 REM *** MUCH AS POSSIBLE WHILE USING MULTIPLE STATEMENTS PER LINE
205 REM *** SOME LINES ARE LONGER THAN 72 CHARACTERS; THIS WAS DONE
210 REM *** BY USING "?" INSTEAD OF "PRINT" WHEN ENTERING LINES
211 REM bWVycnkgY2hyaXN0bWFzIHRvIGFsbCBhbmQgdG8gYWxsIGEgZ29vZCBuaWdodAo=
215 REM ***


https://retrogamecoders.com/c64-emulator/?basic=f0000038.prg
```

## Response

!!! quote "Mark DeVito"

    Excellent work! You've successfully recovered that deleted file and decoded the hidden message.

    Sometimes the old ways are the best ways. Vintage file systems never truly forget what they've seen. Play some Star Trek... it actually works.
