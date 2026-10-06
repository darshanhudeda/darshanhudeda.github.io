# Reflected/DOM-Based XSS in Search Feature (OWASP Juice Shop)

## Objective
Determine if user input in the search feature is properly sanitized before being rendered to the page.

## Method
Payload entered into the Juice Shop search bar:
<iframe src="javascript:alert('XSS')"> ```

Target: OWASP Juice Shop (bkimminich/juice-shop), running locally via Docker on localhost:3000.

Evidence
![XSS alert triggered via search](../images/04-xss-search.png)

Browser executed the payload immediately on search, producing a JavaScript alert box originating from localhost:3000:
images/04-xss-search.png
Note: Juice Shop is an Angular single-page application. This search action is handled entirely client-side and does not generate a new server request — no corresponding entry appears in a network proxy (Burp Suite) for this specific payload, since the vulnerability exists in client-side DOM rendering, not server-side request handling.
Findings
The search feature reflects user-supplied input directly into the DOM without sanitization
This results in a DOM-based XSS: arbitrary JavaScript executes in the context of the victim's browser session
No server interaction is required to trigger the vulnerability
Detection Angle

This is meaningfully different from the network-layer findings in this portfolio (vsftpd, SSH, UnrealIRCd). Server-side logging or a WAF would not detect this, since the malicious payload never traverses the network in a form the server processes — it's rendered and executed purely client-side. Effective detection/mitigation here requires:

A strict Content Security Policy (CSP) blocking inline script execution
Browser-level XSS auditing
Framework-level auto-escaping (Angular includes XSS protections by default; this suggests they were bypassed, disabled, or insufficient for this input path)

Remediation
Sanitize and encode all user-supplied input before DOM insertion
Implement and enforce a strict CSP header
Review Angular template bindings for unsafe innerHTML or bypassed sanitization (e.g. bypassSecurityTrustHtml)







