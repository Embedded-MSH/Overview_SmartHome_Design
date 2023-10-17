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

## 配置去广告 ADGuard

1. 确认核心版本是否有更新到最新。
2. 设置重定向类型，分为如下几种：
   - 作为 dnsmasq 的上游服务器，指的是将 dnsmasq 作为本地 DNS 服务器，将请求转发到 adguardhome 进行解析。
   - 使用 53 端口替换 dnsmasq，指的是使用 adguardhome 代替 dnsmasq，监听 53 端口，作为本地 DNS 服务器。
   - 重定向 53 端口到 adguardhome，指的是将本地 DNS 服务器的 53 端口请求重定向到 adguardhome 上进行处理。
3. 开启使能，点击服务端口进入 ADGuard 后台设置。

<img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20231017201542.png" width="600px"/>

## 配置 Clash

如下配置是在使用 ADGuard 的基础上：

1. DNS 设置禁止劫持。

<img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20231017203415.png" width="600px"/>

2. 将 AdGuard Home 的上游 DNS 设置成 OpenClash 的 DNS 监听端口 127.0.0.1:7874（端口在全局设置 基础设置里可以查看）
3. AdGuard Home 重定向模式选择“使用 53 端口替换 dnsmasq”
4. OpenClash 的 DNS 设置里启用 自定义上游 DNS 服务器，自定义上游 DNS 服务器选择国内和 google 的 DNS 地址。

<img src="https://fastly.jsdelivr.net/gh/HATTER-LONG/Resource@main//images/Embedded-MSH/20231017204340.png"/>
