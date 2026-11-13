---
name: practical-packet-analysis
description: Teach and apply network packet analysis with Wireshark, TShark and tcpdump — how computers talk, where to place a sniffer, capture and display filters, following streams, statistics, ARP/IP/ICMP/TCP/UDP/DHCP/DNS/HTTP/SMTP, troubleshooting slow or broken networks, and spotting scans, spoofing and malware in traffic. Use when someone is learning Wireshark or networking for security, needs to read a pcap, wants a filter written or explained, asks why a network is slow, or wants an "explain like I'm 5" explanation of a protocol.
---

# Practical Packet Analysis — Skill

Knowledge distilled (in original words) from the concepts covered in *Practical Packet Analysis* (3rd ed.) by Chris Sanders (No Starch Press). Not affiliated with the author or publisher. No book text is reproduced; read the book for full depth.

## How to teach with this skill

1. **ELI5 first** — packets are envelopes, IP is the street address, ports are apartment numbers, TCP is a phone call, UDP is shouting across the room.
2. **Then the real thing** — the field, flag, or filter and what it means.
3. **Show the evidence** — what it looks like in Wireshark (column, flag, stream).
4. **Security angle** — what normal looks like vs. suspicious.
5. **Safe practice** — use your own network, a lab, or public sample captures (e.g., wiki.wireshark.org SampleCaptures, malware-traffic-analysis.net). Capturing other people's traffic without permission is illegal in most countries.
6. If the learner writes in Arabic, answer in Arabic and keep protocol names, fields and filters in English.

## Core concepts (by chapter theme)

| # | Theme | ELI5 analogy | Key ideas | Security angle |
|---|---|---|---|---|
| 1 | Network basics | Letters inside envelopes inside boxes | OSI layers, encapsulation, unicast/broadcast/multicast | Every attack still has to travel as packets |
| 2 | Tapping the wire | Where you stand to listen in a busy room | Promiscuous mode, hubs vs switches, port mirroring (SPAN), taps, ARP cache poisoning | Sniffer placement decides what you can see; ARP poisoning is also an attack |
| 3 | Wireshark intro | A microscope for network traffic | Packet list / details / bytes panes, preferences, profiles | Separate profiles for troubleshooting vs. security hunting |
| 4 | Captured packets | Sorting mail by sender and type | Save/merge/export, time formats, ring buffers, **capture filters (BPF)** vs **display filters** | Filter early or drown in noise |
| 5 | Advanced features | Phone bill: who called whom, how long | Endpoints, Conversations, Protocol Hierarchy, Follow TCP/HTTP Stream, IO graphs, Expert Info | Unusual conversations and protocols stand out in statistics |
| 6 | Command line | Wireshark without the screen | `tcpdump -i eth0 -w file.pcap`, `tshark -r file.pcap -Y "dns"`, `-z` statistics | Capture on servers with no GUI |
| 7 | Network layer | Street addresses and "is anyone home?" | ARP request/reply, IPv4/IPv6 header, TTL, fragmentation, ICMP echo/unreachable, traceroute | Gratuitous/duplicate ARP = spoofing sign; ICMP sweeps = recon |
| 8 | Transport layer | TCP is a phone call; UDP is a shout | 3-way handshake (SYN, SYN/ACK, ACK), FIN/RST teardown, sequence numbers, ports | Many SYNs to many ports with RST replies = port scan |
| 9 | Upper-layer protocols | The language spoken on the call | DHCP DORA, DNS query/response, HTTP methods & status codes, SMTP | Plain HTTP/SMTP shows passwords and content in clear text; odd DNS = tunneling or C2 |
| 10 | Real-world scenarios | Detective work from footprints | Missing content, no internet, unreachable host — compare the failing flow to a working one | Baseline "normal" first, then hunt for the difference |
| 11 | Slow networks | A traffic jam and lost letters | Retransmissions, duplicate ACKs, zero window, window size, latency location (client/server/wire), baselining | Sudden slowness can also be exfiltration or a DoS |
| 12 | Security analysis | Spotting a burglar casing the street | SYN scans, OS fingerprinting, ARP poisoning, session hijacking, malware beaconing, exploit kits/ransomware traffic | Recognize patterns; confirm with threat intel |
| 13 | Wireless | Radios shouting through the walls | Monitor mode, 802.11 management/control/data frames, channels, WEP/WPA handshakes | Open Wi-Fi exposes everything; capture only networks you own |

## Display filters cheat sheet

```text
ip.addr == 192.168.1.10          # traffic to/from a host
tcp.port == 443                  # a specific port
http.request                     # web requests
dns                              # DNS only
arp                              # ARP only
tcp.flags.syn == 1 && tcp.flags.ack == 0   # connection attempts (scan hunting)
tcp.flags.reset == 1             # resets (closed ports / refused)
tcp.analysis.retransmission      # lost packets → slowness
tcp.analysis.zero_window         # receiver overwhelmed
http.request.method == "POST"    # form submissions
frame contains "password"        # cleartext secrets (lab only)
!(arp || dns || icmp)            # remove noise
```

## Capture filters (BPF) — set before capturing

```text
host 192.168.1.10
port 53
tcp and not port 22
net 10.0.0.0/8
```

## Command line quick start

```bash
sudo tcpdump -i eth0 -c 100 -w lab.pcap          # capture 100 packets
tcpdump -r lab.pcap -nn 'tcp port 80'            # read with a filter
tshark -r lab.pcap -Y "http.request" -T fields -e ip.src -e http.host -e http.request.uri
tshark -r lab.pcap -q -z conv,ip                 # top conversations
tshark -r lab.pcap -q -z io,phs                  # protocol hierarchy
```

## Investigation workflow

1. **Question first** — what exactly is broken or suspicious?
2. **Place the sniffer** as close to the problem as possible.
3. **Capture** with a capture filter if traffic is heavy.
4. **Statistics** — Protocol Hierarchy, Conversations, Endpoints.
5. **Filter & follow** the relevant stream.
6. **Compare** with a known-good capture (baseline).
7. **Conclude** with evidence: packet numbers, timestamps, flags.

## Guardrails

- Only capture networks and devices you own or are authorized to monitor.
- Pcaps can contain passwords, cookies and personal data — store and share them carefully.
- Open real malware captures only in an isolated VM; never re-run extracted files.

## Learning path

Previous: `linux-basics-for-hackers` · Next: `social-engineering` → `penetration-testing-weidman` → `wireshark-for-security-professionals`.
