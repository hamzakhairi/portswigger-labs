## Lab: Unprotected Admin Functionality

This lab demonstrates an **access control vulnerability** where administrative functionality is exposed without proper authorization checks.

The application contains an admin interface that is **not protected by authentication or role-based access control**, allowing any user to access sensitive administrative actions by directly visiting the admin endpoint.

The objective of this lab is to **identify the hidden admin panel** and perform an unauthorized administrative action, proving that access control is enforced only at the UI level and not on the server side.
