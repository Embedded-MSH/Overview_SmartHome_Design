# OpenWrt 基本配置

## 基本网络设置

### 硬件情况

我这里使用 PVE 虚拟机安装的 Openwrt 举例，四个 2.5G 网口里直通了 `2、3、4` 网口，网口 1 为管理口半虚拟化，硬件配置如下：

<img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20230910202847.png" width="700px"/>

实际进入 OpenWrt 系统后显示如下，其中 eht1 为 WAN 口，其他口为 LAN 口：

> 由于 eth0 半虚化会显示为半双工，实测性能与全双工差别不大损耗 %5 左右，其他口由于接入的设备是千兆所以这里协商没有显示为 2.5G。

<img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20230910203500.png" width="700px"/>
