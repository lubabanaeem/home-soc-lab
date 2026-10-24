# Investigation 01 - Windows User Session Analysis

## Overview

A controlled investigation was performed on a Windows 11 endpoint connected to the Wazuh SIEM environment.

The objective was to analyze user activity following authentication attempts and understand how Windows records:

- Failed logons
- Successful logons
- Process creation
- Administrative command execution
- Service creation activity

The activity was manually generated in a controlled lab environment.



# Environment

## Components

- Windows 11 endpoint
- Wazuh Agent
- Wazuh Manager
- Wazuh Dashboard

## Data Sources

Primary Windows event sources analyzed:

- Security Event Log
- Process creation events (Event ID 4688)
- Authentication events:
  - Event ID 4625 : Failed logon
  - Event ID 4624 : Successful logon



# Initial Scenario

The investigation started with authentication activity involving the "Lab User" account.

The following activity was generated:

1. Multiple failed login attempts were performed.
2. A successful login was performed.
3. User activity after authentication was reviewed.
4. Command execution and service-related activity were analyzed.



# Investigation Timeline

## 1. Failed Authentication Attempts

### Event

failed authentication attempt was observed.

Relevant information:

- Account: Lab User
- Event ID: 4625
- Source: Windows Security Log

### Analysis

The failed attempt was expected because it was manually generated during testing.

The events demonstrated how failed authentication appears in the SIEM and how authentication activity can be filtered by:

- username
- timestamp
- event ID



# 2. Successful Authentication

## Event

A successful logon event followed the failed attempts.

Relevant information:

- Event ID: 4624
- Account: Lab User

### Analysis

The successful authentication established the beginning of the user session that was investigated.

The following activity was reviewed after this event to reconstruct user actions.



# 3. Process Activity Investigation

## Process Creation Monitoring

Windows process creation events (Event ID 4688) were reviewed after the successful logon.

Observed examples included:

- Explorer.exe starting user applications
- CMD.exe execution
- Windows system processes

The investigation focused on:

- New Process Name
- Parent Process Name
- New Process ID
- Parent Process ID
- Subject User
- Logon ID



# 4. Command Execution Activity

## Observation

A command shell was opened during the session.

Observed process relationship:


Explorer.exe
      |
      |--- CMD.exe


The CMD process was created under the Lab User context.

This established normal user-initiated command execution.



# 5. Administrative Command Execution Investigation

## Observation

Later process creation events showed:


CMD.exe
      |
      |--- sc.exe


The process creator was:

- Subject User: SOC Admin

The command was related to Windows Service Control Manager operations.

Multiple sc.exe executions were observed.



# Analysis of Privilege Transition

The investigation examined whether the administrative activity could be directly linked to the original Lab User session.

Evidence reviewed:

- Process IDs
- Parent-child process relationships
- Subject username
- Logon IDs
- Process timestamps

## Findings

The investigation confirmed:

- An administrative command execution occurred.
- sc.exe was executed under the SOC Admin security context.
- Service-related activity was generated.

However:

- The original Lab User CMD process ID did not match the CMD process associated with SOC Admin execution.
- Available logs did not provide enough evidence to prove direct lineage from the Lab User CMD process to the elevated administrative process.



# Service Creation Activity

## Observation

A service creation attempt was performed using Service Control Manager commands.

Relevant activity:


cmd.exe
    |
    |-- sc.exe


The activity generated Windows service-related events.

The investigation reviewed:

- Process creation records
- Service-related events
- User context



# Important Investigation Lesson

A process name match alone does not establish process lineage.

Example:


Lab User:
Explorer.exe
      |
      |--CMD.exe (PID 0x1444)


SOC Admin:
CMD.exe (PID 0x1d7c)
      |
      |-- sc.exe


Although both processes are named CMD.exe, they are different processes because the process identifiers do not match.



# Conclusion

This investigation demonstrated the process of reconstructing Windows user activity using SIEM data.

Confirmed findings:

- Failed authentication attempts were identified.
- Successful authentication was correlated with subsequent activity.
- Process creation events were analyzed using parent-child relationships.
- Administrative command execution and service-related activity were identified.

Limitations:

The available evidence was insufficient to confirm that the Lab User session directly caused the SOC Admin administrative activity.

Further investigation in a production environment would require additional telemetry, such as:

- enhanced process auditing
- Sysmon process lineage data
- UAC-related events
- additional authentication correlation data

The activity was classified as a controlled laboratory simulation.