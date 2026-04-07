# Week 2 – BlueMoon Vulnerability Analysis

**Prepared by:** adam izzuddin
**Email:** adamizzuddin2004@gmail.com

## 1. Network Identification
Command: `ip a`
My Kali Linux is located at **192.168.56.102**.

[<img width="1017" height="736" alt="image" src="https://github.com/user-attachments/assets/b1a8b85e-7a9b-4058-a736-9b24987f8819" />](https://github.com/adamizzuddin-ai/Week2---Bluemoon/blob/main/image_2026-04-03_220719814.png?raw=true)


## 2. Host Discovery
Command: `nmap -sn 192.168.56.0/24`
The BlueMoon machine was identified at **192.168.56.101**.

<img width="1017" height="740" alt="image" src="https://github.com/user-attachments/assets/e7ceca35-1068-4f42-be8c-d4b61c93eb77" />



## 3. Service Enumeration
Command: `nmap -sV 192.168.56.101`
Discovered open ports: FTP (21), SSH (22), and HTTP (80).

[<img width="1012" height="711" alt="image" src="https://github.com/user-attachments/assets/f6ad3689-12e9-4db0-a939-479b2168fbbd" />](https://github.com/adamizzuddin-ai/Week2---Bluemoon/blob/main/README.md)


## 4. Web Enumeration
Accessed the target via the browser to confirm the active web service.

[<img width="1337" height="935" alt="image" src="https://github.com/user-attachments/assets/b3ede991-3e78-4f79-8b1b-31c0d346dd47" />](https://github.com/adamizzuddin-ai/Week2---Bluemoon/edit/main/README.md)


## 5. Directory Discovery
Command: `gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirb/common.txt`
The scan identified hidden directories and files including `/index.html`.

[<img width="992" height="757" alt="image" src="https://github.com/user-attachments/assets/2022b458-5cf1-4cf3-960c-1c0c74b65c62" />](https://github.com/adamizzuddin-ai/Week2---Bluemoon/edit/main/README.md)


# Penetration Testing Writeup: BlueMoon (IP: `192.168.56.101`)

A step-by-step analysis and exploitation guide for the **BlueMoon** CTF machine, as documented through the provided sequence of screenshots.

## Overview

This writeup details the complete penetration testing lifecycle of the target virtual machine, BlueMoon (`192.168.56.101`), running in an Oracle VirtualBox environment. The attack chain involves reconnaissance, credential discovery via QR code decoding, exploitation of FTP and SSH services, and multiple layers of privilege escalation to achieve root access.

## Toolset

The following tools were utilized during this assessment:

* **Gobuster v3.8:** Web directory and file enumeration.
* **Web-based QR Code Scanner:** Decoding sensitive information.
* **FTP Client:** Interacting with the File Transfer Protocol service.
* **Hydra v9.6:** SSH service brute-force attacking.
* **SSH Client:** Secure remote terminal access.
* **Linux Terminal Utility Tools:** Standard Bash commands for system interaction.
* **Docker:** Used as a vector for privilege escalation to root.

---

## Detailed Assessment Walkthrough

### Step 1: Web Directory Enumeration

The assessment began with reconnaissance against the target's web server. A directory enumeration scan was performed using **Gobuster**.

**Command Executed:**
`gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`

The scan identified a potentially interesting directory: **`/hidden_text`**.
<img width="968" height="722" alt="image" src="https://github.com/user-attachments/assets/b80bb1af-585c-42ef-ad75-08bded46615b" />


---

### Step 2: Information Gathering (Credential Discovery)

Accessing the `/hidden_text` directory (implied) revealed a QR code. A web-based QR code scanner was used to decode it.

The decoded result was a Bash script containing hardcoded FTP credentials:
* **FTP Host:** `ip` (implicitly the target IP)
* **FTP User:** `userftp`
* **FTP Password:** `ftpp@ssword`

The script also revealed the intention to automate a connection to retrieve information.
<img width="1415" height="543" alt="image" src="https://github.com/user-attachments/assets/9e2ef5ae-17a0-4c9c-b7f5-a9173ba2a321" />



### Step 3: Exploitation - Gaining Initial Footprint (FTP)

Using the newly discovered credentials, an **FTP** connection was successfully established with the BlueMoon machine.

**Command Executed:**
`ftp 192.168.56.101`

Once connected, the directory contents were listed (`ls`), revealing two files:
* `information.txt`
* `p_lists.txt`

Both files were downloaded to the attacker's machine using the `get` command for further analysis.
<img width="1005" height="767" alt="image" src="https://github.com/user-attachments/assets/1556e716-d8fa-44ea-b0f8-fb7c200b0731" />



### Step 4: Exploitation - Service Brute-Force (SSH)

The downloaded file `p_lists.txt` was identified as a password list. An SSH brute-force attack was launched against the system user `robin` using **Hydra**.

**Command Executed:**
`hydra -l robin -P p_lists.txt ssh://192.168.56.101`

Hydra successfully cracked the password for the user `robin`:
* **SSH User:** `robin`
* **SSH Password:** `k4rv3ndh4nh4ck3r`
<img width="797" height="678" alt="image" src="https://github.com/user-attachments/assets/828869d9-89ee-4e16-93b5-b7f0356a2425" />


### Step 5: Post-Exploitation - User-1 Flag (robin)

An SSH connection was established to BlueMoon as the user `robin`.

**Command Executed:**
`ssh robin@192.168.56.101`

After exploring the filesystem, the user located a file named `user1.txt` within a `/project` directory. Reading this file provided the first flag.

**Flag Found:**
`Fl4g{u5er1r34ch3d5ucc355fully}`
<img width="730" height="907" alt="image" src="https://github.com/user-attachments/assets/7e42b768-f98f-40ff-8181-67ce99f7b43a" />


### Step 6: Privilege Escalation - Lateral Movement (jerry)

The user `robin` discovered an exploitable Python script named "Script For Feedback" running with elevated privileges.

By exploiting a vulnerability in the input handling of the script (likely a command injection vulnerability), the attacker was able to escape the script's intended function and drop into a new shell as a different user: `jerry`.

Entering the feedback field with: `/bin/bash` provided the elevated shell.

The new user (`jerry`) then navigated to their home directory (`cd jerry`), located `user2.txt`, and retrieved the second flag.

**Flag Found:**
`Fl4g{Y0ur34ch3du53r25uc355ful1y}`
<img width="622" height="605" alt="image" src="https://github.com/user-attachments/assets/a7c872fc-17ad-4828-843b-afff46acb922" />


### Step 7: Privilege Escalation to Root (The Docker Breakout)

The user `jerry` attempted to gain root privileges. An initial attempt using `sudo -l` failed because the user did not know `jerry`'s account password.

However, the user `jerry` was identified as having permissions to execute **Docker** commands with superuser privileges. A known Docker "breakout" technique was employed to gain root access to the host machine's file system.

**Command Executed:**
`docker run -v /:/mnt --rm -it alpine chroot /mnt sh`

**Explanation:** This command runs a temporary container (`--rm`) using the `alpine` image. It mounts the BlueMoon host machine's entire root filesystem (`/`) to the container's `/mnt` directory (`-v /:/mnt`). The `chroot /mnt sh` command then changes the root directory for the current process to `/mnt`, giving the user a root shell over the host file system.

Now operating as root, the attacker navigated to `/root`, found `root.txt`, and retrieved the final flag.

**Root Flag Found:**
`Fl4g{r00t-H4ckTh3P14n3t0nc34g41n}`
<img width="771" height="736" alt="image" src="https://github.com/user-attachments/assets/044e7cbf-6f95-42d2-bdf8-228a3bfae306" />


