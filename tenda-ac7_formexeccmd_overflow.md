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
