# Xcode升级后插件失效的原理与修复办法
该文章为转帖加剪裁，原文传送门：[原文地址](http://joeshang.github.io/2015/04/10/fix-xcode-upgrade-plugin-invalid/)

Xcode 的插件丰富了 Xcode 的功能，而且有了 Alcatraz ，插件的管理也非常容易，但是有个非常恼人的问题：一旦升级 Xcode ，插件就失效！

#####问题原因
Xcode 的插件放置在 `~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins` 目录下，为 .xcplugin 格式。每个 xcplugin 中存在一个 Info.plist，其中有一项为 DVTPlugInCompatibilityUUIDs

```
DVTPlugInCompatibilityUUIDs 的作用：

插件通过 DVTPlugInCompatibilityUUIDs 来指定能够运行此插件的 Xcode 版本。
```

因此，`DVTPlugInCompatibilityUUIDs` 中存放的是已适配 Xcode 版本对应的 UUID，Xcode 在启动加载控件时，将当前 UUID 同插件 Info.plist 中 `DVTPlugInCompatibilityUUIDs` 存放的 UUID 数组进行匹配，如果没有匹配项，说明此插件无法在该版本的 Xcode 运行，插件也就失效了。

#####解决办法

将当前版本的 UUID 加到 DVTPlugInCompatibilityUUIDs 中即可。

1. 通过 `find` 命令在插件目录下找到所有插件的 Info.plist 文件。
2. 通过 `defaults read` 读取XCode.app中的Info.plist文件获取UUID
2. 通过 `xargs` 命令对上一步的搜索结果进行循环处理，针对每一个 Info.plist 文件，利用 `defaults write` 命令将当前版本的 UUID 加到 `DVTPlugInCompatibilityUUIDs` 中

```
find ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins -name Info.plist -maxdepth 3 | xargs -I{} defaults write {} DVTPlugInCompatibilityUUIDs -array-add `defaults read /Applications/Xcode.app/Contents/Info.plist DVTPlugInCompatibilityUUID`
```