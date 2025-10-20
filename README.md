# 🕵️‍♂️ Nmap TCP SYN Scan (-sS) Guide ⚡

---

## **Goal**
**Use Nmap’s TCP SYN scan (`-sS`) to discover live hosts and open TCP ports on a local network.**  
This scan is fast, informative, and relatively stealthy.

> 🚨 **WARNING: LEGAL & ETHICAL USE ONLY** 🚨  
> Never scan a network you do not own or have explicit, written permission to test. Unauthorized scanning is intrusive, may be illegal, and can trigger security alerts and defensive actions. Use this information responsibly and ethically. ✍️🛑

---

## 🚀 **Quick Overview**
This guide demonstrates how to:

- Find your local network range.  
- Run a TCP SYN scan with Nmap.  
- Read and interpret the scan results.  
- Save results and follow up with deeper probes.

A TCP SYN Scan (aka "half‑open" scan) sends a SYN packet and waits for a SYN/ACK. If SYN/ACK is received, the port is considered **open** and Nmap sends a RST to tear down the connection — making it faster and less noisy than a full TCP connect (`-sT`) scan. 🧩

---

## ✅ **Prerequisites**
Before you begin, you must have:

- **Nmap Installed** — download from https://nmap.org.  
- **Sudo/Administrator Privileges** — `-sS` requires raw packet creation.  
- **Explicit Permission** — written authorization to scan the target network.

---

## 🔍 **How to Run the Scan**

### **Step 1: Find Your Local IP Range**
Determine your host's IP address and subnet.

- **Linux / macOS**
  ```bash
  ip a

Windows (PowerShell / CMD)

ipconfig

Find the IPv4 and subnet (e.g., 192.168.1.23/24) and derive the network range (e.g., 192.168.1.0/24).

Step 2: Start the TCP SYN Scan

Run Nmap with the -sS flag (you will likely be prompted for password due to sudo):

# Scan a single IP
sudo nmap -sS <target-ip>

# Scan an entire subnet
sudo nmap -sS <target-subnet>     # e.g., sudo nmap -sS 192.168.1.0/24

📊 Reading the Results

Nmap will list each host it finds up and show the state of ports. Below are example output highlights and explanations.

Example Scan Output & Analysis
# SCAN ANALYSIS

Nmap scan report for <target-ip-1>
Host is up (0.0012s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain


Not shown: 999 closed tcp ports (reset): Nmap scanned the default 1000 ports and hid the 999 that were closed to keep output concise.

53/tcp open domain: DNS service detected on port 53. 🧭

Nmap scan report for <target-ip-2>
Host is up (0.0020s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT    STATE SERVICE
135/tcp open  msrpc
445/tcp open  microsoft-ds


Not shown: 998 filtered tcp ports (no-response): Many ports dropped or ignored probes (likely firewall).

Ports 135 and 445 open → likely a Windows host (RPC and SMB). 🔐🪟

Nmap scan report for <target-ip-3>
Host is up (0.00040s latency).
All 1000 scanned ports on <target-ip-3> are in ignored states.


Host responded to host discovery but all scanned ports are filtered/ignored — likely a strict firewall.

Nmap done: 256 IP addresses (4 hosts up) scanned in 16.85 seconds


Final summary: a /24 (256 addresses) was scanned and 4 hosts were up.⏱️


🔎 Common Port States

✅ open — Port is accepting connections (service running).

❌ closed — TCP reachable, but no service listening (RST returned).

🚧 filtered — No response; probe was dropped or blocked (state unknown).

💡 Troubleshooting & Tips

Many filtered ports → firewall likely dropping probes. Try -sT (full connect) only if permitted — it's louder and more likely to be logged.

Speed up scans → --top-ports 100 to scan the 100 most common ports.

Specify ports → -p 80,443,8080 to scan only certain ports.

Stealthier scanning → slow timing: -T2 or -T1 to reduce likelihood of triggering basic IDS.

Service/version detection → add -sV to probe services for versions.

Save results → -oN (normal), -oX (XML), -oG (grepable), -oA (all formats).

Examples:

# Save normal output
sudo nmap -sS 192.168.1.0/24 -oN scan-results.txt

# Save in all formats (normal, XML, grepable)
sudo nmap -sS 192.168.1.0/24 -oA scan-results

📚 References

Nmap Official Documentation: https://nmap.org/docs.html

📜 License

This README is provided as‑is for educational purposes. Use this information at your own risk.