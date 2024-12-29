很多企业官网使用的WordPress搭建。
使用wpscan工具扫描历史漏洞和插件组件漏洞。
如：任意文件读取等。

kali中可以使用wpscan：
```text
wpscan --url https://www.xxxxx.wiki/
```

> 演示案例：

* 我们扫描192.168.43.104:8000这个站点（此站点存在一个WordPress进程）

![](https://pic3.zhimg.com/v2-b90cd10071461ee62a625f52f4698a20_r.jpg)

![](https://pic4.zhimg.com/v2-f30e534aa4bbe3e6d6580aa7c5b32aff_r.jpg)

## 五、扫描指定用户

* 格式：

```text
wpscan --url https://www.xxxxxxx.wiki/ --enumerate u
```

## 六、**暴力破解得到密码**

* **在暴力破解之前，需要提供一个字典文件**
* 格式：

```text
wpscan --url  https://www.xxxxx.wiki/  -e  u --wordlist 字典文件路径
```

## 七、**扫描插件漏洞**

* 插件可以扩展`WordPress`站点的功能，但很多插件中都存在安全漏洞，而这也会给攻击者提供可乘之机
* 我们可以使用下列命令扫描`WordPress`站点中安装的插件：

1. wpscan --url [https://www.xxxxx.wiki/](https://link.zhihu.com/?target=https%3A//www.xxxxx.wiki/) --enumerate p
2.
3. //备注：--url与-u参数相同，下面雷同

* 我们可以使用下列命令来扫描目标插件中的安全漏洞：

```text
wpscan --url https://www.xxxxx.wiki/ --enumerate vp
```

## 八、**主题漏洞扫描**

* 使用下列命令对主题进行扫描：

```text
wpscan --url https://www.xxxxx.wiki --enumerate t
```

> 演示案例：

![](https://pic1.zhimg.com/v2-3673956c30b6f8797b6bd8788b7e17ce_r.jpg)

* 看到总共数据库中的402个主题，发现了3个主题，但是发现的主题不一定有这么多，会在下面显示

![](https://picx.zhimg.com/v2-6e69696ff70649a8dd4a643e0653e2df_r.jpg)

* 使用下列命令扫描主题中存在的漏洞：

```text
wpscan --url https://www.xxxxxx.wiki --enumerate vt
```

## 九、**`TimThumbs`文件漏洞扫描**

```text
wpscan -u https://www.xxxxxx.wiki/ -enumerate tt
```

