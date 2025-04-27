# Booking System – Phase 1 – Penetration Testing Report

## 1. Introduction

This penetration test was conducted on the Booking System application's registration page at `http://localhost:8000/register`. The test was performed in a Kali Linux environment using Burp Suite, Docker, and Firefox. The goal was to identify vulnerabilities and evaluate the security posture of the application.

**Test Date**: April 24, 2025  
**Test Methodology**: Gray-box (access to the system, no source code)

---

## 2. Summary of Findings

| #   | Vulnerability                    | Severity |
| --- | -------------------------------- | -------- |
| 1   | SQL Injection in username field  | Critical |
| 2   | Role escalation via `role=admin` | Critical |
| 3   | Server error on invalid role     | Medium   |
| 4   | Weak error handling              | Low      |

---

## 3. Detailed Findings

### 3.1 SQL Injection in Username Field

**Payload Used:**

role=admin

**Result:** Server responded with a `500 Internal Server Error`, indicating backend attempted to process elevated role.

**Impact:** Reveals improper access control and lack of role validation.

**Recommendation:**

- Never allow role selection via client
- Assign roles only server-side
- Return `400` or `403` instead of crashing

---

### 3.3 Improper Role Validation (Broken Access Control)

**Payload Used:**

role=admin

**Result:** `500 Internal Server Error` returned

**Impact:** Backend crashes on unknown role input. Reveals potential internal logic and database operations.

**Recommendation:**

- Validate roles strictly
- Implement secure error handling
- Log internally but return user-friendly messages

---

### 3.4 Weak Error Handling

**Observation:** When forms fail, server responds with:

Error during registration

**Impact:** Not informative to users, and may expose backend behavior

**Recommendation:**

- Provide clear but secure feedback
- Log server errors internally

---

## 4. Tools Used

- Kali Linux
- Burp Suite Community Edition
- Docker / docker-compose
- Firefox

---

## 5. Conclusion

The Booking System's registration page is vulnerable to multiple critical issues, including SQL injection and broken access control. Role-based permissions are not securely managed. Immediate remediation is advised before deployment.
