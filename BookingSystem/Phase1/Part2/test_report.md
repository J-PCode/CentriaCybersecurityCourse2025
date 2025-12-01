# 1️⃣ Introduction

**Tester(s):**  
- Name: J-PCode

**Purpose:**  
- The system is being developed in phases. In this first phase, i am testing user registration functionality. I act as a white-hat hacker, using a gray-box testing approach. 

Task is to identify as many anomalies and vulnerabilities as possible and categorize findings.

This is part 2 of phase1 meaning i am testing second version of website.

**Scope:**  
- Tested components:  user registration
- Exclusions:  Login page, admin panel not implemented yet.
- Test approach: Gray-box

**Test environment & dates:**  
- Start:  1.12.2025
- End:  
- Test environment: Windows 11, docker enviroment (local), firefox, burb/chromium browser, zap tool.

**Assumptions & constraints:**  
- No credentials provided
- Only phase 1 features (register screen and main page)
- access to database provided

---

# 2️⃣ Executive Summary

**Short summary (1-2 sentences):**  

**Overall risk level:** (Low / Medium / High / Critical)

**Top 5 immediate actions:**  
1.  Remove client controlled 'role' parameter | Not fixed
2.  validate roles on server side | not fixed
3.  Add proper session management with secure cookies | This one is wrong issue should be SQL Injection in registration
4.  Add CSRF protection and basic security headers | Not fixed
5.  Enforce HTTPS and stop sending passwords in cleartext  | Not fixed

---

# 3️⃣ Severity scale & definitions

|  **Severity Level**  | **Description**                                                                                                              | **Recommended Action**           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
|      🔴 **High**     | A serious vulnerability that can lead to full system compromise or data breach (e.g., SQL Injection, Remote Code Execution). | *Immediate fix required*         |
|     🟠 **Medium**    | A significant issue that may require specific conditions or user interaction (e.g., XSS, CSRF).                              | *Fix ASAP*                       |
|      🟡 **Low**      | A minor issue or configuration weakness (e.g., server version disclosure).                                                   | *Fix soon*                       |
| 🔵 **Info** | No direct risk, but useful for system hardening (e.g., missing security headers).                                            | *Monitor and fix in maintenance* |


---

# 4️⃣ Findings (filled with examples → replace)

> Fill in one row per finding. Focus on clarity and the most important issues.

| ID | Severity | Finding | Description | Evidence / Proof | Fixed/ Not Fixed | 
|------|-----------|----------|--------------|------------------|------|
| F-01 | 🔴 High | User can choose own role when registrating | Roles: administrator and Reserver | Screenshot of registration | Not Fixed: user still can choose own role when registrating |
| F-02 | 🔴 High | Client-controlled role parameter allows privilege escalation | Registration API accepts the `role` parameter directly from the client without server-side validation. Attacker can modify the POST request | Burp Repeater screenshot showing POST /register with `role=administrator` and server accepting it. | not fixed: in browser user can still change user role |
| F-03 | 🔴 High | SQL Injection in registration | Registration form accepts raw SQL payloads (e.g., `' ; DROP TABLE users;--`) without validation. Backend processes the request and returns 302 redirect instead of rejecting it. Indicates backend is vulnerable to SQL injection. | Burp Repeater screenshot showing SQLi payload and 302 success response | 
| F-04 | 🔴 High | Missing Session management | GET and POST responses isn't Set-Cookie header and nothing to identify session. This means no session isolation and authentication protection | Burp screenshot: Response headers for get / register contain no set-cookie header | Not fixed: no headers "Set-Cookie: Cookie: sessionid= auth= token=" |
| F-05 | 🟠 Medium | Missing Secure Cookie flags | Since the application sets no cookies, security flags such as `HttpOnly`, `Secure`, and `SameSite` are missing. This leaves the app unprotected against CSRF and XSS-related cookie abuse. | Response headers include only `content-type` and `content-length`, no cookies. | not fixed: no session management (cookie, bearer, session id missing) |

---

> [!NOTE]
> Include up to 5 findings total.   
> Keep each description short and clear.

---

# 5️⃣ OWASP ZAP Test Report (Attachment)

**Purpose:**  
- Attach or link your OWASP ZAP scan results (Markdown format preferred).

---

**Instructions (CMD version):**
1. Run OWASP ZAP baseline scan:  
   ```bash
   zap-baseline.py -t https://example.com -r zap_report_round1.html -J zap_report.json
   ```
2. Export results to markdown:  
   ```bash
   zap-cli report -o zap_report_round1.md -f markdown
   ```
3. Save the report as `zap_report_round1.md` and link it below.

---
> [!NOTE]
> 📁 **Attach full report:** → `check itslearning` → **Add a link here**

---