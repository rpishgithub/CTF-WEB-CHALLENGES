# 前女友{SKCTF}

## 解题过程

---

viewsource 看到code.txt

![1552229057796](C:\Users\rpish\AppData\Roaming\Typora\typora-user-images\1552229057796.png)

进入code.txt看代码

```php
<?php
if(isset($_GET['v1']) && isset($_GET['v2']) && isset($_GET['v3'])){
    $v1 = $_GET['v1'];
    $v2 = $_GET['v2'];
    $v3 = $_GET['v3'];
    if($v1 != $v2 && md5($v1) == md5($v2)){
        if(!strcmp($v3, $flag)){
            echo $flag;
        }
    }
}
?>
```

可见我们要传入v1,v2,v3三个值,v1和v2值不同但是md5值相同(欸嘿嘿 这不就是之前见过的magic hash吗)

那v1,v2好说,但是v3这有个和flag比较就蒙圈了.

(我一开始以为strcmp值相同会返回false,随便输一个就好了,然而试了很多次都没有效果,看了别人的wp和手册才知道相同返回0)

然后就蒙圈了,如果我知道flag,为什么还要给v3比较用,这不是脱裤子放屁吗.

欸 看了下wp,这边还有一个漏洞,是strcmp()的,学习了学习了.

然后我们构造query

```php
?v1=QNKCDZO&v2=240610708&v3[]=1
```

访问获得flag



## 学习知识

---

1.strcmp(string \$str1,string $str2 ):int

Returns <0 if str1 is less than str2; >0 if str1 is greater than str2, and 0 if they are equal.



2.Magic Hash中md5值相同的两个值

QNKCDZO

240610708



3.PHP strcmp函数漏洞

php5.3以及之后版本,用strcmp()将数组和字符串进行比较会返回0

像strcmp([],'flag') => 0



## 参考资料

---

PHP strcmp:http://php.net/manual/en/function.strcmp.php

Magic Hash:https://www.freebuf.com/news/67007.html

CTF之MD5相等值不相等:https://www.cnblogs.com/chasjd/p/9383646.html

PHP strcmp函数漏洞:https://www.jianshu.com/p/d91c3357b4d3