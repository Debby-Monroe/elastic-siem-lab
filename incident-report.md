# Incident Investigation Report

## Case Information

| Field | Value |
|---------|---------|
| Case ID | 2026-07-10 |
| Incident Type | Brute-Force Authentication Attack (Simulated) |
| Severity | Low / Medium |
| Status | Closed / Resolved |
| Target Host | `ubuntu-lab-target` (Internal IP: 10.0.0.15) |
| Time Window | 2026-07-12 23:14:02 to 23:14:45 UTC |
| Analyst | Deborah Lawson |

---

## Executive Summary

A simulated brute-force attack was conducted against an Ubuntu Linux endpoint to generate authentication telemetry for analysis within Elastic Cloud SIEM.

The activity generated 47 failed login attempts targeting multiple user accounts within a 43-second window. Elastic Agent successfully collected the authentication logs, and Kibana was used to investigate the events.

No successful logins occurred, no accounts were locked out during the window, and the endpoint remained uncompromised.

---

## Investigation & Methodology

Authentication events were collected from the standard Linux authentication log:

```text
/var/log/auth.log
```

To isolate the malicious behavior from routine system noise within the Elastic Discover tab, the following Kibana Query Language (KQL) query was executed:

```text
event.category:"authentication" and event.outcome:"failure" and host.name:"ubuntu-lab-target"
```

The investigation revealed repeated authentication failures targeting multiple administrative and default accounts in rapid succession, confirming automated password-guessing behavior.

## Findings

| Metric | Observation |
|---------|-------------|
| Failed Login Attempts | 47 |
| Targeted Accounts | admin, database, guest, support, test |
| Attack Duration | 43 seconds (~1.1 attempts/second) |
| Source IP | 127.0.0.1 (Local loopback via simulation script) |
| Attack Pattern | Password-guessing / Dictionary attack |
| Log Source | Ubuntu auth.log |

**Analyst Note on Source IP:** The source IP appears as 127.0.0.1 because the attack script was executed locally on the host to simulate an insider threat / localized automated attack vector. In a production environment, this would warrant immediate investigation into localized crontabs or compromised local service accounts.

## Recommendations & Mitigation Strategies

To harden the endpoint against actual brute-force vectors discovered during this simulation, the following controls are recommended:

- Implement PAM Tally / Account Lockout: Configure `pam_faillock` on the Ubuntu host to temporarily lock accounts after 5 consecutive failed attempts.
- Disable Unused Default Accounts: Purge or disable the `guest`, `support`, and `test` accounts used in this attack vector if they serve no operational purpose.
- Configure SIEM Threshold Alerting: Establish an Elastic SIEM alert rule to trigger a high-severity notification if a single host experiences >20 failed login attempts within a 1-minute window.
- Enforce SSH Hardening: Restrict root logins and enforce public-key authentication over password authentication within `/etc/ssh/sshd_config`.

## Key Takeaway

This project successfully demonstrated how Elastic SIEM effectively ingests, parses, and surfaces suspicious authentication activity. The distinct signature of multiple common account names being targeted sequentially provides a high-fidelity detection use-case for constructing future automated correlation rules.
```
