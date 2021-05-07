title: Android资源采集
abbrlink: '0'
tags:
  - Android
categories:
  - 技术
author: Gavin
date: 2021-04-28 17:31:00
---
## CPU
<!-- more -->

### 命令：

```bash 
adb -s 042461198L000964 shell dumpsys cpuinfo
```

### 结果解析：
![alt text](/text_image/cpu.png)

##### 第一行

```bash 
其中第一行：Load: 10.1 / 2.49 / 0.83
表示系统1min/5min/10min cpu负载平均值
标准说明：1/0.7---理论上单核满载是1，但是在真实情况下，满载会存在性能问题，一般不超过70%；四核满载则是4.
```
##### 第三行

```bash 
75% 593/system_server: 58% user + 16% kernel / faults: 33566 minor 473 major
该行表示资源消耗情况；
593/75%/58%/16%：pid/cpu负载/用户空间负载/内核空间负载

```

### 命令扩展
```bash 
adb -s 042461198L000964 shell dumpsys cpuinfo | grep -w com.afmobi.boomplayer:

```

参考：[dumpsys工具cpu采集说明](https://blog.csdn.net/lipanpan1030/article/details/108118685)

## MEN

### 命令
```bash
adb -s 042461198L000964 shell  dumpsys  meminfo com.afmobi.boomplayer
```

### 结果解析：
![alt text](/text_image/men.png)
__由图可知：__ 需要解析获取TOTAL之后的值，单位kb

```bash 
#脚本如下
# 得到men的使用情况
def get_men(devices, pkg_name):
    cmd = "adb -s "+devices+" shell  dumpsys  meminfo %s"  %(pkg_name)
    total = "TOTAL"
    get_cmd = os.popen(cmd).readlines()
    print('men:',get_cmd)
    for info in get_cmd:
        print('men_info:',info)
        info_sp = info.strip().split()
        print("info_sp:",info_sp)
        for item in range(len(info_sp)):
            print("item:",item)
            if info_sp[item] == total:
                return int(info_sp[item+1])
    return 0
```
## FPS
__fps为帧率，表示单位时间1s内屏幕刷新的次数，Android6之前存在一个相对标准，帧率不能低于60，即单帧耗时不能大于16.67ms,否则会存在丢帧__
### 命令
```bash
adb -s 042461198L000964 shell dumpsys gfxinfo com.afmobi.boomplayer | grep -A 128 Execute | grep -v '[a-z]' 
```

### 结果解析：
![alt text](/text_image/fps.png)
__如上图 Draw/Prepare/Process/Execute表示一帧被绘制的四个阶段，四个值相加即为一帧的耗时(无数据则很可能是手机的“GPU呈现模式分析”未打开)__

[fps说明](https://blog.csdn.net/weixin_43291944/article/details/98497689)

## FLOW

__流量获取有多个路径，比如通过在tcp_snd文件获取tcp发送流量，在tcp_rcv文件中获取tcp接受流量，在/net/dev中获取总的发送/接受流量，同时也可以通过代理获取流量值__

### 命令
```bash
tcp_send_cmd=adb shell cat /proc/uid_stat/uid_cmd/tcp_snd

tcp_recv_cmd=adb shell cat /proc/uid_stat/uid_cmd/tcp_rcv

sum_send_cmd=adb shell cat /proc/pid_cmd/net/dev|grep "wlan0"|awk "{print $10}"

sum_recv_cmd=adb shell cat /proc/pid_cmd/net/dev|grep "wlan0"|awk "{print $2}"
```

[流量命令]（https://testerhome.com/topics/14310）

## 电量
### 命令
```bash
adb shell dumpsys batterystats --charged com.afmobi.boomplayer
```
__预计的电量值如下__
![alt text](/text_image/dian.png)

[电量/流量说明](https://source.android.com/devices/tech/power/batterystats?hl=zh-cn)

