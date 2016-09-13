macOS中的`TOOLCHAINS`环境变量用来控制执行哪个版本的`swift`。

```
$ xcrun --find swift
```

该命令用于显示当前使用的`TOOLCHAINS`

```
$ swift --version
```

该命令用于显示当前使用的`swift`的版本

```
$ export PATH=/Library/Developer/Toolchains/swift-latest.xctoolchain/usr/bin:"${PATH}"
```

该命令用于设置使用指定的swift版本