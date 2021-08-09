# Baby Web

![date](https://img.shields.io/badge/date-07.08.2021-brightgreen.svg)  
![warmup category](https://img.shields.io/badge/category-web-red.svg)
![score](https://img.shields.io/badge/score-420-blue.svg)
![solves](https://img.shields.io/badge/solves-68-brightgreen.svg)

## Description

Just a place to see list of all challs from bsides noida CTF, maybe some flag too xD
Note : Bruteforce is not required.


## Attached files

- [Website](http://ctf.babyweb.bsidesnoida.in/)
- [Source](https://storage.googleapis.com/noida_ctf/Web/baby_web.zip)

## Summary
This challenge is meant to be one of the beginner challenges for the web category in this CTF.
We get access to a website which takes an ID value and shows data for the challenge corresponding to this ID.

## Detailed Solution
There is a file named karma.db that is being queried by the PHP code to get data for all the questions.

The ctf.conf file tells us that the server is hosted using nginx serving us the index.php file on the root route

```
user www;
pid /run/nginx.pid;
error_log /dev/stderr info;
...

		location / {
            try_files $uri $uri/ /index.php?$query_string;
...
```

Hence, you just ask it to serve the karma.db file to you.
http://ctf.babyweb.bsidesnoida.in/karma.db fetches the database which can be opened in any sqlite3 viewer. You see a table called **flagsss** in it which contains the flag.

## Flag
```
BSNoida{4_v3ry_w4rm_w31c0m3_2_bs1d35_n01d4}
```

## Alternative thoughts
There might be another solution to this, albeit a bit harder and unnecessary involving SQLi.
```
...
if ($args ~ [%]){
    return 500;
}

if ( $arg_chall_id ~ [A-Za-z_.%]){
    return 500;
}

error_page 500 error.html;
...
```
The code above is part of the ctf.conf file. Here is is checking for any % symbols in the query-string. Hence any sort of URL encoded strings are out of question. Similarly, it checks for any letters, underscores, periods or percentage symbols in the *chall_id* parameter. Hence we seem to not be able to execute any straightforward SQL injections like **4 OR true**.

However, we can bypass this in a clever way. If we observe, the **$arg_chall_id** only looks at the first occurrence of the parameter. Hence, we can use a duplicate parameter to input data that was previously not allowed such as letters.

For example, http://ctf.babyweb.bsidesnoida.in/?chall_id=1&chall_id=4+or+true works as an SQLi, even though it is not of any significance.

## Author
[Additya SInghal](https://github.com/UnknownAbyss)
