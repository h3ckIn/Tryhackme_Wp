![image-20250603154439713](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603154439713.png)

![image-20250603155945844](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603155945844.png)

我们先用nmap进行扫描得到开放端口及信息

我们看一下txt中的信息

![image-20250603160152000](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603160152000.png)

我们看到他似乎启用了一个叫做export2pdf的工具

我们用dirb进行目录及文件扫描

![image-20250603160635101](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603160635101.png)

![image-20250603160640415](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603160640415.png)

但是不让登录

我们看一下登录页面

![image-20250603161540827](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603161540827.png)

尝试弱口令 admin:admin直接登录进去

![image-20250603161727862](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603161727862.png)

![image-20250603162043612](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603162043612.png)

当我们点击这个

![image-20250603162457185](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603162457185.png)

我们发现跳转到了一个php文件，以pdf的形式呈现

![image-20250603162528071](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603162528071.png)

我们抓包看看

![image-20250603162725580](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603162725580.png)

我们发现中间带了url，猜测是ssrf

 /internal/admin.php换成这个

![image-20250603163007335](E:\Tryhackme_Wp\CTF-Easy\assets\image-20250603163007335.png)

成功拿到了flag

