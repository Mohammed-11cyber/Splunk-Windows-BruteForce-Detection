# Splunk-Windows-BruteForce-Detection
Detection engineering and incident triage using Splunk SPL to correlate Windows authentication anomalies and post-exploitation process creation (Event IDs 4625, 4624, 4648, 4688).
# Windows Security Log Investigation and Detection Engineering

## Project Description
This project focuses on simulating a real-world Security Operations Center investigation by triaging unparsed Windows Security Event logs inside Splunk. The core goal was to track a complete cyber attack lifecycle from an initial automated access attempt to a successful host compromise and the execution of a persistent backdoor. Because the ingested CSV data was completely unstructured, I utilized manual field extractions and custom Regular Expressions within Splunk Search Processing Language to successfully isolate the attack telemetry.

## Core Windows Event Identifiers Correlated
* **Event ID 4625:** Implemented to capture repeated failed login events targeting specific user accounts. A time-based threshold alert was engineered to trigger when five or more authentication failures occurred within a single minute.
* **Event ID 4624:** Investigated immediately following the brute-force storm to detect a successful breach. The connection was isolated as a Logon Type 3, which flags an unauthorized network-based authentication.
* **Event ID 4648:** Analyzed to identify instances where the threat actor manually supplied explicit administrative credentials to execute elevated actions on the local host.
* **Event ID 4688:** Audited to track post-exploitation command execution telemetry on the endpoint. This captured the attacker launching a legitimate system wrapper process, sshd.exe, with a reverse flag parameter used to establish a covert remote port forwarding tunnel to maintain access.

## Incident Timeline Reconstruction
The investigation revealed a clear, continuous attack chain. First, an automated password spray targeted a specific admin account over a network interface. The attacker successfully guessed the valid credential string, authenticated remotely, and immediately abused internal system privileges. Finally, process tracking exposed the deployment of a background port forward designed to bypass network boundaries and establish persistence.

## Full Technical Report PDF
The complete case file, featuring the raw search queries, custom regex strings, and the interactive Splunk Security Monitoring Dashboard, is fully documented in the attached project file in this repository. 

Please download the PDF directly from the file list below to view the complete multi-page analysis report!
