# Incident Summary

In an office network, an employee's PC was compromised by STRRAT malware that performed C2 connections with an external server and sent its system information to the server. The C2 communication started around **02:40:05 UTC** over **TCP port 12132**. The network traffic around the suspicious activity was captured and analyzed using Suricata (connected through wazuh offline PCAP) and Wireshark.

# Findings

Following the Suricata events, the first **ET Malware C2 Check-in** alert was investigated in Wireshark around the same timeline and showed the victim's machine communicating with an external server. By following the TCP stream, plaintext system information sent from the victim's machine was detected, including the PC hostname, username, country, operating system information, and other details, along with the malware name itself (**STRRAT**).

All later C2 events contained repeated TCP beaconing ("ping") between the victim's machine and the external server.

The Suricata event just before the first C2 check-in showed an external DNS lookup for **ip-api.com** at **02:39:57 UTC**, which increased confidence in the investigation. Following the HTTP stream showed a `GET /json/` request used to retrieve the victim's public IP address and location.

In order to investigate how the machine got compromised, its communication with web traffic, SMB file sharing, and email traffic was analyzed. It was found that only milliseconds before the "ip-api.com" lookup, the victim's machine connected to **github.com** and then downloaded a large amount of Java libraries from **repo1.maven.org**. Visiting GitHub and downloading files or Java libraries are normal activities by themselves and do not prove malicious activity. However, since STRRAT is Java-based and these connections happened only milliseconds before the C2 communication, they may be related to the malware's installation or execution. Due to encrypted HTTPS traffic, this could not be confirmed.

# Indicators of Compromise (IOCs)

 ## Vivtim Details
 • Host name: DESKTOP-SKBR25F
 • IP address: 172.16.1.66
 • MAC address: 00:1e:64:ec:f3:08
 • Windows user account name: ccollier

Suspicious traffic to file sharing domains:
• port 443 - github.com - HTTPS traffic
• port 443 - objects.githubusercontent.com - HTTPS traffic
• port 443 - repo1.maven.org - HTTPS traffic

# Conclusion

Based on the findings, the workstation "DESKTOP-SKBR25F" was identified as the infected host. It was infected with the STRRAT malware family and communicated with the attacker's C2 server through periodic beaconing while sending system information.

# Limitations

From the packet capture alone, no evidence could confirm which files were downloaded from GitHub or which Java libraries were retrieved from "repo1.maven.org" because the traffic was encrypted. Therefore, the exact initial malware download or infection method could not be confirmed from the PCAP alone.