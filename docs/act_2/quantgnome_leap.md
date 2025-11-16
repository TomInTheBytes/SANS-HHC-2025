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

??? success "Solution to question 1"

```
qgnome@quantgnome_leap:/home$ $PATH
-bash: /opt/oqs-ssh/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin: No such file or directory
qgnome@quantgnome_leap:/home$ cd /opt/oqs-ssh/bin
qgnome@quantgnome_leap:/opt/oqs-ssh/bin$ ls
ssh         ssh-add     ssh-agent   ssh-keygen

https://github.com/open-quantum-safe/openssh


qgnome@quantgnome_leap:/opt/oqs-ssh$ ls -la
total 696
drwxr-xr-x    1 root     root          4096 Nov 16 12:53 .
drwxr-xr-x    1 root     root          4096 Oct 28 23:37 ..
drwxr-xr-x    1 root     root          4096 Oct 29 00:29 bin
dr-x------    1 admin    admin         4096 Oct 29 00:29 flag
-rw-------    1 nobody   nobody           0 Oct 29 00:29 key-lookup.log
-r-xr-x---    1 root     nobody        1199 Oct 28 19:20 key-lookup.sh
-rw-------    1 root     root        620105 Oct 28 23:36 moduli
drwxr-xr-x    2 root     root          4096 Oct 28 23:36 sbin
dr-x------    1 root     root          4096 Oct 29 00:29 scripts
drwxr-xr-x    3 root     root          4096 Oct 28 23:36 share
-rw-r--r--    1 root     root           966 Oct 28 19:20 ssh_config
-rw-------    1 root     root         14384 Oct 29 00:29 ssh_host_ecdsa_nistp521_mldsa-87_key
-rw-r--r--    1 root     root          3734 Oct 29 00:29 ssh_host_ecdsa_nistp521_mldsa-87_key.pub
-rw-r--r--    1 root     root         14983 Oct 29 00:29 ssh_known_hosts
-rw-r--r--    1 root     root          1569 Oct 28 19:20 sshd_config
-rw-------    1 root     root            40 Nov 16 12:53 sshd_logfile.log
drwxr-xr-x    2 root     root          4096 Oct 29 00:29 user-keys

qgnome@quantgnome_leap:~/.ssh$ ls -la
total 16
drwxr-xr-x    2 root     root          4096 Oct 29 00:29 .
drwxr-x---    1 qgnome   qgnome        4096 Nov 16 12:55 ..
-rw-------    1 qgnome   qgnome        2590 Oct 29 00:29 id_rsa
-rw-r--r--    1 qgnome   qgnome         560 Oct 29 00:29 id_rsa.pub
qgnome@quantgnome_leap:~/.ssh$ cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCya6rjv+pf55l/EvEIZW+yhBdrpBrS0rmyysVhdR5Zn2aatLVDhRnNQrH+si6SOaDAOPhOhy037auUveLhEFaQBDQIDqisQ8JoTT/TKhyO97h1IUkl3zmsuw4Kcu1r24L2UJCIVStiJR8vQU8U0Kg5eWuDRev9j+2VMGqF2hmYqssTNbxHNeNbEr1R6/wciSAa3hNwksqE3dYjbr07veKAIcWcsaPMRHmjHrHXdLLwyweXhgzidd3AgzDskub9XdAiXs2B93mFNbQWel+nE2smxUVUY+SLsGXDTXAJu5AqYXrDEJtSuCOCHKXyPX7WCbmAllQo1FB/9K59pI552+K062SvGDCeLEPpcELozU52/awX2yeldNOj7Bn/xXdKpSPHLUrhsj8y/9gVTnS/0q6VLzO8qIwzxdGh7P0OtQqMrSRkTLEHtdOjojTmT70WUpUaVWXf65X8ymY72G49lJjFVAyM6AFBQK/K52f0UTl4XnvkSHwxYNFyk7wGkE07pWU= gnome1

qgnome@quantgnome_leap:~/.ssh$ ssh gnome1@127.0.0.1

==== gnome1

gnome1@pqc-server:~/.ssh$ ls -la
total 16
drwxr-xr-x    2 root     root          4096 Oct 29 00:29 .
drwxr-x---    1 gnome1   gnome1        4096 Nov 16 13:02 ..
-rw-------    1 gnome1   gnome1         399 Oct 29 00:29 id_ed25519
-rw-r--r--    1 gnome1   gnome1          88 Oct 29 00:29 id_ed25519.pub
gnome1@pqc-server:~/.ssh$ cat id_ed25519.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKUOOPy0e1+4EzuM5PYc1/lfsXrR9FFDxTxDztvCi0Ce gnome2

gnome1@pqc-server:~/.ssh$ ssh gnome2@127.0.0.1

==== gnome2

gnome2@pqc-server:~/.ssh$ cat id_mayo2.pub
ssh-mayo2 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 gnome3

ssh gnome3@127.0.0.1

==== gnome3

gnome3@pqc-server:~/.ssh$ ls -la
total 16
drwxr-xr-x    2 root     root          4096 Oct 29 00:29 .
drwxr-x---    1 gnome3   gnome3        4096 Oct 29 00:29 ..
-rw-------    1 gnome3   gnome3         744 Oct 29 00:29 id_ecdsa_nistp256_sphincssha2128fsimple
-rw-r--r--    1 gnome3   gnome3         265 Oct 29 00:29 id_ecdsa_nistp256_sphincssha2128fsimple.pub
gnome3@pqc-server:~/.ssh$ cat id_ecdsa_nistp256_sphincssha2128fsimple.pub
ssh-ecdsa-nistp256-sphincssha2128fsimple AAAAKHNzaC1lY2RzYS1uaXN0cDI1Ni1zcGhpbmNzc2hhMjEyOGZzaW1wbGUAAAAIbmlzdHAyNTYAAABBBL6fN38B6kQNiS0vAvGeGjAJ7Da2YbpBaAXkeeDJ3CJIUZc8PzNWCfzW5qN8z0RnS1/Hia1jRt6dydqeiVHBa9cAAAAgRAPMBt8y/4/YdBEw9OutMv37HJy50gIQfmzUY5d82Bg= gnome4

ssh gnome4@127.0.0.1

==== gnome4

gnome4@pqc-server:~/.ssh$ ls -la
total 28
drwxr-xr-x    2 root     root          4096 Oct 29 00:29 .
drwxr-x---    1 gnome4   gnome4        4096 Oct 29 00:29 ..
-rw-------    1 gnome4   gnome4       14396 Oct 29 00:29 id_ecdsa_nistp521_mldsa87
-rw-r--r--    1 gnome4   gnome4        3739 Oct 29 00:29 id_ecdsa_nistp521_mldsa87.pub
gnome4@pqc-server:~/.ssh$ cat id_ecdsa_nistp521_mldsa87.pub
ssh-ecdsa-nistp521-mldsa-87 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 admin

ssh admin@127.0.0.1


==== admin
admin@quantgnome_leap:~$ cd /opt/oqs-ssh/
admin@quantgnome_leap:/opt/oqs-ssh$ ls
bin                                       key-lookup.log                            moduli                                    scripts                                   ssh_config                                ssh_host_ecdsa_nistp521_mldsa-87_key.pub  sshd_config                               user-keys
flag                                      key-lookup.sh                             sbin                                      share                                     ssh_host_ecdsa_nistp521_mldsa-87_key      ssh_known_hosts                           sshd_logfile.log
admin@quantgnome_leap:/opt/oqs-ssh$ cd flag
admin@quantgnome_leap:/opt/oqs-ssh/flag$ ls
flag
admin@quantgnome_leap:/opt/oqs-ssh/flag$ cat flag
HHC{L3aping_0v3r_Quantum_Crypt0}
```

## Response

!!! quote "Charlie Goldner"

    That was wild—who knew quantum gnomes could hide so many secrets?

    Thanks for helping me leap into the future of cryptography!
