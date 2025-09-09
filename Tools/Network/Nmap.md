# General

| Command                        | Description                                                 |
| ------------------------------ | ----------------------------------------------------------- |
| `locate nmap/scripts/{script}` | List various available nmap scripts                         |
| `--packet-trace`               | Follow and display requests and responses                   |
| `--reason`                     | Explain results                                             |
| `--disable-arp-ping`           | Disable ARP ping                                            |
| `-n`                           | Disable DNS resolution, usually for performance improvement |
| `-O`                           | Enable OS detection                                         |
| `-v, -vv, -vvv`                | Verbose levels (Also can press " " while scanning)          |

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

With `-sn` nmap automatically uses  **ICMP ECHO Requests** (`-PE`), 

By analyzing the TTL of the ICMP Requests, we can solve what the OS probably is
* Windows: 128
* Unix/Linux: 64

# Port & Service Scan

| Command                         | Description                                         |
| ------------------------------- | --------------------------------------------------- |
| `-p {port}, -p-, -p{i-f}`       | Specify the port range                              |
| `--top-ports=<number>`          | Scan for the \<number> most used ports              |
| `--exclude-ports <port ranges>` | Exclude the specified ports from scanning           |
| `-sV`                           | Probe open ports to determine service/version info  |
| `--version-trace`               | Show detailed version scan activity (for debugging) |
| `--version-intensity <0-9>`     | Set version scan intensity                          |

## Port States

![[Pasted image 20250909092151.png]]

# Scan Techniques

| Command                       | Description                                                   |
| ----------------------------- | ------------------------------------------------------------- |
| `-sS`                         | TCP-SYN scan, just one SYN packet                             |
| `-sT`                         | Uses full TCP Three-way handshake                             |
| `-sU`                         | Perform a UDP scan                                            |
| `--scanflags`                 | Create a custom scan with TCP flags                           |
| `-sI zombie_host[:probeport]` | Truly blind TCP port scan of the target, using an zombie host |

When using `sudo` nmap defaults to use `-sS`, otherwise it uses `-sT`.

The `-sU` is useful because UDP scanning is generally slower and more difficult than TCP, some security auditors ignore these ports. This is a mistake, as exploitable UDP services are quite common. But we cannot determine if the UDP packet has arrived at all or not. If the UDP port is `open`, we only get a response if the application is configured to do so.

It can be combined with a TCP scan type such as SYN scan to check both protocols during the same run.

In `-sI` no packets are sent to the target from your real IP address, instead, a unique side-channel attack exploits predictable IP fragmentation ID sequence generation on the zombie host to glean information about the open ports on the target. IDS systems will display the scan as coming from the zombie machine you specify (which must be up and meet certain criteria).

You can add a colon followed by a port number to the zombie host if you wish to probe a particular port on the zombie for IP ID changes. Otherwise Nmap will use the port it uses by default for TCP pings (80).

# Nmap Script Engine (NSE)

| Command                                                     | Description                                                                                                                            |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `-sC`                                                       | Performs a script scan using the default set of scripts                                                                                |
| `--script filename\|category\|directory/\|expression[,...]` | Runs a script scan using the comma-separated list of filenames, script categories, and directories                                     |
| `--script-args n1=v1,n2={n3=v3},n4={v4,v5}`                 | Lets you provide arguments to NSE scripts                                                                                              |
| `--script-updatedb`                                         | Updates the script database found in scripts/script.db which is used by Nmap to determine the available default scripts and categories |

Each script contains a field associating it with one or more categories. Currently defined categories are auth, broadcast, default.  discovery, dos, exploit, external, fuzzer, intrusive, malware, safe, version, and vuln.

![[Pasted image 20250909101329.png]]

# Timing & Performance

| Command                            | Description                                                           |
| ---------------------------------- | --------------------------------------------------------------------- |
| `--initial-rtt-timeout <number>ms` | Set start timeout time                                                |
| `--max-rtt-timeout <number>ms`     | Set max timeout time                                                  |
| `--max-retries <number>`           | Set the retry rate of sent packages, default is 10                    |
| `--min-rate <number>`              | Tell nmap to simultaneously send the specified number of packets      |
| `-T <0-5>`                         | Timing templates, determine the aggressiveness of the scan (5 is max) |

We can optimize the scan duration by changing **timeout** times, nmap use default `RTT (Round-Trip-Time)` of 100ms per port. We can ajust the (min, max) values for RTT as
```shell
nmap ... --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

In a white-box environment we usually use `--min-rate` but for black-box, the `-T` are more useful

# Output

| Command | Description                     |
| ------- | ------------------------------- |
| `-oN`   | Normal output with **.nmap**    |
| `-oG`   | Grepable output with **.gnmap** |
| `-oX`   | XML output with **.xml**        |
| `-oA`   | All output formats              |

To convert the stored results from XML format to HTML, we can use the tool `xsltproc`
```shell
xslproc output.xml -o output.html
```

# Firewall & IDS/IPS Evasion






---------------
##### Resources
[# NMAP Full Guide](https://youtu.be/JHAMj2vN2oU?si=2n2NOmU1OPhEBFPp)
[# Advanced NMAP Techniques](https://www.youtube.com/watch?v=7XMIFTRiAGA&pp=ugMICgJwdBABGAHKBQ1hZHZhbmNlZCBubWFw)
[# Firewall and IDS Evasion techniques](https://youtu.be/TjGZJAKfgGI?si=mPAZgLgsnyD-3lEF)
