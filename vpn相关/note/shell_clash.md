### 本节用来记录终端clash使用

1.下载clash
    [clash](https://github.com/Dreamacro/clash/releases)
2.`gunzip clash*`解压
3.创建好`~/.config/clash`目录
4.在clash目录里面

​	 **文件找到连接之后通过下面连接来转换，我们使用的是clash，所以要转换成clash格式的连接**

```
https://acl4ssr-sub.github.io/
```

​	**文件最好通过已经可以连接外网的机器下载，然后传输上去**

```cc
wget -O config.yaml 您的订阅链接
wget -O Country.mmdb https://www.sub-speeder.com/client-download/Country.mmdb
```

注意：
    - 订阅链接一定要在专门的clash网站上面买，（ssr无法解析)

5.改变端口

```cc
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
export no_proxy="localhost, 127.0.0.1"
```

6.执行服务

```cc
chmod +x clash-linux-amd64-v1.9.0
setsid ./clash-linux-amd64-v1.9.0
```

7.测试

```cc
curl www.google.com
```

[原文链接](https://juejin.cn/post/7054941050216906760)