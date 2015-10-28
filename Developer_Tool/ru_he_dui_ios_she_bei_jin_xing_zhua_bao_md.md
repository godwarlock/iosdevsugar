# 如何对iOS设备进行抓包

以往对iOS设备进行抓包都是使用代理或者AP方式，但直接用代理的方式会抓到废数据


正确方法如下：

1. 通过USB口将iPhone连接到Mac上。
2. 使用Xcode的organizer工具获取到iPhone的UDID
3. 使用rvictl命令创建RVI接口(remote virtual interface),使用iPhone的UDID作为参数。

```
$ rvictl -s <UDID>
```

如果想捕获多个设备的网络包，可以使用上述命令创建多个设备的RVI,传递每个iOS设备的UDID作为参数即可。RVI虚拟接口的命名规则为rvi0,rvi1,rvi2,…,可使用ifconfig命令查看

```
$ ifconfig rvi0
 rvi0: flags=3005<UP,DEBUG,LINK0,LINK1> mtu 0
```

在Mac上使用任意抓包工具tcpdump、wireshark等，监听创建的rvi接口即可。使用完之后需要将创建的虚拟接口移除$ rvictl -x <UDID>