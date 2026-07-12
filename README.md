# 🚨 Automated Brute-Force Detection & Telemetry Pipeline (Elastic Cloud SIEM)

## 🎯 Objective

This project demonstrates the deployment of an Elastic Cloud SIEM environment to collect and analyze security logs from a Linux endpoint. The objective was to simulate a brute-force password attack, configure centralized log collection using Elastic Agent, and investigate authentication failures using Kibana Query Language (KQL).

The investigation focused on identifying suspicious login activity, analyzing authentication patterns, and understanding how SIEM platforms can be used to detect and investigate unauthorized access attempts.

---

## 🛠️ Tools & Environment

- **SIEM Platform:** Elastic Cloud with Kibana Security
- **Endpoint:** Ubuntu Linux Virtual Machine (ARM64)
- **Telemetry Collection:** Elastic Agent managed through Fleet
- **Log Source:** Linux Authentication Logs (`/var/log/auth.log`)
- **Query Language:** Kibana Query Language (KQL)

---

## 🏗️ Lab Architecture

The environment consisted of an Ubuntu Linux endpoint connected to Elastic Cloud through Elastic Agent. Fleet Management was used to manage endpoint enrollment and telemetry collection. Authentication logs were forwarded to Kibana for monitoring and investigation.

### Endpoint Successfully Connected to Elastic Fleet

<img width="1412" height="883" alt="Elastic Fleet Agent Connected" src="https://github.com/user-attachments/assets/9797e8b1-c63d-4eb7-afc7-d5a9616f94d2" />

---

## ⚙️ Methodology

### 1. Deploy Elastic Cloud

- Created an Elastic Cloud deployment.
- Accessed Kibana Security.
- Configured Fleet Management.
- Generated an enrollment token.

### 2. Connect the Ubuntu Endpoint

- Created an Ubuntu Linux virtual machine.
- Installed Elastic Agent.
- Enrolled the endpoint into Fleet.
- Verified healthy telemetry collection.

### 3. Collect Authentication Logs

- Enabled the Linux System integration.
- Collected authentication logs from `/var/log/auth.log`.
- Verified log ingestion within Kibana Discover.

### 4. Simulate Brute-Force Activity

- Generated multiple failed SSH authentication attempts.
- Simulated password-guessing behavior against local accounts.
- Created security telemetry for investigation.

### 5. Investigate with Kibana

Applied the following KQL query to isolate failed authentication events:

```kql
event.outcome:"failure"
```

### Failed Authentication Events in Kibana

<img width="1366" height="827" alt="Failed Authentication Events" src="https://github.com/user-attachments/assets/cb8628d6-b87a-4110-a269-ff4074f5b2e2" />

---

## 📊 Findings

The investigation identified a pattern consistent with an automated password-guessing attack targeting multiple user accounts.

| Metric | Observation |
|----------|-------------|
| Attack Type | Automated password-guessing attack |
| Authentication Failures | 47 failed login attempts |
| Targeted Accounts | admin, database, guest, support, test |
| Source IP | 127.0.0.1 (local simulation) |
| Detection Method | KQL query: `event.outcome:"failure"` |
| Log Source | Ubuntu authentication logs |

### Authentication Failure Distribution

<img width="540" height="657" alt="Authentication Failure Analysis" src="https://github.com/user-attachments/assets/c4322e55-7106-44f8-97d8-a1d232e824b8" />

---

## 🔐 Security Recommendations

- Implement account lockout policies to limit repeated login attempts.
- Disable or secure unused default accounts.
- Configure SIEM alerts for excessive authentication failures.
- Enable multi-factor authentication (MFA) where possible.
- Regularly review authentication logs for suspicious activity.

---

## 📝 Key Takeaway

This project provided hands-on experience deploying a cloud-based SIEM, onboarding an endpoint, collecting security telemetry, and investigating authentication failures using KQL.

The simulated attack targeted multiple commonly used account names (`admin`, `database`, `guest`, `support`, and `test`) within a short timeframe, demonstrating a password-guessing attack pattern. Elastic Security successfully captured and indexed the events, enabling rapid investigation and analysis.

---

## 📄 Additional Documentation

A detailed incident investigation report is available in:

**incident-report.md**
