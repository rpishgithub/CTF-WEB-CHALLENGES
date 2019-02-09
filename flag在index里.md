# flag在index里

> 这道题考察的是 本地文件包含漏洞(Local File Inclusion)的利用

##解题过程：

![1549740400771](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549740400771.png)

![1549740414021](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549740414021.png)

进入到解题页面，只有一个链接，查看源码也没有提示。



![1549740544529](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549740544529.png)

![1549740562949](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549740562949.png)

进入链接看看，一样没有什么特别的地方。

![1549740634137](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549740634137.png)



但是注意看它的URL，“ index.php?file= ”,会不会有文件包含漏洞？

我们测试一下，将“ file= ”接上"../"查看上一级目录，如果有报错说明存在LFI漏洞。

![1549740932971](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549740932971.png)

欸！虽然没有报错，不过它给了我们一个提示，这很可能是有这么个漏洞的。

既然题目叫做flag在index里，那我们试试利用LFI漏洞获取一下index.php的源码吧。

![1549741230428](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549741230428.png)

再对源码进行base64解码就获得了flag。

![1549741307020](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549741307020.png)



##学习内容：

> 这一块内容请阅读参考资料大佬的笔记，我基本是把他的再抄了一遍。



###LFI概念

​	没有对用户获取页面进行限制，导致访问任意文件的漏洞。

​	通过查阅DVWA File Inclusion Source我们可以有一个直观的了解，尤其是Low Leval的源码。

​	Medium Leval源码其实是对RFI(Remote File Inclusion)进行的防护，防止直接执行远程恶意代码。

​	High Leval源码则是限定只能获取指定页面，对LFI和RFI都进行了防护。

​	![1549741712464](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549741712464.png)



###LFI漏洞检测

#### ../上一级目录检测

```
?page=../
```

#### 常见页检测

```
?page=index.html
```

#### 直接读取/etc/passwd

```
?page=/etc/passwd
```



### LFI漏洞利用

####getshell

这个还没试过，不大理解。

#### 获取源码

```
?page=php://filter/convert.base64-encode/resource=index.php
```



###PHP 伪协议 

​	PHP支持协议与封装协议

![1549743415791](C:\Users\79825\Documents\GitHub\CTF-WEB-CHALLENGES\Images\1549743415791.png)

​	像上面获取源码的例子中，就用到了php://，它先将index.php读入，再base64编码后输出。



##参考资料：

PHP 本地文件包含漏洞(LFI) 笔记 https://zhuanlan.zhihu.com/p/50445145

PHP手册 http://php.net/manual/zh/wrappers.php.php



