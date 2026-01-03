# Quantgnome Leap

Difficulty: :material-star::material-star::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Charlie in the hotel has quantum gnome mysteries waiting to be solved. What is the flag that you find?

??? quote "Charlie Goldner"

    Hello! I'm not JJ. I like music.

    I accept AI tokens.

    I like quantum pancakes.

    I enjoy social engineeering.

    I just spotted a mysterious gnome - he winked and vanished, or maybe he’s still here?

    Things are getting strange, and I think we’ve wandered into a quantum conundrum!

    If you help me unravel these riddles, we might just outsmart future quantum computers.

    Cryptic puzzles, quirky gnomes, and post-quantum secrets—will you leap with me?

## Hints

??? tip "Quantgnome Leap"

    User keys are like presents. The keys are kept in a hidden location until they need to be used. Hidden files in Linux always start with a dot. Since everything in Linux is a file, directories that start with a dot are also...hidden!

??? tip "Quantgnome Leap"

    When you give a present, you often put a label on it to let someone know that the present is for them. Sometimes you even say who the present is from. The label is always put on the outside of the present so the public knows the present is for a specific person. SSH keys have something similar called a comment. SSH keys sometimes have a comment that can help determine who and where the key can be used.

??? tip "Quantgnome Leap"

    Process information is very useful to determine where an application configuration file is located. I bet there is a secret located in that application directory, you just need the right user to read it!

??? tip "Quantgnome Leap"

    If you want to create SSH keys, you would use the ssh-keygen tool. We have a special tool that generates post-quantum cryptographic keys. The suffix is the same as ssh-keygen. It is only the first three letters that change.

## Solution

This challenge guides us through various encryption algorithms that can be used to generate SSH keys.

We can find SSH keys with increasingly modern encryption for specific users on the system:

```sh
qgnome@quantgnome_leap:~/.ssh$ ls -la
total 16
drwxr-xr-x    2 root     root          4096 Oct 29 00:29 .
drwxr-x---    1 qgnome   qgnome        4096 Nov 16 12:55 ..
-rw-------    1 qgnome   qgnome        2590 Oct 29 00:29 id_rsa
-rw-r--r--    1 qgnome   qgnome         560 Oct 29 00:29 id_rsa.pub
```

Let's leverage these to login as different users:

### gnome1

```sh title="SSH key user 'qgnome'"
qgnome@quantgnome_leap:~/.ssh$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCya6rjv+pf55l/EvEIZW+yhBdrpBrS0rmyysVhdR5Zn2aatLVDhRnNQrH+si6SOaDAOPhOhy037auUveLhEFaQBDQIDqisQ8JoTT/TKhyO97h1IUkl3zmsuw4Kcu1r24L2UJCIVStiJR8vQU8U0Kg5eWuDRev9j+2VMGqF2hmYqssTNbxHNeNbEr1R6/wciSAa3hNwksqE3dYjbr07veKAIcWcsaPMRHmjHrHXdLLwyweXhgzidd3AgzDskub9XdAiXs2B93mFNbQWel+nE2smxUVUY+SLsGXDTXAJu5AqYXrDEJtSuCOCHKXyPX7WCbmAllQo1FB/9K59pI552+K062SvGDCeLEPpcELozU52/awX2yeldNOj7Bn/xXdKpSPHLUrhsj8y/9gVTnS/0q6VLzO8qIwzxdGh7P0OtQqMrSRkTLEHtdOjojTmT70WUpUaVWXf65X8ymY72G49lJjFVAyM6AFBQK/K52f0UTl4XnvkSHwxYNFyk7wGkE07pWU= gnome1
```

```sh title="Login as user 'gnome1'"
qgnome@quantgnome_leap:~/.ssh$ ssh gnome1@127.0.0.1
```

![answer](../media/quantgnome_leap/quantgnome_leap_2)
/// caption
gnome1 login.
///

### gnome2

```sh title="Keys in SSH folder of 'gnome1'"
gnome1@pqc-server:~/.ssh$ ls -la
total 16
drwxr-xr-x 2 root root 4096 Oct 29 00:29 .
drwxr-x--- 1 gnome1 gnome1 4096 Nov 16 13:02 ..
-rw------- 1 gnome1 gnome1 399 Oct 29 00:29 id_ed25519
-rw-r--r-- 1 gnome1 gnome1 88 Oct 29 00:29 id_ed25519.pub
gnome1@pqc-server:~/.ssh$ cat id_ed25519.pub
```

```sh title="Login as user 'gnome2'"
gnome1@pqc-server:~/.ssh$ ssh gnome2@127.0.0.1
```

![answer](../media/quantgnome_leap/quantgnome_leap_3)
/// caption
gnome2 login.
///

### gnome3

```sh title="Keys in SSH folder of 'gnome2'"
gnome2@pqc-server:~$ ls -la .ssh/
total 32
drwxr-xr-x    2 root     root          4096 Oct 29 00:29 .
drwxr-x---    1 gnome2   gnome2        4096 Oct 29 00:29 ..
-rw-------    1 gnome2   gnome2       13532 Oct 29 00:29 id_mayo2
-rw-r--r--    1 gnome2   gnome2        6590 Oct 29 00:29 id_mayo2.pub
```

```sh title="Login as user 'gnome3'"
gnome2@pqc-server:~/.ssh$ ssh gnome3@127.0.0.1
```

![answer](../media/quantgnome_leap/quantgnome_leap_4)
/// caption
gnome3 login.
///

### gnome4

```sh title="Keys in SSH folder of 'gnome3'"
gnome3@pqc-server:~/.ssh$ ls -la
total 16
drwxr-xr-x 2 root root 4096 Oct 29 00:29 .
drwxr-x--- 1 gnome3 gnome3 4096 Oct 29 00:29 ..
-rw------- 1 gnome3 gnome3 744 Oct 29 00:29 id_ecdsa_nistp256_sphincssha2128fsimple
-rw-r--r-- 1 gnome3 gnome3 265 Oct 29 00:29 id_ecdsa_nistp256_sphincssha2128fsimple.pub
```

```sh title="Login as user 'gnome3'"
gnome3@pqc-server:~/.ssh$ ssh gnome4@127.0.0.1
```

![answer](../media/quantgnome_leap/quantgnome_leap_5)
/// caption
gnome4 login.
///

### admin

```sh title="Keys in SSH folder of 'gnome4'"
gnome4@pqc-server:~/.ssh$ ls -la
total 28
drwxr-xr-x 2 root root 4096 Oct 29 00:29 .
drwxr-x--- 1 gnome4 gnome4 4096 Oct 29 00:29 ..
-rw------- 1 gnome4 gnome4 14396 Oct 29 00:29 id_ecdsa_nistp521_mldsa87
-rw-r--r-- 1 gnome4 gnome4 3739 Oct 29 00:29 id_ecdsa_nistp521_mldsa87.pub
```

```sh title="Login as user 'admin'"
gnome4@pqc-server:~/.ssh$ ssh admin@127.0.0.1
```

![answer](../media/quantgnome_leap/quantgnome_leap_6)
/// caption
admin login.
///

We need to look for the flag as instructed in the prompt:

??? success "Solution to challenge"

    ```sh
    admin@quantgnome_leap:~$ cd /opt/oqs-ssh/
    admin@quantgnome_leap:/opt/oqs-ssh$ ls
    bin key-lookup.log moduli scripts ssh_config ssh_host_ecdsa_nistp521_mldsa-87_key.pub sshd_config user-keys
    flag key-lookup.sh sbin share ssh_host_ecdsa_nistp521_mldsa-87_key ssh_known_hosts sshd_logfile.log
    admin@quantgnome_leap:/opt/oqs-ssh$ cd flag
    admin@quantgnome_leap:/opt/oqs-ssh/flag$ ls
    flag
    admin@quantgnome_leap:/opt/oqs-ssh/flag$ cat flag
    HHC{L3aping_0v3r_Quantum_Crypt0}
    ```

    The solution to the challenge is `HHC{L3aping_0v3r_Quantum_Crypt0}`.

## Images

![answer](../media/quantgnome_leap/quantgnome_leap_1)
/// caption
Challenge terminal.
///

## Response

!!! quote "Charlie Goldner"

    That was wild—who knew quantum gnomes could hide so many secrets?

    Thanks for helping me leap into the future of cryptography!

```

```
