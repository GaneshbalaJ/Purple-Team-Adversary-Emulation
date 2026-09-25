Incident Timeline: Reverse Shell Intrusion (2026-09-25)

15:05:12 UTC: Outbound network connection initiated by msedge.exe to malicious HTTP server at 192.168.56.101:8080. (Sysmon Event ID 3 - T1105 Ingress Tool Transfer)

15:05:16 UTC: Malicious executable update_check.exe written to disk at C:\Users\admin\Downloads\. (Sysmon Event ID 11)

15:05:53 UTC: Command and Control (C2) connection established. update_check.exe initiated an outbound TCP connection to 192.168.56.101:4444. (Sysmon Event ID 3 - T1071 Application Layer Protocol)

15:06:05 UTC: Malicious payload executed by the user directly from the browser downloads. Process update_check.exe spawned with parent process msedge.exe. (Sysmon Event ID 1 - T1204.002 User Execution)