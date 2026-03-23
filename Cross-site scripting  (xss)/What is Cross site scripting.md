## What is Cross-Site Scripting (XSS)?

Cross-Site Scripting (XSS) is a common web security vulnerability that allows an attacker to inject malicious scripts (usually JavaScript) into web pages viewed by other users. Instead of being executed on the attacker’s machine, the script runs in the victim’s browser, within the context of a trusted website.

Because the browser trusts the website, it also trusts the injected script. This can lead to serious consequences such as:

- Stealing session cookies
- Hijacking user accounts
- Defacing websites
- Redirecting users to malicious sites

XSS is often one of the first vulnerabilities tested by security researchers due to its simplicity and high impact.

---

## Types of XSS

There are three main types of Cross-Site Scripting:
### 1. Stored XSS (Persistent XSS)

Stored XSS occurs when the malicious payload is permanently stored on the target server (e.g., in a database, comment field, or user profile).

**How it works:**

- Attacker injects a script into a vulnerable input field.
- The server stores the payload.
- When other users access that data, the script executes in their browser.

**Example:**

```html
<script>alert('XSS')</script>
```

**Impact:**  
Very dangerous because it affects every user who views the infected page.

---

### 2. Reflected XSS (Non-Persistent XSS)

Reflected XSS occurs when the malicious script is included in a request (usually via URL) and immediately reflected in the server’s response.

**How it works:**

- Attacker crafts a malicious link.
- Victim clicks the link.
- The server reflects the payload in the response.
- The script executes in the victim’s browser.

**Example:**

```
https://example.com/search?q=<script>alert(1)</script>
```

**Impact:**  
Requires user interaction (clicking a link), but still very effective in phishing attacks.

---

### 3. DOM-Based XSS

DOM XSS happens entirely on the client side. The vulnerability exists in JavaScript code that modifies the DOM without proper validation or sanitization.

**How it works:**

- JavaScript reads user input (e.g., from URL).
- It inserts the data into the page without sanitizing it.
- The browser executes the injected script.

**Example:**

```javascript
document.innerHTML = location.hash;
```

**Impact:**  
Harder to detect because it does not involve the server directly.

---

## How to Protect Your Website from XSS

Preventing XSS requires a combination of secure coding practices and proper defenses:

### 1. Input Validation

- Validate all user inputs (length, format, type).
- Reject unexpected or dangerous input.

### 2. Output Encoding (Most Important)

- Escape data before rendering it in HTML.
- Use context-aware encoding:
    - HTML encoding
    - JavaScript encoding
    - URL encoding

### 3. Use Safe APIs

- Avoid dangerous functions like:
    
    - `innerHTML`
    - `document.write`
- Use safer alternatives:
    
    - `textContent`
    - `setAttribute`

### 4. Content Security Policy (CSP)

- Implement CSP headers to restrict script execution.

```http
Content-Security-Policy: default-src 'self'; script-src 'self'
```

### 5. Use Framework Security Features

Modern frameworks (React, Angular, Vue) automatically escape output, reducing XSS risk.

### 6. HTTPOnly Cookies

- Prevent JavaScript from accessing session cookies:

```http
Set-Cookie: session=abc123; HttpOnly
```

### 7. Sanitize User Input

- Use libraries like DOMPurify to clean HTML input.

---
## Conclusion

Cross-Site Scripting (XSS) is a powerful and widespread vulnerability that targets users rather than servers. Understanding its types—Stored, Reflected, and DOM-based—is essential for both attackers and defenders.

By applying proper input validation, output encoding, and modern security practices, developers can significantly reduce the risk of XSS and protect users from client-side attacks.