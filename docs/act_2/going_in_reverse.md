# Going in Reverse

Difficulty: :material-star::material-star::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Kevin in the Retro Store needs help rewinding tech and going in reverse. Extract the flag and enter it here.

??? quote "Kevin McFarland"

    Hello, I'm Kevin (though past friends have referred to me as 'Heavy K'). If you ever hear any one say that philosophy is a useless college degree, don't believe them; it's not. I've arrived where I am at because of it. It just made the path more interesting.

    I have more hobbies than I can keep up with, including Amateur Astronomy, Shortwave Radio, and retro-gaming. Things like backyard observances of distant galaxies, non-internet involved, around the world communications, and those who program for the Atari 2600 still invoke degrees of awe for me.

    One of the most influential books I've read is "Godel, Escher, and Bach" by Douglas Hofstadter. I'm also a bit of a Tolkien fanatic.

    My wife and my daughter are everything; without them, I surely would still be kicking rusty tin cans down the lonely highways of my past.

    I don't have the details yet, but something's coming up. Check back with me.

    You know, there's something beautifully nostalgic about stumbling across old computing artifacts. Just last week, I was sorting through some boxes in my garage and came across a collection of 5.25" floppies from my college days - mostly containing terrible attempts at programming assignments and a few games I'd copied from friends.

    Finding an old Commodore 64 disk with a mysterious BASIC program on it? That's like discovering a digital time capsule. The C64 was an incredible machine for its time - 64KB of RAM seemed like an ocean of possibility back then. I spent countless hours as a kid typing in program listings from Compute! magazine, usually making at least a dozen typos along the way.

    The thing about BASIC programs from that era is they were often written by clever programmers who knew how to hide things in plain sight. Sometimes the most interesting discoveries come from reading the code itself rather than watching it execute. It's like being a digital archaeologist - you're not just looking at what the program does, you're understanding how the programmer thought.

    Take your time with this one. Those old-school programmers had to be creative within such tight constraints. You'll know the flag by the Christmas phrase that pays.

## Hints

??? tip "Going in Reverse"

    Holy cow! Another retro floppy disk, what are the odds? Well it looks like this one is intact.

??? tip "Going in Reverse"

    Wow! A disk from the 1980s! I remember delivering those computer disks to the good boys and girls. Games were their favorite, but they weren't like they are now.

??? tip "Going in Reverse"

    It looks like the program on the disk contains some weird coding.

## Solution

We receive the following BASIC program:

```basic title="BASIC program with added comments"linenums="1"
# Comment
10 REM *** COMMODORE 64 SECURITY SYSTEM ***
# Set password
20 ENC_PASS$ = "D13URKBT"
# Set encoded flag, ignore old password at the end (comment)
30 ENC_FLAG$ = "DSA|auhts*wkfi=dhjwubtthut+dhhkfis+hnkz" ' old "DSA|qnisf`bX_huXariz"
# Prompt for password
40 INPUT "ENTER PASSWORD: "; PASS$
# Check if provided password is same length as actual password, halt if not
50 IF LEN(PASS$) <> LEN(ENC_PASS$) THEN GOTO 90
# Iterate over provided password length
60 FOR I = 1 TO LEN(PASS$)
# Check provided_pass[i] XOR 7 matches actual_pass[i], halt if not
70 IF CHR$(ASC(MID$(PASS$,I,1)) XOR 7) <> MID$(ENC_PASS$,I,1) THEN GOTO 90
80 NEXT I
# If passwords match, collect flag by iterating over encoded flag with XOR 7 to find original flag
85 FLAG$ = "" : FOR I = 1 TO LEN(ENC_FLAG$) : FLAG$ = FLAG$ + CHR$(ASC(MID$(ENC_FLAG$,I,1)) XOR 7) : NEXT I : PRINT FLAG$
# halt
90 PRINT "ACCESS DENIED"
100 END
```

!!! example "AI usage"

    Used to validate understanding of script.

The flag is encoded as `DSA|auhts*wkfi=dhjwubtthut+dhhkfis+hnkz` with XOR 7. We can use CyberChef to decode it using [this](<https://gchq.github.io/CyberChef/#recipe=XOR(%7B'option':'Hex','string':'7'%7D,'Standard',false)&input=RFNBfGF1aHRzKndrZmk9ZGhqd3VidHRodXQrZGhoa2Zpcytobmt6&oeol=CR>) recipe.

??? success "Solution to question 1"

    The flag is `CTF{frost-plan:compressors,coolant,oil}`.

    Note that the 'old' flag that is still present as comment is `CTF{vintage_Xor_fun}`.

    We could also have found the flag by running the program by decoding the password `D13URKBT`, which is encoded by XOR 7 as well, to `C64RULES`.

## Response

!!! quote "Kevin McFarland"

    Excellent work! You've just demonstrated one of the most valuable skills in cybersecurity - the ability to think like the original programmer and unravel their logic without needing to execute a single line of code.
