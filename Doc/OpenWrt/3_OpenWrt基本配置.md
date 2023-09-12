# OpenWrt 基本配置

## 基本网络设置

### 硬件情况

我这里使用 PVE 虚拟机安装的 Openwrt 举例，四个 2.5G 网口里直通了 `2、3、4` 网口，网口 1 为管理口半虚拟化，硬件配置如下：

<img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20230910202847.png" width="700px"/>

实际进入 OpenWrt 系统后显示如下，其中 eht1 为 WAN 口，其他口为 LAN 口：

> 由于 eth0 半虚化会显示为半双工，实测性能与全双工差别不大损耗 %5 左右，其他口由于接入的设备是千兆所以这里协商没有显示为 2.5G。

<img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20230910203500.png" width="700px"/>

### 接口配置

我们主要需要两种接口 **WAN 与 LAN** 口，在接口管理中添加分别添加如下接口：

<img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20230912214157.png" width="1000px"/>

- 选择添加新接口，按如下配置添加 WAN 口，端口选择你想要设定的端口，注意协议选择 DHCP 客户端：

  <img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20230912214331.png" width="600px"/>

- 按如下配置添加 LAN 口，使用桥接绑定多个 LAN 口桥接，协议选择静态地址，这里配置的 ip 将是内网的 ip 段：

  <img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20230912220641.png" width="600px"/>
