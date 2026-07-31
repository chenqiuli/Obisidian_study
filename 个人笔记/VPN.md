```bash
curl -x http://10.122.10.31:8889 http://ipinfo.io/ip
export HTTP_PROXY=http://10.122.10.31:8889
export HTTPS_PROXY=http://10.122.10.31:8889
```

```bash
# 测试代理是否有效，输出应该为 172.197.176.53
curl -x http://ifca_user:Ifca123456@172.197.176.53:13128 http://ipinfo.io/ip

export HTTP_PROXY=http://ifca_user:Ifca123456@172.197.176.53:13128
export HTTPS_PROXY=http://ifca_user:Ifca123456@172.197.176.53:13128
```


![](Pasted%20image%2020260728233238.png)

![](Pasted%20image%2020260728233255.png)

