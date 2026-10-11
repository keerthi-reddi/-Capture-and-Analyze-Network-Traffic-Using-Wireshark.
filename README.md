# -Capture-and-Analyze-Network-Traffic-Using-Wireshark.
# Task 5: Capture and Analyze Network Traffic Using Wireshark

## Objective
Capture live network packets and identify basic protocols and traffic types.

## Tools
- Wireshark (Windows)
- Npcap (packet capture driver)

## Setup
- OS: Windows 11
- Interface: Wi-Fi (IPv4: 172.20.10.3)
- Capture duration: about 1 minute
- Total packets captured: 37,783 (0 dropped)

## Steps
1. Installed Npcap, then Wireshark.
2. Started a capture on the Wi-Fi interface.
3. Generated traffic by visiting wikipedia.org and github.com, running `ping -n 4 8.8.8.8`, and running `nslookup google.com`.
4. Filtered packets using `dns`, `tcp`, `udp`, `icmp`, `http`, and `tls`.
5. Identified protocols using Statistics > Protocol Hierarchy.
6. Saved the capture as a `.pcap` file.

## Results

### Ping (ICMP)
- `ping -n 4 8.8.8.8` sent 4 packets and received 4 replies (0% loss).
- Round-trip times: 40 ms, 137 ms, 173 ms, 61 ms (average 102 ms).
- In Wireshark, packet 23264 is the Echo (ping) request from 172.20.10.3 to 8.8.8.8, and packet 23265 is the reply.

### DNS
- `nslookup google.com` returned 142.251.43.46 and 2404:6800:4007:834::200e.
- Example DNS query: packet 173, requesting `self.events.data.microsoft.com` (type A). Most DNS traffic came from Windows and Microsoft background services.

### TCP and HTTP
- The `tcp` filter showed TLS 1.2 and 1.3 sessions on port 443, with normal ACK, PSH, and FIN exchanges.
- Some retransmissions appeared, such as packet 23311 (SYN retransmission to port 443).
- The `http` filter showed plain HTTP traffic, including OCSP certificate checks and a 408 Request Timeout response, which is normal for background requests.

### TLS
- Most web traffic was encrypted with TLS, so the page content could not be read in the capture.

## Key Findings
- Most traffic was encrypted (TLS), so `http` showed few results.
- DNS lookups were the first step before every web connection.
- The ICMP ping confirmed that the network path to 8.8.8.8 was working.
