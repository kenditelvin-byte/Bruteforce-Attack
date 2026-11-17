# Lab: Username Enumeration via Different Responses

[![PortSwigger Web Security Academy](https://img.shields.io/badge/Platform-PortSwigger%20Academy-orange?style=flat-square)](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses)
[![Vulnerability Type](https://img.shields.io/badge/Vulnerability-Username%20Enumeration-red?style=flat-square)](https://portswigger.net/web-security/authentication)
[![Status](https://img.shields.io/badge/Status-Solved-success?style=flat-square)]()

A detailed walkthrough and demonstration of exploiting and remediating a username enumeration vulnerability caused by inconsistent application responses.

---

## 🔍 Vulnerability Overview
The target application reveals whether a username exists in its database by returning distinct HTTP responses (e.g., different error messages or response lengths) during login attempts. This allows attackers to harvest valid usernames before launching password brute-force attacks.

## 🛠️ Methodology & Exploitation

### Step 1: Enumerating Valid Usernames
1. Intercepted the login request using **Burp Suite Proxy**.
2. Sent the `POST /login` request to **Burp Intruder**.
3. Configured a **Sniper attack** targeting the `username` parameter while keeping the password static using a list of [Candidate usernames](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses).
4. Sorted the response table by the **Length** column. Identified that a valid username returned an `Incorrect password` message, whereas invalid usernames returned `Invalid username`.

### Step 2: Brute-Forcing the Password
1. Re-configured Burp Intruder with the valid username identified in Step 1.
2. Targeted the `password` parameter using the list of [Candidate passwords](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses).
3. Monitored the HTTP status codes: successful login attempts returned a `302 Found` redirect response, unlike the standard `200 OK` error pages.

---

## 🛡️ Remediation & Secure Coding
To prevent username enumeration and credential brute-forcing, applications should implement uniform responses and defensive controls:

- **Generic Error Messages:** Always return a generic error message for failed login attempts (e.g., *"Invalid username or password"*), regardless of whether the username exists.
- **Rate Limiting & Account Lockouts:** Implement strict rate limiting per IP address and temporary account lockouts or CAPTCHAs after multiple failed login attempts.
- **Multi-Factor Authentication (MFA):** Enforce MFA to mitigate the impact of compromised passwords.

---

## 📺 Community References & Further Reading
- [Rana Khalil - Lab Walkthrough Video](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses)
- [Michael Sommer - Video Solution](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses)
