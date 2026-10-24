
# Home SOC Lab

A self-built security operations lab for practicing log collection, detection and investigation. All activity is simulated in an isolated lab environment.

**Status:** In progress (Week 5 of 8). SIEM deployed with two agents. Endpoint telemetry (Sysmon) and network detection (Suricata) are next.

## Architecture

                         Home SOC Lab

+------------------------------------------------+
|              Wazuh Manager                    |
|           Amazon Linux 2023 VM                |
|                                                |
|  - Wazuh Manager                              |
|  - Wazuh Indexer                              |
|  - Wazuh Dashboard                            |
|  - Filebeat                                   |
+------------------------------------------------+
                     
                     |
          Logs / Security Events
                     |
        -------------------------------
        |                             |
        |                             |
+----------------+          +----------------+
| Windows 11 VM  |          | Ubuntu VM      |
| SOCWin11 Lab   |          | Linux Agent    |
|                |          |                |
| Wazuh Agent    |          | Wazuh Agent    |
| Sysmon         |          | SSH Logs       |
| Event Logs     |          | Auth Logs      |
+----------------+          +----------------+

                     Later additions

+----------------+
| Suricata       |
| Network IDS    |
|               |
| Network Logs   |
+----------------+

- **Wazuh server** (self-hosted Ubuntu VM): manager, indexer and dashboard
- **Linux agent:** Ubuntu VM forwarding authentication and ssh logs
- **Windows agent:** Windows VM forwarding Security, System and Application logs

## Components

 Wazuh (./wazuh) | SIEM: log collection, search, alerting, dashboards : Deployed 
 Sysmon : Windows endpoint telemetry : Planned 
 Suricata : Network intrusion detection : Planned 

## Investigations


 [01: labuser session](./investigations/labuser-session-01.md) | Wazuh (Windows Security logs)  Failed logins, then a successful login, then an admin CMD session and a service being created. Attribution of the admin session to the labuser session could not be established from the available evidence. 

## Skills demonstrated

- Deploying and connecting a SIEM with multiple agents
- Searching and correlating Windows authentication events
- Building dashboards for failed-login analysis
- Separating confirmed findings from hypotheses

## Limitations

This lab currently has no process-level endpoint telemetry or network detection, which limits how far an investigation can be traced. Those gaps are why Sysmon and Suricata are the next two components.


