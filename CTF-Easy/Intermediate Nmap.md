# Intermediate Nmap

![image-20250607152156196](./assets/image-20250607152156196.png)

进行全端口信息扫描。

```
 nmap 10.10.141.93 -p- -sS -sV -A -T5
```

![image-20250607152330429](./assets/image-20250607152330429.png)

可以看到开放了31337端口

![image-20250607152433013](./assets/image-20250607152433013.png)

得到了密码凭证

```
In case I forget - user:pass
ubuntu:Dafdas!!/str0ng
```

ssh连接得到flag