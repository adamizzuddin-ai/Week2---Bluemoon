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

![Service Scan](<img width="1012" height="711" alt="image" src="https://github.com/user-attachments/assets/f6ad3689-12e9-4db0-a939-479b2168fbbd" />
)

## 4. Web Enumeration
Accessed the target via the browser to confirm the active web service.

![Web Interface](<img width="1337" height="935" alt="image" src="https://github.com/user-attachments/assets/b3ede991-3e78-4f79-8b1b-31c0d346dd47" />
)

## 5. Directory Discovery
Command: `gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirb/common.txt`
The scan identified hidden directories and files including `/index.html`.

![Directory Scan](<img width="992" height="757" alt="image" src="https://github.com/user-attachments/assets/2022b458-5cf1-4cf3-960c-1c0c74b65c62" />
)
