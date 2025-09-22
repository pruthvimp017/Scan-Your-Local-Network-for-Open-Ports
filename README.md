# Cybersecurity Internship Task 1 – Network Reconnaissance with Nmap & Wireshark

## 📌 Objective
The goal of this task was to perform a **basic network reconnaissance** on the local subnet `192.168.220.0/24` using **Nmap** and optionally analyze the traffic with **Wireshark**.  
This helps in understanding:
- How port scanning works  
- How services are exposed on a network  
- How to identify potential risks  

---

## 🛠️ Tools Used
- **Nmap v7.94SVN** – for network and port scanning  
- **Wireshark v4.x** – for packet-level analysis  
- **Linux (Ubuntu/Debian-based)** environment  

---

## 🔎 Steps Performed

1. **Installed Nmap & Wireshark**  
   ```bash
   sudo apt update && sudo apt install nmap wireshark xsltproc -y
