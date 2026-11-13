# Suricata Setup

## Environment

- Host OS: Windows 11
- Analysis VM: Ubuntu Desktop (VirtualBox)
- Network Mode: NAT
- IDS: Suricata
- SIEM: Wazuh (running on a separate VM)



# Installation

- Installed Suricata using the Ubuntu package manager.
- Verified the installation by checking the installed version.
- Reviewed the main configuration file (/etc/suricata/suricata.yaml).
- Configured HOME_NET to match the lab network.
- Updated the Emerging Threats (ET Open) rule set using suricata-update.
- Verified that the updated rules were loaded successfully.



# Configuration Validation

- Tested the configuration using:

bash
sudo suricata -T -c /etc/suricata/suricata.yaml


- Confirmed that the configuration loaded without critical errors.


# Detection Testing

Performed several tests to confirm that Suricata was detecting network events correctly.

### Live Traffic

- Generated test traffic by visiting **testmynids.org**.
- Confirmed that Suricata generated alerts.
- Verified that alerts were written to:
  - fast.log
  - eve.json

### Wazuh Integration

- Configured Wazuh to ingest Suricata eve.json events.
- Confirmed that Suricata alerts appeared in the Wazuh Dashboard.
- Compared the original Suricata event timestamp (data.timestamp) with the Wazuh processing timestamp.



# Issues Encountered

- Initial difficulty understanding the relationship between Suricata alerts and the corresponding packets in Wireshark.
- Timestamp differences between Suricata and Wireshark required changing Wireshark's time display format.
- Mapping a single Suricata alert to multiple packets within one TCP conversation required using packet filters and **Follow TCP Stream**.
- jq parsing issues occurred while searching eve.json, requiring alternative methods for locating events.


# Skills Practiced

- Updating and validating Suricata rules.
- Reading Suricata alert logs.
- Understanding alert SID values.
- Correlating Suricata alerts with packet-level evidence in Wireshark.
- Investigating HTTP traffic and distinguishing between true and false positives.
- Performing offline packet analysis using recorded PCAP files.


# Outcome

Successfully configured and validated Suricata, confirmed event generation, integrated alerts with Wazuh, and used both live traffic and offline PCAP analysis to investigate network events. Gained practical experience correlating Suricata detections with Wireshark packet analysis and understanding that IDS alerts require manual verification before concluding malicious activity.