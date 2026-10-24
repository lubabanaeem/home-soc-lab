# Wazuh Setup

## Overview

This document describes the deployment of a home SOC lab using Wazuh as a centralized SIEM platform.

The goal of this environment is to collect, analyze, and investigate security telemetry from both Windows and Linux endpoints.

## Lab Architecture

The environment consists of:

### Wazuh Server

Hosted on a separate machine running:

- Amazon Linux 2023 virtual machine
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Filebeat

The server receives security events from connected endpoint agents and provides centralized search, alerting, and visualization capabilities.

### Monitored Endpoints

Two virtual machines are connected as Wazuh agents:

**Windows 11 VM**
- Windows Security Event Logs
- Authentication events
- Process creation events
- Service-related events

**Ubuntu VM**
- Linux authentication logs
- SSH activity
- System logs
- User activity events

## Architecture Flow

```
Windows 11 VM
(Wazuh Agent)
        |
        |
Ubuntu VM
(Wazuh Agent)
        |
        |
        ↓

Wazuh Manager
(Amazon Linux 2023)

        |
        |
        ↓

Wazuh Dashboard
```

---

# Installation

## Wazuh Server Deployment

The Wazuh server components were deployed on an Amazon Linux 2023 virtual machine.

Installed components:

- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Filebeat

After installation, the dashboard was accessed through the Wazuh web interface for monitoring and investigation.

---

# Agent Deployment

## Windows 11 Agent

A Wazuh agent was installed on the Windows 11 virtual machine.

The agent was configured to communicate with the Wazuh Manager and successfully enrolled.

Collected telemetry included:

- Windows authentication events
- Process creation events
- Service activity

## Ubuntu Agent

A Wazuh agent was installed on the Ubuntu virtual machine.

The agent was configured to communicate with the same Wazuh Manager.

Collected telemetry included:

- SSH authentication attempts
- Linux system events
- User activity logs

---

# Configuration

## Event Collection

The environment was configured to support investigation of:

### Windows

- Event ID 4624 — Successful logon
- Event ID 4625 — Failed logon
- Event ID 4688 — Process creation

### Linux

- Authentication logs
- SSH activity
- System events

---

# Archive Logging

Archive logging was enabled to allow searching of raw events in addition to Wazuh-generated alerts.

This provided visibility into:

- Events that triggered alerts
- Normal endpoint activity
- User activity that required manual investigation

---

# Troubleshooting and Resolution

## Agent Connectivity Verification

### Issue

Agents required verification after deployment to confirm successful communication with the Wazuh Manager.

### Resolution

- Confirmed agent registration
- Verified active agent status in Wazuh Dashboard
- Confirmed event ingestion from endpoints

---

## Raw Event Searching

### Issue

Initial investigations focused mainly on alerts, but many important events did not generate alerts.

### Resolution

Archive logging was enabled to allow searching complete event data.

This allowed investigation of:

- Authentication timelines
- Process creation chains
- User activity

---

## Windows Event Investigation

### Observation

Windows generated a large number of process creation events, including normal operating system activity.

### Resolution

Events were analyzed using:

- Account information
- Logon IDs
- Parent-child process relationships
- Timestamps

This helped distinguish normal system activity from potentially suspicious behavior.

---

# Current Capabilities

The current Wazuh deployment supports:

- Centralized log collection from Windows and Linux endpoints
- Searching events by user and time window
- Authentication investigations
- Process activity investigation
- Dashboard creation
- Timeline reconstruction

# Future Improvements

Planned additions:

- Sysmon integration for enhanced Windows endpoint visibility
- Suricata integration for network intrusion detection
- Additional investigation scenarios
- Custom detection rules after learning detection engineering