## What are Access Control Vulnerabilities?

Access Control Vulnerabilities occur when an application fails to properly check whether a user has permission to access specific information or perform certain actions.

In simple terms, the application may know who you are, but it does not correctly verify what you are allowed to do.

## 1. Authentication vs. Authorization

**Authentication:** Verifies who you are.

**Authorization:** Verifies what you are allowed to do.

### Example: Banking Application

Imagine that you have an account at a bank.

When you log in using your username and password, the bank verifies your identity. This process is called **Authentication**.

After logging in, you can view your balance, transfer money, and update your personal information.

However, the bank must verify that you have permission to perform each action.

For example, you should only be able to view your own account balance, not another customer's private banking information.

This permission verification is called **Authorization**.

## 2. Types of Privilege Escalation

### Vertical Privilege Escalation

Vertical Privilege Escalation occurs when a user can perform actions that require a higher level of privilege.

**Example:**

A normal user accesses an administrator-only page and successfully deletes another user's account.

### Horizontal Privilege Escalation

Horizontal Privilege Escalation occurs when a user accesses resources or performs actions belonging to another user at the same privilege level without permission.

**Example:**

Imagine an application uses the following endpoint to display user information:

`GET /user/1`

An attacker changes the ID:

`GET /user/2`

If the application returns another user's private information without checking ownership or permission, it is vulnerable to Horizontal Privilege Escalation.

This is also a common example of **IDOR (Insecure Direct Object Reference)**.

## Key Takeaways

- Authentication verifies identity.
- Authorization verifies permissions.
- Access control enforces authorization rules.
- Vertical Privilege Escalation allows access to higher-privileged functionality.
- Horizontal Privilege Escalation allows unauthorized access to another user's resources at the same privilege level.
- Access control must be enforced on the server side, not only in the frontend.