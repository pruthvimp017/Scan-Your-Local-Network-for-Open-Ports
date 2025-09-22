# Findings – Cybersecurity Internship Task 1

## 📍 Network Scanned
- Subnet: `192.168.220.0/24`  
- Date: 2025-09-22  
- Tool: Nmap (`-sS` TCP SYN scan)  
- Validation: Wireshark packet capture  

---

## 🖥️ Host Findings

### 🔹 Host: 192.168.220.1
- Status: **Up**
- Ports: All 1000 scanned ports filtered (no response)
- MAC: `00:50:56:C0:00:08` (VMware)
- **Analysis:** No visible services. Likely protected by firewall rules.
- **Risk:** Low exposure. System is alive but does not reveal services.
- **Mitigation:** Maintain firewall policies, monitor for unusual traffic.

---

### 🔹 Host: 192.168.220.2
- Status: **Up**
- Ports:
  - **53/tcp (DNS)** → **Filtered**
- MAC: `00:50:56:FE:89:4A` (VMware)
- **Analysis:** Possible DNS service, but filtered by firewall.
- **Risk:** If DNS is active, it may still be accessible internally. Misconfiguration could allow DNS tunneling or amplification attacks.
- **Mitigation:** Restrict DNS queries to trusted clients, keep patched, apply rate limiting.

---

### 🔹 Host: 192.168.220.254
- Status: **Up**
- Ports: All 1000 scanned ports filtered (no response)
- MAC: `00:50:56:EC:38:D5` (VMware)
- **Analysis:** Likely a virtual gateway or router device.
- **Risk:** Hidden behind firewall filtering. Still identifiable as active host.
- **Mitigation:** Keep firewall rules strict, disable unused services.

---

### 🔹 Host: 192.168.220.128
- Status: **Up**
- Ports: All 1000 scanned ports closed (reset responses)
- **Analysis:** Host replies with RST packets, confirming it is alive but no services are listening.
- **Risk:** Even though ports are closed, RST responses confirm host presence → attackers can use this to identify active devices.
- **Mitigation:** Use a firewall to drop packets instead of sending RST, reducing exposure.

---

## 📊 Wireshark Analysis
- **Filter Used:**  
  - `tcp.flags.syn == 1` → showed SYN packets from scanner (`192.168.220.128`) to multiple hosts.  
  - `tcp.port == 22` → confirmed RST/ACK responses for closed SSH ports.  
- **Observed Behavior:**  
  - SYN → SYN/ACK → RST pattern absent (no open ports).  
  - SYN → RST observed (closed ports, e.g., SSH).  
  - No response (filtered) for many ports.

---

## ⚠️ Key Observations
- **No open ports discovered** in the scanned subnet.  
- Hosts respond in three different ways:
  - **Filtered (firewall dropping packets)** → `.1` and `.254`  
  - **Filtered (specific port DNS)** → `.2`  
  - **Closed (RST responses)** → `.128`  
- Wireshark confirmed Nmap behavior (RST = closed, no response = filtered).

---

## ✅ Security Recommendations
1. Configure firewalls to **drop packets silently** instead of sending RSTs.  
2. Ensure DNS server (if active on `.2`) is hardened and restricted.  
3. Regularly patch and update all systems.  
4. Deploy intrusion detection (IDS/IPS) to log and respond to scans.  
5. Segment the network to reduce exposure of critical services.

---

## 📖 Conclusion
The scan revealed **no actively open ports**, but hosts are detectable.  
The primary lesson: **even closed/filtered ports leak information** about host presence and filtering. Proper firewall hardening and silent packet dropping are key to minimizing exposure.
