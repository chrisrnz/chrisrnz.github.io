---
layout: default
title: "Markdown Test"
date: 2017-01-14
---
# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and
![Image](https://avatars1.githubusercontent.com/u/6965555?v=3&s=460)


Markdown syntax hilighting

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```


bash syntax hilighting

```bash
#!/bin/bash

if [ "$UID" -ne 0 ]
then
 echo "Superuser rights required"
 exit 2
fi
```


nginx syntax hilighting

```nginx
server {
    server_name   one.example.com  www.one.example.com;
    access_log   /var/log/nginx.access_log  main;

    rewrite (.*) /index.php?page=$1 break;

    location / {
        proxy_pass         http://127.0.0.1/;
        proxy_redirect     off;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        charset            koi8-r;
    }
}
```
