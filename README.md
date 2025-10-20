🕵️‍♂️ Nmap TCP SYN Scan (-sS) Guide ⚡
Goal: Use Nmap’s TCP SYN scan (-sS) to discover live hosts and open TCP ports on a local network. This scan is fast, informative, and relatively stealthy.

🚨 WARNING: LEGAL & ETHICAL USE ONLY 🚨

Never scan a network you do not own or have explicit, written permission to test. Unauthorized scanning is intrusive, may be illegal, and can trigger security alerts and defensive actions. Use this information responsibly and ethically. ✍️🛑

🚀 Quick Overview
This guide demonstrates how to:

Find your local network range.

Run a TCP SYN scan with Nmap.

Read and interpret the scan results.

Save results and follow up with deeper probes.

A TCP SYN Scan is often called a "half-open" scan. It works by sending a SYN packet (as if to start a connection) and waiting for a SYN/ACK response. If one is received, the port is open, and Nmap immediately sends a RST (reset) packet to tear down the connection before it's fully established. This makes it faster and less "noisy" in logs than a full TCP connect (-sT) scan. 🧩

✅ Prerequisites
Before you begin, you must have:

Nmap Installed: Download the official installer from nmap.org.

Sudo/Administrator Privileges: The -sS scan requires raw packet creation, which needs elevated permissions.

Explicit Permission: Written authorization to scan the target network.

🔍 How to Run the Scan
Step 1: Find Your Local IP Range
First, you need to determine your host's IP address to identify the network range you want to scan (e.g., 192.168.1.0/24).

On Linux / macOS:

Bash

ip a
On Windows (PowerShell / CMD):

Bash

ipconfig
Look for your ipv4 address and subnet mask.

Step 2: Start the TCP SYN Scan
Run Nmap with the -sS flag and your target. You will be prompted for your password because this scan requires sudo.

Bash

# Scan a single IP address
sudo nmap -sS 10.0.2.15

# Scan an entire subnet
sudo nmap -sS 10.0.2.0/24
📊 Reading the Results
The output will show you the state of scanned ports for each "up" host.

Example Output Highlights
Here are common lines from an Nmap scan and what they mean:

Plaintext

Nmap scan report for 10.0.2.15
Host is up (0.0012s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain
Not shown: 999 closed tcp ports (reset): Nmap scanned the default 1,000 ports and is hiding the 999 that were closed (responded with RST) to keep the output clean.

53/tcp open domain: This host is running a service on port 53, which Nmap identifies as DNS (domain). 🧭

Plaintext

Nmap scan report for 10.0.2.2
Host is up (0.0020s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT    STATE SERVICE
135/tcp open  msrpc
445/tcp open  microsoft-ds
Not shown: 998 filtered tcp ports (no-response): This host (or a firewall in front of it) filtered (dropped or ignored) probes on 998 ports.

135/tcp open & 445/tcp open: This is very likely a Windows host, as it has ports for RPC and SMB open. 🔐🪟

Plaintext

Nmap scan report for 10.0.2.3
Host is up (0.00040s latency).
All 1000 scanned ports on 10.0.2.3 are in ignored states.
All 1000 scanned ports ... are in ignored states: No ports responded. The host is up (it responded to Nmap's initial host-discovery probe), but all scanned ports are filtered, probably by a firewall.

Plaintext

Nmap done: 256 IP addresses (4 hosts up) scanned in 16.85 seconds
Nmap done...: This is the final summary. Here, a full /24 (256 addresses) was scanned, and 4 hosts were found to be online. ⏱️

Common Port States
✅ open: The port is actively accepting connections. A service is running.

❌ closed: The port is reachable, but no service is listening. The host responded with a TCP RST (Reset) packet.

🚧 filtered: Nmap cannot determine the state. Probes were dropped (no response) or blocked by a firewall. The port may be open or closed.

💡 Troubleshooting & Tips
If you see many filtered ports: A firewall is likely dropping your probes. You can try a full TCP Connect scan (-sT), but be aware this is much "louder" and easily logged by monitoring systems.

To speed up scans: Use --top-ports 100 to scan only the 100 most common ports, or specify ports manually with -p 80,443,8080.

For stealthier scans: Slow down your scan with timing templates (e.g., -T2 or -T1) to help avoid tripping basic Intrusion Detection Systems (IDS).

📚 References
Nmap Official Documentation: https://nmap.org/docs.html

📜 License
This README is provided as-is for educational purposes. Use this information at your own risk.