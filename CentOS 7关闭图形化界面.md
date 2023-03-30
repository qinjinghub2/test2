centos

# CentOS 7关闭图形化界面

## 1.为什么要关闭图形化界面

CentOS的图像化界面比较消耗内存，且CentOS在实际时间主要用于服务器端，很少使用图形化界面。而且学习过程中养成使用命令行界面有助于我们熟悉linux命令。以下给出centos 7关闭图形化界面的方式。与centos6的区别在于不用再麻烦的配置inittab文件了。

## **2.关闭图形化界面**

1.直接通过命令将系统启动模式设置成完全的多用户模式即可

```delphi
systemctl set-default multi-user.target
```

2.可以通过以下下命令查看当前的启动模式（检验设置是否成功）

```swift
systemctl get-default
```

3.设置成功后，重启机器（虚拟机）即可

```undefined
reboot
```

再次启动就是默认不开启图形化界面。当然，如果想回到原本的有图形化界面的模式，只需切换成图形GUI模式即可

```delphi
systemctl set-default graphical.target
```

同样重启机器之后设置便能生效。

## 4.临时启动图形化界面

```swift
init 5
```

或者

```undefined
startx
```

 两种启动图像化界面的区别：

init 5会重新启动（需要重新登录），startx不会，建议使用startx。1234
