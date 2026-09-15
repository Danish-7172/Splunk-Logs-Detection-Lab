# 🔐 Brute Force Attack Detection Lab

**DVWA + Burp Suite + Splunk**

A hands-on lab demonstrating brute force attack simulation and log-based detection using a SIEM. Bridges offensive security (attack simulation) with defensive security (log analysis and detection).

---

## 📌 Overview

This lab simulates a brute force login attack against DVWA (Damn Vulnerable Web Application), captures the resulting HTTP traffic and server logs, forwards those logs to Splunk, and walks through the queries used to detect the attack pattern.

It covers the full loop:

- ⚔️ **Offense** — simulate a brute force attack
- 🛡️ **Defense** — detect it purely through log analysis

---

## 🎯 Objectives

- Simulate a brute force attack on a web application
- Capture HTTP requests using Burp Suite
- Generate server logs from attack activity
- Forward logs to Splunk
- Identify brute force patterns through log analysis

---

## 🧰 Tools Used

| Tool | Purpose |
|---|---|
| **Burp Suite** | Intercepting proxy / attack automation (Intruder) |
| **Splunk** | Log ingestion, search, and analysis (SIEM) |
| **Splunk Universal Forwarder** | Forwards logs from target to Splunk |
| **DVWA** | Intentionally vulnerable target web app |
| **Docker** | Hosting the DVWA container |

---

## 🖥️ Lab Setup

| Component | Role |
|---|---|
| Kali Linux | Attacker machine |
| DVWA (Docker) | Target application |
| Splunk | Log analysis / SIEM |
| Splunk Universal Forwarder | Log shipping agent |

---

## ⚔️ Part 1: Attack Simulation

### Step 1 — Access Target

Open the DVWA login page:

```
http://localhost:8080/login.php
```

### Step 2 — Intercept Request

1. Open Burp Suite's built-in browser
2. Turn **Intercept ON**
3. Capture the login request:

```
POST /login.php
```

### Step 3 — Send to Intruder

1. Right-click the intercepted request → **Send to Intruder**
2. Select the **password** field as the payload position

### Step 4 — Launch Brute Force

- **Attack type:** Sniper
- **Payload:** Wordlist
- Start the attack

### 💥 Attack Behavior

- Multiple login attempts fired in sequence
- Same endpoint (`/login.php`) targeted repeatedly
- High-frequency, automated requests

---

## 📂 Part 2: Log Generation

The attack produces a repetitive log pattern on the server side:

```
POST /login.php
POST /login.php
POST /login.php
...
```

### 📌 Observations

- Repeated authentication attempts
- Identical request pattern per attempt
- Rapid, machine-speed request generation

---

## 🔐 Part 3: Log Detection (Splunk)

### Basic Search

```spl
index=* "POST /login.php"
```

### Identify Attack Source

```spl
index=* "POST /login.php"
| stats count by clientip
```

### 🚨 Detection Pattern

- Abnormally high number of requests from a single source IP
- Login attempt frequency far outside normal user behavior

### 🧠 Observation

| User Type | Behavior |
|---|---|
| Normal user | Few login requests |
| Attacker | Many rapid, repeated requests |

👉 This disparity in request volume is a clear indicator of a brute force attack.

---

## 🛡️ Defender's Perspective

### Brute Force Pattern Indicators

- Same endpoint accessed repeatedly
- High number of failed login attempts
- Traffic spike within a short time window

### Example Log Pattern

```
POST /login.php
POST /login.php
POST /login.php
(repeated many times)
```

---

## ⚖️ Attack vs Detection Summary

| Phase | Activity |
|---|---|
| **Attack** | Multiple automated login attempts via Burp Intruder |
| **Logs** | Repeated `POST /login.php` entries |
| **Detection** | High request count from a single client IP in Splunk |

---

## 🔐 Security Recommendations

- ✅ Implement an account lockout policy
- ✅ Add CAPTCHA protection on login forms
- ✅ Enforce strong password policies
- ✅ Continuously monitor authentication logs
- ✅ Configure SIEM alerts for abnormal login frequency

---

## 🧠 Key Learnings

- Brute force attacks are easily visible in raw logs once you know what to look for
- Request frequency is a strong, simple detection signal
- SIEM tools like Splunk make pattern detection fast and scalable
- Even minimal logging can reveal an active attack
- Effective detection doesn't require advanced tooling — just the right queries

---

## ⚠️ Disclaimer

This lab was performed in an isolated, self-hosted environment using **DVWA**, an application intentionally built to be vulnerable for educational purposes. All activity was conducted against systems owned and controlled by the author.

**Do not perform brute force attacks or any offensive security testing against systems you do not own or have explicit written authorization to test.**
