# b3dr0ck

![image-20250603164430290](.\assets\image-20250603164430290.png)

直接访问发现跳转到了4040端口

先用nmap进行一下端口扫描吧

```
PORT      STATE SERVICE      VERSION
22/tcp    open  ssh          OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http         nginx 1.18.0 (Ubuntu)
4040/tcp  open  ssl/yo-main?
| fingerprint-strings:
|   GetRequest, HTTPOptions:
|     HTTP/1.1 200 OK
|     Content-type: text/html
|     Date: Tue, 03 Jun 2025 08:45:34 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html>
|     <head>
|     <title>ABC</title>
|     <style>
|     body {
|     width: 35em;
|     margin: 0 auto;
|     font-family: Tahoma, Verdana, Arial, sans-serif;
|     </style>
|     </head>
|     <body>
|     <h1>Welcome to ABC!</h1>
|     <p>Abbadabba Broadcasting Compandy</p>
|     <p>We're in the process of building a website! Can you believe this technology exists in bedrock?!?</p>
|     <p>Barney is helping to setup the server, and he said this info was important...</p>
|     <pre>
|     Hey, it's Barney. I only figured out nginx so far, what the h3ll is a database?!?
|     Bamm Bamm tried to setup a sql database, but I don't see it running.
|     Looks like it started something else, but I'm not sure how to turn it off...
|     said it was from the toilet and OVER 9000!
|_    Need to try and secure
9009/tcp  open  pichat?
| fingerprint-strings:
|   NULL:
|     ____ _____
|     \x20\x20 / / | | | | /\x20 | _ \x20/ ____|
|     \x20\x20 /\x20 / /__| | ___ ___ _ __ ___ ___ | |_ ___ / \x20 | |_) | |
|     \x20/ / / _ \x20|/ __/ _ \| '_ ` _ \x20/ _ \x20| __/ _ \x20 / /\x20\x20| _ <| |
|     \x20 /\x20 / __/ | (_| (_) | | | | | | __/ | || (_) | / ____ \| |_) | |____
|     ___|_|______/|_| |_| |_|___| _____/ /_/ _____/ _____|
|_    What are you looking for?
54321/tcp open  ssl/unknown
| fingerprint-strings:
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, Kerberos, LDAPSearchReq, LPDString, NULL, NotesRPC, RPCCheck, RTSPRequest, SIPOptions, SMBProgNeg, SSLSessionReq, TerminalServer, TerminalServerCookie, X11Probe, afp, giop, ms-sql-s:
|_    Error: 'undefined' is not authorized for access.
3 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
```





访问54321

![image-20250603170609830](.\assets\image-20250603170609830.png)

访问呢9001

![image-20250603170831964](.\assets\image-20250603170831964.png)

![image-20250603172805544](.\assets\image-20250603172805544.png)

这个可以看到是私钥

![image-20250603172916575](.\assets\image-20250603172916575.png)

这个我们认为是公钥



我们查看一下--help

**![image-20250603173052648](.\assets\image-20250603173052648.png)**

```
socat stdio ssl:MACHINE_IP:54321,cert=<CERT_FILE>,key=<KEY_FILE>,verify=0
```

![image-20250603173505390](.\assets\image-20250603173505390.png)

这个提示这个服务是为了login和password提示用的

输入password，我们就得到了密码

***ssh barney@IP\***

我们直接登录，得到shell

我们通过sudo -l 查询一下权限，我们看到了

![No alt text provided for this image](https://media.licdn.com/dms/image/v2/D4D12AQFEyn2hOJB6Mg/article-inline_image-shrink_400_744/article-inline_image-shrink_400_744/0/1661619728527?e=2147483647&v=beta&t=2mSmUACaiEyjKOttqaOnlI0CQu2pcVFD_BHvLJ6SxJQ)

我们可以查看证书信息

查看证书信息
```
certutil -dump <certificate_file>
```
certutil -h将显示如何列出当前证书。
```
certutil ls   列出证书文件
```

```
sudo certutil -a fred.csr.pem来检索私钥和证书：
```

我们直接拿着这个登录

![image-20250603175243681](.\assets\image-20250603175243681.png)

拿到了fred的shell

![image-20250603175342991](.\assets\image-20250603175342991.png)

sudo -l发现开始送分了

![image-20250603175631983](.\assets\image-20250603175631983.png)

先base64再base32在base64得到md5，在破解

然后得到root的密码

最后直接su root

![image-20250603175859635](.\assets\image-20250603175859635.png)

