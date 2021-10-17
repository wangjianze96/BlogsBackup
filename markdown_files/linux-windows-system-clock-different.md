[reference](https://www.zhihu.com/question/46525639/answer/157272414)

# 问题原因

Windows把计算机硬件时间当作本地时间(local time)，所以在Windows系统中显示的时间跟BIOS中显示的时间是一样的。

Linux/Unix/Mac把计算机硬件时间当作 UTC， 所以在Linux/Unix/Mac系统启动后在该时间的基础上，加上电脑设置的时区数（ 比如我们在中国，它就加上“8” ），
因此，Linux/Unix/Mac系统中显示的时间总是比Windows系统中显示的时间快8个小时。

所以，当你在Linux/Unix/Mac系统中，把系统现实的时间设置正确后，其实计算机硬件时间是在这个时间上减去8小时，所以当你切换成Windows系统后，会发现时间慢了8小时。就是这样个原因。

# 解决方法

在Linux中把计算机硬件时间改成系统显示的时间，即禁用Linux的UTC。
```
timedatectl set-local-rtc 1 --adjust-system-clock
```
然后重新启动Linux。
