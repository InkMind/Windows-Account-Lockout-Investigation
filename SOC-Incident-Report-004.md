# SOC Incident Report – 004
## Windows Account Lockout Investigation

**Incident ID:** SOC-004  
**Incident Type:** Windows Account Lockout  
**Date:** November 26, 2026  
**Affected Endpoint:** DESKTOP-9RNV8GM  
**Monitoring Platform:** Wazuh SIEM  
**Investigation Status:** Completed  
**Environment:** Controlled Windows Security Lab

---

## 1. Incident Summary

A controlled account lockout investigation was conducted on a Windows endpoint to examine the security events generated after repeated failed authentication attempts against a local test account.

The investigation focused on account `SOC-Lab-Test`. Five failed logon event records (Event ID 4625) were identified within the configured 30-minute observation window.

A subsequent Windows Security Event ID 4740 confirmed that the account had been locked out.

The corresponding Wazuh alert was also identified and investigated.

## 2. Evidence Collected

The following evidence was collected during the investigation:

- Windows Security Event ID 4625 — Failed logon attempts.
- Windows Security Event ID 4740 — Account lockout confirmation.
- PowerShell CIM output confirming the account lockout state.
- Wazuh SIEM alert associated with the account lockout.
- Wazuh detection rule details and affected endpoint information.

## 3. Technical Analysis

| Investigation Field | Findings |
|---|---|
| Incident Type | Account Lockout |
| Severity | Medium (lab assessment) |
| Affected Endpoint | DESKTOP-9RNV8GM |
| Targeted Account | SOC-Lab-Test |
| Source Address | ::1 (IPv6 loopback) |
| Failed Logon Event | Event ID 4625 |
| Account Lockout Event | Event ID 4740 |
| Number of Failed Event Records | 5 |
| Failure Reason | Unknown user name or bad password |
| Status Code | 0xC000006D |
| SubStatus Code | 0xC000006A |
| Account Lockout Status | True |
| Account Disabled | False |
| Wazuh Agent | Windows-Laptop |
| Wazuh Agent ID | 001 |
| Wazuh Rule ID | 60115 |
| Wazuh Rule Level | 9 |
| Wazuh Alert Description | User account locked out (multiple login errors) |

## 4. Event Timeline

| Date and Time | Event | Description |
|---|---|---|
| Nov 26, 2026 – 02:53:47 AM | Event ID 4625 | Failed authentication recorded |
| Nov 26, 2026 – 03:00:45 AM | Event ID 4625 | Failed authentication recorded |
| Nov 26, 2026 – 03:02:30 AM | Event ID 4625 | Failed authentication recorded |
| Nov 26, 2026 – 03:03:39 AM | Event ID 4625 | Failed authentication recorded |
| Nov 26, 2026 – 03:03:58 AM | Event ID 4625 / 4740 | Failed authentication and account lockout recorded |
| Nov 26, 2026 – 03:10:38 AM | Wazuh Alert | Account lockout alert identified in Wazuh |

## 5. Investigation Findings

The investigation established the following:

1. Five failed authentication event records were identified for the local test account.
2. Windows generated Event ID 4740, confirming that the account had been locked out.
3. PowerShell verification returned `Lockout = True` and `Disabled = False`.
4. Wazuh identified the account lockout event through Rule ID 60115.
5. The Wazuh detection rule generated a Level 9 alert on the monitored Windows endpoint.

The source address `::1` represents the local IPv6 loopback address, consistent with the controlled local lab activity.

## 6. Recommended Security Actions

For a similar event in a production environment, a SOC Analyst should:

- Verify whether the failed authentication attempts were authorized or expected.
- Identify the affected account and endpoint.
- Review the source address and authentication event details.
- Investigate related login activity for signs of suspicious behavior.
- Confirm whether the account lockout was triggered by user error or potentially malicious activity.
- Follow the organization's account recovery and escalation procedures.
- Continue monitoring for repeated authentication failures.

## 7. Analyst Conclusion

The investigation successfully demonstrated how repeated failed authentication attempts can trigger a Windows account lockout.

Windows Security Event IDs 4625 and 4740 provided the primary evidence for identifying the failed logons and confirming the account lockout.

PowerShell was used to independently verify the account state, while Wazuh SIEM provided centralized alert visibility and detection details.

The activity was deliberately generated within a controlled laboratory environment. No real-world attack or malicious actor was established by this exercise.

**Final Assessment:** Account lockout successfully triggered, investigated, and verified.

**Investigation Status:** Completed.
