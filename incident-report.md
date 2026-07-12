# Incident Investigation Report

## Case Information

| Field | Value |
|---------|---------|
| Case ID | 2026-06-BF |
| Incident Type | Brute-Force Authentication Attack |
| Severity | Medium |
| Status | Closed / Resolved |
| Analyst | Deborah Lawson |

---

## Executive Summary

A simulated brute-force attack was conducted against an Ubuntu Linux endpoint to generate authentication telemetry for analysis within Elastic Cloud SIEM.

The activity generated 47 failed login attempts targeting multiple user accounts within a short period of time. Elastic Agent successfully collected the authentication logs, and Kibana was used to investigate the events.

No successful logins occurred and the endpoint remained uncompromised.

---

## Investigation

Authentication events were collected from:

```text
/var/log/auth.log
```

Failed login activity was isolated using the following KQL query:

```kql
event.outcome:"failure"
```

The investigation revealed repeated authentication failures targeting multiple accounts in rapid succession.

---

## Findings

| Metric | Observation |
|----------|-------------|
| Failed Login Attempts | 47 |
| Targeted Accounts | admin, database, guest, support, test |
| Source IP | 127.0.0.1 |
| Attack Pattern | Password-guessing / Dictionary attack |
| Log Source | Ubuntu auth.log |

---

## Recommendations

- Implement account lockout policies.
- Disable unused default accounts.
- Configure alerts for excessive login failures.
- Enable multi-factor authentication (MFA).

---

## Key Takeaway

This project demonstrated how Elastic SIEM can collect, detect, and investigate suspicious authentication activity. The attack pattern showed multiple common account names being targeted in rapid succession, consistent with automated password-guessing behavior.
