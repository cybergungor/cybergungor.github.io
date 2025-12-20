---
layout: post
title: "CyberGungor LAB: Professional SOC Investigation Suite"
date: 2025-12-20
categories: [labs]
tags: [documentation, blue-team, gungorlab]
description: "Professional SOC Investigation & Response Suite"
---

# GungorLAB: Advanced SOC Investigation & Response Suite

**GungorLAB** is an interactive **CyberGungor LAB** environment designed to simulate a real-world Security Operations Center (SOC). This platform moves beyond simple log reading by requiring analysts to perform deep forensic analysis, execute containment via EDR, and manage the incident lifecycle through professional escalation or closure.

## Accessing the Environment

To initialize your analyst workstation, follow these operational steps:

1.  **Navigate to Labs:** Select the `Labs` section from the main navigation menu.
2.  **Establish Connection:** Click the green `Connect` button to link your browser to the lab environment.
3.  **Authentication:** Enter your analyst codename in the terminal interface and select `Initialize Session`.
4.  **Launch Mission:** Click the `CyberGungor.exe` icon on the virtual desktop to view active investigations.



---

## Technical Features & Bonuses

The platform integrates several "Expert-Level" features to simulate a high-fidelity SOC experience:

* **SIEM Real-Time Filtering:** Instead of manual scrolling, utilize the `SIEM Filter` bar. Use keywords such as `admin`, `4625`, or `UNION` to isolate malicious telemetry from background noise instantly.
* **Interactive EDR Terminal:** Detection is only half the battle. Once evidence is validated, the `EDR Console` provides a command-line interface to execute remote remediation commands within the **CyberGungor LAB** ecosystem.
* **Dynamic Decision Logic:** Not every alert is a threat. You must decide if a case is a `True Positive` (requires escalation) or a `False Positive` (requires justification and closure).
* **High-Density Noise:** Each lab generates hundreds of lines of benign traffic, forcing the analyst to identify subtle "low-and-slow" attack patterns.

---

## Active Mission Profiles

Currently, **GungorLAB** hosts five distinct investigation scenarios:

### ADV-01: Stealthy Brute Force
Identify a low-frequency credential stuffing attack.
* **Objective:** Locate the successful `EventID 4624` following a series of `4625` failures.
* **Remediation:** Use `reset --user` to revoke attacker access.

### ADV-02: Directory Traversal
Detect attempts to bypass web root restrictions to read system files.
* **Objective:** Find the `../../` patterns in Apache logs and identify which file was compromised.
* **Remediation:** Use `isolate --host` to prevent further data leakage.

### ADV-03: Ransomware Staging
Analyze endpoint telemetry for mass file encryption activity.
* **Objective:** Identify the malicious process and the encrypted file extension used by the malware.
* **Remediation:** Use `kill --pid` to terminate the encryption thread immediately.

### ADV-04: Log4Shell Exploitation
Investigate critical JNDI injection attempts within application headers.
* **Objective:** Analyze `User-Agent` strings for `ldap://` signatures and verify outbound connections.
* **Remediation:** Use `service stop` to disable the vulnerable application server.

### ADV-05: DNS Exfiltration
Detect sensitive data being smuggled out of the network via DNS queries.
* **Objective:** Identify high-entropy DNS tunneling queries and the rogue destination domain.
* **Remediation:** Use `block-domain` to sever the exfiltration channel.



---

## Operational Workflow

Analysts must adhere to the following professional incident response standards:

1.  **Analysis:** Utilize the `SIEM Filter` to find technical artifacts (IPs, Usernames, Files).
2.  **Validation:** Enter the findings into the investigation checklist.
3.  **Remediation (True Positive):** Execute the necessary commands via the EDR terminal (e.g., `service stop tomcat` or `reset --user admin`).
4.  **Escalation:** If the threat is confirmed, select `Escalate to L2` and provide a detailed summary.
5.  **Closure (False Positive):** If the activity is benign, justify the closure with an analyst note and select `Close Incident`.

`SYSTEM STATUS: ONLINE`
`ACCESS LEVEL: AUTHORIZED - CYBERGUNGOR LAB`
