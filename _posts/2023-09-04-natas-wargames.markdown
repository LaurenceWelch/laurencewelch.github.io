---
layout: post
title:  "Natas Wargames"
date:   2023-09-04
categories: jekyll update
---

|![a spiderweb](https://images.unsplash.com/photo-1600463241302-88b0e1a51175?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1470&q=80)|
|:--:|
|*photo by Shannon Potter via unsplash*|

# Level 0
Natas teaches serverside security basics. Each level is a website. For this level, navigate to the website, enter and credentials, and then open the dev console to inspect the html elements. It is also possible to use `curl` to get the html.
```bash
curl -u natas0:natas0 [website]
```
We could save some future typing by making a script that will print the correct url for each level.
```bash
#!/bin/bash
if [[ -z $1 ]] ; then
exit
fi
echo "http://natas$1.natas.labs.overthewire.org"
```

# Level 1
Let's curl.
```bash
curl -u natas1:[password] $(./[our echo url script] 1)
```

# Level 2
Let's navigate to the website and open the inspector. looking at the index file, we can see that an image is being referenced. The path being used to locate the image file is a relative path which would indicate that the `files` directory is in the current directory. Let's try adding `/files` to the url and see where it takes us.

# Level 3
This time we are given a hint in the index file. It mentions that not even google will find the leak. Google uses web crawlers to scan the internet. Developers can use a `robots.txt` files to control which urls the crawlers will attempt to access. Let's see if we can find anthing in that text file by appending `/robots.txt` to the url. After doing that, another potential path is given to us. Let's append that path to the original url.

# Level 4
When you navigate to a website, various metadata is attached to the request. One of these attributes is referer, which is used to document where you are coming from. This level wants us to enter this level from natas5. However, we can simply spoof the referer to make it look like we are coming from natas5. We can achieve this with the `--referer` tag using curl.
```bash
curl -u [username]:[password] --referer [natas5 url] [natas4 url]
```

# Level 5
When we enter our credentials for level 5, we are given an error saying that we aren't logged in. One common way websites handle authentication is through cookies. When you perform an action on a website, a cookie is often times included with the request and is used to authenticate your request. This is more conveniant than entering your credentials a bunch of time. Imagine for instance having to type your username and password everytime you clicked on a new page, that would be a pain. Instead of demanding credentials everytime you do something, websites will give you a cookie (or a token, or both) and include these each time you request something from the server. In the inspector, we can see cookies in the storage tab. In this case, we can see a cookie called `loggedin`. Upon clicking that cookie, we can see that it has a value field set to 0. Let's change that to 1 and then refresh the page and see what happens. 

# Level 6
