this lab an unprotected admin panel. it's located at an unpredictable location, but the location is disclosed somewhere in the application.

read the js source code :

```js
<script>
var isAdmin = false;
if (isAdmin) {
	var topLinksTag = document.getElementsByClassName("top-links")[0];
	var adminPanelTag = document.createElement('a');
	adminPanelTag.setAttribute('href', '/admin-16if8m');
	adminPanelTag.innerText = 'Admin panel';
	topLinksTag.append(adminPanelTag);
	var pTag = document.createElement('p');
	pTag.innerText = '\|';
	topLinksTag.appendChild(pTag);
}
</script>
```

simply we can see the rout of admin panel :
```shell
/admin-16if8m
```

![[/images/Screenshot from 2026-02-23 17-10-38.png]]
