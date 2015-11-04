# Cocoapods在El Captain中的安装问题

在10.11系统中，由于/usr/bin/ 被系统保护，在这种情况下，如果你使用如下的命令：

```
sudo gem install cocoapods -v
```

就会出现这样的提示：
```
ERROR:  While executing gem ... (Errno::EPERM) Operation not permitted - /usr/bin/pod。
```

为了解决上面提到的问题，以及Cocoapods在OS X 10.11系统上的正常使用，我们需要改变cocoapods的安装位置，命令如下：


```
sudo gem install -n /usr/local/bin cocoapods
```

这样就能解决Cocoapods在10.11系统上出现的问题了。