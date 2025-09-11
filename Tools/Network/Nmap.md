# General

| Command                        | Description                                                 |
| ------------------------------ | ----------------------------------------------------------- |
| `locate nmap/scripts/{script}` | List various available nmap scripts                         |
| `--packet-trace`               | Follow and display requests and responses                   |
| `--reason`                     | Explain results                                             |
| `-O`                           | Enable OS detection                                         |
| `-v, -vv, -vvv`                | Verbose levels (Also can press " " while scanning)          |

# Target Specification

| Command                  | Description                                                                           |
| ------------------------ | ------------------------------------------------------------------------------------- |
| `-iL <file>`             | Pass a list of hosts to scan                                                          |
| `--exclude <host1>[...]` | Specifies targets to be excluded from the scan                                        |
| `--excludefile <file>`   | Same thing but with a file                                                            |
| `--unique`               | Scan each address only once                                                           |
| `<ip_address>/subnet`    | Nmap accepts the subnetting format                                                    |
| `10.10.1.0-255`          | Nmap accepts the IP range format                                                      |
| `-n`                     | Disable DNS resolution, usually for performance improvement                           |
| `-R`                     | Reverse DNS resolution for all targets (Normally only performed against online hosts) |

Nmap also has several other **DNS** options to customize from.

# Host Discovery

| Command                                    | Description                                                                         |
| ------------------------------------------ | ----------------------------------------------------------------------------------- |
| `-sn`                                      | Disable port scanning                                                               |
| `-Pn`                                      | Threat all hosts as online, skip host discovery                                     |
| `--disable-arp-ping`                       | Disable ARP ping for targets in the local network                                   |
| `--traceroute`                             | Trace path to host                                                                  |
| `-PS <port_list>`                          | Sends an empty TCP packet with the SYN flag set                                     |
| `-PA <port_list>`                          | Sends an empty TCP packet with the ACK flag set                                     |
| `-PU <port_list>`                          | Sends an empty UDP packet                                                           |
| `-PO <port_list>`                          |                                                                                     |
| `--data`, `--data-string`, `--data-length` | Packet content can be affected by these flags if you don't wanna send empty packets |

If no host discovery options are given, Nmap sends an ICMP echo request, a TCP SYN packet to port 443, a TCP ACK packet to port 80, and an ICMP timestamp request - equivalent to the `-PE -PS443 -PA80 -PP` options.

The `-P*` options (which select ping types) can be combined. You can increase your odds of penetrating strict firewalls by sending many probe types using different TCP ports/flags and ICMP codes.

Also note that ARP/Neighbor Discovery is done by default against targets on a local Ethernet network even if you specify other options, because it is almost always faster and more effective.

`-PS`
	The SYN flag suggests to the remote system that you are attempting to establish a connection. Normally the destination port will be closed, and a RST (reset) packet sent back. If the port happens to be open, the target will take the second step of a TCP three-way-handshake by responding with a SYN/ACK TCP packet. The machine running Nmap then tears down the nascent connection by responding with a RST rather than sending an ACK packet which would complete the three-way-handshake and establish a full connection. The RST packet is sent by the kernel of the machine running Nmap in response to the unexpected SYN/ACK, not by Nmap itself.
	

* Nmap does not care whether the port is open or closed. Either the RST or SYN/ACK response discussed previously tell Nmap that the host is available and responsive.

`-PA`
	The TCP ACK ping is quite similar to the just-discussed SYN ping. The difference, as you could likely guess, is that the TCP ACK flag is set instead of the SYN flag. Such an ACK packet purports to be acknowledging data over an established TCP connection, but no such connection exists. So remote hosts should always respond with a RST packet, disclosing their existence in the process.

- The reason for offering both SYN and ACK ping probes is to maximize the chances of bypassing firewalls. Many administrators configure routers and other simple firewalls to block incoming SYN packets except for those destined for public services like the company web site or mail server. This prevents other incoming connections to the organization, while allowing users to make unobstructed outgoing connections to the Internet. This non-stateful approach takes up few resources on the firewall/router and is widely supported by hardware and software filters. When stateless firewall rules such as this are in place, SYN ping probes (`-PS`) are likely to be blocked when sent to closed target ports. In such cases, the ACK probe shines as it cuts right through these rules.

- Another common type of firewall uses stateful rules that drop unexpected packets. This feature was initially found mostly on high-end firewalls, though it has become much more common over the years. A SYN probe is more likely to work against such a system, as unexpected ACK packets are generally recognized as bogus and dropped. A solution to this quandary is to send both SYN and ACK probes by specifying `-PS` and `-PA`.

`-PU`
	A highly uncommon port is used by default because sending to open ports is often undesirable for this particular scan type. Upon hitting a closed port on the target machine, the UDP probe should elicit an ICMP port unreachable packet in return. This signifies to Nmap that the machine is up and available. Many other types of ICMP errors, such as host/network unreachables or TTL exceeded are indicative of a down or unreachable host. A lack of response is also interpreted this way. If an open port is reached, most services simply ignore the empty packet and fail to return any response. This is why the default probe port is 40125, which is highly unlikely to be in use. The primary advantage of this scan type is that it bypasses firewalls and filters that only screen TCP.

In some cases, firewalls may spoof TCP reset (RST) replies in response to probes to unoccupied or disallowed addresses. Since Nmap ordinarily considers RST replies to be proof that the target is up, this can lead to wasted time scanning targets that aren't there. Using the `--discovery-ignore-rst` will prevent Nmap from considering these replies during host discovery. You may need to select extra host discovery options to ensure you don't miss targets in this case.

# Port Scan Techniques

| Command                       | Description                                                   |
| ----------------------------- | ------------------------------------------------------------- |
| `-sS`                         | TCP-SYN scan, just one SYN packet                             |
| `-sT`                         | Uses full TCP Three-way handshake                             |
| `-sU`                         | Perform a UDP scan                                            |
| `--scanflags`                 | Create a custom scan with TCP flags                           |
| `-sI zombie_host[:probeport]` | Truly blind TCP port scan of the target, using an zombie host |

When using `sudo` nmap defaults to use `-sS`, otherwise it uses `-sT`.

Only one method may be used at a time, except that UDP scan (`-sU`) and any one of the SCTP scan types (`-sY`, `-sZ`) may be combined with any one of the TCP scan types.




The `-sU` is useful because UDP scanning is generally slower and more difficult than TCP, some security auditors ignore these ports. This is a mistake, as exploitable UDP services are quite common. But we cannot determine if the UDP packet has arrived at all or not. If the UDP port is `open`, we only get a response if the application is configured to do so.

It can be combined with a TCP scan type such as SYN scan to check both protocols during the same run.

In `-sI` no packets are sent to the target from your real IP address, instead, a unique side-channel attack exploits predictable IP fragmentation ID sequence generation on the zombie host to glean information about the open ports on the target. IDS systems will display the scan as coming from the zombie machine you specify (which must be up and meet certain criteria).

You can add a colon followed by a port number to the zombie host if you wish to probe a particular port on the zombie for IP ID changes. Otherwise Nmap will use the port it uses by default for TCP pings (80).

# Port & Service Scan

| Command                         | Description                                         |
| ------------------------------- | --------------------------------------------------- |
| `-p <port>, -p{i-f}, -p-`       | Specify the port range                              |
| `--top-ports=<number>`          | Scan for the \<number> most used ports              |
| `--exclude-ports <port ranges>` | Exclude the specified ports from scanning           |
| `-sV`                           | Probe open ports to determine service/version info  |
| `--version-trace`               | Show detailed version scan activity (for debugging) |
| `--version-intensity <0-9>`     | Set version scan intensity                          |
## Port States

`open`
An application is actively accepting TCP connections, UDP datagrams or SCTP associations on this port.

`closed`
A closed port is accessible (it receives and responds to Nmap probe packets), but there is no application listening on it. They can be helpful in showing that a host is up on an IP address (host discovery, or ping scanning), and as part of OS detection.

`filtered`
Nmap cannot determine whether the port is open because packet filtering prevents its probes from reaching the port. The filtering could be from a dedicated firewall device, router rules, or host-based firewall software. Filters that simply drop probes without responding are common. This forces Nmap to retry several times just in case the probe was dropped due to network congestion rather than filtering. This slows down the scan dramatically.

`unfiltered`
The unfiltered state means that a port is accessible, but Nmap is unable to determine whether it is open or closed. Only the ACK scan, which is used to map firewall rulesets, classifies ports into this state. Scanning unfiltered ports with other scan types such as Window scan, SYN scan, or FIN scan, may help resolve whether the port is open.

`open|filtered`
Nmap places ports in this state when it is unable to determine whether a port is open or filtered. This occurs for scan types in which open ports give no response. The lack of response could also mean that a packet filter dropped the probe or any response it elicited. So Nmap does not know for sure whether the port is open or being filtered. The UDP, IP protocol, FIN, NULL, and Xmas scans classify ports this way.

`closed|filtered`
This state is used when Nmap is unable to determine whether a port is closed or filtered. It is only used for the IP ID idle scan.

# Nmap Script Engine (NSE)

| Command                                                     | Description                                                                                                                            |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `-sC`                                                       | Performs a script scan using the default set of scripts                                                                                |
| `--script filename\|category \|directory\|expression[,...]` | Runs a script scan using the comma-separated list of filenames, script categories, and directories                                     |
| `--script "expression1 and expression2"`                    | Nmap accepts expressions                                                                                                               |
| `--script "ssh*"`                                           | Nmap accepts regEx                                                                                                                     |
| `--script-help "ssh*"`                                      | Shows which scripts found and give their description                                                                                   |
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
| `--min-rate <number>`              | Tell nmap to simultaneously send the specified number of packets/s    |
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
xsltproc output.xml -o output.html
```

# Firewall & IDS/IPS Evasion






---------------
##### Resources
[# NMAP Full Guide](https://youtu.be/JHAMj2vN2oU?si=2n2NOmU1OPhEBFPp)
[# Advanced NMAP Techniques](https://www.youtube.com/watch?v=7XMIFTRiAGA&pp=ugMICgJwdBABGAHKBQ1hZHZhbmNlZCBubWFw)
[# Firewall and IDS Evasion techniques](https://youtu.be/TjGZJAKfgGI?si=mPAZgLgsnyD-3lEF)
