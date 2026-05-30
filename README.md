# Splunk-Windows-BruteForce-Detection
Detection engineering and incident triage using Splunk SPL to correlate Windows authentication anomalies and post-exploitation process creation (Event IDs 4625, 4624, 4648, 4688).
# SIEM Triage & Detection Engineering: Tracking the Attack Chain in Splunk

## 📌 Project Overview
[cite_start]This project simulates an enterprise-level Security Operations Center (SOC) workflow[cite: 476]. [cite_start]Using **Splunk**, I developed detection logic and conducted an incident response triage over unparsed, unstructured Windows Security Event logs[cite: 476, 477]. 

[cite_start]The investigation maps out a complete cyber attack lifecycle—moving from an automated network brute-force attempt to a confirmed endpoint breach, credential abuse, and a sneaky post-exploitation persistent backdoor[cite: 813, 818].

---

## 🛠️ Tools, Core Logic & Regex Engineering
[cite_start]Because the source logs were ingested as raw, unstructured event data, standard automated parsing failed[cite: 477, 483]. [cite_start]I utilized Splunk's **Search Processing Language (SPL)** paired with **Regular Expressions (`rex`)** to dynamically isolate and track the malicious trail[cite: 477, 480, 481]:

* [cite_start]**Event ID 4625 (Logon Failure):** Traced the volume and velocity of a password-spraying attack targeting `admmig@offsec.lan`[cite: 476, 596]. (Threshold alert set to trigger at $\ge$ 5 failures within 1 minute) [cite_start][cite: 516].
* [cite_start]**Event ID 4624 (Successful Logon):** Caught the exact second the attacker guessed the password using **Logon Type 3** (Remote Network Access via SMB/SSH)[cite: 477, 637, 639].
* [cite_start]**Event ID 4648 (Explicit Credentials):** Uncovered post-authentication activity where the attacker attempted to execute tasks under explicit administrative rights on the local host (`localhost`)[cite: 477, 646, 682].
* [cite_start]**Event ID 4688 (Process Creation):** Audited endpoint execution commands to catch the attacker spawning `sshd.exe` with the **`-R` flag** to open a malicious remote port forwarding tunnel[cite: 477, 745, 747].

---

## 📊 The Attack Chain Timeline Uncovered
1. [cite_start]**The Storm:** The target host registers a rapid burst of failed login attempts under Logon Type 8[cite: 597, 598].
2. [cite_start]**The Compromise:** The alert engineering pipeline flags an immediate transition to a successful Network Logon (Type 3) for the `admmig` profile[cite: 637, 639].
3. [cite_start]**The Pivot:** The compromised account manually passes explicit system privileges to operate locally[cite: 680, 682].
4. [cite_start]**The Backdoor:** The attacker drops an active SSH wrapper (`sshd.exe -R`) to build a covert communication channel and maintain persistent access out of the corporate network[cite: 745, 748, 817].

---

## 📂 Full Investigation Report & Dashboards
[cite_start]The complete multi-page case file features **raw SPL queries, custom Regex strings, step-by-step incident pivot logs, and the final custom Splunk Security Monitoring Dashboard**[cite: 477, 480, 481, 810].

### ➡️ [Click Here to View the Full Analysis Report PDF](./windows%20authentication%20detection%2C%20investigation%20and%20process%20creation%20%20project.pdf)

> ⚠️ **Note on PDF Rendering:** GitHub's built-in viewer can sometimes struggle to display long files with heavy dashboard charts. For the best experience, click the **"Download raw file"** button inside the PDF viewer to open it cleanly on your machine!
