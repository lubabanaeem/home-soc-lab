
# Linux Investigation Report

## Objective

Investigate suspicious Linux activity by correlating authentication logs, auditd events, and Wazuh alerts to reconstruct the sequence of events.

## Timeline of Events

| Time | Event | Evidence Source |
|------|-------|-----------------|
| **17:15:18** | First failed SSH authentication attempt for **testuser**. | Authentication logs (auth.log / Wazuh) |
| **17:15:22** | Second failed SSH authentication attempt for **testuser**. | Authentication logs (auth.log / Wazuh) |
| **17:15:29** | Successful remote SSH login for **testuser**. | Authentication logs (auth.log / Wazuh) |
| **17:15:40** | User attempted to execute a command with **sudo**. The first privilege escalation attempt failed. | sudo logs / Wazuh |
| **17:15:45** | The user's crontab was opened for editing. | auditd / Wazuh |
| **17:15:50** | Changes to the user's crontab were detected. | auditd / Wazuh |

## Investigation

The investigation started with multiple failed SSH login attempts against the **testuser** account. A successful login occurred a few seconds later, indicating that authentication eventually succeeded.

After logging in, the user attempted to execute a command with **sudo**, but the initial privilege escalation attempt failed. Shortly afterwards, the user opened the crontab for editing and made changes to it.

By correlating authentication logs, sudo logs, auditd events, and Wazuh alerts, the complete sequence of activity could be reconstructed. The logs clearly identified the user account involved, the order of events, and the timestamps for each action.

The logs indicated that the crontab had been modified but did not record the exact cron entry that was added. To continue the investigation, endpoint verification was performed by inspecting the user's crontab directly using:

```bash
crontab -l
```

This confirmed the scheduled task currently configured for the user.

## Conclusion

This exercise demonstrated that security logs provide enough evidence to identify suspicious behaviour and reconstruct an attack timeline, but they do not always contain every detail required for a complete investigation. Rather than relying only on SIEM alerts, the investigation continued on the endpoint by examining the user's configuration after the logs identified the suspicious activity.

## Skills Demonstrated

- Linux log analysis
- SSH authentication investigation
- Timeline reconstruction
- Correlation of multiple telemetry sources
- auditd event analysis
- Wazuh investigation
- sudo activity investigation
- Cron persistence investigation
- Endpoint verification following SIEM alerts