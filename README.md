# Purple Team Adversary Emulation Range

## 🛡️ Project Overview
This project demonstrates the full attack lifecycle of a reverse shell intrusion and its detection. I built an isolated lab environment, executed a malicious payload using Kali Linux and Metasploit, and acted as a SOC Analyst to hunt down the indicators of compromise (IoCs) using Windows Sysmon telemetry.

## 🏗️ Architecture
*   **Attacker:** Kali Linux (Metasploit, Python HTTP Server) - IP: 192.168.56.101
*   **Target:** Windows 11 Virtual Machine - IP: 192.168.56.102
*   **Network:** VirtualBox Host-Only Adapter (Isolated Environment)
*   **Defense:** Windows Event Viewer & Sysmon (SwiftOnSecurity configuration)

## ⚔️ Attack Chain (Red Team)
The following MITRE ATT&CK techniques were emulated:
*   **T1105 - Ingress Tool Transfer:** Hosted a reverse-tcp executable (`update_check.exe`) on a Python web server and downloaded it to the target via Microsoft Edge.
*   **T1204.002 - User Execution (Malicious File):** Executed the payload locally on the Windows machine.
*   **T1071 - Application Layer Protocol:** Established a Meterpreter reverse shell Command & Control (C2) session back to the Kali listener on port 4444.

## 🔍 Detection & Threat Hunting (Blue Team)
Using Sysmon logs, I successfully reconstructed the timeline of the attack:
1.  **File Creation (Event ID 11):** Identified `update_check.exe` being downloaded into the `C:\Users\admin\Downloads` directory by `msedge.exe`.
2.  **Process Creation (Event ID 1):** Detected the execution of `update_check.exe`. The parent process was verified as `msedge.exe`, confirming direct user interaction from the browser.
3.  **Network Connection (Event ID 3):** Captured the exact moment `update_check.exe` opened an outbound TCP connection to the attacker IP (`192.168.56.101`) over port `4444`.

## 🚀 Next Steps / Future Enhancements
*   Forward these logs to a SIEM (like Splunk or Wazuh) for centralized monitoring.
*   Write custom Sigma rules to generate automated alerts for Event ID 1 & 3 anomalies.
