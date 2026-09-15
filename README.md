🔐 Brute Force Log Detection Lab (DVWA + Burp Suite + Splunk)
📌 Overview

This project demonstrates a basic cybersecurity detection workflow, where a brute force attack is simulated on a vulnerable web application and identified through log analysis.

The focus of this lab is:

👉 Capturing attack traffic and detecting it in logs using a SIEM

🎯 Objectives
Simulate a brute force attack on a login page
Capture HTTP requests using Burp Suite
Generate server-side logs
Forward logs to Splunk
Identify attack patterns through log analysis
🧰 Tools & Technologies
Burp Suite – Intercepting proxy & attack execution
Splunk – Log ingestion and analysis
DVWA (Damn Vulnerable Web Application) – Target application
Splunk Universal Forwarder – Log forwarding
Docker – Application deployment
🏗️ Lab Architecture
Burp Browser
   ↓
Burp Suite (Proxy)
   ↓
DVWA (Web Application)
   ↓
Web Server Logs
   ↓
Splunk Forwarder
   ↓
Splunk SIEM
⚙️ Setup Summary

⚠️ Sensitive details (IP addresses, file paths, system identifiers) are intentionally omitted.

DVWA deployed in a controlled lab environment
Logs exposed for monitoring
Splunk configured for log ingestion
Forwarder configured to send logs
Burp Suite used with built-in browser
⚔️ Attack Simulation (Brute Force)
🔹 Step 1 — Access Target
Open DVWA login page
Use Burp built-in browser
🔹 Step 2 — Capture Request
Enable intercept in Burp
Capture login request:
POST /login.php
🔹 Step 3 — Perform Attack
Send request to Intruder
Select password field
Use a wordlist to perform brute force
🔁 Observed Behavior
Multiple login attempts generated
Same endpoint repeatedly targeted
High-frequency request pattern
📂 Log Generation

During the attack, the server logs recorded:

Repeated POST /login.php requests
High number of authentication attempts
Consistent access patterns
🔍 Log Detection in Splunk
Basic Search
index=* "POST /login.php"
Identify Repeated Attempts
index=* "POST /login.php"
| stats count by clientip
Detection Pattern
High number of requests from a single source
Rapid sequence of login attempts

👉 This behavior indicates a brute force attack

🧠 Key Findings
Brute force attacks can be identified through log patterns
Even without alerts, abnormal behavior is visible in SIEM
Repeated authentication attempts are a strong indicator
⚠️ Limitations

This project includes:

✔ Attack simulation
✔ Log ingestion
✔ Log analysis

It does NOT include:

❌ Automated alerting
❌ Dashboards
❌ Active blocking/response
🛡️ Suggested Improvements
Implement alert rules in Splunk
Add time-based detection logic
Build dashboards for visualization
Apply rate limiting on login endpoint
🧾 Conclusion

This project demonstrates how a brute force attack:

Can be simulated using Burp Suite
Generates identifiable log patterns
Can be detected using Splunk

It provides a strong foundation for SOC analysis and detection workflows.

🚀 Skills Gained
Web request interception
Brute force attack simulation
Log analysis
SIEM usage
Understanding attack behavior
⚠️ Disclaimer

This project was conducted in a controlled lab environment for educational purposes only.
