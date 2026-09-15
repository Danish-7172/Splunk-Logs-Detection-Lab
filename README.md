🔐 Brute Force Attack Detection Lab (DVWA + Burp Suite + Splunk)
📌 Overview

This lab demonstrates a practical brute force attack simulation and log-based detection workflow. It focuses on capturing web authentication attacks and identifying them through log analysis using a SIEM.

The lab bridges both:

⚔️ Offensive Security (attack simulation)
🛡️ Defensive Security (log detection)
🎯 Objectives
Simulate a brute force attack on a web application
Capture HTTP requests using Burp Suite
Generate server logs from attack activity
Forward logs to Splunk
Identify brute force patterns through log analysis
🧰 Tools Used
Burp Suite
Splunk
Splunk Universal Forwarder
DVWA (Damn Vulnerable Web Application)
Docker
🖥️ Lab Setup
Component	Role
Kali Linux	Attacker
DVWA (Docker)	Target
Splunk	Log Analysis
Forwarder	Log Sender
⚔️ Part 1: Attack Simulation
🔹 Step 1: Access Target

Open DVWA login page:

http://localhost:8080/login.php
🔹 Step 2: Intercept Request
Open Burp built-in browser
Turn Intercept ON
Capture request:
POST /login.php
🔹 Step 3: Send to Intruder
Right click → Send to Intruder
Select password field as payload position
🔹 Step 4: Launch Brute Force
Attack type: Sniper
Use wordlist
Start attack
💥 Attack Behavior
Multiple login attempts
Same endpoint targeted repeatedly
High-frequency requests
📂 Part 2: Log Generation
🔍 Server Logs

Attack generates logs like:

POST /login.php
POST /login.php
POST /login.php
📌 Observations
Repeated authentication attempts
Same request pattern
Rapid request generation
🔐 Part 3: Log Detection (Splunk)
🔍 Basic Search
index=* "POST /login.php"
🔎 Identify Attack Source
index=* "POST /login.php"
| stats count by clientip
🚨 Detection Pattern
High number of requests from one source
Abnormal frequency of login attempts
🧠 Observation
Normal user → few requests
Attacker → many rapid requests

👉 This clearly indicates a brute force attack

🛡️ Defender’s Perspective
🚨 Brute Force Pattern
Same endpoint repeatedly accessed
High number of failed login attempts
Traffic spike in short time
📊 Example Pattern
POST /login.php
POST /login.php
POST /login.php
(repeated many times)
⚖️ Attack vs Detection
Phase	Activity
Attack	Multiple login attempts
Logs	Repeated POST requests
Detection	High count from single source
🔐 Security Recommendations
Implement account lockout policy
Add CAPTCHA protection
Enforce strong passwords
Monitor authentication logs
Use SIEM alerts for detection
🧠 Key Learnings
Brute force attacks are easily visible in logs
High request frequency is a strong indicator
SIEM tools help detect abnormal patterns
Even simple logs can reveal attacks
Detection is possible without advanced tools
