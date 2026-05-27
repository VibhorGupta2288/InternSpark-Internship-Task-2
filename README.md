# InternSpark-Internship-Task-2
 Web Application Vulnerability Assessment Report


## Table of Contents

1. Overview
2. Target Information
3. Tool Used
4. Identified Vulnerabilities
5. Risk Summary
6. Key Security Observations
7. Recommendations
8. Conclusion


## Overview

This project demonstrates a manual web application security assessment performed on the OWASP Juice Shop application using Burp Suite. The assessment focused on identifying common web vulnerabilities by intercepting and modifying HTTP requests.

The testing identified two critical vulnerabilities: SQL Injection leading to authentication bypass, and Cross-Site Scripting (XSS). The objective was to understand how insecure input validation and improper output handling can lead to serious security risks in web applications.


## Target Information

| Parameter | Value |
|-----------|-------|
| Application | OWASP Juice Shop |
| URL | http://localhost:3000 |
| Testing Method | Manual Testing |
| Testing Tool | Burp Suite |


## Tool Used

Burp Suite was used as an intercepting proxy throughout the assessment. It was used to capture HTTP requests and responses, modify request parameters, test authentication mechanisms, inject malicious payloads, and analyze overall application behavior.


## Identified Vulnerabilities


### SQL Injection (Authentication Bypass)

Risk Level: High

The login functionality failed to properly validate user supplied input, allowing SQL Injection attacks through the authentication fields. By manipulating the login request, authentication controls were bypassed successfully.

**Testing Steps Performed**

The login request was intercepted using Burp Suite and sent to the Repeater module. The input fields were then modified with a SQL Injection payload and the manipulated request was forwarded to the server.

**Payload Used**

```json
{
  "email": "' OR 1=1--",
  "password": "anything"
}
```

**Evidence**

The server returned an HTTP 200 OK response along with a valid authentication token, the admin user email address admin@juice-sh.op, and full administrative access privileges. This confirms successful authentication bypass through SQL Injection.

**Impact**

Successful exploitation may result in unauthorized admin access, exposure of sensitive information, full application compromise, privilege escalation, and unauthorized data manipulation.

**Mitigation**

Use parameterized queries and prepared statements. Validate and sanitize all user supplied input. Avoid dynamic SQL query construction. Implement secure authentication logic. Use ORM frameworks where possible.


### Cross-Site Scripting (XSS)

Risk Level: High

The application allowed execution of malicious JavaScript through user controlled input fields without proper sanitization or output encoding.

**Testing Steps Performed**

A vulnerable input field was identified within the application. A malicious XSS payload was injected into the field and the request was submitted. The resulting browser execution behavior was then observed.

**Payload Used**

```
<script>alert(1)</script>
```

**Evidence**

The browser displayed a popup dialog containing the value 1 originating from localhost:3000. This confirms successful execution of injected JavaScript within the application context.

**Impact**

Successful XSS attacks may lead to session hijacking, cookie theft, execution of malicious scripts, user impersonation, and unauthorized actions performed on behalf of legitimate users.

**Mitigation**

Sanitize all user inputs before processing. Implement proper output encoding. Use a Content Security Policy header. Avoid rendering raw user input directly into HTML. Validate user supplied data on both the client and server sides.


## Risk Summary

| Vulnerability | Risk Level |
|--------------|-----------|
| SQL Injection (Authentication Bypass) | High |
| Cross-Site Scripting (XSS) | High |


## Key Security Observations

The application lacked proper server side input validation. User input was processed insecurely throughout the application. Authentication mechanisms were vulnerable to injection based attacks. Browser executed scripts were not adequately sanitized before rendering. Security controls for preventing common OWASP Top 10 vulnerabilities were insufficient.


## Recommendations


### Authentication Security

Implement prepared statements for all database interactions. Enforce strict input validation on all authentication fields. Prevent direct query string concatenation in any form.


### Application Security

Encode all user generated output before rendering it in the browser. Apply secure coding practices throughout the development lifecycle. Implement Content Security Policy headers across the application. Use modern and well maintained security frameworks.


### Monitoring and Testing

Conduct regular vulnerability assessments on a scheduled basis. Perform penetration testing periodically. Monitor authentication logs for anomalies and suspicious patterns. Review application logs consistently for signs of malicious activity.


## Conclusion

The assessment successfully identified critical web application vulnerabilities in the OWASP Juice Shop application through manual testing using Burp Suite. The vulnerabilities demonstrated how improper input validation and insecure output handling can lead to authentication bypass, unauthorized access, client side script execution, and potential full application compromise.

The findings emphasize the importance of secure coding practices, proper input validation, and continuous security testing throughout the web application development lifecycle.


## Disclaimer

This assessment was conducted on a deliberately vulnerable application in a controlled lab environment strictly for educational purposes. Never perform vulnerability testing or inject payloads into applications you do not own or have explicit written permission to test.
