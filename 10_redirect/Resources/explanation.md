# 10_redirect

Redirect links stored in the front-end are unsecure and can be manipulated

## Method

Go to any footer page <http://192.168.1.102>
Inspect any of the social media icons
Edit the target link by changing the href to something else

```

<a href="index.php?page=redirect&site=facebook" class="icon fa-facebook">

```

Change it to this, for example

```

<a href="index.php?page=redirect&site=My_own_site" class="icon fa-facebook">

```

This will then give us the flag
Which is:   B9E775A0291FED784A2D9680FCFAD7EDD6B8CDF87648DA647AAF4BBA288BCAB3

## Manipulation

We can use this flaw to redirect users to a false website for phishing. Imagine that your bank website had such a flaw, hackers could create false websites and the link would seem legit since it would contain your bank's address, as soon as it happens to someone not very observant, or isn't used to technology, he/she will think to be on the site of his bank and will enter his banking information.

## Prevention

Create the  redirect link from the backend. You could use an API for example.
