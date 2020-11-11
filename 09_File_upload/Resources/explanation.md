# 09_File_upload

Unsecure file upload of image in the upload. <http://192.168.1.102/index.php?page=upload>

## Method

Go to the image upload page(<http://192.168.1.102/index.php?page=upload>). 
Inspect the form input.
We can see that it's set to `type=file`
Which means that we can force the frontend to accept a php file by using curl.
Since there is no check of the file in backend our php file will pass.

What we will do there is just curl this empty.php file which contains nothing

```

curl -X POST -H 'Content-Type: multipart/form-data' -F 'Upload=send' -F 'uploaded=@empty.php;type=image/jpeg' http://192.168.99.100/index.php\?page\=upload\# | grep "flag"

```

This will return an empty html page with the flag
which is:   46910d9ce35b385885a9f7e2b336249d622f29b267a1771fbacf52133beddba8

## Manipulation

This is really dangerous. Image being able to send php request into the website / backend.
Imagine being able to put .php code on a site? It opens the doors to everything.
You could send sql statements to request passwords and sensitive data.

## Prevention

Check in backend the type and the header of the file, not only in frontend.
This way you limit the type of files uploaded to files tht can't do any harm.
