# 127.0.0.53 DNS服务器解析域名失败解决方法

127.0.0.53 通常是ubuntu的默认本地DNS服务器，但是它存在胡乱解析的情况。比如：

```bash
$ nslookup raw.githubusercontent.com
Server:        127.0.0.53
Address:    127.0.0.53#53

Non-authoritative answer:
Name:    raw.githubusercontent.com
Address: 0.0.0.0
Name:    raw.githubusercontent.com
Address: ::
```

可以看到raw.githubusercontent.com被127.0.0.53解析成了0.0.0.0，从而导致我们不能访问到正确的站点。解决的方式就是把dns服务器换成8.8.8.8。

可以参考[Ubuntu无法解析域名DNS指向127.0.0.53问题处理_ubuntu 127.0.0.53-CSDN博客](https://blog.csdn.net/u010784529/article/details/134946658)，这里也做简单的说明。

我们直接编辑`/etc/resolv.conf `文件修改dns服务器即可：

```bash
# 用你喜欢的编辑器打开
$ sudo vim  /etc/resolv.conf
# 把 nameserver修改成8.8.8.8
nameserver 8.8.8.8
# 保存并退出
# 再次使用nslookup 进行查找
$ nslookup raw.githubusercontent.com
Server:        8.8.8.8
Address:    8.8.8.8#53

Non-authoritative answer:
Name:    raw.githubusercontent.com
Address: 185.199.108.133
Name:    raw.githubusercontent.com
Address: 185.199.111.133
Name:    raw.githubusercontent.com
Address: 185.199.109.133
Name:    raw.githubusercontent.com
Address: 185.199.110.133
Name:    raw.githubusercontent.com
Address: 2606:50c0:8000::154
Name:    raw.githubusercontent.com
Address: 2606:50c0:8001::154
Name:    raw.githubusercontent.com
Address: 2606:50c0:8002::154
Name:    raw.githubusercontent.com
Address: 2606:50c0:8003::154

# 可以看到此时的值为非0.0.0.0
```

之后从raw.githubusercontent.com下载文件应该就可以了。
