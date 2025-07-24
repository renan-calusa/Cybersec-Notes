
| Command                        | Description                               |
| ------------------------------ | ----------------------------------------- |
| `locate nmap/scripts/{script}` | List various available nmap scripts       |
| `--packet-trace`               | Follow and display requests and responses |
| `--reason`                     | Explain results                           |
| `--disable-arp-ping`           | Disable ARP ping                          |
| `-n`                           | Disable DNS resolution                    |

* Usually `-n` and `--disable-arp-ping` are used with `--packet-trace` in order to the feed become cleaner
# Host Discovery

| Command | Description                                     |
| ------- | ----------------------------------------------- |
| `-sn`   | Disable port scanning, just ICMP Echo           |
| `-Pn`   | Threat all hosts as online, skip host discovery |
| `-PE`   | Ping scan with ICMP Echo                        |
| `-sL`   | List of targets to scan                         |

`sudo nmap <ip addrss>/<range> -sn`

To determine if hosts are alive or receiving packets, without looking into it's ports and services. It's not possible to determine if a firewall is in place.

With `-sn` nmap automatically uses  **ICMP ECHO Requests** (`-PE`)

By analyzing the TTL of the ICMP Requests, we can solve what the OS probably is
* Windows: 128
* Unix/Linux: 64

# Port Scan

| Command                   | Description                    |
| ------------------------- | ------------------------------ |
| `-p {port}, -p-, -p{i-f}` | Specify the port range         |
| `--top-ports=x`           | Scan for the X most used ports |
//

## Port States
![[Pasted image 20250508171644.png]]
# Scan Techniques

| Command | Description                       |
| ------- | --------------------------------- |
| `-sS`   | TCP-SYN scan, just one SYN packet |
| `-sT`   | Uses full TCP Three-way handshake |

When using `sudo` nmap defaults to use `-sS`, otherwise it uses `-sT`


----

##### Resources
[# NMAP Full Guide](https://youtu.be/JHAMj2vN2oU?si=2n2NOmU1OPhEBFPp)
[# Advanced NMAP Techniques](https://www.youtube.com/watch?v=7XMIFTRiAGA&pp=ugMICgJwdBABGAHKBQ1hZHZhbmNlZCBubWFw)
[# Firewall and IDS Evasion techniques](https://youtu.be/TjGZJAKfgGI?si=mPAZgLgsnyD-3lEF)
