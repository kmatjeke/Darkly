# 00_Recovery_Page

Unsecure email address stored in the front-end.

## Method

Go to the recovery page (<http://192.168.1.59/?page=recover#>)  
Inspect the submit button element  
Change email value to (your own email)  
Click submit  
It then sends the recovery email to your email address  

## Manipulation

By hiding the email in the front-end, we can inspect the submit button element and change the email value to anything we want.
We can then gain login information.

## Prevention

Data sent to the user must be validated on the server once the last page has been received.
And hiding information on the front-end is unwise, as it doesn't even need an experienced hacker to gain that information
