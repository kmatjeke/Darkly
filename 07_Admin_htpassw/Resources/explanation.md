# 07_Admin_htpassw

Robots.txt files give information to web robots, these can be used as an exploit.

## Method

Go to <http://192.168.1.102/robots.txt>  
We get the following information

```

User-agent: *
Disallow: /whatever
Disallow: /.hidden

```

## Manipulation

We will find the flag in the `/whatever` folder.
Go to <http://192.168.1.102/whatever>  
We can then download a file called `htpasswd`
Which contains:

```

root:8621ffdbc5698829397d97767ac13db3

```

We can then exploit this as the user login & hashed password
decode the hash(which is 'dragon').

We can now go to <http://192.168.1.102/admin/> and login with:

```

username:   root
password:   dragon

```

Which will then give us the flag: d19b4823e0d5600ceed56d5e896ef328d7a2b9e7ac7e80f4fcdb9b10bcb3e7ff

## prevention

robots.txt is not considered reliable, so avoid using them.
if you insist on using them make sure that the folders listed within
have restricted access. You can do this by using a router on the front-end.
This is also done to ensure users do not have accessto the servers files using ../
