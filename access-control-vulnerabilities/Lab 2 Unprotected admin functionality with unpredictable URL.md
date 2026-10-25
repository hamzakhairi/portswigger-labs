
**Category:** Access Control Vulnerabilities  
**Difficulty:** Apprentice  
**Vulnerability:** Vertical Privilege Escalation  
**Platform:** PortSwigger Web Security Academy

## 1. Objective

The objective of this lab is to delete the user `carlos`.

The lab description mentions that the administrator panel is located at an unpredictable URL, but its location is disclosed somewhere within the application.

Our goal is to discover this URL and access the administrator panel without administrator privileges.

## 2. Reconnaissance

First, I inspected the HTML source code of the page using `Ctrl + U` in Chrome.

While reading the source code, I found the following JavaScript:

```html 
<script>
var isAdmin = false;
if (isAdmin) {
    var topLinksTag = document.getElementsByClassName("top-links")[0];
    var adminPanelTag = document.createElement('a');
    adminPanelTag.setAttribute('href', '/admin-lhtuzv');
    adminPanelTag.innerText = 'Admin panel';
    topLinksTag.append(adminPanelTag);
    var pTag = document.createElement('p');
    pTag.innerText = '|';
    topLinksTag.appendChild(pTag);
}
</script>
```

The JavaScript checks whether `isAdmin` is `true` before displaying the administrator panel link.

However, even though `isAdmin` is set to `false`, the administrator panel URL is still visible in the source code:

```html
/admin-lhtuzv
```

This reveals the location of the hidden administrator panel.

## 3. Exploitation

After discovering the administrator panel URL, I navigated directly to:

```
/admin-lhtuzv
```

The application allowed me to access the administrator panel without administrator privileges.

Inside the panel, I found a list of users with the option to delete their accounts.

I located the user `carlos` and clicked the delete button.

The application deleted the user, and the lab was successfully completed.

## 4. Root Cause

The main vulnerability is the lack of proper server-side authorization.

The application uses client-side JavaScript to control the visibility of the administrator panel link, but hiding the link does not restrict access to the actual endpoint.

The administrator URL is also exposed in the page source, making it easier to discover.

However, the primary security issue is that the server allows unauthorized users to access administrative functionality and delete user accounts.

## 5. Vulnerability Classification

**Type:** Broken Access Control

**Subtype:** Unprotected Administrative Functionality / Vertical Privilege Escalation

The vulnerability allows a user without administrator privileges to perform actions that should only be available to administrators.

## 6. Impact

An attacker can access the administrator panel without having administrator privileges.

This vulnerability could allow an attacker to:

- Access restricted administrative functionality.
- Delete user accounts without authorization.
- Perform other sensitive administrative actions if they are available in the panel.

## 7. Remediation

To prevent this vulnerability, developers should:

- **Enforce server-side authorization:** Verify that the user has administrator privileges before allowing access to the admin panel or executing administrative actions.
- **Never rely on hidden URLs:** An unpredictable admin panel URL is not a security mechanism.
- **Avoid exposing sensitive paths unnecessarily:** Do not include administrator-only URLs in publicly accessible JavaScript unless necessary.
- **Implement Role-Based Access Control (RBAC):** Restrict administrative functionality to users with the appropriate permissions.
- **Deny access by default:** Return `403 Forbidden` when an authenticated user does not have permission to access an administrative resource.

## 8. Lessons Learned

From this lab, I learned that:

- Reading the HTML source code can reveal hidden endpoints and sensitive application paths.
- JavaScript code running in the browser is visible to users, even when certain functionality is hidden using conditions such as `isAdmin = false`.
- Hiding an admin panel link does not prevent attackers from accessing the actual endpoint.
- Authentication and authorization must be enforced on the server side.
- Unpredictable URLs should never replace proper access control mechanisms.