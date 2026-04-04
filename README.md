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


# Penetration Testing Report: BlueMoon-2021

## 1. Web Enumeration & Directory Discovery
Using **Gobuster**, a hidden directory was discovered at `/hidden_text`.
<img width="968" height="722" alt="image" src="https://github.com/user-attachments/assets/b80bb1af-585c-42ef-ad75-08bded46615b" />


## 2. FTP Data Exfiltration
Credentials from a QR code were used to log into FTP and retrieve `p_lists.txt`.
<img width="1415" height="543" alt="image" src="https://github.com/user-attachments/assets/9e2ef5ae-17a0-4c9c-b7f5-a9173ba2a321" />



## 3. SSH Brute Force
**Hydra** cracked the password for user `robin`: `k4rv3ndh4nh4ck3r`.
![Hydra Success](<img width="1005" height="767" alt="image" src="https://github.com/user-attachments/assets/12ad940d-ed9d-4df7-86ff-2a0fffa2290f" />
)

## 4. Initial Access (User-1 Flag)
Accessed the system via SSH and retrieved the first flag.
![User-1 Flag](image_9e8c69.png)

## 5. Privilege Escalation Path
Analysis of sudo permissions showed that `robin` can run a script as `jerry`.
![Sudo Permissions](image_9e9c42.png)

## 6. Escalation to User-2 (Jerry Flag)
Exploited command injection in the feedback script to become `jerry`.
![User-2 Flag](image_9f1aea.png)

## 7. Final Root Compromise
Exploited `jerry`'s membership in the **docker** group to escape to the host filesystem as **root**.
![Root Flag](image_9f75d9.png)

