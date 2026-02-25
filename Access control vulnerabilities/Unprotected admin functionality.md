this lab has an unprotected admin panel.

we see file `robots.txt`
**`Robots.txt`** is a plain file placed in root of directory of a website , that provide instructions to web crawlers .

open file :
```shell
/robots.txt
```

result :
```
User-agent: *
Disallow: /administrator-panel
```

we visit this page :
```
/administrator-panel
```

![[/images/Screenshot from 2026-02-23 16-49-20.png]]

delete user `User deleted successfully!`
