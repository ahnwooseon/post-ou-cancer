# Incident Response Plan

This document outlines the procedure for responding to security incidents, data breaches, and service outages for the Post Ou Cancer platform.

## 1. Objectives

- **Detect and Analyze**: Quickly identify and understand security incidents.
- **Contain and Eradicate**: Limit the impact and remove the threat.
- **Recover**: Restore services to normal operation securely.
- **Communicate**: Inform internal and external stakeholders transparently.
- **Learn**: Conduct post-mortems to prevent recurrence.

## 2. Roles and Responsibilities

- **Incident Commander (IC)**: The single point of contact responsible for leading the incident response. (Initially: Project Lead).
- **Technical Lead**: Responsible for technical investigation and remediation.
- **Communications Lead**: Manages all internal and external communications.

## 3. Incident Severity Levels

| Level | Name      | Description                                                                 | Response Time |
| :---- | :-------- | :-------------------------------------------------------------------------- | :------------ |
| **SEV-1** | Critical  | Major data breach, platform-wide outage, significant user impact.         | < 15 minutes  |
| **SEV-2** | Major     | Partial service degradation, security vulnerability with potential impact.  | < 1 hour      |
| **SEV-3** | Minor     | Localized bug, minor performance degradation, low-impact security issue.    | < 24 hours    |

## 4. Incident Response Phases

### Phase 1: Detection and Triage

1.  **Detection**: Incidents are detected via monitoring alerts (Grafana, OpenTelemetry), user reports, or security scans.
2.  **Triage**: The on-call person assesses the alert to determine if it's a real incident and assigns a severity level.
3.  **Declaration**: An incident is formally declared. A dedicated communication channel (e.g., a specific Slack/Discord channel) is created.

### Phase 2: Containment, Eradication, and Recovery

1.  **Containment**: Isolate affected systems to prevent further damage (e.g., block IP addresses, disable a feature, take a service offline).
2.  **Eradication**: Identify and remove the root cause (e.g., patch a vulnerability, remove malware).
3.  **Recovery**: Restore systems from a known good state (e.g., restore from backup, redeploy services). Verify that the system is secure and fully functional.

### Phase 3: Post-Incident Activities

1.  **Communication**:
    - **Internal**: Regular status updates to the team.
    - **External**: If user data is impacted or service is down, communicate transparently via a status page, social media, or email. Adhere to the 72-hour GDPR notification window for data breaches.
2.  **Root Cause Analysis (RCA) / Post-Mortem**:
    - Within 5 business days of a SEV-1 or SEV-2 incident, a blameless post-mortem is conducted.
    - The goal is to understand the timeline, impact, and contributing factors.
    - **Action Items**: Create concrete, actionable tasks with owners and due dates to prevent recurrence.
3.  **Documentation**: The incident and its resolution are documented for future reference.

## 5. Communication Templates

*(This section will contain pre-written templates for different scenarios, such as service outages or security updates, to ensure fast and consistent communication.)*

---

## References

- [NIST - Computer Security Incident Handling Guide](https://csrc.nist.gov/publications/detail/sp/800-61/rev-2/final)
- [Atlassian - Incident Response Handbook](https://www.atlassian.com/incident-management/handbook)
